# Architecture Summary & Wrap-Up

## Consolidated technical picture

ResumeDoc v1 is a **single Spring Boot monolith** on **Java 17** that:

1. Serves a static **HTML/CSS/JavaScript** dashboard from `src/main/resources/static`
2. Exposes **`POST /resume/upload`** for PDF + role analysis
3. Uses **Apache PDFBox** to extract text, then runs **keyword-based analysis** in a central **service** class
4. Returns a **labeled plain-text body** the browser parses into charts and cards
5. Optionally scaffolds **JPA/MySQL** without requiring it for the core demo

No microservices, no separate frontend build, no auth, no external AI APIs in v1.

---

## Stack summary

| Concern | Choice |
|---------|--------|
| Runtime | JVM, Spring Boot 3.x, embedded Tomcat |
| Build | Maven (`mvnw`) |
| API | Spring MVC, multipart upload |
| PDF | PDFBox 2.0.30 |
| UI | Static `index.html` + Chart.js CDN |
| Optional DB | Spring Data JPA + MySQL |
| Dev tools | Lombok, DevTools, Actuator, Postman |

Full detail: [tech-stack.md](./tech-stack.md).

---

## Layer summary

```
Browser (static/)  →  ResumeController  →  ResumeService  →  PDFBox + analysis helpers
                                              ↳ ResumeRepository (optional, not on v1 hot path)
```

| Package | Key class | Role |
|---------|-----------|------|
| `com.resume.analyzer` | `BackendApplication` | Boot entry point |
| `.controller` | `ResumeController` | HTTP in/out |
| `.service` | `ResumeService` | All business logic |
| `.model` / `.repository` | `Resume`, `ResumeRepository` | Optional persistence |

Full detail: [layers-and-packages.md](./layers-and-packages.md).

---

## Request path summary

| Step | What happens |
|------|--------------|
| 1 | User submits PDF + role from browser |
| 2 | `POST /resume/upload` hits controller |
| 3 | Service loads PDF, extracts text |
| 4 | Service runs skills → score → suggestions → match → ATS → sections → roadmap |
| 5 | Controller returns text body `200 OK` |
| 6 | JavaScript parses fields, renders dashboard |

Full detail: [request-flow.md](./request-flow.md).

---

## Diagram summary

Ten views in [diagrams.md](./diagrams.md):

- System context and container (monolith JAR)
- Layer and pipeline flowcharts
- Sequence diagram for one analyze action
- Local deployment and URL routing
- Data flow from inputs to dashboard cards
- Optional persistence (future)

Use diagram **#1** for stakeholders, **#5** for debugging uploads, **#4** for implementing service logic.

---

## API contract (by reference)

Architecture does **not** redefine the API. The authoritative contract is:

**[requirements/api-contract.md](../requirements/api-contract.md)**

| Item | Value |
|------|-------|
| Endpoint | `POST /resume/upload` |
| Inputs | `file` (PDF), `role` (string) |
| Output | Plain text with `Skills:`, `Score:`, `ATS Score:`, `Resume Sections:`, `Career Roadmap`, etc. |

Deliverable check: **architecture diagram** ✓ (diagrams.md) + **API contract** ✓ (requirements folder).

---

## Architecture document index

| # | Document | Purpose |
|---|----------|---------|
| 1 | [overview.md](./overview.md) | Scope, monolith style, document map |
| 2 | [tech-stack.md](./tech-stack.md) | Dependencies and tooling |
| 3 | [layers-and-packages.md](./layers-and-packages.md) | Code structure and boundaries |
| 4 | [request-flow.md](./request-flow.md) | Step-by-step processing |
| 5 | [diagrams.md](./diagrams.md) | Visual architecture set |
| 6 | [summary-wrapup.md](./summary-wrapup.md) | This file — consolidated close |

---

## Traceability across documentation folders

| Folder | Answers |
|--------|---------|
| `definition/` | Why ResumeDoc exists, who it serves |
| `requirements/` | What features must ship, acceptance criteria |
| `architecture/` | How it is structured and which tools are used |

```
definition  →  requirements  →  architecture  →  (next: scaffolding & code)
     why            what              how
```

---

## Design decisions recorded

| Decision | Rationale |
|----------|-----------|
| Monolith over microservices | One upload-analyze flow; simpler portfolio demo |
| Plain-text API over JSON (v1) | Faster UI regex parsing; JSON deferred |
| Logic in one service class | Clear pipeline; easy to read for learners |
| Static UI over React (v1) | Meets F11 without npm build chain |
| PDFBox over cloud OCR | Local, free, deterministic |
| Optional JPA | Scaffold for learning; stateless demo still works |
| Keyword matching over ML | Predictable, testable, no API keys |

---

## Readiness for scaffolding

A developer can start **project scaffolding** when they can answer:

- [x] Which JDK and Spring Boot version?
- [x] Which Maven dependencies are required?
- [x] What packages and classes to create?
- [x] What endpoint signature and response shape?
- [x] Where does the HTML file live?

Next documentation segment: **project scaffolding and environment setup** — generate the Spring Boot project, `pom.xml`, package layout, `application.properties`, `.gitignore`, and verify the app boots on port 8080.

---

## Success criteria — met

| Criterion | Status |
|-----------|--------|
| Tech stack chosen and documented | ✓ tech-stack.md |
| Layer layout defined | ✓ layers-and-packages.md |
| Request flow documented | ✓ request-flow.md |
| Architecture diagrams provided | ✓ diagrams.md |
| API contract linked (not duplicated) | ✓ api-contract.md |
| Ready for empty runnable app work | ✓ |

---

## Architecture complete

ResumeDoc’s technical shape is defined: one JVM, one upload endpoint, one service pipeline, one static dashboard. Implementation and environment setup can proceed without reopening stack or layer decisions.

**Architecture documentation for this segment is complete.**
