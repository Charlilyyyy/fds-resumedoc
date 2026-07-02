# Architecture Overview

## Purpose

Requirements in `requirements/` define **what** ResumeDoc must do. This `architecture/` folder defines **how** it is built: chosen technologies, system layout, layer responsibilities, and request flow before scaffolding or implementation.

The goal is a shared technical picture so backend, frontend, and tooling decisions stay aligned from the first commit of code through deployment demos.

---

## Why architecture comes before scaffolding

| Risk without architecture | How this folder helps |
|---------------------------|----------------------|
| Random dependency additions | Tech stack is chosen and justified upfront |
| Logic scattered across layers | Controller → Service → (Repository) boundaries are fixed |
| Frontend/backend mismatch | Single request-flow story from browser to PDFBox |
| Duplicate API design work | Endpoint shape points to [api-contract.md](../requirements/api-contract.md) |

Implementation can start once a developer can answer: *What runs where, and what calls what?*

---

## Architectural style

ResumeDoc v1 is a **monolithic Spring Boot application** with a **server-hosted static frontend**.

| Property | Choice |
|----------|--------|
| Deployment unit | One JVM process (backend JAR) |
| Frontend delivery | Static HTML/CSS/JS from `src/main/resources/static` |
| API style | REST-style POST endpoint; v1 response is plain text |
| Persistence | Optional — JPA/MySQL scaffold may exist; core flow is stateless |
| Auth | None in v1 |

No microservices, message queues, or separate frontend build pipeline for v1. Complexity stays appropriate for a portfolio demo and a single upload-analyze-display loop.

---

## High-level system context

```
┌─────────────┐         HTTP          ┌──────────────────────────────────┐
│   Browser   │ ◄──────────────────► │  Spring Boot (single process)   │
│  HTML/JS    │   POST /resume/upload │  ┌──────────┐    ┌────────────┐ │
│  Chart.js   │   GET  /  (static)    │  │Controller│───►│  Service   │ │
└─────────────┘                       │  └──────────┘    │  + PDFBox  │ │
                                      │                  └────────────┘ │
                                      │  Optional: JPA ──► MySQL        │
                                      └──────────────────────────────────┘
```

---

## What this segment delivers

By the end of the architecture documentation set:

1. **Tech stack** — Languages, frameworks, libraries, and dev tools with versions
2. **Layer design** — Package layout and responsibilities (controller, service, model, repository)
3. **Request flow** — Step-by-step path from upload to dashboard response
4. **Diagrams** — Visual architecture and sequence views
5. **Wrap-up** — Link to requirements API contract; readiness for project scaffolding

Deliverable from the rebuild plan: **architecture diagram + API contract**. The API contract already lives in `requirements/api-contract.md`; architecture docs reference it rather than duplicate it.

---

## Architecture documents (planned)

| Document | Contents |
|----------|----------|
| [overview.md](./overview.md) | This file — scope, style, document map |
| tech-stack.md | Backend, frontend, PDF, build, optional DB |
| layers-and-packages.md | Layer boundaries and package structure |
| request-flow.md | End-to-end processing pipeline |
| diagrams.md | System context, layer, and sequence diagrams |
| summary-wrapup.md | Consolidated view + segment close |

This segment is split into **6 commits** (one document per commit, in the order above).

---

## Layer summary (preview)

| Layer | Responsibility |
|-------|----------------|
| **Static UI** | Upload form, role select, fetch API, parse response, Chart.js dashboard |
| **Controller** | HTTP mapping, multipart binding, delegate to service, return `ResponseEntity` |
| **Service** | PDF extraction, skill/score/ATS/role/section/suggestion logic |
| **Model / Repository** | Optional JPA entity for resume metadata; not on critical path for v1 demo |

Detailed boundaries are in `layers-and-packages.md`.

---

## Traceability

| Requirements | Architecture response |
|--------------|----------------------|
| F1–F3 Upload & parsing | Spring MVC + PDFBox in service layer |
| F4–F10 Analysis logic | Centralized in service class |
| F11 Dashboard | Static assets co-located in Spring Boot |
| F12 Text response | Controller returns `String` body |
| [api-contract.md](../requirements/api-contract.md) | `POST /resume/upload` — params and response format |

---

## Environment assumptions (preview)

| Assumption | Typical value |
|------------|---------------|
| Java | 17 |
| Server port | 8080 |
| Local URL | `http://localhost:8080/` |
| Database | MySQL optional for scaffold; analysis works without persisting |
| IDE | IntelliJ or Eclipse |
| API testing | Postman or cURL |

Full stack versions and dependencies are documented in `tech-stack.md`.

---

## Success criteria for this segment

Architecture work is complete when:

- Every major technology choice is named and justified
- Layer responsibilities are clear with no ambiguous “where does this go?”
- Request flow is traceable from browser click to response string
- At least one system diagram and one sequence diagram exist
- A developer can scaffold the project without revisiting requirements for stack decisions

---

## Next document

**tech-stack.md** — concrete tools: Java 17, Spring Boot 3, Maven, PDFBox, HTML/CSS/JS, Chart.js, and optional JPA/MySQL.
