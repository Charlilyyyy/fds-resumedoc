# Job Role Matching — Overview

## Purpose

Skills, score, suggestions, and ATS coverage are in place. This `role-matching/` folder documents **job role matching**: comparing detected skills against the required skills for the user-selected role and reporting a match percentage plus missing skills.

Goal: use the `role` parameter (previously unused in analysis) to produce a role match block. Controller and prior analysis stay unchanged.

---

## What this segment delivers

| Deliverable | Description |
|-------------|-------------|
| `jobRoles()` | Map of role → required skills |
| `matchJobRole(skills, role)` | Match %, missing skills for selected role |
| Response wiring | Append role match block to service output |

**Not in this segment:** section detection, career roadmap, dashboard UI.

---

## Where code changes live

```
ResumeService.processResume()
        │  skills (F4), role (request param)
        ▼
matchJobRole(skills, role)  ──►  "Role: ... / Match Score: N% / Missing Skills: [...]"
```

Package: `com.resume.analyzer.service`.

---

## Feature mapping

| Requirement | This segment |
|-------------|--------------|
| F7 Job role matching | `jobRoles`, `matchJobRole` |

Acceptance: [acceptance-scoring-and-matching.md](../requirements/acceptance-scoring-and-matching.md) (AC-M01–M07).

---

## Documents in this segment

| Document | Contents |
|----------|----------|
| [overview.md](./overview.md) | This file — scope |
| role-definitions.md | The three roles and required skills |
| matching-logic.md | Match % and missing skills |
| implementation.md | Service source for role matching |
| verify-and-wrapup.md | Tests and close |

This segment is split into **5 commits** (one document per commit).

---

## Prerequisites

- [ ] Skills detection working ([scoring/verify-and-wrapup.md](../scoring/verify-and-wrapup.md))
- [ ] `role` param reaches the service (from api-layer)

---

## Success criteria

- [ ] Three roles defined with required skills
- [ ] Match % = matched / required (integer)
- [ ] Missing skills listed for the selected role
- [ ] Role block appended to response
- [ ] No section/roadmap logic required yet

---

## Next document

**role-definitions.md** — the role → required-skills map.
