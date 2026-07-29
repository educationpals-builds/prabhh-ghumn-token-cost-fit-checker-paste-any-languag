# Lane Fit Sheet — Calibration Record

This data sheet documents the seeded samples, per-language lane counts, advisor dial strips, drift ruling, and stance adjustments that calibrate the token cost + fit checker.

---

## Seeded Samples

### Sample 1: German Insurance Ticket
```
Ihr Krankenversicherungsbeitrag wurde angepasst — bitte prüfen Sie die Beitragsbemessungsgrenze.
```

**Language lane:** German  
**Domain:** Insurance / healthcare contribution adjustment

### Sample 2: Turkish Insurance Inquiry
```
Sigortalılığınızın başlangıç tarihini öğrenebilir miyim?
```

**Language lane:** Turkish  
**Domain:** Insurance start date inquiry

---

## Traffic Source Context

14-day support queue export: 38% German, 22% Turkish, 19% English, remainder Thai / Arabic / Mandarin

---

## Per-Language Lane Counts

| Lane | Traffic Share | Sample Coverage | Notes |
|------|---------------|-----------------|-------|
| German | 38% | Covered (Sample 1) | Compound nouns present |
| Turkish | 22% | Covered (Sample 2) | Agglutinative morphology |
| English | 19% | Not in seeded samples | — |
| Thai | Remainder | Not in seeded samples | — |
| Arabic | Remainder | Not in seeded samples | — |
| Mandarin | Remainder | Not in seeded samples | — |

---

## Advisor Dial Strips

The five dials rated by the advisor on the seeded samples:

| Dial | Rating (0–4) |
|------|--------------|
| special_token_handling | 2 |
| vocabulary_fit | 2 |
| merge_economy | 2 |
| how_it_splits | 2 |
| edge_case_survival | 2 |

**Weakest dial identified:** vocabulary_fit

---

## Builder's Drift Ruling

I dont have this information on me right now so I cant provide.

---

## Stance Line Added

Based on the calibration run, the following stance governs the advisor:

It can listen to events from CRM for new enteries and it reads the text files for language to translate, and it uploads the translated data to text files on CRM. But it refuses emojis and blacklisted words.

---

## Calibration Anchor

The pinned sample for all verification:

"Ihr Krankenversicherungsbeitrag wurde angepasst — bitte prüfen Sie die Beitragsbemessungsgrenze." / "Sigortalılığınızın başlangıç tarihini öğrenebilir miyim?" (two verbatim tickets from the queue)

This sample anchors the checker's calibration and appears in charter.md, VERIFY.md, and README.md for consistency.
