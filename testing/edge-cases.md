# Edge Cases

## Purpose

Document boundary and failure inputs and the expected behavior, confirming the app degrades gracefully.

Implements the robustness part of requirement **F13**.

---

## Input edge cases

| # | Input | Expected behavior |
|---|-------|-------------------|
| 1 | Non-PDF file (`.txt` renamed `.pdf`) | Load fails → caught → `Error while processing resume ❌`; server stays up |
| 2 | Corrupt/truncated PDF | Same as above |
| 3 | Empty PDF (no text) | Skills `[]`, Score `0/100`, all sections Missing, ATS `0%` |
| 4 | Image-only (scanned) PDF | Little/no text extracted → low/zero scores (no OCR in v1) |
| 5 | Encrypted/password PDF | Load throws → caught → error string |
| 6 | Very large PDF | Processes; slower; try-with-resources frees memory |

See [error-handling.md](../pdf-extraction/error-handling.md).

---

## Analysis edge cases

| # | Input | Expected |
|---|-------|----------|
| 7 | JavaScript-only resume | Java also flagged (substring `java` in `javascript`) — known v1 characteristic |
| 8 | No ATS keywords | `ATS Score: 0%`, all 14 missing |
| 9 | All ATS keywords | `ATS Score: 100%`, missing empty |
| 10 | Data Analyst role | Match capped (Python/Excel not detected) — known v1 characteristic |
| 11 | Unknown role via API | `matchJobRole` NPE → caught → error string (UI restricts roles) |

---

## Client edge cases

| # | Input | Expected |
|---|-------|----------|
| 12 | Submit empty form | Browser blocks (required fields) |
| 13 | Error body from server | Dashboard renders blank fields, no JS crash |
| 14 | Re-submit | Charts destroyed and redrawn, no stacking |
| 15 | Suggestion with commas | May split into fragments — known v1 rendering limit |

---

## Stability checks

| Check | How |
|-------|-----|
| No crash on bad input | Server still answers after error cases |
| No resource leak | Repeated uploads; memory stable (try-with-resources) |
| Consistent output | Same input → same output (deterministic) |

---

## Known v1 characteristics (not bugs)

- Substring skill matching (JavaScript → Java).
- Data Analyst match capped by limited skill set.
- Comma-split rendering of suggestions.
- HTTP 200 with error string for processing failures (not 4xx/5xx).

All are documented and deferred to future scope.

---

## Checklist

- [ ] Non-PDF/corrupt → error string, no crash
- [ ] Empty PDF → zero scores
- [ ] Encrypted PDF → error string
- [ ] ATS extremes correct
- [ ] Client handles error body
- [ ] Server stable across repeated bad inputs

---

## Link to prior context

API tests: [api-testing.md](./api-testing.md).  
Next: [verify-and-wrapup.md](./verify-and-wrapup.md) — full integration run and close.
