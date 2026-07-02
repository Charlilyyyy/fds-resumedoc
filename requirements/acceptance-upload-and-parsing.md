# Acceptance Criteria — Upload & Parsing

## Scope

This document defines **pass/fail conditions** for:

- **F1** — PDF resume upload via HTTP  
- **F2** — Job role parameter  
- **F3** — PDF text extraction  

Analysis features (skills, score, ATS, etc.) are covered in [acceptance-scoring-and-matching.md](./acceptance-scoring-and-matching.md).

Reference: [api-contract.md](./api-contract.md), [must-have-features.md](./must-have-features.md).

---

## How to use these criteria

| Column | Meaning |
|--------|---------|
| **ID** | Unique test reference |
| **Given** | Starting state / input |
| **When** | Action performed |
| **Then** | Required outcome — all must be true to **PASS** |

Verify via **Postman** (API) and/or **browser** (UI + network tab). Mark each ID PASS or FAIL before calling upload & parsing complete.

---

## F1 — Upload endpoint

### AC-U01 — Valid PDF upload returns analysis

| | |
|---|---|
| **Given** | Server running on port 8080; valid text-based PDF resume |
| **When** | `POST /resume/upload` with `file` = PDF and `role` = `Java Developer` |
| **Then** | HTTP `200`; body is non-empty; body contains `Skills:` and `Score:` labels |

### AC-U02 — Multipart field name for file

| | |
|---|---|
| **Given** | Server running |
| **When** | Request uses param name `file` (not `resume` or other alias) |
| **Then** | Controller receives file; analysis proceeds |

**FAIL if:** Wrong param name results in empty file or 400/500 without clear behavior documented.

### AC-U03 — Missing file parameter

| | |
|---|---|
| **Given** | Server running |
| **When** | `POST /resume/upload` with `role` only, no `file` |
| **Then** | Request does not succeed with a normal analysis body (Spring returns 400 Bad Request or equivalent) |

### AC-U04 — UI file input required

| | |
|---|---|
| **Given** | User on landing page |
| **When** | User attempts submit without choosing a file |
| **Then** | Browser blocks submit (HTML `required` on file input) or equivalent client validation |

### AC-U05 — UI triggers upload on submit

| | |
|---|---|
| **Given** | User selected PDF and role |
| **When** | User clicks analyze/submit button |
| **Then** | `POST` request sent to `/resume/upload` with `FormData` containing `file` and `role` |

### AC-U06 — Loading state during request

| | |
|---|---|
| **Given** | User submitted valid form |
| **When** | Request is in flight |
| **Then** | Loading indicator visible; hero/upload area hidden or de-emphasized per UI design |

---

## F2 — Job role parameter

### AC-R01 — Role sent with every successful upload

| | |
|---|---|
| **Given** | Valid PDF |
| **When** | Upload with `role=Full Stack Developer` |
| **Then** | Response body contains `Role: Full Stack Developer` |

### AC-R02 — All three roles accepted

| | |
|---|---|
| **Given** | Same PDF uploaded three times |
| **When** | `role` = `Java Developer`, then `Full Stack Developer`, then `Data Analyst` |
| **Then** | Each returns `200` with `Role:` line matching the sent value; roadmap header matches role |

### AC-R03 — Role affects downstream output

| | |
|---|---|
| **Given** | Same PDF, different roles |
| **When** | Compare responses for Java Developer vs Data Analyst |
| **Then** | `Match Score` and/or `Missing Skills` and/or `Career Roadmap` content differs appropriately (not identical for all roles) |

### AC-R04 — UI role dropdown required

| | |
|---|---|
| **Given** | User on landing page |
| **When** | User tries submit with default empty option |
| **Then** | Submit blocked (`required` on select) or validation prevents request |

### AC-R05 — UI role options match contract

| | |
|---|---|
| **Given** | Role dropdown rendered |
| **When** | User opens dropdown |
| **Then** | Exactly three options: `Java Developer`, `Full Stack Developer`, `Data Analyst` (plus empty placeholder if used) |

