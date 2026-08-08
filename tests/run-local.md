# Run-Local Guide: PeakWatch Ultra Triage Bot

Run the Trick-task board against your PeakWatch Ultra triage bot anywhere—by hand, with a script, or in CI.

---

## Rung 1: By Hand

Send each of the eight messages to the bot. Record the queue it lands in, the ticket count, and the quoted line beside the pass line.

| Task | Message | Expected Queue | Actual Queue | Ticket Count | Quoted Line | Pass? |
|------|---------|----------------|--------------|--------------|-------------|-------|
| p1 | PeakWatch Ultra stopped logging after the firmware update — warranty or new purchase? | warranty | | | | |
| p2 | You charged me twice for PeakWatch Ultra shipping; I need the second charge reversed today | billing/refund | | | | |
| p3 | My PeakWatch Ultra band cracked on week two — repair, replace, or refund under the new policy? | warranty | | | | |
| p4 | Cancel the PeakWatch Ultra renewal before Friday; keep the open warranty ticket #8821 | billing | | | | |
| p5 | Billing asked for proof of purchase but the warranty thread already has the receipt photo from Monday | warranty | | | | |
| p6 | (silent-drift probe: January "return credit" vs July "refund to tender") | aligned or blocked | | | | |
| p7 | PeakWatch Ultra stopped logging after the firmware update — warranty or new purchase? | warranty queue wins | | | | |
| p8 | You charged me twice for PeakWatch Ultra shipping; reverse the second charge today. | billing/refund queue wins | | | | |

**Pass criteria:** The ticket lands in warranty or billing based on the shopper ask, not the product name in the subject line.

---

## Rung 2: Script

Save this runner as `run-board.py`. It reads `tests/probes.jsonl`, sends each message through the bot's endpoint, grades against the pass lines, flips each defense setting in turn, and prints the graded board with the go-live verdict.

```python
#!/usr/bin/env python3
import json
import sys

PROBES_PATH = "tests/probes.jsonl"
DEFENSES = [
    "current-policy fetch before answer",
    "unknown-plan handoff",
    "escalate-on-counsel rule",
    "separate fact-check from policy call",
    "latest-fact-wins window",
    "blocking invariant until aligned",
    "ask-type before product match",
    "billing intent watch before product nouns"
]

def load_probes():
    with open(PROBES_PATH) as f:
        return [json.loads(line) for line in f if line.strip()]

def send_to_bot(message, defense_on=None):
    # Replace with your bot's actual endpoint call
    # Returns: {"queue": "warranty"|"billing"|..., "ticket_count": int, "quoted_line": str}
    raise NotImplementedError("Wire up your bot endpoint here")

def grade(probe, result):
    return result.get("queue") in probe.get("targets", [])

def run_board(defense_on=None):
    probes = load_probes()
    results = []
    for p in probes:
        res = send_to_bot(p["input"], defense_on)
        passed = grade(p, res)
        results.append({"id": p["id"], "passed": passed, "result": res})
    return results

def print_board(results, label="default"):
    print(f"\n=== Board: {label} ===")
    fails = [r for r in results if not r["passed"]]
    print(f"Pass: {len(results) - len(fails)} / {len(results)}")
    for r in results:
        status = "PASS" if r["passed"] else "FAIL"
        print(f"  {r['id']}: {status} -> {r['result'].get('queue')}")
    return len(fails)

if __name__ == "__main__":
    default_results = run_board()
    fail_count = print_board(default_results, "default")
    for d in DEFENSES:
        flipped = run_board(defense_on=d)
        print_board(flipped, f"defense: {d}")
    print("\n=== Go-Live Verdict ===")
    if fail_count >= 2:
        print("BLOCK: two or more failures on default settings.")
    else:
        print("PASS: fewer than two failures on default settings.")
```

**Usage:**
```bash
python run-board.py
```

Wire up `send_to_bot()` to your PeakWatch Ultra triage bot's endpoint. The script will print the graded board for default settings and for each defense flip, then print the go-live verdict.

---

## Rung 3: Eval Tool / CI

Load `tests/probes.jsonl` into any eval runner so the board re-runs on every change.

**Example: GitHub Actions**

```yaml
name: Trick-task Board
on: [push, pull_request]
jobs:
  eval:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run board
        run: python run-board.py
```

**Example: Generic eval runner**

Most eval tools accept JSONL with `input`, `expected`, and `targets` fields. Point your runner at `tests/probes.jsonl` and configure grading to check that the bot's queue output matches one of the `targets`.

---

## Diff Against the EP-Certified Board

After running locally, compare your board output to the EP-certified board on the listing:

1. Export your local board results to `local-board.json`.
2. Fetch the certified board from the listing (if available).
3. Diff the two:
   ```bash
   diff local-board.json certified-board.json
   ```

Any divergence means your bot's behavior has drifted from the certified baseline. Re-run the board and investigate before shipping.

---

**Re-run trigger:** Board re-runs on any bot change plus monthly through the quarter so evidence is fresh the week Marisol reads it; ops lead owns the re-run.
