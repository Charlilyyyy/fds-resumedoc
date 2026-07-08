# PDFBox Setup

## Purpose

Confirm Apache PDFBox is on the classpath and document the classes and imports used to load a PDF and extract plain text inside `ResumeService`.

No controller changes. No skill analysis yet — only the library foundation for extraction.

---

## Maven dependency

Already required in scaffolding. Confirm `pom.xml` contains:

```xml
<dependency>
    <groupId>org.apache.pdfbox</groupId>
    <artifactId>pdfbox</artifactId>
    <version>2.0.30</version>
</dependency>
```

| Property | Value |
|----------|-------|
| Group | `org.apache.pdfbox` |
| Artifact | `pdfbox` |
| Version | `2.0.30` (pin explicitly; not managed by Spring BOM) |
| Scope | compile (default) |

Verify resolve:

```bash
./mvnw dependency:tree | grep pdfbox
```

Expected: `org.apache.pdfbox:pdfbox:jar:2.0.30`

If missing, add the block above and run `./mvnw clean compile`.

---

## Core classes

| Class | Package | Role in ResumeDoc |
|-------|---------|-------------------|
| `PDDocument` | `org.apache.pdfbox.pdmodel` | In-memory PDF model after load |
| `PDFTextStripper` | `org.apache.pdfbox.text` | Converts PDF content to a `String` |
| `MultipartFile` | `org.springframework.web.multipart` | Uploaded file from controller |
| `InputStream` | `java.io` | Stream of PDF bytes from multipart |

### `PDDocument`

- Created via `PDDocument.load(InputStream)` or `PDDocument.load(File)`
- Holds pages and document structure
- **Must be closed** when finished (`close()` or try-with-resources)
- Throws `IOException` (and related) on corrupt or non-PDF data

### `PDFTextStripper`

- Instantiated with `new PDFTextStripper()`
- `getText(PDDocument)` returns concatenated page text
- Works best on text-based PDFs (not scanned image-only pages without OCR)

### Load sources

| Source | Method | When |
|--------|--------|------|
| Upload | `PDDocument.load(file.getInputStream())` | Primary path for ResumeDoc |
| Local file | `PDDocument.load(new File(...))` | Manual debugging only |

---

## Java imports for extraction stage

Add to `ResumeService`:

```java
import java.io.InputStream;

import org.apache.pdfbox.pdmodel.PDDocument;
import org.apache.pdfbox.text.PDFTextStripper;
import org.springframework.stereotype.Service;
import org.springframework.web.multipart.MultipartFile;
```

Do **not** need (yet):

- Skill / collections (`ArrayList`, `Map`) — scoring segment
- JPA repositories
- Controller-related annotations in the service

---

## Minimal usage pattern

```java
InputStream inputStream = file.getInputStream();
PDDocument document = PDDocument.load(inputStream);
PDFTextStripper stripper = new PDFTextStripper();
String text = stripper.getText(document);
document.close();
```

Preferred safer style (try-with-resources) is covered in [error-handling.md](./error-handling.md) and [implementation.md](./implementation.md):

```java
try (InputStream inputStream = file.getInputStream();
     PDDocument document = PDDocument.load(inputStream)) {
    String text = new PDFTextStripper().getText(document);
    // use text
}
```

---

## Transitive artifacts (informational)

`pdfbox` may pull related modules (e.g. `fontbox`). You do not declare them separately unless you customize PDF rendering beyond text strip.

---

## Compatibility notes

| Topic | Guidance |
|-------|----------|
| Java 17 | PDFBox 2.0.30 works on JDK 17 |
| Spring Boot 3 | No special Spring integration — call PDFBox as plain Java |
| Encrypted PDFs | May fail load without password — treat as error message in catch |
| Scanned PDFs | OCR not in v1; empty or sparse text is possible |

---

## Setup checklist

- [ ] `pdfbox:2.0.30` in `pom.xml`
- [ ] `./mvnw dependency:tree` shows PDFBox
- [ ] Imports for `PDDocument` and `PDFTextStripper` resolve in IDE
- [ ] No PDFBox code left only in comments — ready for extraction-flow

---

## Link to prior context

Overview: [overview.md](./overview.md).  
Stack justification: [tech-stack.md](../architecture/tech-stack.md).  
Next: [extraction-flow.md](./extraction-flow.md) — ordered steps inside `processResume`.
