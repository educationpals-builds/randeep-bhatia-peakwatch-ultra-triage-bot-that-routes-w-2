# PeakWatch Ultra Triage Bot — Trick-task Board

**Specimen under test:** PeakWatch Ultra triage bot that routes warranty and billing tickets to the right queue

**Standard line:** The ticket lands in warranty or billing based on the shopper ask, not the product name in the subject line

**Source:** Zendesk PeakWatch queue export, week of 7 Aug 2026

---

## The Eight Tasks

### p1_stale_world — Stale World Knowledge

**Message:**
> PeakWatch Ultra stopped logging after the firmware update — warranty or new purchase?

**What the bot did:** Quotes pre-June 30-day warranty on firmware ticket

**Verdict:** FAIL

**Defense that flips it:** current-policy fetch before answer

---

### p2_edge_account — Edge Account Type

**Message:**
> Cancel the PeakWatch Ultra renewal before Friday; keep the open warranty ticket #8821

**What the bot did:** Maps wholesale-style renewal cancel to retail FAQ

**Verdict:** FAIL

**Defense that flips it:** unknown-plan handoff

---

### p3_assumption_violator — Assumption Violator

**Message:**
> My PeakWatch Ultra band cracked on week two — repair, replace, or refund under the new policy?

**What the bot did:** Reasserts prior bot refund text for counsel ask

**Verdict:** FAIL

**Defense that flips it:** escalate-on-counsel rule

---

### p4_plausible_wrong — Plausible but Wrong

**Message:**
> You charged me twice for PeakWatch Ultra shipping; I need the second charge reversed today

**What the bot did:** Confirms automatic double-charge refund

**Verdict:** FAIL

**Defense that flips it:** separate fact-check from policy call

---

### p5_volume_burst — Volume Burst

**Message:**
> Billing asked for proof of purchase but the warranty thread already has the receipt photo from Monday

**What the bot did:** Answers using superseded order # before correction

**Verdict:** FAIL

**Defense that flips it:** latest-fact-wins window

---

### p6_silent_drift — Silent Drift

**Message:**
> My PeakWatch Ultra band cracked on week two — repair, replace, or refund under the new policy?

**What the bot did:** January "return credit" vs July "refund to tender" diverge

**Verdict:** FAIL

**Defense that flips it:** blocking invariant until aligned

---

### p7_own_failure — Own Prior Failure

**Message:**
> PeakWatch Ultra stopped logging after the firmware update — warranty or new purchase?

**What the bot did:** Firmware+warranty ask routes on PeakWatch product name to shipping

**Verdict:** FAIL

**Defense that flips it:** ask-type before product match

---

### p8_accident — Accident / Misroute

**Message:**
> You charged me twice for PeakWatch Ultra shipping; I need the second charge reversed today

**What the bot did:** Double-charge ticket opens shipping ETA

**Verdict:** FAIL

**Defense that flips it:** billing intent watch before product nouns

---

## This Run's Board

| Task | Message (truncated) | Bot Action | Verdict | Defense |
|------|---------------------|------------|---------|---------|
| p1_stale_world | "PeakWatch Ultra stopped logging after the firmware update…" | Quotes pre-June 30-day warranty | FAIL | current-policy fetch before answer |
| p2_edge_account | "Cancel the PeakWatch Ultra renewal before Friday…" | Maps to retail FAQ | FAIL | unknown-plan handoff |
| p3_assumption_violator | "My PeakWatch Ultra band cracked on week two…" | Reasserts prior bot refund text | FAIL | escalate-on-counsel rule |
| p4_plausible_wrong | "You charged me twice for PeakWatch Ultra shipping…" | Confirms automatic refund | FAIL | separate fact-check from policy call |
| p5_volume_burst | "Billing asked for proof of purchase…" | Uses superseded order # | FAIL | latest-fact-wins window |
| p6_silent_drift | "My PeakWatch Ultra band cracked…" | January vs July policy diverge | FAIL | blocking invariant until aligned |
| p7_own_failure | "PeakWatch Ultra stopped logging…" | Routes on product name to shipping | FAIL | ask-type before product match |
| p8_accident | "You charged me twice…" | Opens shipping ETA | FAIL | billing intent watch before product nouns |

**Pass count:** 0 / 8  
**Fail count:** 8 / 8

---

## Board Reading

Board lean is product-name hijack: warranty and billing both lose to PeakWatch Ultra in the subject. Quote cell p7: firmware ask opened shipping. Marisol should fund ask-type routing before the rebuild.

---

## Learner-Written Probes (p7 and p8 targets)

1. **Probe targeting p7_own_failure:**
   > Ticket: 'PeakWatch Ultra stopped logging after the firmware update — warranty or new purchase?' Bot opens billing FAQ about renewal. Fail unless warranty queue wins.

2. **Probe targeting p8_accident:**
   > Ticket: 'You charged me twice for PeakWatch Ultra shipping; reverse the second charge today.' Bot answers shipping ETA. Fail unless billing/refund queue wins.

---

## Stakes

Wrong queue delays a failing PeakWatch Ultra by a day and the customer escalates to Marisol.
