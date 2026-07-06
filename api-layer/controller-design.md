# Controller Design

## Purpose

Define `ResumeController` — the web layer entry point for resume upload. The controller handles HTTP mapping and parameter binding only; all processing is delegated to `ResumeService`.

File location: `src/main/java/com/resume/analyzer/controller/ResumeController.java`

---

## Class responsibilities

| Does | Does not |
|------|----------|
| Map URL `/resume/upload` to Java method | Parse PDF bytes |
| Bind `file` and `role` from multipart request | Calculate scores or skills |
| Call `resumeService.processResume(file, role)` | Access database |
| Return `ResponseEntity<String>` with `200 OK` | Format full analysis (service job) |

**Thin controller rule:** No business logic in the controller class.

---

## Annotations

### `@RestController`

Combines `@Controller` + `@ResponseBody`. Return values are written directly to the HTTP response body (not a view template).

```java
@RestController
public class ResumeController { ... }
```

### `@RequestMapping("/resume")`

Class-level base path. All handler methods on this controller are prefixed with `/resume`.

| Combined path | Annotation |
|---------------|------------|
| `/resume` + `/upload` | `@PostMapping("/upload")` on method |
| **Full path** | `POST /resume/upload` |

### `@PostMapping("/upload")`

| Property | Value |
|----------|-------|
| HTTP method | POST only |
| Relative path | `/upload` |
| Content type | `multipart/form-data` (Spring auto-detects) |

GET, PUT, DELETE are not defined on this controller in v1.

---

## Method signature

```java
@PostMapping("/upload")
public ResponseEntity<String> uploadResume(
        @RequestParam("file") MultipartFile file,
        @RequestParam("role") String role) {
    // delegate to service
}
```

### Return type: `ResponseEntity<String>`

| Choice | Reason |
|--------|--------|
| `ResponseEntity<String>` | Explicit HTTP status; body is plain text for v1 |
| Not `String` alone | Clear control over status code (200) |
| Not JSON DTO | api-contract specifies text response in v1 |

Stub implementation returns:

```java
return ResponseEntity.ok(result);
```

`ResponseEntity.ok(body)` → HTTP **200** with body as `text/plain` (default for String).

---

## Request parameters

### `@RequestParam("file") MultipartFile file`

| Aspect | Detail |
|--------|--------|
| Form field name | Must be exactly `file` |
| Type | `org.springframework.web.multipart.MultipartFile` |
| Source | Spring MVC multipart resolver |
| Client | Postman File field; browser `FormData.append("file", ...)` |

`MultipartFile` provides:

- `getInputStream()` — used later by PDFBox in service
- `getOriginalFilename()` — optional logging or persistence
- `isEmpty()` — future validation

### `@RequestParam("role") String role`

| Aspect | Detail |
|--------|--------|
| Form field name | `role` |
| Type | `String` |
| Examples | `Java Developer`, `Full Stack Developer`, `Data Analyst` |

At stub stage the controller does not validate role values; service may ignore `role` until matching logic is added.

---

## Dependency on service

Controller requires `ResumeService` injected:

```java
@Autowired
private ResumeService resumeService;
```

Constructor injection (preferred style for tests):

```java
private final ResumeService resumeService;

public ResumeController(ResumeService resumeService) {
    this.resumeService = resumeService;
}
```

Both work; reference implementation uses field `@Autowired`.

**Flow:**

```
uploadResume(file, role)
    → result = resumeService.processResume(file, role)
    → return ResponseEntity.ok(result)
```

---

## URL resolution diagram

```mermaid
flowchart LR
    REQ["POST /resume/upload"]
    DS[DispatcherServlet]
    RC[ResumeController]
    RS[ResumeService]

    REQ --> DS
    DS --> RC
    RC -->|"processResume(file, role)"| RS
    RS --> RC
    RC -->|"200 + String body"| REQ
```

---

## Imports (reference)

```java
package com.resume.analyzer.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.multipart.MultipartFile;

import com.resume.analyzer.service.ResumeService;
```

---

## Error handling (v1 stub)

| Scenario | Stub behavior |
|----------|---------------|
| Missing `file` | Spring returns **400 Bad Request** (required param missing) |
| Missing `role` | Spring returns **400 Bad Request** |
| Service throws | Unhandled 500 unless service catches (later) |

Controller does not wrap service calls in try/catch at stub stage.

---

## Design constraints for later segments

When analysis is added, **keep the controller unchanged**:

- Same URL, same params, same return type
- Only `ResumeService.processResume` body grows

Optional future enhancements (not stub):

- `@Valid` on a request DTO instead of raw `@RequestParam`
- `produces = MediaType.TEXT_PLAIN_VALUE` explicit on method
- `@ExceptionHandler` for consistent error responses

---

## Controller checklist

- [ ] Class in package `com.resume.analyzer.controller`
- [ ] `@RestController` present
- [ ] `@RequestMapping("/resume")` on class
- [ ] `@PostMapping("/upload")` on handler method
- [ ] Parameters: `MultipartFile file`, `String role`
- [ ] `ResumeService` injected
- [ ] Returns `ResponseEntity.ok(serviceResult)`
- [ ] No PDF or analysis code in controller

---

## Link to prior context

Endpoint contract: [api-contract.md](../requirements/api-contract.md).  
Next: [service-stub.md](./service-stub.md) — `ResumeService` placeholder `processResume` design.
