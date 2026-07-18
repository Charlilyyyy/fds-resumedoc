# API Testing

## Purpose

Document Postman and cURL cases for `POST /resume/upload` that exercise the full analysis response.

Implements the API testing part of requirement **F13**.

---

## Endpoint

```
POST http://localhost:8080/resume/upload
Content-Type: multipart/form-data
Body: file (File), role (Text)
```

See [api-contract.md](../requirements/api-contract.md).

---

## Postman setup

1. Method **POST**, URL `http://localhost:8080/resume/upload`.
2. Body → **form-data**.
3. Key `file` → type **File** → choose a PDF.
4. Key `role` → type **Text** → e.g. `Java Developer`.
5. Send.

---

## cURL cases

### Happy path

```bash
curl -X POST "http://localhost:8080/resume/upload" \
  -F "file=@rich.pdf" \
  -F "role=Java Developer"
```

Expect 200 with full labeled body.

### Each role

```bash
curl -X POST http://localhost:8080/resume/upload -F "file=@rich.pdf" -F "role=Full Stack Developer"
curl -X POST http://localhost:8080/resume/upload -F "file=@rich.pdf" -F "role=Data Analyst"
```

Role match block differs per role.

### Missing file

```bash
curl -i -X POST http://localhost:8080/resume/upload -F "role=Java Developer"
```

Expect **400** (required `file` param missing).

### Missing role

```bash
curl -i -X POST http://localhost:8080/resume/upload -F "file=@rich.pdf"
```

Expect **400** (required `role` param missing).

### Wrong field name

```bash
curl -i -X POST http://localhost:8080/resume/upload -F "resume=@rich.pdf" -F "role=Java Developer"
```

Expect **400** — field must be `file`.

---

## Expected body markers

A successful response contains all of:

```text
Skills: [...]
Score: N/100
Suggestions: [...]
Role: ...
Match Score: N%
Missing Skills: [...]
ATS Score: N%
Missing Keywords: [...]
Resume Sections:
Career Roadmap for ...
```

---

## Case table

| Case | Params | Expected |
|------|--------|----------|
| Happy | file + role | 200, full body |
| Roles | file + each role | 200, role block varies |
| No file | role only | 400 |
| No role | file only | 400 |
| Bad field | resume + role | 400 |

---

## Checklist

- [ ] Happy path returns full body
- [ ] All three roles tested
- [ ] Missing file → 400
- [ ] Missing role → 400
- [ ] Wrong field name → 400

---

## Link to prior context

Matrix: [manual-test-matrix.md](./manual-test-matrix.md).  
Next: [edge-cases.md](./edge-cases.md) — malformed inputs and boundaries.
