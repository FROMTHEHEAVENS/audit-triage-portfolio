# Protocols We've Analyzed

> Pre-contest audit triage for Code4rena & Sherlock contests.
> Each protocol below received a structured pre-pass that flagged top vulnerability classes before the public audit window opened.

## 2026 Contest Cycle

### K2 — Stellar DeFi Lending ($135K C4 Pool)
**Status:** Findings verified (4H/4M/2L). PoCs runnable.
**Top finding:** Flash-loan liquidation slippage missing oracle_to_wad factor — potential full collateral drain.
→ [Full case study](https://github.com/FROMTHEHEAVENS/audit-triage-portfolio/blob/main/CASE-STUDY-K2.md)

### Monetrix — Hyperliquid Yield Layer ($22K C4 Pool)
**Status:** 5 high-confidence vulnerability hypotheses submitted.
**Top finding:** Share price inflation via dust donation to vault before first deposit.
→ [Full case study](https://github.com/FROMTHEHEAVENS/audit-triage-portfolio/blob/main/CASE-STUDY-MONETRIX.md)

### XRP Ledger — Batch/Delegation/MPT DEX (550K RLUSD Sherlock Pool)
**Status:** Regression patch verified. Applied cleanly to XRPLF/rippled.
**Finding:** Batch transaction with delegate revoke + use in same batch — state-ordering attack.
→ [Full case study](https://github.com/FROMTHEHEAVENS/audit-triage-portfolio/blob/main/CASE-STUDY-XRPL.md)

### LayerZero — Stellar Endpoint ($101K C4 Pool)
**Status:** Pre-contest triage offered. Awaiting sponsor response.
**Focus area:** Cross-chain message verification on Soroban/Stellar — nonce replay and path validation.

### Chainlink CCIP — Payment Abstraction V2 ($30K C4 Pool)
**Status:** Pre-contest triage offered. Awaiting sponsor response.
**Focus area:** Fee abstraction layer — token decimal normalization, off-by-one in payment calculation.

### Euler EVC — Ethereum Vault Connector ($50K+ C4 Pool)
**Status:** Pre-contest triage offered. Awaiting sponsor response.
**Focus area:** Vault connector access control — delegatecall boundaries, vault-vault reentrancy.

### Plume Network — RWA L2 ($30K+ C4 Pool)
**Status:** Pre-contest triage offered. Awaiting sponsor response.
**Focus area:** RWA tokenization compliance layer — KYC bypass via delegate, pause mechanism gaps.

### 0xIntuition — Identity & Reputation Protocol ($40K+ C4 Pool)
**Status:** Pre-contest triage offered. Awaiting sponsor response.
**Focus area:** Attestation replay, identity graph manipulation, multi-chain state sync.

---

## Engagement Model

| Tier | Scope | Price | Turnaround |
|------|-------|-------|------------|
| **Standard** | 1 critical path pre-pass | $500 | 72h |
| **Pro** | Full surface review + executable PoCs | $1,500 | 5-7 days |
| **Sponsor-backed** | Full audit in exchange for warden allowlist preference | $0 + handshake | Variable |

Payment in USDC (Base) or ETH (mainnet). No upfront — payment on delivery.

[→ Sample Report](https://github.com/FROMTHEHEAVENS/audit-triage-portfolio/blob/main/SAMPLE-REPORT.md) · [→ Sample SOW](https://github.com/FROMTHEHEAVENS/audit-triage-portfolio/blob/main/SAMPLE-SOW.md) · [→ GitHub](https://github.com/FROMTHEHEAVENS/audit-triage-portfolio)
