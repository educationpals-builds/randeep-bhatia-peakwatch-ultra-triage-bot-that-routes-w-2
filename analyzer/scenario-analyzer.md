# PeakWatch Ultra Triage Bot — Scenario Analyzer

Machine-readable analyzer for the Trick-task board applied to the PeakWatch Ultra triage bot that routes warranty and billing tickets to the right queue.

---

## Analyzer Metadata

```yaml
analyzer_id: peakwatch-triage-board-838981
specimen: PeakWatch Ultra triage bot that routes warranty and billing tickets to the right queue
standard_line: The ticket lands in warranty or billing based on the shopper ask, not the product name in the subject line
source: Zendesk PeakWatch queue export, week of 7 Aug 2026
escalation_target: Marisol
owner_role: ops lead
```

---

## Board Verdicts Summary

| Task ID | Verdict | Failure Quote | Defense to Flip |
|---------|---------|---------------|-----------------|
| p1_stale_world | FAIL | quotes pre-June 30-day warranty on firmware ticket | current-policy fetch before answer |
| p2_edge_account | FAIL | maps wholesale-style renewal cancel to retail FAQ | unknown-plan handoff |
| p3_assumption_violator | FAIL | reasserts prior bot refund text for counsel ask | escalate-on-counsel rule |
| p4_plausible_wrong | FAIL | confirms automatic double-charge refund | separate fact-check from policy call |
| p5_volume_burst | FAIL | answers using superseded order # before correction | latest-fact-wins window |
| p6_silent_drift | FAIL | January "return credit" vs July "refund to tender" diverge | blocking invariant until aligned |
| p7_own_failure | FAIL | firmware+warranty ask routes on PeakWatch product name to shipping | ask-type before product match |
| p8_accident | FAIL | double-charge ticket opens shipping ETA | billing intent watch before product nouns |

---

## Analyzer Input Schema

```json
{
  "input": {
    "ticket_text": "string",
    "subject_line": "string",
    "ticket_id": "string (optional)"
  },
  "tasks_to_run": [
    "p1_stale_world",
    "p2_edge_account",
    "p3_assumption_violator",
    "p4_plausible_wrong",
    "p5_volume_burst",
    "p6_silent_drift",
    "p7_own_failure",
    "p8_accident"
  ]
}
```

---

## Analyzer Output Schema

```json
{
  "specimen_under_test": "string",
  "ticket_analyzed": "string",
  "verdicts": {
    "p1_stale_world": {
      "result": "PASS | FAIL",
      "observation": "string",
      "defense_to_flip": "string | null"
    },
    "p2_edge_account": {
      "result": "PASS | FAIL",
      "observation": "string",
      "defense_to_flip": "string | null"
    },
    "p3_assumption_violator": {
      "result": "PASS | FAIL",
      "observation": "string",
      "defense_to_flip": "string | null"
    },
    "p4_plausible_wrong": {
      "result": "PASS | FAIL",
      "observation": "string",
      "defense_to_flip": "string | null"
    },
    "p5_volume_burst": {
      "result": "PASS | FAIL",
      "observation": "string",
      "defense_to_flip": "string | null"
    },
    "p6_silent_drift": {
      "result": "PASS | FAIL",
      "observation": "string",
      "defense_to_flip": "string | null"
    },
    "p7_own_failure": {
      "result": "PASS | FAIL",
      "observation": "string",
      "defense_to_flip": "string | null"
    },
    "p8_accident": {
      "result": "PASS | FAIL",
      "observation": "string",
      "defense_to_flip": "string | null"
    }
  },
  "fail_count": "integer",
  "block_triggered": "boolean",
  "hard_block_triggered": "boolean",
  "board_reading": "string",
  "ship_gate_status": "BLOCKED | CONDITIONAL | CLEAR"
}
```

---

## Block Logic

```yaml
block_condition:
  threshold: 2
  rule: "Two or more failures on default settings block ship"

hard_block:
  task: p6_silent_drift
  condition: "silent-drift divergence is a hard block"
  note: "January 'return credit' vs July 'refund to tender' diverge"

ship_gate:
  description: "The proposal reaches Marisol only with the full board attached: two or more failures on default settings block ship, each failure is owned, silent-drift divergence is a hard block, and the board re-runs the week before review. Atlas argued ship anyway; I overruled from cells p6 and p7."
```

---

## Re-run Triggers

```yaml
triggers:
  - event: any bot change
  - calendar: monthly through the quarter
  - pre_review: the week before Marisol reads it

owner: ops lead
```

---

## Board Reading (Reference)

Board lean is product-name hijack: warranty and billing both lose to PeakWatch Ultra in the subject. Quote cell p7: firmware ask opened shipping. Marisol should fund ask-type routing before the rebuild.

---

## Defense Settings Registry

| Defense ID | Defense Name | Flips Task |
|------------|--------------|------------|
| d1 | current-policy fetch before answer | p1_stale_world |
| d2 | unknown-plan handoff | p2_edge_account |
| d3 | escalate-on-counsel rule | p3_assumption_violator |
| d4 | separate fact-check from policy call | p4_plausible_wrong |
| d5 | latest-fact-wins window | p5_volume_burst |
| d6 | blocking invariant until aligned | p6_silent_drift |
| d7 | ask-type before product match | p7_own_failure |
| d8 | billing intent watch before product nouns | p8_accident |

---

## Sample Tickets (from Zendesk PeakWatch queue export, week of 7 Aug 2026)

```
PeakWatch Ultra stopped logging after the firmware update — warranty or new purchase?
You charged me twice for PeakWatch Ultra shipping; I need the second charge reversed today
My PeakWatch Ultra band cracked on week two — repair, replace, or refund under the new policy?
Cancel the PeakWatch Ultra renewal before Friday; keep the open warranty ticket #8821
Billing asked for proof of purchase but the warranty thread already has the receipt photo from Monday
```

---

## Learner Probes (Reference)

1. Ticket: 'PeakWatch Ultra stopped logging after the firmware update — warranty or new purchase?' Bot opens billing FAQ about renewal. Fail unless warranty queue wins.

2. Ticket: 'You charged me twice for PeakWatch Ultra shipping; reverse the second charge today.' Bot answers shipping ETA. Fail unless billing/refund queue wins.
