# Historical validation: what the real backtest dataset actually shows

**Published:** November 2026, rebuilt from a full line-by-line audit of the real dataset.
**Source:** `formation_labels.json`, BCSentinel's internal case-labeling file — 197 raw entries, of which 1 is a non-case administrative placeholder, leaving **196 real investigated cases**. Every number on this page was computed directly from that file by independent script, not carried over from an earlier summary — the methodology is stated explicitly so anyone can reproduce it.
**Scope of this claim, stated precisely:** this is a retrospective audit of BCSentinel's case-labeling history, most of it built with `investigate_wallet_v6` plus a separate real-time detection layer, not a live output of the current production engine (`investigate_wallet_v7`). See "Which engine produced this" below for why that distinction matters and isn't hidden.

## The honest breakdown of all 196 cases

| Outcome | Count |
| --- | --- |
| Confirmed real institutional formation | 142 |
| Correctly rejected (not a formation) | 20 |
| Still pending human review | 32 |
| Genuine edge case (ambiguous founder role, or no wallet to study) | 2 |

Of the 142 confirmed formations, 57 have both a first-signal date (T1) and a public-visibility date (T3) recorded, which is what's needed to measure lead time honestly. The other 85 confirmed formations are real but don't have a usable T3 yet (still privately known, or publicly known but without a documented date) — they are excluded from the lead-time numbers below rather than guessed at.

## Lead time: 43 early detections, 14 after-the-fact validations

Of the 57 measurable cases, **43 were detected before the formation became public** (genuine early warning) and **14 were detected after** — the wallet belonged to an already-public entity's later operational infrastructure (its treasury, an operator multisig), not its founding moment. This isn't noise: it's a documented, expected pattern (see the Gitcoin multisig case below) and these 14 are excluded from the lead-time statistics, not averaged in to pad the numbers.

Distribution of lead time across the 43 genuine early detections:

| Metric (days of lead time) | Value |
| --- | --- |
| Median | 23 |
| 25th percentile | 1 |
| 75th percentile | 208.5 |
| Mean | 178.2 |
| Minimum | 0 |
| Maximum | 1,651 |

**Why a negative-SLT case still counts as a correct result, not a failure:** one excluded case is the operator wallet of Gitcoin's treasury multisig, deployed in 2018 — a full year after Gitcoin itself was already public. The wallet is a real, correctly-identified piece of institutional infrastructure; it just isn't evidence of early detection, because the entity it belongs to was never hidden. Reporting this as a "miss" would be dishonest in the other direction. The system distinguishes "is this a real formation" from "did we see it before anyone else" on purpose, and only the second question produces a lead-time number.

## Named, independently checkable examples

Everything below is a real on-chain address with a confirmed Etherscan label or deployment history — anyone can verify T1 (first on-chain signal) and T3 (public date) themselves, no trust required.

**Legitimate institutional formations, detected early:**

| Case | Lead time | What the wallet is |
| --- | --- | --- |
| Rarible (RARI) | 648 days | Token deployer, 107 direct contract deployments |
| Across Protocol | 496 days | Hub Pool Owner multisig deployer (Risk Labs) |
| Synthetix (SNX, then Havven) | 276 days | Single deployment of the legacy SNX/Havven contract |
| Keep3r Network | 281 days | Deployer wallet |
| Morpho | 109 days | 112 Safe `execTransaction` calls, treasury-led pattern |
| Convex Finance | 85 days | Booster contract deployer |

**Fraud and exit-scam cases, also detected early** — the same methodology that finds legitimate formations also catches bad actors before the damage is done public knowledge:

| Case | Lead time | Outcome |
| --- | --- | --- |
| Multichain / Anyswap (Router deployer) | 813 days | Later flagged by Chainalysis as a probable insider rug |
| SharedStake (Deployer 1) | 209 days | Confirmed slow rug / insider |
| Safereum | 52 days | Confirmed exit scam |
| Frosties NFT | 23 days | First US federal criminal NFT-rug prosecution |
| AnubisDAO (token deployer) | 1 day | Confirmed exit scam |
| ANDY / TNSR / MONAD Token (deployers) | 1 day each | Confirmed exit scams |

## Which engine produced this, and why that's disclosed rather than smoothed over

Per BCSentinel's own freeze documentation for the current production engine (`investigate_wallet_v7_FREEZE_DOC.md`, frozen 28-Oct-2026): `investigate_wallet_v7.py` **never writes to `formation_labels.json`** — it only reads it, as context, when investigating a wallet live. The historical backtest dataset behind the numbers on this page is a separate layer: checking the recorded CSV filenames, roughly 59% of entries were generated through `investigate_wallet_v6`, 23% through a newer real-time detection layer, and the rest were added through manual, Etherscan-label-confirmed identification. None are tagged as `v7` output, because the backtest pipeline and the live single-wallet engine are architecturally separate by design — v7 is the tool that investigates one wallet today; this dataset is the historical record it reads for context, built over time by the methodology's earlier versions.

That matters for one reason: it would be misleading to say "v7 found all of this." What's true, and verifiable, is that the underlying detection methodology — selector analysis, Safe payload decoding, deploy tracking, now consolidated and extended in v7 — produced these real, checkable results across its earlier versions, and v7 carries the same core logic forward with documented improvements (see `CASE_STUDY.md`).

## Why the system still requires a human, in numbers

Of the 125 cases where the automated Confidence Engine recorded a raw classification before any human review, that automated score agreed with the final, human-confirmed verdict in only **39% of cases (49/125)**. In the other 61%, a human reviewer corrected the machine's initial read — overwhelmingly in the direction of catching real formations the automated score had under-rated (the Morpho case above, scored "low relevance" by the engine despite 112 real Safe transactions, is typical of the pattern). This is published specifically because it's the evidence for why no finding in this system is ever auto-confirmed: the raw score alone is wrong more often than it's right, and it's treated that way by design, not retrofitted after the fact.

## How to check this yourself

1. Look up each wallet address on Etherscan and confirm the first transaction or deployment date.
2. Independently confirm each project's public launch date (or, for the fraud cases, its exposure date) from any public source.
3. Compare the gap to the lead-time figure above.

No part of this requires trusting BCSentinel's word for any date, and the aggregate statistics above were computed by a script reading the raw dataset directly — the methodology is reproducible, not asserted.
