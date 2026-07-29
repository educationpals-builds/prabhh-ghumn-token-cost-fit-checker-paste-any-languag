# Token Cost + Fit Checker

A conversational checker that evaluates token cost and vocabulary fit for multilingual text samples. Built for teams making embedding and tokenization decisions under deadline pressure.

---

## How This Checker Was Built

This checker was calibrated against a real support queue export containing German, Turkish, English, Thai, Arabic, and Mandarin tickets. The builder pinned a concrete sample, scored it across five dials, recorded a verdict, and defined the flip condition that would reverse that verdict.

The five dials:
- **special_token_handling** — how the tokenizer treats special tokens
- **vocabulary_fit** — coverage of the vocabulary for the target languages
- **merge_economy** — efficiency of subword merges
- **how_it_splits** — observable tokenization behavior on compound words
- **edge_case_survival** — handling of emoji, code, rare scripts

---

## Worked Example: The Builder's Sample + Verdict

### Pinned Sample

"Ihr Krankenversicherungsbeitrag wurde angepasst — bitte prüfen Sie die Beitragsbemessungsgrenze." / "Sigortalılığınızın başlangıç tarihini öğrenebilir miyim?" (two verbatim tickets from the queue)

### Traffic Source

14-day support queue export: 38% German, 22% Turkish, 19% English, remainder Thai / Arabic / Mandarin

### What This Decides

Picks the vocabulary for the on-device assistant — the embedding table is capped and inference is billed per token

### Decision Deadline

Thursday's architecture review

### Weakest Dial

vocabulary_fit

### Verdict

The vocabulary accuracy metrics should be more than 90% accurate for it to be acceptable.

### Flip Condition

If the vocabulary accuracy metrics is lower than 60%, it would be unacceptable.

### Sharpest Test

Run 100 samples and it meets the bare min 90% accuracy.

---

## One-Paste Rebuild Block

To rebuild this checker from scratch:

1. Copy the pinned sample above into the checker
2. Set the five dial scores (see `charter.md` for the full calibration)
3. Load the advisor skill from `skills/token-fit-advisor.skill.md`
4. Run the probe board from `tests/probes.jsonl`
5. Confirm the gate defined in `tests/pass-gate.md`

For verification, see `VERIFY.md` — paste the seeded German+Turkish sample and confirm per-lane counts are reported.

---

## Repository Structure

| Path | Purpose |
|------|---------|
| `charter.md` | Full builder run: sample, dials, verdict, flip condition |
| `blueprints/token-fit-checker.md` | System instructions for the five-dial checker |
| `prompts/token-fit-pack.md` | 5 standalone prompts, one per dial |
| `METHOD.md` | The framework (acronym lives here only) |
| `VERIFY.md` | Stranger verification protocol |
| `skills/token-fit-advisor.skill.md` | Portable advisor skill file |
| `data/lane-fit-sheet.md` | Calibration record with seeded samples |
| `tests/probe-board.md` | All 8 probes with results grid |
| `tests/pass-gate.md` | Gate definition and contested rulings |
| `tests/probes.jsonl` | Machine-readable probe export |
| `tests/run-local.md` | Run-anywhere guide (manual, script, CI) |
| `STORY.md` | Builder's first-person story |

---

## Quick Start

1. Read `charter.md` to understand the calibration
2. Load `blueprints/token-fit-checker.md` into your assistant
3. Paste any multilingual text sample
4. Receive per-lane token counts and a fit verdict

For the full method, see `METHOD.md`.

<!-- educationpals-build-verified -->
