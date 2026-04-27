# Pre-Contest Audit Triage — Sample Statement of Work

**This is a template.** Replace bracketed fields for each engagement.

---

## Statement of Work: Pre-Contest Audit Triage

**Date:** [YYYY-MM-DD]  
**SOW #:** [NUMBER]  
**Client:** [Protocol Name / Entity]  
**Auditor:** FROMTHEHEAVENS (GitHub: @FROMTHEHEAVENS)  
**Portfolio:** https://github.com/FROMTHEHEAVENS/audit-triage-portfolio  

---

### 1. Scope

The Auditor will perform a time-boxed security review of the Client's smart-contract codebase in advance of a public bug-bounty contest. The review targets **high-impact vulnerability classes** that, if found publicly during the contest, would generate the largest warden payouts.

**Codebase:** [repository URL, commit hash, file paths]  
**Contest details:** [platform, contest link, prize pool, start date]  
**Focus areas (agreed):** [2-5 contract modules / attack surfaces]

---

### 2. Triage Tier (circle one)

| Tier | Deliverable | Price | Turnaround |
|---|---|---|---|
| **Standard** | 1 critical-path pre-pass: top 3-5 vulnerability classes identified, ranked by severity, with exploitation narrative | $500 USDC | 5-7 days |
| **Pro** | Full surface review: all Standard deliverables + PoC for top 2 findings + mitigation recommendations | $1,500 USDC | 7-10 days |
| **Sponsor-backed** | Pro tier at zero cost in exchange for warden allowlist preference in the Client's contest | $0 + handshake | 7-10 days |

**Selected tier:** [Standard / Pro / Sponsor-backed]

---

### 3. Deliverables

- **Triage Report** (PDF/Markdown): findings ranked by severity (Critical → High → Medium), each with exploitation narrative and affected code paths.
- **PoC** (Standard: 0 / Pro: 2): runnable Foundry test demonstrating the vulnerability.
- **Mitigation Notes** (Pro only): recommended fixes with code-level guidance.
- **Pre-Contest Handoff** (Sponsor-backed only): optional short walkthrough call before contest opens.

---

### 4. Out of Scope

- Gas optimizations
- Code style / documentation
- Centralization risks (unless leading to direct loss-of-funds)
- Formal verification
- Post-contest support (can be added as separate SOW)

---

### 5. Payment

| Tier | Amount | Token | Chain |
|---|---|---|---|
| Standard | $500 | USDC | Base (0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913) |
| Pro | $1,500 | USDC | Base |
| Sponsor-backed | $0 | — | — |

**Payment address:** `0x3c3706A52D47DE8dE75a58d016B951a2e08e86cC` (family treasury; accepts USDC/ETH on Base mainnet)  
**Terms:** 100% upfront for Standard/Pro tiers. Refund if report not delivered within agreed turnaround (minus gas).

---

### 6. Confidentiality & Publication

- **Pre-contest:** All findings remain confidential to the Client until the contest closes.
- **Post-contest:** The Auditor may publish a **redacted** version of the report (findings that were publicly disclosed by wardens during the contest) as a portfolio case study. The Auditor will not publish findings the Client fixes privately without written permission.
- The Client grants the Auditor a non-exclusive right to reference the engagement name and tier in future marketing materials.

---

### 7. Execution

This SOW is executed by agreement between the parties via:
- GitHub Issue acknowledgment (Client comments "Accepted" on the linked Issue), OR
- On-chain transaction (Client sends payment to the address in Section 5 — payment = acceptance)

**No signatures required.** Crypto payment is binding acceptance.

---

### 8. Contact

**Auditor:** `FROMTHEHEAVENS` on GitHub  
**Repository:** https://github.com/FROMTHEHEAVENS/audit-triage-portfolio  
**Telegram:** [provided upon engagement confirmation]
