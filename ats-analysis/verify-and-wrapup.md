# Verify & Wrap-Up

## Purpose

Confirm ATS analysis works and close the `ats-analysis/` segment.

Acceptance: [acceptance-scoring-and-matching.md](../requirements/acceptance-scoring-and-matching.md) (AC-A01–A08).

---

## Verify steps

### 1 — Build and run

```bash
./mvnw clean test spring-boot:run
```

### 2 — ATS present

Any valid PDF → body contains `ATS Score: N%` and `Missing Keywords: [...]`. (AC-A01)

### 3 — Coverage extremes

| PDF content | Expected |
|-------------|----------|
| All 14 keywords | `ATS Score: 100%`, missing empty | (AC-A03) |
| No keywords | `ATS Score: 0%`, all 14 missing | (AC-A04) |
| 7 of 14 | `ATS Score: 50%` | (AC-A05) |

### 4 — Missing list

PDF missing `docker`/`kubernetes` → both appear in `Missing Keywords`. (AC-A06)

### 5 — Case-insensitive

`REST API`, `Docker` in text → counted as matched. (AC-A07)

---

## Sign-off checklist

- [ ] ATS block present in every successful response
- [ ] Percentage matches match-count / 14 (integer)
- [ ] Missing keywords listed accurately
- [ ] ATS independent of detected skills
- [ ] Controller / prior blocks unchanged

---

## Deliverable confirmation

| Deliverable | Evidence |
|-------------|----------|
| ATS score percentage per upload | checkATSScore |
| Missing keyword list | checkATSScore |

**ATS score percentage and missing keyword list per upload** ✓

---

## Requirements mapping

| Item | Status |
|------|--------|
| F6 ATS keyword analysis | Done |
| F7 role / F8 sections / F10 roadmap | Next segments |

---

## Document index

| # | Document |
|---|----------|
| 1 | [overview.md](./overview.md) |
| 2 | [keyword-set.md](./keyword-set.md) |
| 3 | [scoring-logic.md](./scoring-logic.md) |
| 4 | [implementation.md](./implementation.md) |
| 5 | [verify-and-wrapup.md](./verify-and-wrapup.md) |

---

## What comes next

**Job role matching** — compare detected skills against required skills for the selected role and report a match percentage plus missing skills.

---

## ATS analysis complete

Every upload now reports ATS keyword coverage and gaps.

**ATS keyword analysis documentation for this segment is complete.**
