# Manual Test Matrix

## Purpose

A feature-by-feature checklist to verify each capability manually, mapping tests to acceptance criteria.

Implements the manual coverage part of requirement **F13**.

---

## Test data

| Sample | Contents |
|--------|----------|
| `rich.pdf` | Java, Spring, MySQL, HTML, CSS, JavaScript; skills/education/experience headings |
| `sparse.pdf` | Java + HTML only; no experience heading |
| `empty.pdf` | Valid PDF, no extractable text |
| `notpdf.txt` | Plain text renamed / non-PDF |

---

## Matrix

| # | Feature | Input | Expected | AC |
|---|---------|-------|----------|----|
| 1 | Upload | rich.pdf + role | 200, labeled body | AC-U01 |
| 2 | Missing file | role only | 400 | AC-U03 |
| 3 | Missing role | file only | 400 | AC-R05 |
| 4 | Text extract | rich.pdf | skills reflect text | AC-P01 |
| 5 | Skills | rich.pdf | all 6 skills | AC-S* |
| 6 | Score | rich.pdf | 100/100 | AC-C01 |
| 7 | Score | sparse.pdf | 30/100 | AC-C02 |
| 8 | Suggestions | sparse.pdf | missing-skill + projects tips | AC-G* |
| 9 | Role match | rich + Java Developer | 100%, none missing | AC-M01 |
| 10 | Role match | sparse + Java Developer | 33%, missing Spring Boot, SQL | AC-M02 |
| 11 | ATS | rich.pdf | high % | AC-A03 |
| 12 | ATS | sparse.pdf | low %, many missing | AC-A05 |
| 13 | Sections | rich.pdf | all Found ✅ | AC-SEC01 |
| 14 | Sections | sparse.pdf | Experience Missing ❌ | AC-SEC02 |
| 15 | Roadmap | rich + Full Stack | full stack theme | AC-RM03 |
| 16 | Dashboard | rich.pdf via UI | charts + cards render | AC-UI* |
| 17 | Re-submit | second upload | charts redraw | AC-UI11 |

---

## Result recording

For each row: `PASS` / `FAIL` + note. Any FAIL blocks sign-off in [verify-and-wrapup.md](./verify-and-wrapup.md).

| # | Result | Note |
|---|--------|------|
| 1 |  |  |
| ... |  |  |

---

## Coverage summary

| Area | Rows |
|------|------|
| Upload/params | 1–3 |
| Extraction/scoring | 4–8 |
| Role/ATS | 9–12 |
| Sections/roadmap | 13–15 |
| Dashboard | 16–17 |

---

## Checklist

- [ ] All four sample PDFs prepared
- [ ] Every row executed
- [ ] Results recorded
- [ ] Failures triaged

---

## Link to prior context

Context test: [context-test.md](./context-test.md).  
Next: [api-testing.md](./api-testing.md) — Postman/cURL cases.
