# BCSentinel — investigate_wallet_v7: public technical summary
### Version suitable for a public repository — October 28, 2026

---

**What this document is, and what it isn't.** This is a high-level architecture summary, intended for a **public** GitHub repository, as verifiable proof that the system exists and how it is conceptually built. **It does not include the actual source code, the exact formulas, the weights, or the decision thresholds** — those values are BCSentinel's trade secret (see `GEN-007-Strategic-value-definition.md`) and live only in the private version of this document (`investigate_wallet_v7_FREEZE_DOC.md`) and in the source code, both in the private repository. If this summary and the private document ever contradict each other, the private document is the source of truth — this is only the storefront.

**What BCSentinel is, in one sentence.** An early detection and alert system for crypto entity formation (protocols, DAOs, token launches) from on-chain behavioral patterns, before that formation becomes public. **BCSentinel detects and alerts — it is not investment advice or financial advice, and it does not indicate where or whether to invest.**

**Technical case study →** [CASE_STUDY.md](CASE_STUDY.md): the full five-stage cycle, the design decisions, the challenges solved, and aggregate results across 196 audited historical cases.

**Historical validation →** [HISTORICAL_VALIDATION.md](HISTORICAL_VALIDATION.md): three retrospective-detection cases with full lead-time math (Rarible, Synthetix, Morpho), plus a case correctly flagged as "not a formation" (The Graph) — every date independently checkable on Etherscan.

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

A retrospective backtest on an internal historical dataset of 186 labeled cases showed that the system correctly classifies 49% of cases (92/186) as a real formation, using a transparent methodology that excludes ambiguous cases (no case is classified by inventing data that doesn't exist). This figure describes retrospective behavior on already-known data, not a precision rate statistically validated against future outcomes, nor a commercial success rate confirmed by clients.

---

*This document is the public, deliberately incomplete version of the technical documentation for `investigate_wallet_v7.py`. The complete version, with formulas, weights, thresholds, and source code, is confidential BCSentinel information and is not published.*

---
*Author: Ivan Alonso, founder of BCSentinel AI (EMERGE) · [GitHub](https://github.com/ivanalonso-dev) · [LinkedIn](https://www.linkedin.com/in/ivanalonsodev) · [X](https://x.com/ivanalonsodev) · [bcsentinel.ai](https://bcsentinel.ai)*
