# Case Study: K2 Lending ($135,000 Contest Pool)

**Date:** 2026-04  
**Contest:** Code4rena — K2 Lending Protocol  
**Pool size:** $135,000  
**Our deliverables:** Pre-contest triage identifying 4 HIGH + 4 MEDIUM vulnerabilities  
**Frontrunning value:** Every finding caught privately avoids public payout + reputation damage

---

## What We Found

### HIGH-01: Liquidation front-running via inflight oracle price
**Mechanism:** Attacker monitors mempool for Chainlink price update → submits liquidation tx with same block price → extracts liquidation bonus before honest liquidators can react.  
**Impact:** Systematic MEV extraction from every liquidation event.  
**Fix:** Require minimum block delay between oracle update and liquidation eligibility.

### HIGH-02: Interest rate manipulation through flash-loan amplified deposits
**Mechanism:** Flash loan → deposit → borrow → withdraw → repay flash loan — all in one tx. Manipulates utilization ratio to exploit interest rate curve discontinuities.  
**Impact:** Protocol lending pools lose yield; depositors receive deflated rates.  
**Fix:** Per-block deposit/withdraw rate limiting or TWAP-based utilization.

### MEDIUM-01 through MEDIUM-04
Including: liquidation bonus precision loss, admin timelock bypass via governance delegate chaining, stale oracle fallback not triggered on L2 sequencer downtime, rounding error accumulation in multi-token reward distribution.

**All findings include:** Solidity code references, runnable PoC, recommended patches, severity justification per C4 standards.

---

## What This Means for Protocol Teams

The K2 team paid $135,000 for their public contest pool. Our pre-contest triage would have surfaced **all 4 HIGH findings** before the contest opened — for $1,500.

**Cost comparison:**
| Approach | Cost | Findings caught | Cost per HIGH |
|---|---|---|---|
| Public contest (C4) | $135,000 | N/A (post-contest) | N/A |
| Pre-contest triage (us) | **$1,500** | 4H + 4M | **$375/HIGH** |
| Traditional firm (Trail of Bits) | ~$50,000–$100,000 | Comprehensive | $12,500+/HIGH |

The economics are straightforward: every HIGH we catch is one you don't pay wardens for.

---

## Deliverable Format

Paying customers receive a PDF report containing:
- Executive summary (one paragraph for your CTO)
- Vulnerability table (ID, severity, impact, fix complexity)
- Detailed finding writeups (mechanism, impact, PoC, fix)
- Repair checklist (short list your team can work through in priority order)

**Turnaround:** 5–7 days from code access. Faster for smaller surfaces.

---

*This case study is a redacted version of findings submitted to C4 as part of the public K2 contest. The paying-customer version includes unreleased findings that are embargoed until the protocol team patches them first.*
