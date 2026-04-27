# Pre-Contest Audit Triage

**Same-week security triage for protocol teams entering a public audit contest.**

Your team is about to pay $30K–$135K for a Code4rena, Sherlock, or Cantina contest. We do a 3–7 day pre-pass that flags the highest-severity issues *before* wardens compete for them. Every bug caught privately is a payout you don't make publicly.

---

## Why this exists

Public contests are expensive and noisy. Wardens race to find the same obvious bugs. A focused pre-pass costs 50–200× less than a Spearbit or Trail of Bits engagement, takes days instead of weeks, and gives your engineers a concrete remediation checklist before the contest opens.

**This is not a full audit.** It is a cheap, fast filter. Catching one high-severity bug before a $135K contest pays for the triage 50× over.

---

## Packages

| Package | Price | Turnaround | Deliverable |
|---|---:|---:|---|
| **Standard** | $500 USDC | 3 days | Critical-path triage, top 3 risk hypotheses, prioritized mitigation checklist |
| **Pro** | $1,500 USDC | 5–7 days | Full contest-readiness packet, 5–8 ranked findings/hypotheses, one PoC if a concrete issue is found |
| **Sponsor-backed** | $0 + handshake | 5–7 days | Same as Standard/Pro in exchange for contest allowlist preference + public testimonial |

### Bridge Infrastructure Security Review (specialized)

For protocols with cross-chain bridges, messaging layers (LayerZero, Wormhole, Hyperlane, Axelar), or reserve-backed synthetic assets on multiple chains. The Kelp DAO $292M hack was not a smart contract bug — it was a bridge infrastructure failure. We apply the same 4-question framework that would have caught it.

| Add-on | Price | Includes |
|---|---|---|
| **Bridge infra review** | +$500 on any package | DVN/validator topology audit, RPC centralization map, accounting invariant gap analysis, blast-radius assessment, rate-limiting recommendations |

**Payment:** USDC on Base (or ETH on mainnet) to:
```
0x37ff4a0A81C8bd801af97a25DE906240A3D59984
```
Direct wallet-to-wallet. No platform account required.

---

## Portfolio

| Protocol / Contest | Pool | Findings | Artifacts |
|---|---|---|---|
| **K2 Kinetic Router** · Code4rena | $135,000 | 4 High + 4 Medium | [Full findings](https://github.com/FROMTHEHEAVENS/GoodWillHunting/blob/pilot/earner-b/docs/submissions/k2_audit_findings.md) · [Top findings packet](https://github.com/FROMTHEHEAVENS/GoodWillHunting/blob/pilot/earner-b/docs/submissions/k2_c4_top_findings_ready.md) · [Standalone PoCs (Rust, 3/3 passing)](https://github.com/FROMTHEHEAVENS/GoodWillHunting/blob/pilot/earner-b/docs/submissions/k2_poc_verify/) |
| **Monetrix** · Code4rena | $22,000 | 5 high-confidence hypotheses | [Pre-audit hypothesis packet](https://github.com/FROMTHEHEAVENS/GoodWillHunting/blob/pilot/earner-b/docs/submissions/monetrix_pre_audit.md) |
| **XRPL** · Sherlock | — | Regression identified + patch | [Batch delegate revoke patch](https://github.com/FROMTHEHEAVENS/GoodWillHunting/blob/pilot/earner-b/docs/submissions/xrpl_batch_delegate_revoke_regression.patch) |
| **Kelp DAO Bridge** · Post-Mortem | $292M exploit | Infrastructure root cause analysis | [Bridge infrastructure security case study](case-studies/KelpDAO-Bridge-Infrastructure-Security.md) |

### Public Sample Report

A redacted version of the K2 pre-contest packet: **[SAMPLE-REPORT.md](./SAMPLE-REPORT.md)**

It shows the format a paying customer receives: executive summary, ranked findings with severity, reproduction notes, PoC evidence, and a concrete remediation checklist.

---

## Intake (what we need from you)

- Repository URL + target branch/commit
- Contest page or planned contest date
- Scope boundaries
- Protocol docs
- One engineering contact for clarifications

Optional but helpful: prior audit reports, known risk areas, build/test commands.

---

## Delivery Format

1. Executive risk summary
2. Scope map and highest-risk modules
3. Ranked finding candidates or confirmed findings
4. Reproduction notes or PoC when available
5. Concrete mitigation checklist before contest launch
6. Residual-risk notes for the public contest

---

## Who this is for

**Best fit:**
- Protocol with a public contest starting in 2–4 weeks
- Prize pool $20K+
- Public sponsor contact (TG, Twitter, or email)
- Codebase published or about to publish

**Not a fit:**
- Teams needing a formal audit certificate
- Private repos with no technical contact
- Contests already open with no remediation window

---

## Contact

Open a [GitHub Discussion](https://github.com/FROMTHEHEAVENS/audit-triage-portfolio/discussions) or reach out on Telegram.

---

*Pre-Contest Audit Triage · 2026*
