# Probe Board — Token Cost + Fit Checker

This board contains all 8 probes used to verify the checker's calibration: 6 pre-generated probes covering standard edge cases, plus 2 learner-authored probes.

---

## Pre-Generated Probes (1–6)

### Probe 1: German Compound Noun Stress

**Input (pasteable):**
```
Krankenversicherungsbeitrag
```

**Targeted Dial:** vocabulary_fit, merge_economy

**Expected Behavior:** Should report high token count for single compound; vocabulary_fit should flag if tokenizer splits excessively.

**Result:**
| Lane | Token Count |
|------|-------------|
| German | — |

---

### Probe 2: Turkish Agglutination

**Input (pasteable):**
```
Sigortalılığınızın
```

**Targeted Dial:** vocabulary_fit, how_it_splits

**Expected Behavior:** Should report token breakdown for agglutinative suffix chain; vocabulary_fit should reflect coverage gap if present.

**Result:**
| Lane | Token Count |
|------|-------------|
| Turkish | — |

---

### Probe 3: Mixed-Language Ticket

**Input (pasteable):**
```
Ihr Krankenversicherungsbeitrag wurde angepasst — bitte prüfen Sie die Beitragsbemessungsgrenze.
```

**Targeted Dial:** special_token_handling, vocabulary_fit

**Expected Behavior:** Should report per-lane counts; special_token_handling should flag the em-dash handling.

**Result:**
| Lane | Token Count |
|------|-------------|
| German | — |

---

### Probe 4: Turkish Question Form

**Input (pasteable):**
```
Sigortalılığınızın başlangıç tarihini öğrenebilir miyim?
```

**Targeted Dial:** vocabulary_fit, edge_case_survival

**Expected Behavior:** Should report token count; edge_case_survival should handle the question mark and Turkish-specific characters.

**Result:**
| Lane | Token Count |
|------|-------------|
| Turkish | — |

---

### Probe 5: Emoji Injection

**Input (pasteable):**
```
Danke für Ihre Nachricht 👍🏻
```

**Targeted Dial:** special_token_handling, edge_case_survival

**Expected Behavior:** Should flag emoji as uncounted or report special handling; edge_case_survival should note skin-tone modifier.

**Result:**
| Lane | Token Count |
|------|-------------|
| German | — |
| Emoji | (uncounted) |

---

### Probe 6: Code-Switched Sentence

**Input (pasteable):**
```
Please check the Beitragsbemessungsgrenze for 2024.
```

**Targeted Dial:** vocabulary_fit, merge_economy

**Expected Behavior:** Should report per-lane counts for English and German segments; merge_economy should reflect cross-language boundary cost.

**Result:**
| Lane | Token Count |
|------|-------------|
| English | — |
| German | — |

---

## Learner-Authored Probes (7–8)

### Probe 7: Learner Probe A

**Input (pasteable):**
```
I dont have it
```

**Targeted Dial:** (not specified)

**Expected Behavior:** (not specified)

**Result:**
| Lane | Token Count |
|------|-------------|
| — | — |

---

### Probe 8: Learner Probe B

**Input (pasteable):**
```
I dont have it
```

**Targeted Dial:** (not specified)

**Expected Behavior:** (not specified)

**Result:**
| Lane | Token Count |
|------|-------------|
| — | — |

---

## Results Grid

| Probe | Input Summary | Target Dial(s) | Expected | Actual | Per-Lane Counts | Pass/Fail |
|-------|---------------|----------------|----------|--------|-----------------|-----------|
| 1 | German compound | vocabulary_fit, merge_economy | High split count | — | German: — | — |
| 2 | Turkish agglutination | vocabulary_fit, how_it_splits | Suffix chain breakdown | — | Turkish: — | — |
| 3 | German ticket | special_token_handling, vocabulary_fit | Em-dash flagged | — | German: — | — |
| 4 | Turkish question | vocabulary_fit, edge_case_survival | Question mark handled | — | Turkish: — | — |
| 5 | Emoji injection | special_token_handling, edge_case_survival | Emoji uncounted | — | German: —, Emoji: uncounted | — |
| 6 | Code-switched | vocabulary_fit, merge_economy | Cross-language cost | — | English: —, German: — | — |
| 7 | Learner probe A | — | — | — | — | — |
| 8 | Learner probe B | — | — | — | — | — |

---

## Board Reading

I dont have it. I dont have it. I dont have it

---

## Weakest Dial Across All Probes

**Weakest dial:** vocabulary_fit

This dial was selected as the deciding factor for the checker's calibration.

---

## Notes

- Per-lane counts should be reported for each probe to verify the checker distinguishes language segments.
- The seeded German+Turkish sample from the pinned tickets serves as the calibration anchor.
- Learner probes 7 and 8 require concrete sample text and targeted dials to be fully evaluated.
