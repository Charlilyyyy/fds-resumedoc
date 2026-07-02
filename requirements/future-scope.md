# Future Scope & Deferred Features

## Purpose

Not every good idea belongs in v1. This document lists capabilities that are **explicitly deferred** so the team can ship the core upload-analyze-dashboard loop without scope creep.

Items here are **not blockers** for v1. They may be prioritized after the must-have checklist in [must-have-features.md](./must-have-features.md) is complete and tested.

---

## Priority tiers

| Tier | Meaning | When to consider |
|------|---------|------------------|
| **P2 — Near-term** | High value, moderate effort; natural v1.1 candidates | After stable v1 release |
| **P3 — Medium-term** | Larger architectural change | After usage feedback |
| **P4 — Long-term** | Strategic or optional polish | Roadmap planning only |

---

## P2 — Near-term enhancements

### User login & upload history

| Aspect | Detail |
|--------|--------|
| What | Accounts, authentication, saved past analyses |
| Why deferred | v1 is single-session by design; no persistence required for core value |
| v1 note | JPA model/repository scaffold may exist but is optional and unused in the main flow |
| Trigger to build | Users ask to compare revisions over weeks or save results |

### Deploy online

| Aspect | Detail |
|--------|--------|
| What | Host backend (e.g. cloud runtime) and static frontend (e.g. static site CDN) |
| Why deferred | Local `localhost:8080` is enough for portfolio demo and development |
| Trigger to build | Public shareable URL needed for recruiters or career office |

### JSON API response

| Aspect | Detail |
|--------|--------|
| What | Replace plain-text response with structured JSON (`skills`, `score`, `ats`, etc.) |
| Why deferred | v1 frontend parses labeled text; works for demo scope |
| Trigger to build | Mobile client, third-party integrations, or fragile regex parsing |

### Expanded job roles

| Aspect | Detail |
|--------|--------|
| What | More roles beyond Java Developer, Full Stack Developer, Data Analyst |
| Why deferred | Three roles prove role-matching pattern; more roles need curated skill maps |
| Trigger to build | Persona research shows demand for DevOps, QA, Mobile, etc. |

### Additional file formats

| Aspect | Detail |
|--------|--------|
| What | DOCX, plain text, or LinkedIn export import |
| Why deferred | PDF is the common application format; PDFBox path is already defined |
| Trigger to build | User research shows PDF-only blocks real users |

---

## P3 — Medium-term enhancements

### OpenAI / external AI integration

| Aspect | Detail |
|--------|--------|
| What | LLM-powered rewrite suggestions, semantic skill inference, natural-language summary |
| Why deferred | v1 uses deterministic keyword rules—predictable, free, no API keys |
| Risks if rushed | Cost, latency, inconsistent output, privacy of resume content |
| Trigger to build | Keyword-only suggestions feel too shallow in user testing |

### React frontend

| Aspect | Detail |
|--------|--------|
| What | Replace static `index.html` with a React SPA (components, state management, build pipeline) |
| Why deferred | Static HTML + Chart.js delivers full dashboard for portfolio scope |
| Trigger to build | Complex UI state, routing, or design system justify SPA overhead |

### Advanced analytics dashboard

| Aspect | Detail |
|--------|--------|
| What | Trend charts across uploads, historical score comparison, export reports |
| Why deferred | Requires persistence (login/history) first |
| Depends on | User login & upload history |

### Resume persistence (full implementation)

| Aspect | Detail |
|--------|--------|
| What | Store uploads, scores, and metadata in MySQL via JPA |
| Why deferred | Analysis is stateless in v1; entity/repository may be scaffold-only |
| Trigger to build | Audit trail, admin view, or history feature requested |

### Stronger PDF handling

| Aspect | Detail |
|--------|--------|
| What | OCR for scanned PDFs, layout-aware parsing, multi-column recovery |
| Why deferred | v1 assumes text-based PDFs; scanned resumes are edge cases |
| Trigger to build | High rate of “empty text” errors on real uploads |

---

## P4 — Long-term / optional

### Automatic resume rewriting

Generate edited resume PDF or DOCX from suggestions. Out of scope for feedback-only v1; blurs product into a writing tool.

### Multi-language resumes

Detection and keyword sets for non-English documents. v1 keywords are English tech terms.

### Employer / recruiter mode

Bulk candidate screening inverted for hiring managers. ResumeDoc targets job seekers, not HR teams.

### Native mobile apps

iOS/Android clients. Web-first is sufficient until mobile traffic is proven.

### Paid tiers & coaching marketplace

Monetization and human expert handoff. Portfolio/education project stays free and self-contained.

### CI/CD & observability hardening

Production-grade logging, metrics, feature flags, blue-green deploy. Relevant after public deployment, not for local demo.

---

## Explicit v1 exclusions (quick reference)

| Excluded | Documented in |
|----------|---------------|
| User authentication | This file — P2 |
| OpenAI / LLM APIs | This file — P3 |
| React SPA | This file — P3 |
| JSON response contract | This file — P2 (v1 uses text) |
| Non-PDF uploads | This file — P2 |
| Guaranteed job outcomes | [vision-summary.md](../definition/vision-summary.md) |
| Auto-edit resume content | [vision-summary.md](../definition/vision-summary.md) |

---

## How to promote an item from future → must-have

Before moving a deferred feature into active scope:

1. **User evidence** — Persona pain (e.g. Alex needs history) or repeated feedback
2. **Dependency check** — Does it require login, JSON API, or deploy first?
3. **Acceptance criteria** — Write pass/fail tests before implementation
4. **Must-have update** — Add to [must-have-features.md](./must-have-features.md) in a new version section (v1.1, v2)

---

## Relationship to README “future enhancements”

The reference implementation’s public README lists deploy, React, OpenAI, auth, and advanced analytics. This document aligns with those themes but ties each item to **priority, rationale, and trigger** so deferred work is intentional, not forgotten.

---

## Link to prior context

Must-have v1 scope is fixed in [must-have-features.md](./must-have-features.md). Next: **api-contract.md** — formal inputs, outputs, and sample response for the upload endpoint.
