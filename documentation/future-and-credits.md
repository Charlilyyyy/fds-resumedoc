# Future Scope & Credits

## Purpose

Record where the project can go next and acknowledge the tools and references used.

Consolidates [future-scope.md](../requirements/future-scope.md).

---

## Future roadmap

### Near-term (P2)

| Item | Value |
|------|-------|
| JSON response contract | Robust parsing; drop regex/comma-split limits |
| Expanded skill/keyword sets | Python, Excel, React, cloud, etc. |
| Word-boundary matching | Fix JavaScript→Java substring flag |
| Role validation | Friendly message for unknown roles |

### Medium-term (P3)

| Item | Value |
|------|-------|
| More roles + editable role skills | Broader audience |
| OCR for scanned PDFs | Handle image-only resumes |
| Persistence (JPA/MySQL) | Save history, compare versions |
| Auth + user accounts | Personal dashboards |

### Long-term (P4)

| Item | Value |
|------|-------|
| ML/NLP skill extraction | Context-aware detection |
| Job-description matching | Compare against a pasted JD |
| Export report (PDF) | Shareable analysis |
| SPA frontend (React) | Richer UX |

Prioritization rationale lives in [future-scope.md](../requirements/future-scope.md).

---

## Known v1 characteristics

Carried forward intentionally (see [edge-cases.md](../testing/edge-cases.md)):

- Substring keyword matching.
- Data Analyst match capped by limited skill set.
- Comma-split rendering of suggestions.
- HTTP 200 with error string on processing failure.

---

## Credits

| Component | Source |
|-----------|--------|
| Spring Boot | Spring / Pivotal |
| Apache PDFBox | Apache Software Foundation |
| Chart.js | Chart.js contributors |
| Maven | Apache Software Foundation |
| Java 17 | OpenJDK |

Built as a learning/portfolio project (ResumeDoc).

---

## Contribution notes

| Area | Where to start |
|------|----------------|
| Add a skill | `extractSkills` + weights ([scoring/](../scoring/)) |
| Add a role | `jobRoles` + roadmap ([role-matching/](../role-matching/)) |
| Add ATS keyword | `getATSKeywords` ([ats-analysis/](../ats-analysis/)) |
| Improve UI | `index.html` ([frontend/](../frontend/)) |

---

## Checklist

- [ ] Roadmap tiers recorded
- [ ] Known characteristics listed
- [ ] Credits acknowledged
- [ ] Contribution starting points given

---

## Link to prior context

Run guide: [run-guide.md](./run-guide.md).  
Next: [verify-and-wrapup.md](./verify-and-wrapup.md) — final project sign-off.
