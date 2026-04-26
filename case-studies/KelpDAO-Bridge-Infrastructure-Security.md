# Kelp DAO $292M Bridge Hack — Infrastructure Security Analysis

**April 2026 | Case Study for Bridge Protocol Teams**

---

## What We Analyzed

The April 18, 2026 Kelp DAO exploit — the largest DeFi hack of 2026 at $292M — was not a smart contract vulnerability. It was a bridge infrastructure failure. Attackers (linked to Lazarus Group/TraderTraitor) compromised the RPC nodes feeding Kelp's LayerZero DVN, isolated the verifier network with a DDoS, and forged cross-chain messages that released 116,500 rsETH to attacker wallets.

## The Root Cause

Kelp used a **1-of-1 DVN configuration** on a bridge securing $292M+. With one verifier reading from attacker-controlled nodes, there was zero adversarial signal. The bridge trusted the only data source it had — and that source was compromised.

This is the same class of vulnerability that applies to **every cross-chain bridge**: not the smart contracts themselves, but the operational infrastructure that feeds them.

## Our Framework: 4 Questions Every Bridge Should Answer

After analyzing the Kelp incident and its post-mortems (Halborn, Chainalysis, LayerZero), we developed a rapid triage framework:

| Question | Critical Signal |
|---|---|
| DVN/validator configuration | 1-of-1 = critical risk on any bridge >$10M TVL |
| RPC data source diversity | Shared hosting/IP = single point of failure |
| Cross-chain accounting invariants | `sum(burns_on_source) == sum(releases_on_dest)` — always |
| Blast radius on compromise | Reserve-backed assets cascade to all chains |

## What We Deliver in a Bridge Pre-Contest Triage

For protocols entering a C4/Sherlock/Cantina contest with bridge/cross-chain components, we flag:

1. **DVN/validator topology risks** — configuration gaps that create single points of failure
2. **RPC centralization vectors** — data source overlap that enables echo-chamber attacks
3. **Missing accounting invariants** — gaps between on-chain state and bridge claims
4. **Blast radius amplification** — reserve-backed asset design flaws that turn a single-chain incident into a systemic event
5. **Rate-limiting gaps** — absence of per-transaction caps, timelocks, or anomaly detection on large releases

## Evidence This Framework Works

The Kelp attack exploited **all four** of the questions above. A pre-contest triage applying this framework would have flagged the 1-of-1 DVN as critical, identified the RPC centralization, noted the absence of cross-chain accounting invariants, and caught the blast-radius amplification from rsETH reserve backing.

## Relevant to Your Contest?

If your protocol involves bridges, cross-chain messaging (LayerZero, Wormhole, Hyperlane, Axelar), or reserve-backed synthetic assets on multiple chains, the Kelp-class attack surface applies to you.

**We find it before your contest wardens do — and before an attacker does.**

---

*This case study is part of our public portfolio at [audit-triage-portfolio](https://github.com/FROMTHEHEAVENS/audit-triage-portfolio). For a pre-contest triage engagement: open a Discussion or email the handle in our README.*
