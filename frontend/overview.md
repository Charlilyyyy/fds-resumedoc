# Frontend Dashboard UI — Overview

## Purpose

The backend now returns a complete labeled analysis for each upload. This `frontend/` folder documents the **single-page dashboard** that lets a user upload a PDF, choose a role, call the endpoint, and see results as charts and cards.

Goal: an interactive `index.html` served by Spring Boot from `src/main/resources/static/`.

---

## What this segment delivers

| Deliverable | Description |
|-------------|-------------|
| `index.html` | One static page: hero, upload form, dashboard |
| Layout | Navbar, upload area, role dropdown, result grid |
| Styling | Dark theme, gradient, glassmorphism cards, loader |
| Upload logic | `FormData` + `fetch` POST to `/resume/upload` |
| Response parsing | Regex extraction of labeled fields |
| Visualization | Chart.js doughnut (score) + bar (skills), result cards |

Implements requirement **F11** (and the client side of F12).

---

## Where code lives

```
src/main/resources/static/index.html
```

Served automatically at `http://localhost:8080/` by Spring Boot's static resource handler. No npm build, no React (deferred in [future-scope.md](../requirements/future-scope.md)).

---

## Documents in this segment

| Document | Contents |
|----------|----------|
| [overview.md](./overview.md) | This file — scope |
| layout-structure.md | HTML structure and elements |
| styling.md | CSS theme and animations |
| upload-and-fetch.md | Form submit, FormData, fetch |
| response-parsing.md | Regex parsing of response |
| charts-and-cards.md | Chart.js + card rendering + insight |
| verify-and-wrapup.md | Browser tests and close |

This segment is split into **7 commits** (one document per commit).

---

## Prerequisites

- [ ] Backend returns full labeled response ([sections-roadmap/verify-and-wrapup.md](../sections-roadmap/verify-and-wrapup.md))
- [ ] `static/` folder exists in resources
- [ ] Chart.js reachable via CDN

---

## User flow (recap)

```
Landing (hero + upload)  →  pick PDF + role  →  submit
    →  loader  →  fetch POST /resume/upload  →  parse text
    →  show dashboard (score, skills, ATS, sections, roadmap, insight)
```

See [user-flow.md](../definition/user-flow.md).

---

## Success criteria

- [ ] Page served at `/`
- [ ] Upload posts file + role
- [ ] Response parsed into fields
- [ ] Charts and cards render
- [ ] Dark themed, responsive-ish layout
- [ ] Re-submit does not duplicate charts

---

## Next document

**layout-structure.md** — HTML skeleton for hero, form, and dashboard grid.
