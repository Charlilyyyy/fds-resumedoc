# Error Handling & Resource Cleanup

## Purpose

Define how ResumeDoc behaves when PDF load or text strip fails, and how `PDDocument` is closed so repeated uploads stay stable. Extraction can succeed technically with empty text — that case is also covered.

Aligns with F3 acceptance cases in [acceptance-upload-and-parsing.md](../requirements/acceptance-upload-and-parsing.md).

---

## Goals

| Goal | Behavior |
|------|----------|
| No server crash | Catch exceptions inside `processResume` |
| Clear client message | Fixed error string in response body |
| Resource cleanup | Document (and stream) closed on success and failure paths |
| Alive after bad upload | Next request still works |

v1 keeps HTTP **200** with error text in the body (api-contract). Improving to 4xx/5xx is future hardening.

---

## Standard error response

```text
Error while processing resume ❌
```

Returned whenever:

- File is not a valid PDF
- PDF bytes are truncated/corrupt
- Encrypted PDF cannot be opened without a password
- Unexpected `IOException` / runtime failure during load or strip

Optional developer logging:

```java
} catch (Exception e) {
    e.printStackTrace();
    return "Error while processing resume ❌";
}
```

Do not rethrow to the controller for v1 — rethrow would surface as HTTP 500 unless an exception handler exists.

---

## Cases and expected behavior

### Corrupt or non-PDF upload

| Given | When | Then |
|-------|------|------|
| `.txt` / truncated bytes sent as `file` | `POST /resume/upload` | Body contains error message; process stays up |

Maps to **AC-P04**.

### Empty or image-only PDF

| Given | When | Then |
|-------|------|------|
| Blank PDF or scanned pages with no text layer | Extract runs | Load may succeed; `text` empty or nearly empty; **no crash**; later scores will be zero |

Maps to **AC-P05**. Do not treat empty text as a hard error at this stage unless you choose to:

```java
if (text == null || text.isBlank()) {
    return "Error while processing resume ❌";
}
```

Optional. Reference implementation often returns empty extraction and continues.

### Encrypted PDF

| Given | When | Then |
|-------|------|------|
| Password-protected PDF | Load | Exception → error string |

### Repeated uploads (leak check)

| Given | When | Then |
|-------|------|------|
| Same valid PDF uploaded 5+ times | Sequential requests | Each returns 200 + text; no progressive OOM / file-handle exhaustion |

Maps to **AC-P06**.

---

## Resource cleanup patterns

### Preferred — try-with-resources

```java
try (InputStream inputStream = file.getInputStream();
     PDDocument document = PDDocument.load(inputStream)) {

    String text = new PDFTextStripper().getText(document);
    return "Extracted Text:\n" + text;

} catch (Exception e) {
    e.printStackTrace();
    return "Error while processing resume ❌";
}
```

`close()` runs automatically when leaving the try block (success or extract failure after load).

### Acceptable — explicit close in try

```java
PDDocument document = null;
try {
    InputStream inputStream = file.getInputStream();
    document = PDDocument.load(inputStream);
    String text = new PDFTextStripper().getText(document);
    return "Extracted Text:\n" + text;
} catch (Exception e) {
    e.printStackTrace();
    return "Error while processing resume ❌";
} finally {
    if (document != null) {
        try {
            document.close();
        } catch (Exception ignore) {
            // swallow close failures
        }
    }
}
```

### Avoid

```java
PDDocument document = PDDocument.load(stream);
String text = stripper.getText(document);
document.close(); // skipped if getText throws
```

Missing `finally` / try-with-resources risks leaks under intermittent strip errors.

---

## What not to surface to the client (v1)

| Avoid returning | Why |
|-----------------|-----|
| Full stack traces | Noise / possible path leaks |
| Different messages per exception type | Keep message stable for UI/Postman |
| `null` body | Controller would NPE or empty response |

Fine for console: `e.printStackTrace()` or a logger.

---

## Controller boundary

Controller remains:

```java
String result = resumeService.processResume(file, role);
return ResponseEntity.ok(result);
```

| Layer | Responsibility |
|-------|----------------|
| Service | Catch parse errors → error string |
| Controller | Always wraps service string in 200 OK |
| Spring | 400 if `file` multipart part missing (before service) |

Missing multipart field is **not** a PDFBox issue — covered in api-layer testing.

---

## Error-handling checklist

- [ ] Broad `catch (Exception e)` around load + strip
- [ ] Stable error message string for clients
- [ ] Document closed on all paths (prefer try-with-resources)
- [ ] Corrupt sample tested → error body, server still up
- [ ] Valid PDF still returns extracted text after a failed attempt

---

## Known v1 limitations (not bugs)

| Limitation | Doc |
|------------|-----|
| Error uses HTTP 200 | [api-contract.md](../requirements/api-contract.md) |
| No OCR for scanned PDFs | [future-scope.md](../requirements/future-scope.md) |
| Empty text may look like “success with blank Extracted Text” | Document and improve later if needed |

---

## Link to prior context

Implementation: [implementation.md](./implementation.md).  
Next: [verify-and-wrapup.md](./verify-and-wrapup.md) — verification steps and segment close.
