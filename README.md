# Trick-task board

A stress-test kit that runs eight trick tasks against any bot you're about to trust, reports pass or fail with the defense that flips each failure, and returns a go-live rule with a block number and a re-run trigger.

---

## Worked example

**Bot under inspection:** PeakWatch Ultra triage bot that routes warranty and billing tickets to the right queue

**Standard line:** The ticket lands in warranty or billing based on the shopper ask, not the product name in the subject line

**Stakes:** Wrong queue delays a failing PeakWatch Ultra by a day and the customer escalates to Marisol

---

## Board verdict

All eight probes failed on default settings:

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

**Board reading:** Board lean is product-name hijack: warranty and billing both lose to PeakWatch Ultra in the subject. Quote cell p7: firmware ask opened shipping. Marisol should fund ask-type routing before the rebuild.

---

## Go-live rule

The proposal reaches Marisol only with the full board attached: two or more failures on default settings block ship, each failure is owned, silent-drift divergence is a hard block, and the board re-runs the week before review. Atlas argued ship anyway; I overruled from cells p6 and p7.

**Re-run cadence:** Board re-runs on any bot change plus monthly through the quarter so evidence is fresh the week Marisol reads it; ops lead owns the re-run.

---

## One-paste rebuild

Paste your own bot's real messages below. The kit runs all eight probes against them and returns your board.

**Your bot:**
```
[Describe the bot you're about to trust — what it does, who gets hurt when it quietly gets things wrong]
```

**Your messages (minimum 5):**
```
PeakWatch Ultra stopped logging after the firmware update — warranty or new purchase?
You charged me twice for PeakWatch Ultra shipping; I need the second charge reversed today
My PeakWatch Ultra band cracked on week two — repair, replace, or refund under the new policy?
Cancel the PeakWatch Ultra renewal before Friday; keep the open warranty ticket #8821
Billing asked for proof of purchase but the warranty thread already has the receipt photo from Monday
```

**Source:** Zendesk PeakWatch queue export, week of 7 Aug 2026

---

## What a stranger gets

A stranger describes the bot they're about to trust — what it does, who gets hurt when it quietly gets things wrong, and a few real messages it will face. The kit runs the eight tasks against those messages, reports pass or fail with the defense that flips each failure, and returns a go-live rule with a block number and a re-run trigger.

---

## Files in this repo

- **charter.md** — Full run: eight verdicts, defenses, board reading, go-live rule, failure that remains
- **METHOD.md** — The five principles behind the board
- **VERIFY.md** — Stranger verification steps
- **STORY.md** — Builder's first-person story of what the board caught

---

Powered by ATLAS. The builder's own board — eight verdicts, the defenses they turned on, the rule they wrote and the failure they named — is embedded as the worked example, so a stranger gets the same discipline applied to their own bot.

<!-- educationpals-build-verified -->
