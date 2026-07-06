# REST API Layer — Overview

## Purpose

Scaffolding in `scaffolding/` produced an **empty bootable** Spring Boot app. This `api-layer/` folder documents how to expose the first working **upload endpoint** wired to a **stub service** — before PDF parsing or analysis logic exist.

Goal: `POST /resume/upload` accepts a file and role, returns HTTP `200` with a placeholder body. Postman can verify the contract; real resume processing comes in later segments.

---

## What this segment delivers

| Deliverable | Description |
|-------------|-------------|
| `ResumeController` | `@RestController` mapped to `/resume` |
| `POST /resume/upload` | Multipart `file` + `role` parameters |
| `ResumeService` stub | `processResume()` returns placeholder string |
| Dependency injection | Controller autowires service |
| Postman verification | PDF + role → `200 OK` |

**Not in this segment:** PDFBox extraction, skill detection, scoring, or dashboard UI.

---

## Endpoint summary

| Property | Value |
|----------|-------|
| Method | `POST` |
| Path | `/resume/upload` |
| Class mapping | `@RequestMapping("/resume")` on controller |
| Method mapping | `@PostMapping("/upload")` |
| Request | `multipart/form-data` |
| Params | `file` (`MultipartFile`), `role` (`String`) |
| Response | `200 OK`, plain text body |
| Auth | None |

Full contract: [api-contract.md](../requirements/api-contract.md).

---

## Layer placement

```
Client (Postman / future browser)
        │
        ▼
ResumeController     ← this segment
        │
        ▼
ResumeService (stub) ← this segment
        │
        ▼
(real analysis)      ← later segments
```

Aligns with [layers-and-packages.md](../architecture/layers-and-packages.md).

---

## Classes to create

| Class | Package | Segment state |
|-------|---------|---------------|
| `ResumeController` | `com.resume.analyzer.controller` | Full upload endpoint |
| `ResumeService` | `com.resume.analyzer.service` | Stub only — placeholder return |

---

## Stub vs final behavior

| Aspect | This segment (stub) | Later segments |
|--------|---------------------|----------------|
| `processResume` body | Fixed placeholder string | PDF parse + full analysis |
| Response format | Simple text or minimal labels | Full labeled contract |
| Dependencies used | Spring Web only | PDFBox, keyword logic |
| Postman test | Confirms route + 200 | Confirms real skills/score |

Stub example return:

```text
Resume analysis will appear here. Upload received successfully.
```

Or a minimal structured placeholder:

```text
Skills: []
Score: 0/100
Suggestions: [Analysis pending]
```

Either is acceptable for stub; final format must match api-contract before UI integration.

---

## API layer documents (planned)

| Document | Contents |
|----------|----------|
| [overview.md](./overview.md) | This file — scope, endpoint summary |
| controller-design.md | Annotations, mappings, method signature |
| service-stub.md | `ResumeService` stub design |
| implementation.md | Complete controller + service source |
| postman-testing.md | Manual API verification steps |
| verify-and-wrapup.md | Acceptance checklist + segment close |

This segment is split into **6 commits** (one document per commit).

---

## Prerequisites

Before starting this segment:

- [ ] Scaffold boots on port 8080 ([verify-and-wrapup.md](../scaffolding/verify-and-wrapup.md))
- [ ] Empty `controller/` and `service/` packages exist
- [ ] `spring-boot-starter-web` in `pom.xml`
- [ ] App restarted after adding new classes

---

## Success criteria

API layer segment is complete when:

- [ ] `ResumeController` compiles and is a Spring bean
- [ ] `ResumeService` compiles with stub `processResume(MultipartFile, String)`
- [ ] `POST /resume/upload` returns `200` via Postman with PDF + role
- [ ] Response body is non-empty string from service
- [ ] No analysis logic required yet

Maps to requirements **F1** (upload) at stub level — full F1 acceptance in later segments with real parsing.

---

## Traceability

| Prior doc | This segment |
|-----------|--------------|
| [api-contract.md](../requirements/api-contract.md) | Endpoint shape implemented |
| [acceptance-upload-and-parsing.md](../requirements/acceptance-upload-and-parsing.md) | AC-U01 partial (200 + body) |
| [scaffolding/package-structure.md](../scaffolding/package-structure.md) | Where files live |

---

## What comes after

1. **PDF upload & text extraction** — PDFBox inside `processResume`
2. Skill extraction, scoring, ATS, role match, sections, suggestions
3. **Frontend dashboard** — browser calls same endpoint

---

## Next document

**controller-design.md** — `ResumeController` annotations, URL mappings, and request parameter binding.
