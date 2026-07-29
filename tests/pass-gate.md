# Pass Gate

## Gate Definition

**Metric:** Vocabulary accuracy  
**Threshold:** I dont have it. I dont have it. I dont have it  
**Re-run cadence:** Not specified

---

## Learner's Gate Statement

> I dont have it. I dont have it. I dont have it

---

## Contested-Call Rulings

### Atlas's Opposing Case

*No contested-call data available — learner did not provide board reading or probe results.*

### Builder's Ruling

*Unable to document contested calls without probe board data.*

---

## Gate Verification

To verify this checker holds its gate:

1. Run all 8 probes from `tests/probes.jsonl`
2. Compare results against expected behaviors in `tests/probe-board.md`
3. Check that the metric meets the threshold defined above
4. If gate fails, review the weakest dial: **vocabulary_fit**

---

## Re-certification Trigger

Re-run the gate when:
- The prompt or stance changes
- New probe types are added
- The traffic mix shifts significantly from the original: 38% German, 22% Turkish, 19% English, remainder Thai / Arabic / Mandarin

---

*Gate defined for: Token cost + fit checker*  
*Decision deadline: Thursday's architecture review*
