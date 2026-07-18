# Verify & Wrap-Up

## Purpose

Confirm skill extraction, scoring, and suggestions work end-to-end, then close the `scoring/` segment.

Acceptance reference: [acceptance-scoring-and-matching.md](../requirements/acceptance-scoring-and-matching.md) (F4, F5, F9).

---

## Verify steps

### 1 — Build and run

```bash
./mvnw clean test spring-boot:run
```

### 2 — Skills detected

Upload a PDF containing `java`, `mysql`, `html`:

| Check | Expected |
|-------|----------|
| Status | 200 |
| Body | `Skills: [Java, SQL, HTML]` |

Maps to AC-S01, AC-S03, AC-S04.

### 3 — Score correct

| Skills | Expected score |
|--------|----------------|
| [Java, SQL, HTML] | `Score: 50/100` |
| all six | `Score: 100/100` |
| none | `Score: 0/100` |

Maps to AC-C01–C03.

### 4 — Suggestions present

| Given | Expected |
|-------|----------|
| Missing Spring Boot | Suggestion mentions Spring Boot / REST API |
| Fewer than 3 skills | Suggestion mentions real-world projects |

Maps to AC-G01–G03.

---

## Sign-off checklist

- [ ] `Skills:` list reflects text keywords (case-insensitive)
- [ ] `Score:` matches weighted formula
- [ ] `Suggestions:` reflects missing skills + thin-resume rule
- [ ] Empty resume → empty skills, score 0, suggestions present
- [ ] Controller / upload contract unchanged

---

## Deliverable confirmation

| Deliverable | Evidence |
|-------------|----------|
| Skills list from any resume text | extractSkills |
| Numeric score | calculateScore |
| General suggestions | getSuggestions |

**Skills list, numeric score, and general suggestions returned from any resume text** ✓

---

## Requirements mapping

| Item | Status |
|------|--------|
| F4 Skill extraction | Done |
| F5 Resume score | Done |
| F9 Smart suggestions | Done |
| F6 ATS / F7 role / F8 sections | Next segments |

---

## Document index

| # | Document |
|---|----------|
| 1 | [overview.md](./overview.md) |
| 2 | [skill-extraction.md](./skill-extraction.md) |
| 3 | [scoring-logic.md](./scoring-logic.md) |
| 4 | [suggestions.md](./suggestions.md) |
| 5 | [implementation.md](./implementation.md) |
| 6 | [verify-and-wrapup.md](./verify-and-wrapup.md) |

---

## What comes next

**ATS keyword analysis** — measure how many common hiring keywords appear in the resume text and list missing ones.

---

## Scoring complete

Resume text now yields a skills list, a 0–100 score, and actionable suggestions.

**Skill extraction and scoring documentation for this segment is complete.**
