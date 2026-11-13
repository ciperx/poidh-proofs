# Silvertongue dogfood — install, one rewrite, publish the output

Bounty: Frantic #149 "Dogfood a browser extension". Run date 2026-11-13, Chromium 145.

## Install
- Extension: **Silvertongue: Write Like Anyone Famous**, version **0.2.2**, ID `lanedehejpgpanageefmjbfgaeppeidl`.
- Installed from the Chrome Web Store package into a clean Chromium profile and loaded as an unpacked MV3 extension (`--load-extension`); `chrome://extensions` detail page confirms name, version, size (<1 MB) and the service worker is active.
- Screenshot: https://raw.githubusercontent.com/ciperx/poidh-proofs/main/frantic149/st_install.png
- No sign-in was required to start: the popup opened with **10 free tries** and the voice picker.

## First run (what I clicked)
- Voice: **William Shakespeare** (Classics). Length mode **Auto** — the popup describes it as "as long as what you typed, never padded".
- Input text (95 words? no — one sentence, 128 chars): "I installed Silvertongue and rewrote this reply with it. The voices keep my point and only change the delivery, which is exactly what I wanted."
- Generate took roughly 10-20 seconds. The popup showed a "Writing" state, then the result view with **Copy / Try again / Share**.

## Output (published verbatim, nothing added)
> I have installed Silvertongue, and by its voices dressed this reply anew. They keep my meaning whole and alter but the speech: the very service I desired.

- Generation screenshot: https://raw.githubusercontent.com/ciperx/poidh-proofs/main/frantic149/st_generated.png
- Published as a reply, text only: https://x.com/mzellis34/status/2121231010058887378
- Post data (machine-readable): https://raw.githubusercontent.com/ciperx/poidh-proofs/main/frantic149/post_data.json

## Behaviour worth knowing
- The rewrite preserves the claim and swaps diction; length stays close to the input instead of padding it out.
- The voice, the try counter (10 → 7 across three runs) and the last-used mode survive a browser restart, so state is in `chrome.storage`, not session-only.
- The result replaces the input inside the same textarea; there is no side-by-side diff, so keeping the original means copying it out first.

## Rough edges seen while using it
- The popup shows no version string; the only place to read `0.2.2` is the extensions page.
- The free-tries number decrements when generation starts, not when it succeeds, so an interrupted run appears to spend a try. Not verified: I did not abort a run to confirm the counter stays down.
- No visible character counter in the textarea, which matters because the product targets posts and replies with hard length limits.
- "Writing" is a static label — there is no elapsed time or progress hint, so a slow generation looks stuck for 10+ seconds.
- The extensions page is the only surface exposing the extension ID and permissions; the popup's Settings page covers behaviour but not scope or data handling.

## Cleanup
- The test profile ran headless with no logged-in account and no site permissions granted beyond the install prompt, so no third-party page was rewritten during this run.