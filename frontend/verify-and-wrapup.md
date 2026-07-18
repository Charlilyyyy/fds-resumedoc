# Verify & Wrap-Up

## Purpose

Confirm the dashboard renders end-to-end in the browser and close the `frontend/` segment.

Acceptance: [acceptance-dashboard-and-wrapup.md](../requirements/acceptance-dashboard-and-wrapup.md) (F11, AC-UI, AC-PARSE).

---

## Verify steps

### 1 — Serve the page

```bash
./mvnw spring-boot:run
```

Open `http://localhost:8080/` → hero + upload form visible. (AC-UI01)

### 2 — Happy path

Pick a PDF, select `Java Developer`, click Analyze:

| Check | Expected |
|-------|----------|
| Loader | Spinner shows during request | (AC-UI02) |
| View switch | Hero hides, dashboard shows | (AC-UI03) |
| Score | Doughnut + `N/100` text | (AC-UI04) |
| Skills | Skill tags rendered | (AC-UI05) |
| ATS | Score % + missing keywords | (AC-UI06) |
| Sections | Found/Missing lines | (AC-UI07) |
| Roadmap | Role roadmap text | (AC-UI08) |
| Insight | Band message from score | (AC-UI09) |

### 3 — Re-submit

Analyze a second PDF without reload → charts redraw (no stacking), cards update. (AC-UI11)

### 4 — Missing input

Submit with no file or no role → browser blocks (required fields). (AC-UI12)

### 5 — Error body

Upload a non-PDF → response is the error string; dashboard renders blank fields without crashing. (AC-PARSE02)

---

## Sign-off checklist

- [ ] Page served at `/`
- [ ] Upload posts `file` + `role`
- [ ] Response parsed into all fields
- [ ] Score + skills charts render
- [ ] All result cards populated
- [ ] Insight band shown
- [ ] Re-submit does not duplicate charts
- [ ] Dark theme + responsive layout

---

## Deliverable confirmation

| Deliverable | Evidence |
|-------------|----------|
| Static dashboard served by Spring Boot | index.html at `/` |
| Upload → analyze → visualize flow | end-to-end test |
| Charts + cards + insight | render functions |

**Interactive dashboard that uploads, analyzes, and visualizes a resume** ✓

---

## Requirements mapping

| Item | Status |
|------|--------|
| F11 Dashboard UI | Done |
| F12 Unified response (client) | Done |

The full application is now feature-complete end-to-end.

---

## Document index

| # | Document |
|---|----------|
| 1 | [overview.md](./overview.md) |
| 2 | [layout-structure.md](./layout-structure.md) |
| 3 | [styling.md](./styling.md) |
| 4 | [upload-and-fetch.md](./upload-and-fetch.md) |
| 5 | [response-parsing.md](./response-parsing.md) |
| 6 | [charts-and-cards.md](./charts-and-cards.md) |
| 7 | [verify-and-wrapup.md](./verify-and-wrapup.md) |

---

## What comes next

**Testing & integration** — context test, manual test matrix, API and edge-case checks across the whole flow.

---

## Frontend dashboard complete

Users can upload a resume, pick a role, and see a full visual analysis in the browser.

**Frontend dashboard documentation for this segment is complete.**
