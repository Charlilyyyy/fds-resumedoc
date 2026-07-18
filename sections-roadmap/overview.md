# Section Detection & Career Roadmap — Overview

## Purpose

With skills, score, suggestions, ATS, and role match complete, this `sections-roadmap/` folder documents the final backend analysis pieces: **resume section detection** and a **role-based career roadmap**, plus assembling the complete labeled response the dashboard consumes.

Goal: report whether standard sections exist, produce role-specific guidance, and finalize the unified response format.

---

## What this segment delivers

| Deliverable | Description |
|-------------|-------------|
| `detectSections(text)` | Found/Missing for Skills, Education, Experience |
| `getJobSuggestions(skills, role)` | Role-specific career roadmap |
| Response assembly | Full labeled body in fixed order |

This completes the **backend analysis pipeline**. Frontend dashboard is the next segment.

---

## Where code changes live

```
ResumeService.processResume()
        ├── detectSections(text)          → section status block
        └── getJobSuggestions(skills,role)→ career roadmap block
        → concatenate all blocks into final response
```

Package: `com.resume.analyzer.service`.

---

## Feature mapping

| Requirement | This segment |
|-------------|--------------|
| F8 Resume section detection | `detectSections` |
| F10 Career roadmap | `getJobSuggestions` |
| F12 Unified response | Full block assembly |

Acceptance: [acceptance-dashboard-and-wrapup.md](../requirements/acceptance-dashboard-and-wrapup.md) (F8, F10, F12).

---

## Documents in this segment

| Document | Contents |
|----------|----------|
| [overview.md](./overview.md) | This file — scope |
| section-detection.md | Section heading checks |
| career-roadmap.md | Role-based roadmap logic |
| response-assembly.md | Full labeled response order |
| implementation.md | Complete final service source |
| verify-and-wrapup.md | Tests and close |

This segment is split into **6 commits** (one document per commit).

---

## Prerequisites

- [ ] Role matching working ([role-matching/verify-and-wrapup.md](../role-matching/verify-and-wrapup.md))
- [ ] ATS and scoring blocks in response

---

## Success criteria

- [ ] Sections reported as Found/Missing
- [ ] Career roadmap varies by role
- [ ] Final response includes all blocks in contract order
- [ ] Backend pipeline complete (ready for UI)

---

## Next document

**section-detection.md** — heading detection for Skills/Education/Experience.
