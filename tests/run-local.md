# Run-Local Guide

Three rungs for running the token-fit checker probes anywhere.

---

## Rung 1: Manual — Paste Protocol

For each probe in `tests/probes.jsonl`, follow this protocol:

1. Open your chat model of choice
2. Load the system instructions from `blueprints/token-fit-checker.md`
3. Paste the probe input **exactly as written** — do not reformat, trim whitespace, or normalize characters
4. Compare the checker's output against the expected behavior listed beside each probe

### Byte-Preservation Warning

**Critical:** Many probes contain non-ASCII characters (German umlauts, Turkish dotted/dotless i, compound nouns). Copy-paste can silently corrupt these bytes. Always:
- Use raw copy (not "smart quotes" conversion)
- Verify byte length matches before and after paste
- If your terminal or editor normalizes Unicode, use a hex viewer to confirm the input

### Manual Checklist

| Probe | Input (paste verbatim) | Target Dial | Expected Behavior |
|-------|------------------------|-------------|-------------------|
| See `tests/probe-board.md` for the full 8-probe table with pasteable inputs |

Record each result. Mark PASS if the checker's dial rating and per-lane counts match expected; mark FAIL otherwise.

---

## Rung 2: Script — Embedded Runner

A ~20-line runner that reads `tests/probes.jsonl`, calls your model, and prints the graded grid.

```python
#!/usr/bin/env python3
"""Token-fit checker probe runner. Reads probes.jsonl, prints graded grid."""
import json
import os
import sys

API_KEY = os.environ.get("LLM_API_KEY")
if not API_KEY:
    sys.exit("Set LLM_API_KEY in environment")

def load_probes(path="tests/probes.jsonl"):
    with open(path, "r", encoding="utf-8") as f:
        return [json.loads(line) for line in f if line.strip()]

def call_checker(input_text):
    # Replace with your model's API call
    # Must load system instructions from blueprints/token-fit-checker.md
    raise NotImplementedError("Wire to your model API")

def grade(result, expected):
    # Compare dial ratings and per-lane counts
    return "PASS" if expected.lower() in result.lower() else "FAIL"

def main():
    probes = load_probes()
    results = []
    for p in probes:
        out = call_checker(p["input"])
        verdict = grade(out, p["expected"])
        results.append({"id": p["id"], "name": p["name"], "verdict": verdict})
    print("\n=== GRADED GRID ===")
    for r in results:
        print(f"{r['id']:8} | {r['name']:30} | {r['verdict']}")
    passed = sum(1 for r in results if r["verdict"] == "PASS")
    print(f"\n=== GATE VERDICT ===")
    print(f"Passed: {passed}/{len(results)}")
    # Reference pass-gate.md for threshold
    print("See tests/pass-gate.md for gate threshold and re-run trigger")

if __name__ == "__main__":
    main()
```

### Usage

```bash
export LLM_API_KEY="your-key-here"
python3 tests/run-local.py
```

The script prints:
- Per-probe PASS/FAIL grid
- Total passed count
- Gate verdict (compare against threshold in `tests/pass-gate.md`)

---

## Rung 3: Eval Tool / CI Integration

Load `tests/probes.jsonl` into any eval runner so the board re-runs automatically on prompt or stance changes.

### Loading into Eval Runners

The `probes.jsonl` format is standard JSONL with fields:
- `id`: unique probe identifier
- `name`: human-readable probe name
- `input`: the exact bytes to send
- `targets`: which dial(s) this probe tests
- `expected`: the expected dial behavior
- `invariant`: what must remain true across runs

Most eval frameworks accept JSONL directly. Example configurations:

**Generic eval runner:**
```yaml
dataset: tests/probes.jsonl
system_prompt: blueprints/token-fit-checker.md
grader: substring_match
```

**CI pipeline (GitHub Actions example):**
```yaml
- name: Run token-fit probes
  env:
    LLM_API_KEY: ${{ secrets.LLM_API_KEY }}
  run: python3 tests/run-local.py
```

### Re-Run Triggers

Run the board when:
- System instructions in `blueprints/token-fit-checker.md` change
- Stance or refusal rules in `skills/token-fit-advisor.skill.md` change
- New probes are added to `tests/probes.jsonl`
- Model version changes

---

## Diffing Against the EP-Certified Board

To compare your local run against the EducationPals-certified board on the listing:

1. Run locally and save output:
   ```bash
   python3 tests/run-local.py > local-board.txt
   ```

2. Download the certified board from the EP listing (available after certification)

3. Diff the two:
   ```bash
   diff local-board.txt certified-board.txt
   ```

Any differences indicate:
- **Local failures on certified passes:** Your environment may be corrupting bytes or using a different model version
- **Local passes on certified failures:** The checker may have drifted; re-run calibration
- **New probes not in certified:** Submit for re-certification if adding probes

The certified board is the source of truth. Local runs should match within the tolerance defined in `tests/pass-gate.md`.
