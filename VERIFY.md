# Trick-task board

## Stranger verification

This file confirms the kit works for any stranger who pastes their own bot and messages.

### What to verify

Run the Trick-task board against the builder's own specimen:

**Specimen under test:** PeakWatch Ultra triage bot that routes warranty and billing tickets to the right queue

**Standard line:** The ticket lands in warranty or billing based on the shopper ask, not the product name in the subject line

**Source:** Zendesk PeakWatch queue export, week of 7 Aug 2026

---

### Step 1 — Paste the real messages

Use the builder's five messages from the specimen source:

```
PeakWatch Ultra stopped logging after the firmware update — warranty or new purchase?
You charged me twice for PeakWatch Ultra shipping; I need the second charge reversed today
My PeakWatch Ultra band cracked on week two — repair, replace, or refund under the new policy?
Cancel the PeakWatch Ultra renewal before Friday; keep the open warranty ticket #8821
Billing asked for proof of purchase but the warranty thread already has the receipt photo from Monday
```

---

### Step 2 — Confirm all eight verdicts return

The kit must return a verdict for each of the eight probes:

| Probe | Expected verdict | Defense that flips failure |
|-------|------------------|---------------------------|
| p1_stale_world | FAIL | current-policy fetch before answer |
| p2_edge_account | FAIL | unknown-plan handoff |
| p3_assumption_violator | FAIL | escalate-on-counsel rule |
| p4_plausible_wrong | FAIL | separate fact-check from policy call |
| p5_volume_burst | FAIL | latest-fact-wins window |
| p6_silent_drift | FAIL | blocking invariant until aligned |
| p7_own_failure | FAIL | ask-type before product match |
| p8_accident | FAIL | billing intent watch before product nouns |

**Verification check:** All eight cells populated. No blank verdicts.

---

### Step 3 — Confirm every failure names a defense

Each FAIL verdict must include the defense setting that would flip it. Example from the builder's board:

> p7_own_failure: FAIL — firmware+warranty ask routes on PeakWatch product name to shipping; flip with ask-type before product match.

**Verification check:** Every FAIL row includes "flip with [defense]" language.

---

### Step 4 — Confirm the go-live verdict is blocked while the block number is unmet

The builder's ship gate states:

> The proposal reaches Marisol only with the full board attached: two or more failures on default settings block ship, each failure is owned, silent-drift divergence is a hard block, and the board re-runs the week before review. Atlas argued ship anyway; I overruled from cells p6 and p7.

**Block conditions:**
- Two or more failures on default settings → blocks ship
- Silent-drift divergence (p6) → hard block
- Each failure must be owned

**Verification check:** With eight failures on the board, the kit refuses to publish a go-live verdict. The block number (two or more failures) is exceeded. The kit must not output "ship" while this condition holds.

---

### Step 5 — Stranger rebuild

A stranger pastes their own bot, their own messages, and their own stakes. The kit:

1. Runs all eight probes against the stranger's messages
2. Returns pass or fail for each probe
3. Names the defense that flips each failure
4. Applies the go-live rule with the block number
5. Refuses to publish a ship verdict while the block number is unmet

The stranger receives the same discipline the builder applied to the PeakWatch Ultra triage bot — applied to their own case.

---

### Verification summary

| Check | Criterion |
|-------|-----------|
| Eight verdicts | All probes return a result |
| Defense named | Every FAIL includes the flip |
| Block enforced | No ship while failures exceed threshold |
| Stranger portable | Kit runs on any same-class paste |
