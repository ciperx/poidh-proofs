# poidh WebMCP / MCP edge-case findings (probe 2026-10-21, for bounty base#1495, 0.0108 ETH ~ $26)

Endpoint: POST https://poidh.xyz/api/mcp (JSON-RPC 2.0, header `Accept: application/json, text/event-stream`)
Raw evidence: `work/mcp_edge_probe.json`

## Findings (all reproduced with curl-equivalent POST, 2026-10-21)

1. **Tool-not-found uses the wrong JSON-RPC error code.**
   `tools/call` with `name: "nope_tool"` returns `MCP error -32602` ("Tool nope_tool not found").
   JSON-RPC 2.0 defines -32601 *Method not found*; -32602 means invalid params. MCP clients that
   branch on -32601 to fall back (`tools/list` refresh) will instead misread this as a bad-argument
   error and retry with corrected args forever.

2. **`find_bounties` with no arguments returns the whole board in one shot (46 KB / 23 KB of text).**
   No documented default limit is enforced; a model that calls the tool "bare" spends ~6k tokens
   on the first call. No field projection parameter exists, so `description` (often 2-4 KB of prose
   per bounty) always comes back — the poison vector for a small context window.

3. **`get_bounty` is a heavyweight aggregate: 71 KB wire / 35 KB text for a single bounty with 4 claims.**
   Response embeds every claim's full `description` plus `comments`. An agent that fetches 10 bounties
   burns ~350 KB. There is no `fields` / `include` selector and no `limit` on embedded claims/comments.

4. **Invalid `cursor` fails silently instead of erroring.**
   `find_bounties {limit: 2, cursor: "0"}` → `{"bounties": [], "nextCursor": null}` (HTTP/JSON-RPC success).
   Cursor is actually an opaque timestamp; a stale/hallucinated cursor looks identical to
   "board is empty". Agents cannot distinguish end-of-data from bad-input — a silent-truncation bug
   for any pagination loop.

5. **`limit` rejects everything except a bare integer, and the error never states the accepted range.**
   `limit: 1000 / 0 / -5 / "3" / 2.7` all return `Invalid input at limit`.
   Message contains no min/max, so a caller must binary-search the bound by trial (and each trial is a
   round trip). Contrast: many MCP servers return the zod issue path + expected type.

6. **Two different id spaces with confusingly similar names.**
   `get_bounty` returns `id: 1495` (display id) and `onChainId: 509`. Tool args take the display `id`,
   but on-chain data and poidh URLs on some surfaces use `onChainId`. Nothing in the response or tool
   schema warns about it, so an agent that feeds `onChainId` back gets another bounty's data with no error.

## Suggested fixes
- Return -32601 for unknown tool (or keep -32602 but add `data.reason: "tool_not_found"`).
- Enforce `limit` default (e.g. 10) and add `fields: ["id","title","reward"]` projection.
- Make `get_bounty` accept `include=[]` and cap embedded claims/comments.
- Validate `cursor` (opaque, must decode) → error on malformed value instead of empty result.
- Include allowed range in validation errors.
- Document `id` vs `onChainId` explicitly in both tool descriptions.
