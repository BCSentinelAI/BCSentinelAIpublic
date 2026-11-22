# BCSentinel — investigate_wallet_v7: public technical summary
### Version suitable for a public repository — October 28, 2026

---

**What this document is, and what it isn't.** This is a high-level architecture summary, intended for a **public** GitHub repository, as verifiable proof that the system exists and how it is conceptually built. **It does not include the actual source code, the exact formulas, the weights, or the decision thresholds** — those values are BCSentinel's trade secret (see `GEN-007-Strategic-value-definition.md`) and live only in the private version of this document (`investigate_wallet_v7_FREEZE_DOC.md`) and in the source code, both in the private repository. If this summary and the private document ever contradict each other, the private document is the source of truth — this is only the storefront.

**What BCSentinel is, in one sentence.** An early detection and alert system for crypto entity formation (protocols, DAOs, token launches) from on-chain behavioral patterns, before that formation becomes public. **BCSentinel detects and alerts — it is not investment advice or financial advice, and it does not indicate where or whether to invest.**

---

## 1. What `investigate_wallet_v7.py` answers

Given a wallet address on an EVM-compatible network, the system evaluates whether there is evidence that wallet is acting as a founder or operator of an emerging institutional formation, and what phase of that formation it is in. The system never automatically assigns an entity name — that always requires explicit human confirmation.

## 2. Architecture, at a conceptual level

The analysis combines several sources of on-chain evidence (invoked function selectors, deployed contracts, multisig/governance structure, recent activity) into a four-level abstraction:

```
Level 1: Individual wallet
Level 2: Behavioral signature (activity pattern blocks)
Level 3: Formation Candidate (grouping of related wallets, with a confidence level)
Level 4: Named entity (only after human confirmation)
```

On that basis, a proprietary (unpublished) scoring engine combines three axes of evidence — structural, institutional, and future potential — into a single composite score, which feeds an escalation-level classification (from "no change" to "immediate review"). The exact weighting values and thresholds for each level are trade secrets and are not published here.

The system organizes the analysis into independent enrichment layers (persistent signal memory, silence/pivot pattern detection, archetype classification, estimated time remaining until public visibility, an automatic narrative engine, among others), designed to degrade gracefully: if one layer is unavailable, the system keeps working with the remaining layers, without failing.

## 3. What it does NOT do (scope limits, public by design)

- It does not run arbitrary queries against on-chain sources hidden from view — every query used comes from a fixed definition, audited internally.
- Coverage is limited to a small number of explicitly supported EVM networks.
- It does not automatically merge the identity of the same entity across different networks beyond a simple presence log.
- It never automatically assigns a "confirmed" or "discarded" result — both require human review.
- It does not identify the real identity of the person or organization behind a wallet — it delivers on-chain evidence for a human analyst to investigate that identity.

## 4. Honesty about known limitations

The system internally documents its own known weak points instead of hiding them — among them: the link between wallets in the same "Formation Candidate" is based on pattern overlap, not verification of a real relationship by itself (it requires additional verification before being confirmed as an institutional core); there is a "temporal data confidence" axis that can be in tension with the institutional relevance score, and the system exposes both separately instead of hiding them behind a single number; and multi-network analysis does not automatically deduplicate the same entity active across several networks. None of these limitations is new or hidden — they have been documented in the code itself since they were discovered.

## 5. Empirical validation

A full audit of BCSentinel's internal case-labeling history (196 real investigated cases) found 142 confirmed real institutional formations, 20 correctly rejected cases, and 32 still pending human review. Of the 125 cases where the automated Confidence Engine recorded a raw classification before human review, that raw score agreed with the final human-confirmed verdict in only 39% of cases (49/125) — in the other 61%, a human reviewer corrected the engine's initial read, generally in the direction of catching a real formation the automated score had under-rated. This figure is published specifically because it is the evidence for why the system never auto-confirms a finding: the raw automated score alone is wrong more often than it is right, by design treated as a signal for a human to review, not a verdict. Full methodology and named examples: `HISTORICAL_VALIDATION.md` in the public repository.

---

*This document is the public, deliberately incomplete version of the technical documentation for `investigate_wallet_v7.py`. The complete version, with formulas, weights, thresholds, and source code, is confidential BCSentinel information and is not published.*
