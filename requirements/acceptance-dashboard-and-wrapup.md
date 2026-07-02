# Acceptance Criteria — Dashboard & Wrap-Up

## Scope

This document defines **pass/fail conditions** for:

- **F8** — Resume section detection  
- **F9** — Smart suggestions  
- **F10** — Career roadmap (role-based)  
- **F11** — Interactive dashboard UI  
- **F12** — Unified analysis response (UI parsing side)  

It also **closes** the requirements documentation segment with a full index and sign-off.

Prior acceptance docs: [acceptance-upload-and-parsing.md](./acceptance-upload-and-parsing.md), [acceptance-scoring-and-matching.md](./acceptance-scoring-and-matching.md).

---

## F8 — Resume section detection

### AC-SEC01 — All sections found

| | |
|---|---|
| **Given** | PDF text contains `skills`, `education`, `experience` (any casing) |
| **When** | Analyzed |
| **Then** | Response includes: `Skills Section: Found ✅`, `Education Section: Found ✅`, `Experience Section: Found ✅` |

### AC-SEC02 — Missing experience

| | |
|---|---|
| **Given** | PDF has `skills` and `education` but not `experience` |
| **When** | Analyzed |
| **Then** | `Experience Section: Missing ❌`; other two Found ✅ |

### AC-SEC03 — All sections missing

| | |
|---|---|
| **Given** | PDF with no section keywords |
| **When** | Analyzed |
| **Then** | All three lines show `Missing ❌` |

### AC-SEC04 — Block label present

| | |
|---|---|
| **Given** | Any successful analysis |
| **When** | Body inspected |
| **Then** | Block starts with `Resume Sections:` followed by exactly three section lines |

### AC-SEC05 — UI sections card populated

| | |
|---|---|
| **Given** | Successful browser upload |
| **When** | Dashboard visible |
| **Then** | Resume Sections card (`#sections`) shows three section status lines from API |

---

## F9 — Smart suggestions

### AC-G01 — Suggestion for missing Java

| | |
|---|---|
| **Given** | PDF without `java` in text |
| **When** | Analyzed |
| **Then** | `Suggestions:` includes text about Java foundation / OOP |

### AC-G02 — Suggestion for missing Spring Boot

| | |
|---|---|
| **Given** | PDF without `spring` in text |
| **When** | Analyzed |
| **Then** | `Suggestions:` includes REST API / Spring Boot project tip |

### AC-G03 — Low skill count bonus suggestion

| | |
|---|---|
| **Given** | Fewer than 3 skills detected overall |
| **When** | Analyzed |
| **Then** | `Suggestions:` includes recommendation to add real-world projects (≥2) |

### AC-G04 — No duplicate suggestions for present skills

| | |
|---|---|
| **Given** | Fixture with all six skills detected |
| **When** | Analyzed |
| **Then** | Skill-specific missing suggestions absent (project rule may still apply if skill count < 3) |

### AC-G05 — UI suggestions list

| | |
|---|---|
| **Given** | Response with non-empty `Suggestions: [...]` |
| **When** | Dashboard rendered |
| **Then** | `#suggestions` `<ul>` contains one `<li>` per suggestion |

---

## F10 — Career roadmap

### AC-RM01 — Java Developer roadmap header

| | |
|---|---|
| **Given** | `role=Java Developer` |
| **When** | Analyzed |
| **Then** | Body contains `Career Roadmap for Java Developer:` |

### AC-RM02 — Java Developer conditional tips

| | |
|---|---|
| **Given** | Java Developer role; missing Spring Boot |
| **When** | Analyzed |
| **Then** | Roadmap includes Spring Boot / REST API learning line |

### AC-RM03 — Full Stack Developer roadmap

| | |
|---|---|
| **Given** | `role=Full Stack Developer` |
| **When** | Analyzed |
| **Then** | Roadmap includes full stack project theme; JS/HTML tips if those skills missing |

### AC-RM04 — Data Analyst roadmap

| | |
|---|---|
| **Given** | `role=Data Analyst` |
| **When** | Analyzed |
| **Then** | Roadmap includes Python, SQL, visualization, and real datasets themes |

### AC-RM05 — UI roadmap card

| | |
|---|---|
| **Given** | Successful upload |
| **When** | Dashboard visible |
| **Then** | `#roadmap` displays full roadmap text from API |

---

## F11 — Interactive dashboard UI

### AC-UI01 — Static page served at root

| | |
|---|---|
| **Given** | Spring Boot running with static resources |
| **When** | Browser opens `http://localhost:8080/` |
| **Then** | Landing page loads without separate frontend server |

### AC-UI02 — Dashboard hidden initially

| | |
|---|---|
| **Given** | First page load |
| **When** | Inspecting DOM |
| **Then** | `#dashboard` not visible; `#hero` visible |

### AC-UI03 — Dashboard shown after success

| | |
|---|---|
| **Given** | Valid upload completes |
| **When** | Response parsed |
| **Then** | `#dashboard` displayed; `#hero` hidden |

### AC-UI04 — Score card

| | |
|---|---|
| **Given** | Parsed score = N |
| **When** | Dashboard rendered |
| **Then** | Doughnut chart on `#scoreChart`; `#scoreText` shows `N / 100` |

### AC-UI05 — Skills card

| | |
|---|---|
| **Given** | Parsed skills list non-empty |
| **When** | Dashboard rendered |
| **Then** | `#skills` contains styled `<span>` per skill |

### AC-UI06 — Skill analysis bar chart

| | |
|---|---|
| **Given** | Skills list non-empty |
| **When** | Dashboard rendered |
| **Then** | Chart.js bar chart rendered on `#barChart` with skill labels |

### AC-UI07 — ATS card

