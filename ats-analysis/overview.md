# ATS Keyword Analysis — Overview

## Purpose

The `scoring/` segment produced skills, a score, and suggestions. This `ats-analysis/` folder documents **ATS keyword analysis**: measuring how many common applicant-tracking keywords appear in the resume text and listing which are missing.

Goal: append an ATS percentage and missing-keyword list to the service response. Controller and prior analysis stay unchanged.

---

## What this segment delivers

| Deliverable | Description |
|-------------|-------------|
| `getATSKeywords()` | Fixed list of common hiring keywords |
| `checkATSScore(text)` | Count matches, compute %, list missing |
| Response wiring | Append ATS block to service output |

**Not in this segment:** role matching, section detection, career roadmap, dashboard UI.

---

## Where code changes live

```
ResumeService.processResume()
        │  text (from PDFBox)
        ▼
checkATSScore(text)  ──►  "ATS Score: N%" + "Missing Keywords: [...]"
```

Package: `com.resume.analyzer.service`.

---

## Feature mapping

| Requirement | This segment |
|-------------|--------------|
| F6 ATS keyword analysis | `getATSKeywords`, `checkATSScore` |

Acceptance: [acceptance-scoring-and-matching.md](../requirements/acceptance-scoring-and-matching.md) (AC-A01–A08).

---

## Documents in this segment

| Document | Contents |
|----------|----------|
| [overview.md](./overview.md) | This file — scope |
| keyword-set.md | The 14 ATS keywords |
| scoring-logic.md | Match count and percentage |
| implementation.md | Service source for ATS |
| verify-and-wrapup.md | Tests and close |

This segment is split into **5 commits** (one document per commit).

---

## Prerequisites

- [ ] Skills/score/suggestions working ([scoring/verify-and-wrapup.md](../scoring/verify-and-wrapup.md))
- [ ] Extracted text available in `processResume`

---

## Success criteria

- [ ] ATS keyword list defined (14 terms)
- [ ] ATS % computed from matches / total
- [ ] Missing keywords listed
- [ ] ATS block appended to response
- [ ] No role/section logic required yet

---

## Next document

**keyword-set.md** — the ATS keyword list and rationale.
