# Go-Live Rule — PeakWatch Ultra Triage Bot

## Block Number

**Two or more failures on default settings block ship.**

---

## Owned Conditions

| Condition | Owner | Status |
|-----------|-------|--------|
| Each failure is owned | Assigned per task | Required |
| Silent-drift divergence (p6) | Hard block | Must be resolved before ship |
| Full board attached to proposal | Ops lead | Required for Marisol review |

---

## Re-Run Trigger

Board re-runs on any bot change plus monthly through the quarter so evidence is fresh the week Marisol reads it; ops lead owns the re-run.

---

## Failure That Remains

**Cell p6 — Silent Drift**

- **Finding:** FAIL — January "return credit" vs July "refund to tender" diverge; blocking invariant until aligned.
- **Owner:** Ops lead
- **Date:** Week of 7 Aug 2026
- **Resolution required:** Blocking invariant must be aligned before ship proceeds.

**Cell p7 — Own Failure**

- **Finding:** FAIL — firmware+warranty ask routes on PeakWatch product name to shipping; flip with ask-type before product match.
- **Owner:** Ops lead
- **Date:** Week of 7 Aug 2026

---

## Opposing Case and Recorded Ruling

### Atlas Argued

> Ship anyway.

### Recorded Ruling

> I overruled from cells p6 and p7.

**Full gate statement:**

The proposal reaches Marisol only with the full board attached: two or more failures on default settings block ship, each failure is owned, silent-drift divergence is a hard block, and the board re-runs the week before review. Atlas argued ship anyway; I overruled from cells p6 and p7.

---

## Summary

This go-live rule applies to the PeakWatch Ultra triage bot that routes warranty and billing tickets to the right queue. The standard for passing: The ticket lands in warranty or billing based on the shopper ask, not the product name in the subject line.

The board currently shows eight failures. Until p6 (silent drift) and p7 (product-name hijack) are resolved with their respective defenses, ship is blocked.