| | |
|---|---|
| **Given** | Parsed ATS score and missing keywords |
| **When** | Dashboard rendered |
| **Then** | `#atsScore` and `#atsMissing` populated |

### AC-UI08 — Insight card (client-side)

| | |
|---|---|
| **Given** | Parsed score |
| **When** | Dashboard rendered |
| **Then** | `#ai` shows tiered message: ≥80 excellent, ≥60 good, ≥40 needs work, else weak |

### AC-UI09 — Chart.js loaded

| | |
|---|---|
| **Given** | Page source |
| **When** | Inspecting `<head>` |
| **Then** | Chart.js included via CDN script |

### AC-UI10 — Visual design baseline

| | |
|---|---|
| **Given** | Dashboard visible |
| **When** | Visual review |
| **Then** | Dark theme, card grid layout, gradient/accent styling present |

### AC-UI11 — Re-submit destroys old charts

| | |
|---|---|
| **Given** | User analyzes twice in same session without refresh |
| **When** | Second response returns |
| **Then** | No duplicate chart instances; prior charts destroyed before redraw |

---

## F12 — Unified response & UI parsing

### AC-PARSE01 — All required labels parseable

| | |
|---|---|
| **Given** | Successful API response |
| **When** | Frontend regex applied per [api-contract.md](./api-contract.md) |
| **Then** | Skills, score, suggestions, ATS score, ATS missing, sections, roadmap all extract without throwing |

### AC-PARSE02 — Empty skills array handled

| | |
|---|---|
| **Given** | `Skills: []` |
| **When** | Dashboard renders |
| **Then** | Skills card empty; score chart shows 0; no JS crash |

### AC-PARSE03 — Error response handling

| | |
|---|---|
| **Given** | Body starts with `Error while processing resume` |
| **When** | Returned to UI |
| **Then** | **Recommended:** user-visible error; **v1 minimum:** no silent success dashboard with empty garbage |

### AC-PARSE04 — Label stability

| | |
|---|---|
| **Given** | Backend response format |
| **When** | Compared to api-contract |
| **Then** | No renames of `Skills:`, `Score:`, `ATS Score:`, `Resume Sections:`, `Career Roadmap` without UI update |

---

## End-to-end requirements sign-off

### Master checklist — all v1 features

| Feature | Doc | Accepted |
|---------|-----|----------|
| F1 Upload | acceptance-upload-and-parsing | ☐ |
| F2 Role | acceptance-upload-and-parsing | ☐ |
| F3 PDF parse | acceptance-upload-and-parsing | ☐ |
| F4 Skills | acceptance-scoring-and-matching | ☐ |
| F5 Score | acceptance-scoring-and-matching | ☐ |
| F6 ATS | acceptance-scoring-and-matching | ☐ |
| F7 Role match | acceptance-scoring-and-matching | ☐ |
| F8 Sections | this document | ☐ |
| F9 Suggestions | this document | ☐ |
| F10 Roadmap | this document | ☐ |
| F11 Dashboard | this document | ☐ |
| F12 Unified response | this document | ☐ |

### E2E scenario — full v1 demo

| Step | Action | Pass criteria |
|------|--------|---------------|
| 1 | Start app on 8080 | No startup errors |
| 2 | Open browser root | Hero + upload form |
| 3 | Upload rich PDF + Java Developer | Dashboard appears |
| 4 | Verify cards | Score, skills, charts, suggestions, insight, ATS, sections, roadmap all populated |
| 5 | Upload minimal PDF + Data Analyst | Lower scores; suggestions present; no crash |

---

## Requirements segment — document index

| # | Document | Purpose |
|---|----------|---------|
| 1 | [overview.md](./overview.md) | Scope, goals, document map |
| 2 | [must-have-features.md](./must-have-features.md) | F1–F12 feature checklist |
| 3 | [future-scope.md](./future-scope.md) | Deferred enhancements |
| 4 | [api-contract.md](./api-contract.md) | Endpoint, inputs, outputs, samples |
| 5 | [acceptance-upload-and-parsing.md](./acceptance-upload-and-parsing.md) | F1–F3 acceptance |
| 6 | [acceptance-scoring-and-matching.md](./acceptance-scoring-and-matching.md) | F4–F7 acceptance |
| 7 | [acceptance-dashboard-and-wrapup.md](./acceptance-dashboard-and-wrapup.md) | F8–F12 acceptance + close |

---

## Deliverable confirmation

The requirements segment deliverable was:

> Feature checklist and acceptance criteria for each feature.

| Deliverable item | Location | Status |
|------------------|----------|--------|
| Must-have feature checklist | must-have-features.md | Complete |
| Nice-to-have / future items | future-scope.md | Complete |
| Inputs: PDF + role | api-contract.md | Complete |
| Outputs: skills, score, ATS %, match %, sections, suggestions | api-contract.md, must-have-features.md | Complete |
| Acceptance criteria per feature area | Three acceptance-*.md files | Complete |

---

## Traceability

| Definition (why) | Requirements (what) |
|------------------|---------------------|
| [value-proposition.md](../definition/value-proposition.md) | F4–F11 must-haves |
| [user-flow.md](../definition/user-flow.md) | F1, F2, F11 + UI acceptance |
| [vision-summary.md](../definition/vision-summary.md) | future-scope.md exclusions |

---

## What comes next

With **what** locked in `requirements/`, the next documentation work covers **tech stack and architecture design**: chosen tools, layer layout, API flow diagram, and environment assumptions before scaffolding code.

---

## Requirements complete

ResumeDoc v1 behavior is specified, bounded, and testable. Implementation and testing can proceed against this folder without re-negotiating scope.

**Requirements documentation for this segment is complete.**
