# API Contract

## Overview

ResumeDoc v1 exposes one primary analysis endpoint. The browser (or Postman) sends a PDF and a job role; the server returns a **plain-text body** with labeled sections the dashboard parses into cards and charts.

| Property | Value |
|----------|-------|
| Base URL (local) | `http://localhost:8080` |
| Protocol | HTTP |
| Auth | None in v1 |
| Content type (request) | `multipart/form-data` |
| Content type (success response) | `text/plain` (Spring `ResponseEntity<String>`) |

Structured JSON is listed in [future-scope.md](./future-scope.md); v1 intentionally uses parseable text.

---

## Endpoint

### `POST /resume/upload`

Analyzes an uploaded resume PDF against the selected job role.

| Attribute | Detail |
|-----------|--------|
| Method | `POST` |
| Path | `/resume/upload` |
| Controller mapping | Class-level `/resume` + method-level `/upload` |
| Success status | `200 OK` |
| Error status | `200 OK` with error message in body (v1); no separate 4xx for corrupt PDF |

---

## Request

### Content type

```
multipart/form-data
```

### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file` | `MultipartFile` | Yes | Resume document; must be a readable PDF |
| `role` | `String` | Yes | Target job role; one of the three supported values |

### Supported `role` values

| Value | Notes |
|-------|-------|
| `Java Developer` | Exact string match used in matching logic |
| `Full Stack Developer` | Case-sensitive in roadmap branches; UI sends exact option text |
| `Data Analyst` | Roadmap uses fixed suggestions regardless of detected skills |

**Unknown role:** If `role` is not in the internal role map, matching logic may throw or behave unexpectedly—v1 UI restricts choices to the three options above.

### Example — cURL

```bash
curl -X POST "http://localhost:8080/resume/upload" \
  -F "file=@/path/to/resume.pdf" \
  -F "role=Java Developer"
```

### Example — JavaScript (browser)

```javascript
const fd = new FormData();
fd.append("file", fileInput.files[0]);
fd.append("role", roleSelect.value);

fetch("http://localhost:8080/resume/upload", {
  method: "POST",
  body: fd
}).then(res => res.text()).then(data => { /* parse */ });
```

### Example — Postman

1. Method: `POST`
2. URL: `http://localhost:8080/resume/upload`
3. Body → form-data:
   - `file` — type File, select PDF
   - `role` — type Text, e.g. `Full Stack Developer`

---

## Response — success body structure

The service concatenates analysis segments in a fixed order. Labels are **stable** so the frontend can extract values with regular expressions.

### Segment order

1. Skills line  
2. Score line  
3. Suggestions line  
4. Blank line  
5. Role match block  
6. Blank line  
7. ATS block  
8. Blank line  
9. Resume sections block  
10. Blank line  
11. Career roadmap block  

### Field reference

| Output | Label / pattern | Type | Example |
|--------|-----------------|------|---------|
| Skills list | `Skills: [...]` | List of strings | `Skills: [Java, SQL, HTML]` |
| Resume score | `Score: N/100` | Integer 0–100 | `Score: 50/100` |
| General suggestions | `Suggestions: [...]` | List of strings | `Suggestions: [Create a REST API project using Spring Boot, ...]` |
| Selected role | `Role: {role}` | String | `Role: Java Developer` |
| Role match % | `Match Score: N%` | Integer 0–100 | `Match Score: 66%` |
| Missing role skills | `Missing Skills: [...]` | List of strings | `Missing Skills: [Spring Boot]` |
| ATS match % | `ATS Score: N%` | Integer 0–100 | `ATS Score: 42%` |
| Missing ATS keywords | `Missing Keywords: [...]` | List of strings | `Missing Keywords: [docker, kubernetes, ...]` |
| Section status | `Resume Sections:` + 3 lines | Found ✅ / Missing ❌ | See sample below |
| Career roadmap | `Career Roadmap for {role}:` + lines | List of strings | Role-specific tips |

