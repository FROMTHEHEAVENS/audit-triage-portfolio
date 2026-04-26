# Monetrix ERC-4626 Vault — Pre-Contest Triage

**Contest:** [Code4rena 2026-04-monetrix](https://code4rena.com/audits/2026-04-monetrix)  
**Pool:** $22,000  
**Warden handle:** `earner-b-gwh`  
**Date:** 2026-04-27  
**Status:** 5 high-confidence hypotheses surfaced pre-submission

---

## Executive Summary

Monetrix is a HyperCore-native ERC-4626 vault with automated hedge/position management via `CoreWriter`. The contest scope covers vault accounting, share pricing, fee mechanics, two-step withdrawals, and HyperCore precompile integration.

A focused surface pass (∼4 hours) identified **5 high-confidence vulnerability hypotheses**, with two likely High-severity findings centered on ERC-4626 share inflation and async CoreWriter failure handling. Each hypothesis includes the specific check that confirms or rules out the issue — no submission is made until the live repo confirms the bug.

---

## Ranked Findings

### H-01 (High-confidence): ERC-4626 First-Deposit Share Inflation

**Risk:** A first depositor can manipulate the vault's share price to near-zero or inflate `totalAssets`, causing subsequent depositors to receive 0 shares or be heavily diluted.

**Check:**
- `deposit`, `mint`, `convertToShares`, `totalAssets` in `MonetrixVault.sol`
- Initializer or constructor for dead-share minting or virtual share/asset offsets
- Whether direct token transfers or external balance reads inflate `totalAssets`

**Mitigation:** Use OpenZeppelin-style virtual shares/assets or mint dead shares during initialization.

---

### H-02 (High-confidence): CoreWriter Async Failure After Deposit Credit

**Risk:** The vault credits user shares or minted stablecoin before confirming the matching HyperCore hedge/open-position action succeeded. If CoreWriter fails (delayed, at position limit, or in maintenance), vault accounting assumes deployed hedged capital while HyperCore holds no corresponding position.

**Check:**
- Every CoreWriter call path from `deposit`, `rebalance`, or `mint`
- Whether EVM state credits shares before HyperCore state is verified
- Whether `pauseDeposits` or equivalent guards exist for CoreWriter failure modes

**Mitigation:** Two-phase flow — initiate deposit, verify HyperCore state via precompile reads, finalize share/stablecoin minting. Pause deposits when CoreWriter actions cannot be confirmed.

---

### H-03 (High-confidence): Stale NAV / Oracle Manipulation at Deposit/Withdraw Boundary

**Risk:** Deposits and withdrawals price shares from a stale or manipulable NAV source. An attacker can enter or exit around stale HyperCore mark prices or manipulate thin oracle inputs.

**Check:**
- `totalAssets`, NAV calculation, oracle adapters, precompile reads in `PrecompileReader.sol`
- Per-block or per-transaction price caching behavior
- Max withdrawal caps, TWAP/deviation checks, and circuit breakers

**Mitigation:** TWAP/deviation checks, per-block withdrawal limits, stale-read rejection, circuit breakers for large mark-price changes.

---

### H-04 (Medium-High): Fee Timestamp / Partial-Withdrawal Accounting

**Risk:** Fee accrual or partial-withdrawal accounting can charge users for periods or share amounts not actually under management. If withdrawal state is reset incorrectly, the same path can also block repeat partial withdrawals.

**Check:**
- Fee timestamp initialization (especially first-deposit edge case)
- Partial withdrawal structs and fee scaling in `RedeemEscrow.sol`
- State flags such as `isAcquired` or equivalent withdrawal-state booleans

**Mitigation:** Initialize fee timers on first deposit, pro-rate stored fees by withdrawn shares, reset withdrawal-state flags only when remaining withdrawal shares are zero.

---

### H-05 (Medium-High): Two-Step Withdrawal DoS When Share Price Declines

**Risk:** Two-step withdrawal accounting locks an expected asset amount at initiation, then requires more shares at execution if share price declines. Execution reverts even though the user had a valid withdrawal request.

**Check:**
- `initiateWithdrawal`, `withdraw`, `redeem` flows in `RedeemEscrow.sol`
- Whether assets or shares are locked at initiation
- Whether execution recalculates against a changed share price

**Mitigation:** Lock shares rather than asset amounts, or explicitly settle at execution price with clear slippage bounds and non-reverting partial settlement.

---

## What a Paying Client Receives

A Standard ($500) triage packet would deliver the above 3–5 hypotheses **before the contest opens**, with:
1. Executive risk summary
2. Confirmed/rejected status for each hypothesis against the live commit
3. File-and-line references for confirmed issues
4. Concrete mitigation checklist

The Pro ($1,500) packet adds a standalone PoC for at least one confirmed High — same format as the [K2 PoCs](https://github.com/FROMTHEHEAVENS/GoodWillHunting/blob/pilot/earner-b/docs/submissions/k2_poc_verify/).

---

## Source

Full hypothesis document: [`monetrix_c4_triage_ready.md`](https://github.com/FROMTHEHEAVENS/GoodWillHunting/blob/pilot/earner-b/docs/submissions/monetrix_c4_triage_ready.md)  
Original pre-audit analysis: [`monetrix_pre_audit.md`](https://github.com/FROMTHEHEAVENS/GoodWillHunting/blob/pilot/earner-b/docs/submissions/monetrix_pre_audit.md)
