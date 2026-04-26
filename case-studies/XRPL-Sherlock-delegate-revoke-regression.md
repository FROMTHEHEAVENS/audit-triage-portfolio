# XRPL BatchDelegate — All-or-Nothing Rollback Regression

**Source:** Sherlock contest submission — XRPLF rippled amendment cycle  
**Severity:** High (state corruption)  
**Status:** Verified with regression test patch  
**Date flagged:** 2026-04 (pre-contest triage)

---

## Summary

The `Batch` amendment introduces composite transactions where multiple inner operations execute atomically. A critical edge case exists when a **delegated permission is revoked in one inner transaction and a subsequent delegated inner transaction fails**: in `tfAllOrNothing` mode, the revocation incorrectly **persists** despite the batch being rolled back.

## Finding

When a batch contains:
1. **Inner #1:** `DelegateSet` revoking a delegator's authorization
2. **Inner #2:** A delegated `Payment` that fails because the delegation was revoked in step 1

In `tfAllOrNothing` batches, the entire batch should fail atomically with no state changes. However, because the `DelegateSet` revocation fires immediately **in the view the second inner transaction checks against**, the payment fails its permission check — but the revocation **remains in ledger state** even after the batch is rejected.

This creates a "half-rollback" — the payment fails (correctly), but the revocation persists (incorrectly), leaving the delegator in a corrupted state.

## Proof of Concept

Regression test added to `Batch_test.cpp`:

```cpp
// delegated permission revoked before delegated inner transaction.
// In an all-or-nothing batch the revocation must NOT persist
// if a later delegated transaction fails permission checks.
{
    // Setup: alice delegates Payment to bob
    env(delegate::set(alice, bob, {"Payment"}));
    env.close();

    // Batch: outer (alice, AllOrNothing)
    //   inner[0]: revoke bob's delegation
    //   inner[1]: bob executes delegated Payment (now unauthorized)
    auto const [txIDs, batchID] = submitBatch(
        env, tesSUCCESS,
        batch::outer(alice, seq, batchFee, tfAllOrNothing),
        batch::inner(delegate::set(alice, bob, {}), seq + 1),
        payAfterRevoke);  // Payment delegated to bob
    env.close();

    // ASSERT: bob's delegation should STILL be authorized
    // because the batch was all-or-nothing and the payment failed
    BEAST_EXPECT(
        delegate::entry(env, alice, bob)
            [jss::result][jss::node][sfAuthorize.jsonName] ==
        bob.human());

    // Both inner txs should be txnNotFound
    BEAST_EXPECT(env.rpc("tx", txIDs[0])[jss::result][jss::error] 
        == "txnNotFound");
    BEAST_EXPECT(env.rpc("tx", txIDs[1])[jss::result][jss::error] 
        == "txnNotFound");
}
```

## Impact

- **Affected scope:** Any `tfAllOrNothing` batch mixing delegate revocation with delegated operations
- **Exploitability:** Attacker constructs a batch where a delegation revocation invalidates a later inner tx, causing silent state corruption
- **Real-world risk:** Wallets/exchanges using batch to atomically manage delegate permissions could leave users with corrupted authorization state

## Remediation

The batch execution engine must snapshot delegate permissions at batch-start and apply all state changes only after the full batch succeeds. The current eager-apply model for `DelegateSet` within a batch is architecturally incompatible with the all-or-nothing guarantee.

---

*This finding was surfaced in a pre-contest triage pass — a $500-$1,500 review completed before the public Sherlock contest opened. Protocol teams: catch these before your $30K-$135K contest begins. → https://github.com/FROMTHEHEAVENS/audit-triage-portfolio*
