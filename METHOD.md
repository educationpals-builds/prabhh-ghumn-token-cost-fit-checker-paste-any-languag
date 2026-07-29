# METHOD.md

## The TFLEC Framework

This checker uses the **TFLEC** framework — five dials that measure how well a tokenizer fits a given text stream.

### The Five Dials

| Dial | What It Measures |
|------|------------------|
| **T** — Special Token Handling | How the tokenizer treats special tokens, control characters, and reserved sequences |
| **F** — Vocabulary Fit | Whether the tokenizer's vocabulary covers the language mix in the stream |
| **L** — Merge Economy | How efficiently the tokenizer merges subwords — fewer tokens per semantic unit is better |
| **E** — How It Splits | The actual piece count when compound words, technical terms, and mixed-script text hit the tokenizer |
| **C** — Edge Case Survival | Whether the tokenizer degrades gracefully on emoji, code-switching, rare scripts, and malformed input |

### How the Dials Work

Each dial scores 0–4:

- **0** — Fails outright; unusable for this stream
- **1** — Severe problems; would require major workarounds
- **2** — Marginal; works but with known cost or quality penalties
- **3** — Adequate; fits the stream with minor friction
- **4** — Strong fit; no concerns for this dial

### Using the Framework

1. **Pin a sample** — real bytes from the stream you care about
2. **Score each dial** — based on observed behavior, not assumptions
3. **Identify the weakest dial** — this is the one that decides your verdict
4. **Call it** — fit or not, with the cost of being wrong
5. **Name the flip** — what measurement would change your read

The framework forces a position. A checker built on TFLEC doesn't hedge — it names the deciding dial and the failure cost.

---

*This is the only file where the TFLEC acronym appears. All other files reference the dials by their full names.*
