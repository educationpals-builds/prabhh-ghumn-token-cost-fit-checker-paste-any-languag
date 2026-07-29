# Token Fit Prompt Pack

Five standalone prompts for evaluating token cost and vocabulary fit. Each prompt targets one dial and can be used in any chat model. Paste the sample text where indicated.

---

## 1. Special Token Handling

**Dial:** `special_token_handling`

```
You are a token analyst. Examine the following text for special token requirements.

TEXT TO ANALYZE:
[paste sample here]

Report:
1. Count any special characters, punctuation marks, or symbols that may require dedicated tokens
2. Identify language-specific characters (umlauts, cedillas, diacritics) that tokenizers handle inconsistently
3. Note any control characters or whitespace variants
4. Rate special token handling complexity from 0 (trivial) to 4 (severe)

Output format:
- Special characters found: [list]
- Language-specific marks: [list]
- Complexity rating: [0-4]
- Reasoning: [one sentence]
```

---

## 2. Vocabulary Fit

**Dial:** `vocabulary_fit`

```
You are a vocabulary coverage analyst. Evaluate how well a standard multilingual tokenizer vocabulary covers the following text.

TEXT TO ANALYZE:
[paste sample here]

Report:
1. Identify domain-specific terms that may be out-of-vocabulary (insurance, medical, legal, technical)
2. Flag compound words that will fragment into many subword pieces
3. Note any proper nouns, abbreviations, or acronyms
4. Rate vocabulary fit from 0 (excellent coverage) to 4 (severe gaps)

Output format:
- Domain terms: [list]
- Likely fragmenting compounds: [list]
- OOV risk items: [list]
- Vocabulary fit rating: [0-4]
- Reasoning: [one sentence]
```

---

## 3. Merge Economy

**Dial:** `merge_economy`

```
You are a tokenization efficiency analyst. Evaluate the merge economy of the following text — how efficiently common byte sequences will compress.

TEXT TO ANALYZE:
[paste sample here]

Report:
1. Identify repeated substrings that should merge efficiently
2. Flag language mixing that prevents cross-language merges
3. Note any patterns that will resist compression (rare bigrams, code-switching mid-word)
4. Rate merge economy from 0 (highly efficient) to 4 (poor compression)

Output format:
- Efficient merge candidates: [list]
- Merge blockers: [list]
- Language mixing impact: [description]
- Merge economy rating: [0-4]
- Reasoning: [one sentence]
```

---

## 4. How It Splits

**Dial:** `how_it_splits`

```
You are a subword segmentation analyst. Predict how a BPE or SentencePiece tokenizer will split the following text.

TEXT TO ANALYZE:
[paste sample here]

Report:
1. Estimate total token count for the full text
2. Identify the longest words and predict their piece counts
3. Flag any words that will split into 5+ pieces
4. Rate splitting severity from 0 (minimal fragmentation) to 4 (extreme fragmentation)

Output format:
- Estimated total tokens: [number]
- Worst-case word: [word] → [estimated pieces]
- High-fragment words (5+ pieces): [list]
- Splitting severity rating: [0-4]
- Reasoning: [one sentence]
```

---

## 5. Edge Case Survival

**Dial:** `edge_case_survival`

```
You are a tokenizer robustness analyst. Evaluate how well the following text will survive edge cases in tokenization pipelines.

TEXT TO ANALYZE:
[paste sample here]

Report:
1. Check for encoding edge cases (mixed scripts, RTL markers, zero-width characters)
2. Identify truncation risks if the text hits a sequence-length ceiling
3. Flag any content that may tokenize differently across model versions
4. Rate edge case risk from 0 (robust) to 4 (fragile)

Output format:
- Encoding risks: [list]
- Truncation vulnerability: [description]
- Version instability risks: [list]
- Edge case survival rating: [0-4]
- Reasoning: [one sentence]
```

---

## Usage Notes

- **Calibration anchor:** These prompts were calibrated against a German+Turkish support ticket sample from a 14-day queue export (38% German, 22% Turkish, 19% English, remainder Thai/Arabic/Mandarin)
- **Weakest dial for this domain:** `vocabulary_fit` — insurance and administrative compound nouns fragment heavily
- **Per-lane reporting:** When analyzing multilingual text, run each language segment separately and report per-language token counts before summing

## Sample for Testing

```
"Ihr Krankenversicherungsbeitrag wurde angepasst — bitte prüfen Sie die Beitragsbemessungsgrenze." / "Sigortalılığınızın başlangıç tarihini öğrenebilir miyim?"
```

This German+Turkish pair from the support queue demonstrates the vocabulary fit challenge: German insurance compounds and Turkish agglutinative forms both resist efficient tokenization.
