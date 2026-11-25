# Historical validation: what the real backtest dataset actually shows

**Published:** November 2026, rebuilt from a full line-by-line audit of the real dataset, reconstructed entirely from public sources (the public Ethereum ledger plus open, independently checkable announcements and press coverage).
**Source:** `formation_labels.json`, BCSentinel's internal case-labeling file — 187 raw keys, of which 2 are empty administrative placeholders with no address attached, leaving **185 real investigated cases**. Every number on this page was computed directly from that file by an independent script, not carried over from an earlier summary — the methodology is stated explicitly so anyone can reproduce it.
**Scope of this claim, stated precisely:** this is a retrospective audit of BCSentinel's case-labeling history, most of it built with `investigate_wallet_v6` plus a separate real-time detection layer, not a live output of the current production engine (`investigate_wallet_v7`). See "Which engine produced this" below for why that distinction matters and isn't hidden.

## The honest breakdown of all 187 keys

| Outcome | Count |
| --- | --- |
| Formation confirmed and measurable (exact T1 and T3) | 35 |
| Formation confirmed, partial date (month, not exact day) | 2 |
| Risk event confirmed and measurable (exact T1 and T3) | 15 |
| Risk event confirmed, partial date | 2 |
| Special case (ambiguous attribution, excluded from the measurable count) | 1 |
| Formation detected, no public source for T3 yet | 15 |
| Formation detected, source found but no exact date | 9 |
| Risk pattern detected, no public source for T3 yet | 7 |
| Risk pattern detected, source found but no exact date | 5 |
| Preliminary identification, pending final human review | 19 |
| Pattern detected, project not yet identified | 46 |
| Identified: internal component of an already-public protocol | 10 |
| Correctly rejected by the system itself (not relevant) | 9 |
| Reviewed and discarded (not a formation — control group) | 10 |
| Empty entries (no content) | 2 |
| **Total** | **187** |

Of the 185 real cases, **50 meet the strict bar for measuring lead time honestly**: both T1 (first on-chain signal) and T3 (public-visibility date) exact and backed by a verifiable public source — 35 formations and 15 risk events. The other 135 are real, identified addresses that simply don't clear that bar yet (no exact public date, an internal component of an already-public project that structurally has no announcement of its own, or still pending the final human review step) — they are excluded from the lead-time numbers below rather than guessed at.

## Lead time: 38 early detections, 12 after-the-fact validations

Of the 50 measurable cases, **38 were detected before the fact became public** (genuine early warning: 27 of 35 formations, 11 of 15 risk events) and **12 were detected the same day or after** — in most of these, the wallet belonged to an already-public entity's later operational infrastructure (its treasury, an operator multisig), not its founding moment. This isn't noise: it's a documented, expected pattern (see the Gitcoin multisig case below) and these 12 are excluded from the lead-time statistics, not averaged in to pad the numbers.

Distribution of lead time across the 38 genuine early detections:

| Metric (days of lead time) | Value |
| --- | --- |
| Median | 98.5 |
| 25th percentile | 23.3 |
| 75th percentile | 400.3 |
| Mean | 284.6 |
| Minimum | 1 |
| Maximum | 1,651 |

Split by type: formations (27 of 35 early) have a median of 98 days; risk events (11 of 15 early) have a median of 209 days — risk events tend to sit dormant longer before the triggering incident than formations do before launch.

**Why a negative-lead case still counts as a correct result, not a failure:** one excluded case is the operator wallet of Gitcoin's treasury multisig, deployed on 2018-07-08 — 286 days after Gitcoin itself was already public (2017-09-25). The wallet is a real, correctly-identified piece of institutional infrastructure; it just isn't evidence of early detection, because the entity it belongs to was never hidden. Reporting this as a "miss" would be dishonest in the other direction. The system distinguishes "is this a real formation" from "did we see it before anyone else" on purpose, and only the second question produces a lead-time number.

## Named, independently checkable examples

Everything below is a real on-chain address with a confirmed Etherscan/Blockscout label or deployment history — anyone can verify T1 (first on-chain signal) and T3 (public date) themselves, no trust required. Each T1 below comes directly from the public Ethereum ledger (via Blockscout), not from any third-party analytics provider.

**Legitimate institutional formations, detected early:**

| Case | Lead time | What the wallet is |
| --- | --- | --- |
| ENS | 1,651 days | Early deployer wallet, long predating the ENS DAO's public token launch |
| Safe | 826 days | Early infrastructure deployer, predating the public Safe{Wallet} rebrand |
| Rarible (RARI) | 646 days | Token deployer, extensive direct contract deployments |
| Across Protocol | 496 days | Hub Pool Owner multisig deployer (Risk Labs) |
| Keep3r Network | 281 days | Deployer wallet |
| Synthetix (SNX, then Havven) | 175 days | Deployer of the legacy SNX/Havven contract |
| Convex Finance | 87 days | Booster contract deployer |
| EigenLayer | 5 days | Mainnet-role deployer (a second EigenLayer-labeled wallet in the library has no confirmed public date and is correctly excluded from this list) |

**Risk events, also detected early** — the same methodology that finds legitimate formations also flags risk before the damage becomes public knowledge:

| Case | Lead time | Outcome |
| --- | --- | --- |
| Multichain / Anyswap | 843 days | Later the subject of a major cross-chain exploit, covered by CoinDesk |
| SharedStake (Deployer 1) | 209 days | Confirmed slow rug / insider |
| Safereum | 52 days | Confirmed exit scam |
| Frosties NFT | 23 days | First US federal criminal NFT-rug prosecution |
| AnubisDAO (token deployer) | 1 day | Confirmed exit scam |

A note on integrity: an earlier version of this page also listed three imitation-token cases (tokens sharing a well-known name but not the real project behind it) as "confirmed exit scams." On this rebuild, direct Blockscout verification found no public source tying those specific addresses to a documented incident — they have been moved to "pattern detected, no public source" in the breakdown table above, and removed from this named list, rather than kept on the strength of the earlier claim alone.

## Which engine produced this, and why that's disclosed rather than smoothed over

Per BCSentinel's own freeze documentation for the current production engine (`investigate_wallet_v7_FREEZE_DOC.md`, frozen 28-Oct-2026): `investigate_wallet_v7.py` **never writes to `formation_labels.json`** — it only reads it, as context, when investigating a wallet live. The historical backtest dataset behind the numbers on this page is a separate layer, built over time by the methodology's earlier versions. None of it is tagged as `v7` output, because the backtest pipeline and the live single-wallet engine are architecturally separate by design — v7 is the tool that investigates one wallet today; this dataset is the historical record it reads for context.

That matters for one reason: it would be misleading to say "v7 found all of this." What's true, and verifiable, is that the underlying detection methodology — selector analysis, Safe payload decoding, deploy tracking, now consolidated and extended in v7 — produced these real, checkable results across its earlier versions, and v7 carries the same core logic forward with documented improvements (see `CASE_STUDY.md`).

## How to check this yourself

1. Look up each wallet address on Blockscout or Etherscan and confirm the first transaction or deployment date.
2. Independently confirm each project's public launch date (or, for the risk cases, its exposure date) from any public source.
3. Compare the gap to the lead-time figure above.

No part of this requires trusting BCSentinel's word for any date — the T1 side of every figure above comes directly from the public Ethereum ledger, and the aggregate statistics were computed by a script reading the raw dataset directly. The methodology is reproducible, not asserted.
