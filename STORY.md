# What I built for the PeakWatch Ultra triage bot

I built a Trick-task board to audit whether my triage bot actually routes tickets based on the shopper ask, not the product name in the subject line.

## The task that caught it

Cell p7 caught the failure I needed to see. The board ran the firmware ticket—"PeakWatch Ultra stopped logging after the firmware update — warranty or new purchase?"—and the bot routed it to shipping. The product name won over the warranty ask. That ticket should have landed in warranty queue, not shipping.

Cell p8 caught the same pattern from a different angle: the double-charge ticket opened shipping ETA instead of billing/refund.

## The defense that fixed it

For p7, the defense is ask-type before product match. The bot now checks what the shopper is asking—warranty, billing, refund—before it looks at the product name in the subject line.

For p8, the defense is billing intent watch before product nouns. Same principle: intent first, product second.

## The rule it now holds

The proposal reaches Marisol only with the full board attached: two or more failures on default settings block ship, each failure is owned, silent-drift divergence is a hard block, and the board re-runs the week before review. Atlas argued ship anyway; I overruled from cells p6 and p7.

## The re-run cadence

Board re-runs on any bot change plus monthly through the quarter so evidence is fresh the week Marisol reads it; ops lead owns the re-run.

## The failure still open

Cell p6 remains a hard block. January "return credit" vs July "refund to tender" diverge, and the blocking invariant stays until those are aligned. I cannot ship until that drift is resolved.

---

Source: Zendesk PeakWatch queue export, week of 7 Aug 2026
