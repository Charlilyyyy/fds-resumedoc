# PDF Extraction — Overview

## Purpose

The API layer in `api-layer/` exposed `POST /resume/upload` with a **stub** `ResumeService`. This `pdf-extraction/` folder documents how to replace that stub so the service **reads the uploaded PDF and extracts plain text** using Apache PDFBox.

Goal: a valid PDF resume becomes a reliable text string inside `processResume`. The controller stays unchanged. Skill scoring and dashboard parsing come later.

---

## What this segment delivers

| Deliverable | Description |
|-------------|-------------|
| PDFBox integration | Load PDF from `MultipartFile` input stream |
| Text extraction | `PDFTextStripper` produces full resume text |
| Resource cleanup | Document closed after read (try-with-resources or equivalent) |
| Error handling | Invalid / corrupt PDF → clear error message string |
| Verification | Return or inspect extracted text to confirm parsing works |

**Not in this segment:** skill lists, scores, ATS %, role match, or dashboard UI parsing of those fields.

---

## Where code changes live

```
ResumeController          ← unchanged
        │
        ▼
ResumeService.processResume()  ← replace stub with PDFBox pipeline
        │
        ├── MultipartFile.getInputStream()
        ├── PDDocument.load(...)
        ├── PDFTextStripper.getText(...)
        └── document.close() / try-with-resources
```

Package: `com.resume.analyzer.service`  
Dependency: `org.apache.pdfbox:pdfbox:2.0.30` (already in scaffolding POM)

---

## Pipeline at this stage

```
PDF file  →  InputStream  →  PDDocument  →  plain text String
                                              │
                                              ├─ temporary: return text in HTTP body
                                              └─ later: feed into extractSkills, etc.
```

For verification, returning the extracted text (or a short prefix + length) is enough. Role may still be ignored until matching is implemented.

---

## Prerequisites

Before this segment:

- [ ] `POST /resume/upload` works with stub ([api-layer/verify-and-wrapup.md](../api-layer/verify-and-wrapup.md))
- [ ] PDFBox on classpath ([scaffolding/dependencies.md](../scaffolding/dependencies.md))
- [ ] Sample text-based PDF resume available for Postman
- [ ] App boots on port 8080

---

## Documents in this segment

| Document | Contents |
|----------|----------|
| [overview.md](./overview.md) | This file — scope and pipeline |
| pdfbox-setup.md | Dependency, imports, classes used |
| extraction-flow.md | Step-by-step processResume extraction |
| implementation.md | Complete service source for extraction stage |
| error-handling.md | Corrupt PDF, empty text, cleanup |
| verify-and-wrapup.md | Tests, acceptance, segment close |

This segment is split into **6 commits** (one document per commit).

---

## Success criteria

PDF extraction is complete when:

- [ ] Valid PDF upload returns extracted text (or confirms text was read)
- [ ] Corrupt / non-PDF returns a clear error message without crashing the server
- [ ] `PDDocument` is closed after use (no resource leaks on repeated uploads)
- [ ] Controller and URL/contract for upload remain the same
- [ ] No skill/score/ATS logic required yet

Maps to requirement **F3** (PDF text extraction) and acceptance criteria in [acceptance-upload-and-parsing.md](../requirements/acceptance-upload-and-parsing.md) for extraction cases.

---

## Traceability

| Prior doc | This segment |
|-----------|--------------|
| [api-layer/service-stub.md](../api-layer/service-stub.md) | Stub replaced with real PDF load |
| [architecture/tech-stack.md](../architecture/tech-stack.md) | PDFBox 2.0.30 |
| [architecture/request-flow.md](../architecture/request-flow.md) | Steps 4.1–4.4 implemented |
| [must-have-features.md](../requirements/must-have-features.md) F3 | Extraction requirement |

---

## Stub vs extraction stage

| Aspect | Previous (stub) | This segment |
|--------|-----------------|--------------|
| Input | File ignored | File stream opened |
| Library | None | PDFBox |
| Output body | Placeholder string | Extracted text (or error) |
| Role | Echoed / ignored | Still unused for analysis |

---

## What comes after

1. **Skill extraction & resume scoring** — scan text for keywords, compute 0–100 score  
2. ATS keywords, role matching, sections, suggestions  
3. Unified labeled response format for the dashboard  

---

## Next document

**pdfbox-setup.md** — Maven dependency confirmation, Java imports, and core PDFBox classes (`PDDocument`, `PDFTextStripper`).