### AC-R06 — Unknown role (API hardening note)

| | |
|---|---|
| **Given** | Valid PDF |
| **When** | `role=Unknown Role` via Postman |
| **Then** | **Known v1 gap:** may error at runtime; **PASS** for v1 if UI never sends unknown roles; **future:** return clear error without server crash |

---

## F3 — PDF text extraction

### AC-P01 — Text extracted from standard PDF

| | |
|---|---|
| **Given** | PDF containing visible text `"Java"`, `"Skills"`, `"Education"` |
| **When** | Upload and analyze |
| **Then** | Response includes `Java` in skills list; section detection runs (not empty error) |

### AC-P02 — PDFBox used for parsing

| | |
|---|---|
| **Given** | Code review or dependency check |
| **When** | Inspecting service implementation |
| **Then** | `PDDocument.load`, `PDFTextStripper.getText`, document closed after read |

### AC-P03 — Extracted text drives keyword detection

| | |
|---|---|
| **Given** | PDF body contains lowercase `"javascript"` and `"sql"` |
| **When** | Upload analyzed |
| **Then** | `Skills:` includes `JavaScript` and `SQL` (case-insensitive match on extracted text) |

### AC-P04 — Corrupt or non-PDF file

| | |
|---|---|
| **Given** | File that is not a valid PDF (e.g. `.txt` renamed to `.pdf`, or truncated bytes) |
| **When** | Uploaded via Postman |
| **Then** | Response body contains `Error while processing resume` (or equivalent); server process stays alive (no unhandled crash) |

### AC-P05 — Empty PDF / no extractable text

| | |
|---|---|
| **Given** | PDF with no text layer (scanned image only, or blank pages) |
| **When** | Uploaded |
| **Then** | Request completes without server crash; skills list empty or minimal; score `0` or low; suggestions may still appear |

### AC-P06 — Resource cleanup

| | |
|---|---|
| **Given** | Multiple sequential uploads (5+ requests) |
| **When** | Same server instance |
| **Then** | No progressive slowdown or OOM from leaked file handles; each request completes |

### AC-P07 — Special characters in PDF text

| | |
|---|---|
| **Given** | PDF with bullets, line breaks, and mixed casing (`"JAVA"`, `"Spring Boot"`) |
| **When** | Uploaded |
| **Then** | Extraction succeeds; keyword matching still finds `java` / `spring` via lowercasing |

---

## Integration — upload through extraction

### AC-I01 — End-to-end happy path (API)

| Step | Action | Expected |
|------|--------|----------|
| 1 | Start Spring Boot app | Listening on `8080` |
| 2 | POST PDF + `Java Developer` | `200 OK` |
| 3 | Inspect body | Contains `Skills:`, `Score:`, `Role:`, `ATS Score:`, `Resume Sections:` |

### AC-I02 — End-to-end happy path (browser)

| Step | Action | Expected |
|------|--------|----------|
| 1 | Open `http://localhost:8080/` | Landing page loads |
| 2 | Select PDF + role, submit | Dashboard appears after response |
| 3 | Network tab | Single `POST /resume/upload` with multipart body |

---

## Sign-off checklist

Upload & parsing is **accepted** when:

- [ ] AC-U01 through AC-U06 — PASS (or documented exception for U06 if minimal UI)
- [ ] AC-R01 through AC-R05 — PASS
- [ ] AC-P01 through AC-P07 — PASS (P05/P06 best-effort for v1 demo)
- [ ] AC-I01 and AC-I02 — PASS

---

## Known v1 limitations (not failures)

| Limitation | Documented in |
|------------|---------------|
| Error responses use HTTP 200 with error string | [api-contract.md](./api-contract.md) |
| Scanned PDFs without OCR | [future-scope.md](./future-scope.md) |
| Unknown role may NPE on API | AC-R06; UI prevents in normal flow |

---

## Link to prior context

Next: [acceptance-scoring-and-matching.md](./acceptance-scoring-and-matching.md) — acceptance criteria for skills, score, ATS analysis, and job role matching.