### Sample success response

```text
Skills: [Java, SQL, HTML]
Score: 50/100
Suggestions: [Create a REST API project using Spring Boot, Work on frontend using JavaScript and build projects, Improve frontend skills using HTML, CSS and responsive design]

Role: Java Developer
Match Score: 66%
Missing Skills: [Spring Boot]

ATS Score: 42%
Missing Keywords: [spring boot, rest api, microservices, hibernate, docker, kubernetes, aws, javascript, css]

Resume Sections:
Skills Section: Found ✅
Education Section: Found ✅
Experience Section: Missing ❌

Career Roadmap for Java Developer:
[Learn Spring Boot and build REST APIs, Build a full backend project using Spring Boot]
```

*Note: List formatting uses Java `List.toString()` — square brackets, comma-separated items.*

---

## Response — error body

On processing failure (invalid file, corrupt PDF, I/O error):

```text
Error while processing resume ❌
```

| Aspect | v1 behavior |
|--------|-------------|
| HTTP status | Still `200 OK` with error string |
| UI handling | Should detect error prefix and show user-friendly message (recommended hardening) |

---

## Frontend parsing contract

The dashboard treats the response as one string and extracts fields with regex. **Changing backend labels breaks the UI** unless both are updated together.

| UI element | Extraction pattern (conceptual) |
|------------|----------------------------------|
| Skills | `/Skills: \[(.*?)\]/` → split on `,` |
| Score | `/Score: (\d+)/` |
| Suggestions | `/Suggestions: \[(.*?)\]/` → split on `,` |
| ATS score | `/ATS Score: (\d+)%/` |
| ATS missing | `/Missing Keywords: \[(.*?)\]/` |
| Sections | `/Resume Sections:\n([\s\S]*?)\n\n/` |
| Roadmap | `/Career Roadmap[\s\S]*/` |

Role match fields may be displayed inside the roadmap card or extended in a future UI pass; match data is present in the raw body for Postman users.

### Insight card (client-side only)

Score-band messages (e.g. “Excellent profile”) are **not** returned by the API—they are computed in JavaScript from the parsed score.

---

## Inputs → processing → outputs (pipeline)

```
file (PDF)  ──┐
              ├──► processResume(file, role) ──► plain-text response
role (string) ┘         │
                          ├── PDFBox → text
                          ├── extractSkills(text)
                          ├── calculateScore(skills)
                          ├── getSuggestions(skills)
                          ├── matchJobRole(skills, role)
                          ├── checkATSScore(text)
                          ├── detectSections(text)
                          └── getJobSuggestions(skills, role)
```

---

## Contract summary table

| Direction | Name | Format |
|-----------|------|--------|
| **In** | `file` | PDF binary (`MultipartFile`) |
| **In** | `role` | String — `Java Developer` \| `Full Stack Developer` \| `Data Analyst` |
| **Out** | `skills` | Embedded in `Skills: [...]` |
| **Out** | `score` | Embedded in `Score: N/100` |
| **Out** | `atsPercent` | Embedded in `ATS Score: N%` |
| **Out** | `matchPercent` | Embedded in `Match Score: N%` |
| **Out** | `sectionStatus` | Embedded in `Resume Sections:` block |
| **Out** | `suggestions` | Embedded in `Suggestions: [...]` and roadmap block |

---

## Versioning & compatibility

| Change type | Policy |
|-------------|--------|
| New optional JSON endpoint | Add alongside text endpoint (v1.1+); do not break text contract |
| Label rename | Breaking — requires UI + docs update |
| New role value | Add to role map + dropdown + docs together |
| HTTP 4xx for bad files | Improvement; document new status codes when adopted |

---

## Link to prior context

Inputs and outputs listed here implement F1, F2, and F12 from [must-have-features.md](./must-have-features.md). Next: **acceptance-upload-and-parsing.md** — pass/fail criteria for the endpoint and PDF extraction.
