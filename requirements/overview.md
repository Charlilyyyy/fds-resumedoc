# Requirements Overview

## Purpose

ResumeDoc’s idea and problem space are defined in the `definition/` folder. This `requirements/` folder locks down **what the application must do** before any implementation begins.

The goal is a shared, testable contract: which capabilities ship in the first version, which are deferred, what goes in and out of the system, and how we know each feature is done.

---

## Why requirements come before code

| Risk without requirements | How this folder helps |
|---------------------------|----------------------|
| Scope creep mid-build | Must-have list is fixed upfront |
| “Done” means different things to different people | Acceptance criteria per feature area |
| Frontend and backend disagree on shape | API contract defines inputs and outputs |
| Future ideas block v1 | Nice-to-have items are documented but excluded |

Building without this step often produces a working demo that still misses role matching, section checks, or dashboard parsing because nobody wrote down that they were required.

---

## What this segment delivers

By the end of the requirements documentation set:

1. **Must-have feature checklist** — Every v1 capability named and described
2. **Future scope list** — Explicitly out-of-v1 items (auth, external AI, React rewrite, etc.)
3. **API contract** — Request parameters and response fields the UI and backend share
4. **Acceptance criteria** — Pass/fail conditions for each feature area so manual and automated tests have a target

Together these satisfy the deliverable: **feature checklist + acceptance criteria for each feature**.

---

## v1 product boundary (summary)

ResumeDoc v1 is a **single-session resume feedback tool**:

```
PDF file  +  job role string  →  analysis  →  structured text response  →  dashboard display
```

**In v1:**
- One upload endpoint
- PDF text extraction
- Skill detection, numeric score, ATS keyword check, role match, section scan, suggestions, career roadmap
- Browser dashboard with charts and result cards
- Three supported roles: Java Developer, Full Stack Developer, Data Analyst

**Not in v1** (documented separately):
- User accounts and upload history
- Third-party AI APIs for rewriting or deep NLP
- React SPA replacement of the static frontend
- Non-PDF formats, batch upload, PDF export of results

---

## Requirements documents (planned)

| Document | Contents |
|----------|----------|
| [overview.md](./overview.md) | This file — scope, goals, document map |
| must-have-features.md | Checklist of required v1 capabilities |
| future-scope.md | Nice-to-have and deferred enhancements |
| api-contract.md | Inputs, outputs, endpoint shape, sample response |
| acceptance-upload-and-parsing.md | Criteria for upload endpoint and PDF extraction |
| acceptance-scoring-and-matching.md | Criteria for skills, score, ATS, role match |
| acceptance-dashboard-and-wrapup.md | Criteria for sections, suggestions, UI, segment close |

---

## Commit plan for this segment

This segment is split into **7 commits** (one document per commit, in the order above).

---

## Traceability to prior definition work

| Definition artifact | Requirement it drives |
|--------------------|------------------------|
| [problem-statement.md](../definition/problem-statement.md) | Pre-submit feedback → all analysis features required |
| [value-proposition.md](../definition/value-proposition.md) | Score, skills, ATS %, match %, sections, suggestions → must-have list |
| [user-personas.md](../definition/user-personas.md) | Low friction, role dropdown → v1 scope and three roles |
| [user-flow.md](../definition/user-flow.md) | Upload → role → dashboard → acceptance criteria for UI flow |
| [vision-summary.md](../definition/vision-summary.md) | ResumeDoc principles and non-goals → future-scope exclusions |

---

## Success criteria for this segment

Requirements work is complete when:

- Every v1 feature appears on the must-have checklist
- Deferred work is listed with rationale, not silently dropped
- API inputs and outputs are specified clearly enough for Postman and frontend parsing
- Each feature area has measurable acceptance criteria (happy path + key edge cases)
- A reader can implement or test ResumeDoc v1 without guessing missing behavior

---

## Next document

The first concrete artifact is **must-have-features.md** — the authoritative checklist of what ships in version one.
