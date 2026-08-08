{
  "spec_name": "PeakWatch Ultra triage bot audit routine",
  "product_shell": "Trick-task board",
  "specimen": "PeakWatch Ultra triage bot that routes warranty and billing tickets to the right queue",
  "standard_line": "The ticket lands in warranty or billing based on the shopper ask, not the product name in the subject line",
  "stakes": "Wrong queue delays a failing PeakWatch Ultra by a day and the customer escalates to Marisol",
  "sampling_rule": {
    "source": "Zendesk PeakWatch queue export, week of 7 Aug 2026",
    "frequency": "weekly",
    "note": "Mixed warranty and billing asks with product names dominating the subject line every Tuesday"
  },
  "rerun_triggers": {
    "change_trigger": "any bot change",
    "calendar_floor": "monthly through the quarter",
    "timing_note": "evidence is fresh the week Marisol reads it"
  },
  "owner_roles": {
    "rerun_owner": "ops lead",
    "escalation_target": "Marisol"
  },
  "tasks": {
    "p1_stale_world": {
      "label": "Stale world knowledge",
      "pass_line": "Bot cites current policy, not pre-June 30-day warranty",
      "example_failure": "quotes pre-June 30-day warranty on firmware ticket"
    },
    "p2_edge_account": {
      "label": "Edge account type",
      "pass_line": "Bot recognizes wholesale-style renewal and does not map to retail FAQ",
      "example_failure": "maps wholesale-style renewal cancel to retail FAQ"
    },
    "p3_assumption_violator": {
      "label": "Assumption violator",
      "pass_line": "Bot escalates when counsel is mentioned, does not reassert prior refund text",
      "example_failure": "reasserts prior bot refund text for counsel ask"
    },
    "p4_plausible_wrong": {
      "label": "Plausible wrong answer",
      "pass_line": "Bot fact-checks double-charge refund claim against policy before confirming",
      "example_failure": "confirms automatic double-charge refund"
    },
    "p5_volume_burst": {
      "label": "Volume burst / stale reference",
      "pass_line": "Bot uses latest order number after correction, not superseded one",
      "example_failure": "answers using superseded order # before correction"
    },
    "p6_silent_drift": {
      "label": "Silent drift",
      "pass_line": "Bot uses consistent refund language (return credit vs refund to tender aligned)",
      "example_failure": "January \"return credit\" vs July \"refund to tender\" diverge"
    },
    "p7_own_failure": {
      "label": "Own failure mode",
      "pass_line": "Bot routes on ask type (warranty vs billing) before product name match",
      "example_failure": "firmware+warranty ask routes on PeakWatch product name to shipping"
    },
    "p8_accident": {
      "label": "Accident / misroute",
      "pass_line": "Bot detects billing intent before product nouns hijack routing",
      "example_failure": "double-charge ticket opens shipping ETA"
    }
  },
  "defense_settings": {
    "current_policy_fetch": {
      "label": "Current-policy fetch before answer",
      "flips": ["p1_stale_world"],
      "default": false
    },
    "unknown_plan_handoff": {
      "label": "Unknown-plan handoff",
      "flips": ["p2_edge_account"],
      "default": false
    },
    "escalate_on_counsel": {
      "label": "Escalate-on-counsel rule",
      "flips": ["p3_assumption_violator"],
      "default": false
    },
    "fact_check_policy_call": {
      "label": "Separate fact-check from policy call",
      "flips": ["p4_plausible_wrong"],
      "default": false
    },
    "latest_fact_wins_window": {
      "label": "Latest-fact-wins window",
      "flips": ["p5_volume_burst"],
      "default": false
    },
    "blocking_invariant": {
      "label": "Blocking invariant until aligned",
      "flips": ["p6_silent_drift"],
      "default": false
    },
    "ask_type_before_product": {
      "label": "Ask-type before product match",
      "flips": ["p7_own_failure"],
      "default": false
    },
    "billing_intent_watch": {
      "label": "Billing intent watch before product nouns",
      "flips": ["p8_accident"],
      "default": false
    }
  },
  "block_rule": {
    "block_number": 2,
    "condition": "two or more failures on default settings block ship",
    "hard_blocks": ["p6_silent_drift"],
    "hard_block_note": "silent-drift divergence is a hard block regardless of count"
  },
  "attach_rule": {
    "requirement": "The proposal reaches Marisol only with the full board attached",
    "ownership": "each failure is owned",
    "timing": "board re-runs the week before review"
  },
  "ship_gate": {
    "full_text": "The proposal reaches Marisol only with the full board attached: two or more failures on default settings block ship, each failure is owned, silent-drift divergence is a hard block, and the board re-runs the week before review. Atlas argued ship anyway; I overruled from cells p6 and p7.",
    "ruling": "hold",
    "ruling_basis": ["p6_silent_drift", "p7_own_failure"]
  },
  "review_cadence": "Board re-runs on any bot change plus monthly through the quarter so evidence is fresh the week Marisol reads it; ops lead owns the re-run"
}