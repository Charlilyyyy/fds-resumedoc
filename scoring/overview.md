# Skill Extraction & Scoring — Overview

## Purpose

The `pdf-extraction/` segment made `ResumeService` turn an uploaded PDF into a plain-text string. This `scoring/` folder documents the first analysis layer on top of that text: **detect skills**, **compute a 0–100 score**, and **produce improvement suggestions**.

Goal: given extracted resume text, return a skills list, a numeric score, and general suggestions. Controller and upload contract stay unchanged.

---

## What this segment delivers

| Deliverable | Description |
|-------------|-------------|
| `extractSkills(text)` | Scan text for known technical keywords |
| `calculateScore(skills)` | Weighted score, max 100 |
| `getSuggestions(skills)` | Tips for missing skills / thin resumes |
| Response wiring | Include skills, score, suggestions in service output |

**Not in this segment:** ATS keyword %, role matching, section detection, career roadmap, dashboard UI.

---

## Where code changes live

```
ResumeService.processResume()
        │  text (from PDFBox)
        ▼
extractSkills(text)  ──►  List<String> skills
        ▼
calculateScore(skills)  ──►  int score
        ▼
getSuggestions(skills)  ──►  List<String> suggestions
```

Package: `com.resume.analyzer.service` (private helper methods on `ResumeService`).

---

## Feature mapping

| Requirement | This segment |
|-------------|--------------|
| F4 Skill extraction | `extractSkills` |
| F5 Resume score | `calculateScore` |
| F9 Smart suggestions | `getSuggestions` |

See [must-have-features.md](../requirements/must-have-features.md) and acceptance in [acceptance-scoring-and-matching.md](../requirements/acceptance-scoring-and-matching.md).

---

## Documents in this segment

| Document | Contents |
|----------|----------|
| [overview.md](./overview.md) | This file — scope |
| skill-extraction.md | Keyword-to-skill detection |
| scoring-logic.md | Weighted score formula |
| suggestions.md | Suggestion rules |
| implementation.md | Combined service source |
| verify-and-wrapup.md | Tests and close |

This segment is split into **6 commits** (one document per commit).

---

## Prerequisites

- [ ] PDF extraction returns text ([pdf-extraction/verify-and-wrapup.md](../pdf-extraction/verify-and-wrapup.md))
- [ ] App boots on 8080
- [ ] Sample resumes with known skills for testing

---

## Success criteria

- [ ] Skills detected match keywords in text (case-insensitive)
- [ ] Score computed from weighted skill points (0–100)
- [ ] Suggestions appear for missing skills and thin resumes
- [ ] Results included in service response
- [ ] No ATS / role / section logic required yet

---

## Next document

**skill-extraction.md** — keyword list and detection logic for `extractSkills`.
