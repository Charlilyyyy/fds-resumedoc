# Verify & Wrap-Up

## Purpose

Confirm role matching works for all three roles and close the `role-matching/` segment.

Acceptance: [acceptance-scoring-and-matching.md](../requirements/acceptance-scoring-and-matching.md) (AC-M01–M07).

---

## Verify steps

### 1 — Build and run

```bash
./mvnw clean test spring-boot:run
```

### 2 — Full match

Skills [Java, Spring Boot, SQL] + role `Java Developer` → `Match Score: 100%`, `Missing Skills: []`. (AC-M01)

### 3 — Partial match

Skills [Java, SQL] + `Java Developer` → `Match Score: 66%`, missing `[Spring Boot]`. (AC-M02)

### 4 — Full Stack

Skills [Java, HTML, CSS] + `Full Stack Developer` → `Match Score: 75%`, missing `[JavaScript]`. (AC-M04)

### 5 — Data Analyst characteristic

Skills [SQL] + `Data Analyst` → `Match Score: 33%`, missing `[Python, Excel]`. (AC-M05)

### 6 — Role echoed

`Role:` line matches the submitted role exactly. (AC-M06)

### 7 — Role changes output

Same PDF with different roles yields different match / missing. (AC-R03)

---

## Sign-off checklist

- [ ] Role block present in every successful response
- [ ] Match % = matched / required (integer)
- [ ] Missing skills correct per role
- [ ] Uses detected skills, not raw text
- [ ] Controller / prior blocks unchanged

---

## Deliverable confirmation

| Deliverable | Evidence |
|-------------|----------|
| Role-specific match % | matchJobRole |
| Missing skills for role | matchJobRole |
| Three supported roles | jobRoles |

**Role-specific match % and missing skills for the three supported roles** ✓

---

## Requirements mapping

| Item | Status |
|------|--------|
| F7 Job role matching | Done |
| F8 sections / F10 roadmap | Next segment |

---

## Document index

| # | Document |
|---|----------|
| 1 | [overview.md](./overview.md) |
| 2 | [role-definitions.md](./role-definitions.md) |
| 3 | [matching-logic.md](./matching-logic.md) |
| 4 | [implementation.md](./implementation.md) |
| 5 | [verify-and-wrapup.md](./verify-and-wrapup.md) |

---

## What comes next

**Resume section detection & career roadmap** — check for Skills/Education/Experience headings and generate role-based improvement guidance, then assemble the full labeled response.

---

## Role matching complete

The selected role now drives a match percentage and missing-skill list.

**Job role matching documentation for this segment is complete.**
