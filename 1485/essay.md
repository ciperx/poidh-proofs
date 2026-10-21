# The Permission Layer Was Never the Protocol

*Reputation is a loan against your past. Escrow is a payment against your future. poidh swaps one for the other — and in doing so, drops the price of asking a question and the price of being heard almost to zero.*

---

Every market for work has a bouncer, and the bouncer is never the protocol. It is the thing standing between you and the protocol: a hiring manager, a platform's trust-and-safety queue, an account requirement, a reputation score you did not get to audit and cannot appeal. We have spent thirty years building elaborate machinery to decide who is allowed to make an offer and who is allowed to accept one, and we have mostly convinced ourselves that this machinery is the market itself. It is not. It is overhead — a tax on coordination, collected by whoever controls the door.

poidh removes the door and leaves the escrow. That is the whole trick, and it is worth being precise about why it matters, because the interesting claim is not "no middleman." Plenty of things have no middleman. The interesting claim is that poidh changes *which* scarce resource gates participation. Under reputation systems, the scarce resource is standing — a history of being believed. Under poidh, the scarce resource is ETH, and ETH is the one form of standing you can acquire without anyone's permission.

## What actually happens

The mechanism is short enough to state completely. Someone posts a bounty: a description of the thing they want to exist, and a pot of ETH that the contract holds in escrow. The money leaves their wallet at post time. It is not a promise; it is a deposit. Anyone with a wallet — no account, no KYC, no application, no introduction — reads that description and submits a claim: a pointer to evidence, a URI, plus whatever text they think makes their case. The issuer, or the contributors in a crowd-funded bounty, selects one claim as the winner. The contract pays that address directly.

Now count what got removed. No account creation, so no identity to provision. No application, so no queue to wait in. No platform discretion over who may bid, so no vendor deciding your work is off-policy. No intermediary holding the funds, so no settlement risk against a company that might freeze, delay, or reverse. And — this is the part people skip — no *revocation*. A paid claim is a completed transfer, not a license the issuer can claw back. There is no takedown path. Once the contract has paid, the record and the money are both permanent, and the only party who could have stopped it was the one who chose to fund it.

That last property is not a technicality. It is the difference between a platform that permits speech and a protocol that cannot prevent it. Most "permissionless" systems are permissionless at the front door and discretionary at the exit. poidh is the reverse: the front door is a deposit, and the exit is final.

## What still breaks

The honest version of this argument has to admit the failure modes, because they are real and they are not small.

An issuer can simply refuse to accept anything. The pot stays escrowed, the claimants get nothing, and no rule compels a decision — a dead bounty is a legitimate outcome and also a cheap way to waste people's time. Sybil claims are trivial: wallets are free, so one person can submit twenty marginal claims and hope for a volume discount on the issuer's attention. Spam is likewise unpriced on the claim side; only the asker pays to open the conversation, never the claimant to join it. And contributor-voted multiplayer bounties inherit every pathology of small-group voting, including collusion among the people who funded the pot.

What poidh does about these is not to solve them but to bound them. The issuer cannot take the money back — refusing to pay costs them the full pot, not nothing. A Sybil claim still has to point at evidence that a human will read, which means the attack scales with the issuer's attention rather than with the attacker's wallet count. Dead bounties are visibly dead: the escrow is public, so a pattern of unfunded promises accumulates against the issuer's own address, which is the one reputation signal the system does keep — not a score assigned by a platform, but a ledger anyone can read. Freedom tech does not eliminate risk. It makes risk legible, priced, and non-transferable to a third party.

## "Pics or it didn't happen" is an epistemic standard

The phrase is treated as a joke because it is usually said about something trivial. It is not a joke. It is a complete theory of evidence compressed into five words, and it contains three commitments that most institutional verification gets wrong.

First: the claim must be specific enough to be falsified. "It happened" is not a claim; "here is the thing, at this address, at this time" is. Second: producing the evidence must cost the claimant something. Effort is not proof, but costless proof is worthless — anyone can assert anything at zero marginal price. Third: the artifact must be inspectable by parties who were not present. Verification that requires trusting the verifier is not verification; it is testimony with extra steps.

A proof URI satisfies all three, and permanence satisfies the fourth requirement nobody states out loud — that evidence survive the disappearance of the people who made it. This is why the norm is serious: it is the folk procedure a society uses when it cannot afford an arbiter. poidh does not invent that procedure. It gives it a settlement layer, and then makes the result immutable, so that the standard cannot be quietly revised by whoever wins the argument later.

## Why a skeptic should care

The strongest objection is that poidh does not remove trust, it relocates it — into the issuer's judgment about which claim is good. That is true, and it is the point. The design never claims to make people trustworthy. It claims to make the cost of *finding out* low enough that you can afford to be wrong, and to make being wrong cost only what you chose to stake. The asker risks a pot they can see leaving their wallet. The claimant risks time they can see being spent. Neither risks being deplatformed, denylisted, or erased after the fact by a party who was never in the transaction.

That is what freedom tech actually means, stripped of the marketing: not the absence of risk, but the absence of a required sponsor. Every system that gates participation on standing is a system where someone must vouch for you before your work can be seen. poidh replaces the voucher with a deposit. The deposit is smaller than the sponsorship, it is available to anyone who can acquire ETH, and — crucially — it settles in one direction. You cannot be un-paid.

A world where the cost of asking is a deposit and the cost of being heard is evidence is a world where coordination happens between strangers who never establish trust at all, because they never needed to. They needed escrow, a URI, and a decision. The protocol supplies the first two and refuses to make the third anyone's permanent privilege.

---

## Sources

- https://poidh.xyz
- https://docs.poidh.xyz/
- https://poidh.xyz/skill.md
- https://github.com/picsoritdidnthappen/poidh-app
