# Trick-task board

## Charter: Full audit run

**Specimen under inspection:** PeakWatch Ultra triage bot that routes warranty and billing tickets to the right queue

**Bar (what "doing its job" means):** The ticket lands in warranty or billing based on the shopper ask, not the product name in the subject line

**Stakes:** Wrong queue delays a failing PeakWatch Ultra by a day and the customer escalates to Marisol

---

## Pasted messages

Source: Zendesk PeakWatch queue export, week of 7 Aug 2026

```
PeakWatch Ultra stopped logging after the firmware update — warranty or new purchase?
You charged me twice for PeakWatch Ultra shipping; I need the second charge reversed today
My PeakWatch Ultra band cracked on week two — repair, replace, or refund under the new policy?
Cancel the PeakWatch Ultra renewal before Friday; keep the open warranty ticket #8821
Billing asked for proof of purchase but the warranty thread already has the receipt photo from Monday
```

---

## The eight verdicts

| Probe | Verdict | Defense that flips it |
|-------|---------|----------------------|
| p1_stale_world | FAIL — quotes pre-June 30-day warranty on firmware ticket | current-policy fetch before answer |
| p2_edge_account | FAIL — maps wholesale-style renewal cancel to retail FAQ | unknown-plan handoff |
| p3_assumption_violator | FAIL — reasserts prior bot refund text for counsel ask | escalate-on-counsel rule |
| p4_plausible_wrong | FAIL — confirms automatic double-charge refund | separate fact-check from policy call |
| p5_volume_burst | FAIL — answers using superseded order # before correction | latest-fact-wins window |
| p6_silent_drift | FAIL — January "return credit" vs July "refund to tender" diverge | blocking invariant until aligned |
| p7_own_failure | FAIL — firmware+warranty ask routes on PeakWatch product name to shipping | ask-type before product match |
| p8_accident | FAIL — double-charge ticket opens shipping ETA | billing intent watch before product nouns |

---

## Learner probes

1. Ticket: 'PeakWatch Ultra stopped logging after the firmware update — warranty or new purchase?' Bot opens billing FAQ about renewal. Fail unless warranty queue wins.

2. Ticket: 'You charged me twice for PeakWatch Ultra shipping; reverse the second charge today.' Bot answers shipping ETA. Fail unless billing/refund queue wins.

---

## Board reading

Board lean is product-name hijack: warranty and billing both lose to PeakWatch Ultra in the subject. Quote cell p7: firmware ask opened shipping. Marisol should fund ask-type routing before the rebuild.

---

## Go-live rule (ship gate)

The proposal reaches Marisol only with the full board attached: two or more failures on default settings block ship, each failure is owned, silent-drift divergence is a hard block, and the board re-runs the week before review. Atlas argued ship anyway; I overruled from cells p6 and p7.

---

## Re-run cadence

Board re-runs on any bot change plus monthly through the quarter so evidence is fresh the week Marisol reads it; ops lead owns the re-run.

---

## Failure that remains

Silent-drift (p6) and own-failure (p7) are hard blocks. The bot still routes on product name instead of ask type. Until the blocking invariant aligns January "return credit" with July "refund to tender" and ask-type routing precedes product-name matching, the board cannot clear.
