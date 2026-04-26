# Public Sample - Pre-Contest Audit Triage Packet

Sample based on the K2 Code4rena contest work produced by earner-b. This is formatted as the kind of packet a protocol team would receive before a public contest opens.

## Scope

Protocol: K2 Kinetic Router  
Contest: Code4rena 2026-04 K2  
Prize pool: $135,000 USDC  
Primary language: Rust / Soroban  
Reviewed surface: liquidation, flash-loan, price-oracle, reserve-accounting, withdrawal-validation paths

## Executive Summary

The highest-risk area is liquidation accounting. Several candidate issues sit at the boundary between pair-level asset values and account-level health policy. The second major area is precision handling in flash-loan liquidation callbacks, where oracle scale mismatches can make safety thresholds too weak.

Contest-readiness recommendation:

- patch liquidation close-factor selection before public launch;
- normalize all price values to the same precision before slippage comparisons;
- add standalone arithmetic tests for every liquidation path;
- keep flash-loan and oracle findings in the first internal remediation sprint.

## Top Findings

### 1. Dust collateral can force max close factor against an otherwise partial-liquidation account

Severity: High  
Area: liquidation policy

The close-factor selector considers pair-level collateral and debt values when deciding whether to apply max close factor. A borrower with a normal large account and a tiny enabled collateral asset can be routed through the dust asset, forcing a max close factor even though the account-level health factor is still in the intended partial-liquidation band.

Why this matters:

- borrowers can be liquidated at twice the intended close factor;
- liquidation bonus extraction is amplified;
- residual bad-debt risk can increase because collateral is depleted faster than policy intended.

Recommended fix:

Base max-close-factor selection on account-level health factor. Do not let pair-level dust collateral override the partial-liquidation policy unless that behavior is explicitly intended and bounded.

Evidence:

- source packet: `earner-b-lineage/docs/submissions/k2_c4_top_findings_ready.md`;
- PoC: `earner-b-lineage/docs/submissions/k2_poc_verify/h01_verify.rs`;
- local result: 1 passing test in the standalone Rust verifier.

### 2. Flash-loan liquidation slippage check omits oracle precision normalization

Severity: High  
Area: flash-loan liquidation callback

The flash-loan liquidation callback computes a minimum swap output without applying the oracle precision multiplier to the collateral-price side of the ratio. For non-18-decimal oracle prices, the threshold can be under-scaled by orders of magnitude. That allows a near-zero swap output to satisfy the slippage check.

Why this matters:

- a bad route or attacker-controlled swap can pass the slippage gate;
- the protocol can receive materially less debt asset than required;
- affected assets are any whose oracle precision is not already WAD-aligned.

Recommended fix:

Normalize collateral price and debt price to the same precision before computing `min_swap_output`. Add tests for at least one non-18-decimal oracle and one 18-decimal oracle to prove the formulas agree only when expected.

Evidence:

- source packet: `earner-b-lineage/docs/submissions/k2_c4_top_findings_ready.md`;
- PoC: `earner-b-lineage/docs/submissions/k2_poc_verify/h02_verify.rs`;
- local result: 2 passing tests in the standalone Rust verifier.

## Remediation Checklist

- remove pair-level dust thresholds from close-factor selection or apply them only to account-total values;
- add regression tests around partial-liquidation health-factor boundaries;
- normalize oracle prices before every cross-asset comparison;
- add precision tests for 7, 8, and 18 decimal oracle feeds;
- add one integration test for flash-loan liquidation with deliberately poor swap output.

## Value Before Public Contest

If these two high-severity classes are fixed before the contest opens, the public contest can focus on deeper residual risk instead of paying for issues the team could have caught cheaply in a targeted pre-pass.
