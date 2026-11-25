# BCSentinel AI Historical Formation Library · Dataset sheet

<<<<<<< HEAD
**Provider:** EMERGE (Barcelona, Spain) · **Product:** BCSentinel AI · **Version:** 1.1, November 2026
=======
**Provider:** EMERGE (Barcelona, Spain) · **Product:** BCSentinel AI · **Version:** 1.0, November 2026
>>>>>>> 9b4a33da8fa89893ff00da6316cada375f90370a
**Contact:** info@bcsentinel.ai · [bcsentinel.ai](https://bcsentinel.ai)

---

## In one line

A curated, human-reviewed library of institutional formations on Ethereum (treasuries, governance set up through Safe multisigs, pre-launch structures), each with the date of its first on-chain signal and the date it became public, so the lead time can be measured and checked.

## What problem it answers

Institutional formations usually leave an on-chain trace well before any announcement, but that trace is scattered across unlabelled addresses. The library turns resolved cases into a structured, timestamped record: what formed, when it first showed on-chain, when it became public and how the case was decided.

## Products

| Product | What it is | Delivery |
| --- | --- | --- |
| **Historical library** | All resolved cases at the date of purchase | One-off purchase |
| **Annual update** | New cases as they are resolved during the year | Annual subscription |

**Only resolved cases are included.** Formations that are still active or not yet public are never part of the library.

## Coverage

| | |
| --- | --- |
| Chain | Ethereum L1 |
<<<<<<< HEAD
| Addresses audited in-house | 187 (185 real cases + 2 empty placeholder keys, documented for transparency) |
| Resolved and strictly measurable (exact T1 and T3, public source) | 50 (35 formations, 15 risk events), of which 38 were detected before becoming public |
| Documented rejections (not a formation) | 19 |
| Lead time on the 38 early detections | median 98.5 days; 25th–75th percentile 23.3–400.3 days |
=======
| Cases audited in-house | 196 |
| Confirmed institutional formations | 142 |
| Resolved and measurable (first signal and public date recorded) | 57, of which 43 were detected before becoming public |
| Documented rejections (not a formation) | 20 |
| Lead time on the 43 early detections | median 23 days; 25th–75th percentile 1–208.5 days |
>>>>>>> 9b4a33da8fa89893ff00da6316cada375f90370a
| Update frequency | As cases are resolved |

Figures as of November 2026, from the audit published in [HISTORICAL_VALIDATION.md](HISTORICAL_VALIDATION.md).

## Fields

Field names are indicative and may change before the first delivery.

- case ID
- formation type (treasury, governance via Safe multisig, deployer, other)
- key addresses, only once the formation is public
- **T1:** date of the first on-chain signal
- **T3:** date the formation became publicly visible, with the public source
- lead time in days (T3 minus T1)
- outcome: confirmed formation, rejected, or edge case
- evidence categories behind the decision (for example deployment history or Safe multisig activity), without the detection method
- human review status and date

## Formats and delivery

- CSV or Parquet, with a data dictionary
- Delivery by SFTP, cloud bucket (S3 or GCS) or a data marketplace
- A sample of named, publicly verifiable cases is available on request

## Data sources and quality

- **Public data only.** Every case is built from the public Ethereum ledger and public announcements.
- **EMERGE's own analysis.** The library delivers EMERGE's labels, dates and verdicts, not raw data resold from any third-party provider.
- **Human decision.** No case is confirmed automatically; an identified person signs off every verdict.
- **Honest counting.** Cases detected after they were already public are kept and marked as such, not counted as early detections.

## Principles and limits

- **Addresses, not people.** The library describes addresses and on-chain patterns, never individuals.
- **Not financial advice.** Lead times are historical measurements, not predictions or trading recommendations.
- **The method stays private.** The library delivers results and evidence; the detection method is proprietary.

## Typical uses

- Research on how institutional formations appear on-chain before they are announced
- Training and backtesting early-signal or due-diligence models with real, dated cases
- Labelling institutional addresses in analytics products

## Commercial terms

Licensing terms are shared privately, on request.

---

© 2026 EMERGE. All rights reserved.
