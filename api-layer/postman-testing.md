# Postman Testing

## Purpose

Manual verification that `POST /resume/upload` is live, accepts multipart PDF + role, and returns **HTTP 200** with the stub service body.

Run these tests after [implementation.md](./implementation.md). Full analysis labels (`Skills:`, `Score:`) are validated in later segments when service logic is real.

---

## Prerequisites

| Requirement | Check |
|-------------|-------|
| App running | `./mvnw spring-boot:run` or IDE run on `BackendApplication` |
| Port | `8080` (or your `server.port`) |
| Sample PDF | Any `.pdf` file on disk (content irrelevant for stub) |
| Postman | Desktop or web client installed |

Base URL for examples:

```text
http://localhost:8080
```

---

## Test 1 — Happy path (stub)

### Request setup

| Field | Value |
|-------|-------|
| Method | `POST` |
| URL | `http://localhost:8080/resume/upload` |
| Body type | **form-data** (not raw JSON) |

### Body fields

| Key | Type | Value |
|-----|------|-------|
| `file` | **File** | Select a PDF from disk |
| `role` | **Text** | `Java Developer` |

**Critical:** Key must be `file`, not `resume` or `document`.

### Send and verify

| Check | Expected (stub) |
|-------|-----------------|
| Status | `200 OK` |
| Body | Non-empty text |
| Body contains | `role: Java Developer` (or your role string) |
| Body contains | Original PDF filename |
| Body contains | `Analysis pipeline not yet implemented` (if using reference stub) |

Example response:

```text
Resume received — file: my-resume.pdf, role: Java Developer. Analysis pipeline not yet implemented.
```

---

## Test 2 — All three roles

Repeat Test 1 with same PDF; change `role` only:

| Run | `role` value |
|-----|--------------|
| 2a | `Java Developer` |
| 2b | `Full Stack Developer` |
| 2c | `Data Analyst` |

| Check | Expected |
|-------|----------|
| Status | `200` each time |
| Body | Role string echoed matches sent value |

Confirms role param binding; matching logic not tested at stub stage.

---

## Test 3 — Missing file (negative)

| Field | Value |
|-------|-------|
| Body | `role` = `Java Developer` only — **no** `file` field |

| Expected | Detail |
|----------|--------|
| Status | `400 Bad Request` (typical Spring behavior) |
| Body | Error message from Spring, not stub success text |

Maps to **AC-U03** in requirements (partial).

---

## Test 4 — Missing role (negative)

| Field | Value |
|-------|-------|
| Body | `file` = PDF only — **no** `role` field |

| Expected | Detail |
|----------|--------|
| Status | `400 Bad Request` or empty role in stub echo |
| Note | Controller does not mark `required=false`; Spring may reject missing param |

---

## Test 5 — Wrong file field name (negative)

| Field | Value |
|-------|-------|
| Body | Key `resume` (File) + `role` (Text) — wrong key name |

| Expected | Detail |
|----------|--------|
| Status | `400 Bad Request` |
| Cause | Controller expects `@RequestParam("file")` |

Maps to **AC-U02** — proves correct param name matters.

---

## Test 6 — Non-PDF file (stub behavior)

| Field | Value |
|-------|-------|
| `file` | `.txt` or `.jpg` renamed or selected as file |
| `role` | `Java Developer` |

| Stub expected | Status `200`; stub still echoes filename |
| Later segment | Service may return processing error for invalid PDF |

Document result; no failure required for stub segment if 200 returns.

---

## Postman collection setup (optional)

Save a collection **ResumeDoc — Local** with:

| Request name | Method | URL |
|--------------|--------|-----|
| Upload — Java Developer | POST | `{{baseUrl}}/resume/upload` |
| Upload — Full Stack | POST | `{{baseUrl}}/resume/upload` |
| Upload — Data Analyst | POST | `{{baseUrl}}/resume/upload` |

Environment variable:

| Variable | Value |
|----------|-------|
| `baseUrl` | `http://localhost:8080` |

---

## cURL equivalents

Happy path:

```bash
curl -v -X POST "http://localhost:8080/resume/upload" \
  -F "file=@/path/to/resume.pdf" \
  -F "role=Java Developer"
```

Missing file:

```bash
curl -v -X POST "http://localhost:8080/resume/upload" \
  -F "role=Java Developer"
```

Look for `HTTP/1.1 200` vs `400` in verbose output.

---

## Troubleshooting

| Symptom | Likely cause | Fix |
|---------|--------------|-----|
| Connection refused | App not running | Start `BackendApplication` |
| 404 Not Found | Wrong URL | Use `/resume/upload` not `/upload` |
| 415 Unsupported Media Type | Body not form-data | Switch Postman body to form-data |
| 400 Required part 'file' | Wrong key or type | Key `file`, type File |
| Empty response body | Service returns null | Fix stub return string |
| 500 on startup | DB/JPA config | See [configuration.md](../scaffolding/configuration.md) |

---

## Stub vs full API acceptance

| Requirement ID | Stub pass criteria | Full pass (later) |
|----------------|-------------------|-------------------|
| AC-U01 | `200`, non-empty body | + `Skills:`, `Score:` labels |
| AC-U02 | `file` key works | Same |
| AC-U03 | Missing file → 400 | Same |
| AC-R01 | Role echoed in body | `Role:` line in analysis format |

---

## Postman test checklist

- [ ] Test 1 — PDF + role → 200 + stub message
- [ ] Test 2 — All three roles echo correctly
- [ ] Test 3 — Missing file → 400
- [ ] Test 5 — Wrong key name → 400
- [ ] Screenshot or save response for portfolio (optional)

---

## Link to prior context

Implementation source: [implementation.md](./implementation.md).  
Next: [verify-and-wrapup.md](./verify-and-wrapup.md) — segment sign-off and what comes next (PDF extraction).
