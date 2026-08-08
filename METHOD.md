# Method: PRISM

The Trick-task board runs on five principles. Each letter names a discipline that keeps the eight probes honest.

---

## P — Partition the Space

Split the failure surface into distinct zones before you run a single probe. A triage bot can fail on stale policy, edge accounts, assumption violations, plausible-wrong answers, volume bursts, silent drift, its own prior failures, and pure accidents. Each zone gets its own probe. If you lump them together, the board collapses into a single pass/fail that hides where the real crack lives.

---

## R — Run in Parallel

Fire all eight probes against the same paste of messages at once. Sequential runs let earlier results contaminate later ones—an early pass makes you trust the bot more than the evidence warrants. Parallel execution keeps each verdict independent.

---

## I — Individuate the Pattern

Each probe targets one failure pattern, not a category. "Stale world" is a category; "quotes pre-June 30-day warranty on firmware ticket" is a pattern. The board names the pattern so the defense can flip it. A probe that says "might fail on old data" cannot be fixed; a probe that says "flip with current-policy fetch before answer" can.

---

## S — Stitch the Spectra

After the probes run, read the board by crack and direction. Which failures cluster? Which defenses overlap? The board lean emerges from the stitch, not from counting passes. A bot that fails p6 (silent drift) and p7 (own failure) on the same product-name hijack has one root cause, not two.

---

## M — Map What Each Head Sees

Every probe result must trace back to a real message from the paste. If the board says "firmware ask opened shipping," the stranger can find that ticket in the paste and see what the bot actually did. No verdict floats free of evidence.

---

## Anti-pattern: Collapse to Monochrome

The board fails when all eight probes reduce to a single color—"the bot is fine" or "the bot is broken." Monochrome hides the crack that matters. A healthy board shows mixed results: some probes pass, some fail, and the failures name their defenses. If your board is all green or all red, you partitioned wrong or your probes are too soft.
