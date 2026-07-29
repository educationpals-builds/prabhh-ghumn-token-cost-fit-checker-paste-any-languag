# VERIFY.md

How a stranger confirms this checker works as claimed.

---

## The Seeded Sample

Paste this exact text into the checker (or `/verify` if using a chat interface):

```
"Ihr Krankenversicherungsbeitrag wurde angepasst — bitte prüfen Sie die Beitragsbemessungsgrenze." / "Sigortalılığınızın başlangıç tarihini öğrenebilir miyim?"
```

These are two verbatim tickets from the queue — one German, one Turkish.

---

## What to Confirm

1. **Per-lane counts appear.** The checker must report token counts broken out by language lane (German lane, Turkish lane), not just a single total.

2. **The uncounted lane is named.** The traffic source includes 19% English, remainder Thai / Arabic / Mandarin — but the seeded sample contains no English. The checker should flag which lane is missing from this sample (English lane uncounted, or note the absence of Thai / Arabic / Mandarin representation).

3. **Five dials are scored.** Each dial should receive a rating:
   - special_token_handling
   - vocabulary_fit
   - merge_economy
   - how_it_splits
   - edge_case_survival

4. **Weakest dial is identified.** The checker should call out which dial is the limiting factor. For this build, the calibrated weakest filter is: vocabulary_fit.

---

## Pass Criteria

A stranger's verification passes when:

- The checker outputs per-language lane token counts (not a single merged number)
- The checker explicitly names at least one lane that is *not* represented in the pasted sample
- All five dials receive a score

If any of these are missing, the checker has drifted from its calibration.

---

## Traffic Context (for reference)

The builder's traffic source: 14-day support queue export: 38% German, 22% Turkish, 19% English, remainder Thai / Arabic / Mandarin

The seeded sample covers German and Turkish only. A working checker notices what's absent.
