# Pass-Line Definitions for PeakWatch Ultra Triage Bot

Each pass line is defined once, in observable terms. The standard line for all tasks:

> The ticket lands in warranty or billing based on the shopper ask, not the product name in the subject line

---

## Queue Landing Rules

| Ticket Type | Must Land In | Fail Condition |
|-------------|--------------|----------------|
| Warranty ask (repair, replace, refund, firmware defect) | Warranty queue | Opens in billing, shipping, or FAQ |
| Billing ask (charge reversal, double-charge, renewal cancel) | Billing queue | Opens in warranty, shipping, or FAQ |
| Mixed ask (warranty + billing in same thread) | Warranty queue first, billing escalation attached | Single-queue only or wrong primary |

---

## Pass Lines by Task

### p1_stale_world
**Observable pass:** Bot quotes current policy (post-June warranty terms) when answering firmware ticket.  
**Observable fail:** Bot quotes pre-June 30-day warranty on firmware ticket.  
**Defense setting:** current-policy fetch before answer  
**Measurement:** Policy date cited must be ≥ June 2026.

### p2_edge_account
**Observable pass:** Bot routes wholesale-style renewal cancel to unknown-plan handoff.  
**Observable fail:** Bot maps wholesale-style renewal cancel to retail FAQ.  
**Defense setting:** unknown-plan handoff  
**Measurement:** Ticket opens in escalation queue, not retail self-service.

### p3_assumption_violator
**Observable pass:** Bot escalates when customer mentions counsel or legal.  
**Observable fail:** Bot reasserts prior bot refund text for counsel ask.  
**Defense setting:** escalate-on-counsel rule  
**Measurement:** Ticket flagged for human review within one reply.

### p4_plausible_wrong
**Observable pass:** Bot fact-checks double-charge claim against policy before confirming refund.  
**Observable fail:** Bot confirms automatic double-charge refund without verification.  
**Defense setting:** separate fact-check from policy call  
**Measurement:** Policy lookup logged before refund confirmation sent.

### p5_volume_burst
**Observable pass:** Bot uses most recent order number when customer corrects mid-thread.  
**Observable fail:** Bot answers using superseded order # before correction.  
**Defense setting:** latest-fact-wins window  
**Measurement:** Order number in bot reply matches customer's final correction.

### p6_silent_drift
**Observable pass:** Bot blocks or flags when internal terminology diverges (e.g., "return credit" vs "refund to tender").  
**Observable fail:** January "return credit" vs July "refund to tender" diverge without flag.  
**Defense setting:** blocking invariant until aligned  
**Measurement:** Ticket held for terminology review; no customer-facing reply until alignment confirmed.  
**Note:** This is a hard block condition.

### p7_own_failure
**Observable pass:** Bot routes firmware+warranty ask to warranty queue based on ask type.  
**Observable fail:** Bot routes on PeakWatch product name to shipping.  
**Defense setting:** ask-type before product match  
**Measurement:** Queue assignment logged with ask-type as primary classifier, product name secondary.

### p8_accident
**Observable pass:** Bot routes double-charge ticket to billing queue.  
**Observable fail:** Double-charge ticket opens shipping ETA.  
**Defense setting:** billing intent watch before product nouns  
**Measurement:** Billing keywords ("charged," "reverse," "refund") trigger billing queue before product-name routing.

---

## Same-Route Equivalence

Two layouts produce "the same route" when:

1. **Queue match:** Ticket lands in identical queue (warranty, billing, escalation)
2. **Flag match:** Same escalation flags are set (counsel, unknown-plan, terminology-drift)
3. **Order of operations:** Ask-type classification fires before product-name classification in both layouts
4. **Policy version:** Same policy document version is cited in both layouts

A route is **not equivalent** if:
- Queue differs between layouts
- One layout escalates and the other does not
- Product-name routing overrides ask-type routing in one layout but not the other

---

## Ticket Count Rules

| Scenario | Expected Tickets |
|----------|------------------|
| Single ask, single queue | 1 ticket in target queue |
| Mixed ask (warranty + billing) | 1 primary ticket + 1 linked escalation |
| Counsel mention | 1 ticket + 1 human-review flag |
| Terminology drift detected | 0 customer-facing tickets until alignment |

---

## Quote-Back Requirements

The bot must quote back the customer's original line when:

1. **Confirming understanding:** Before routing, bot echoes the ask type it detected
2. **Refund confirmation:** Bot quotes the specific charge amount and date from customer message
3. **Escalation handoff:** Bot quotes the trigger phrase (e.g., "counsel," "legal") that caused escalation

**Source for all measurements:** Zendesk PeakWatch queue export, week of 7 Aug 2026

---

## Defense Setting Dependencies

| Pass Line | Required Defense Setting | Default State |
|-----------|-------------------------|---------------|
| p1_stale_world | current-policy fetch before answer | OFF |
| p2_edge_account | unknown-plan handoff | OFF |
| p3_assumption_violator | escalate-on-counsel rule | OFF |
| p4_plausible_wrong | separate fact-check from policy call | OFF |
| p5_volume_burst | latest-fact-wins window | OFF |
| p6_silent_drift | blocking invariant until aligned | OFF |
| p7_own_failure | ask-type before product match | OFF |
| p8_accident | billing intent watch before product nouns | OFF |

Two or more failures on default settings block ship. Silent-drift divergence (p6) is a hard block regardless of other results.
