# Implementation

## Purpose

Replace the API-layer stub body of `ResumeService.processResume` with PDFBox text extraction. Controller stays the same. This file is the copy-ready source for the extraction stage.

Design path: [extraction-flow.md](./extraction-flow.md), [pdfbox-setup.md](./pdfbox-setup.md).

---

## File to update

```
src/main/java/com/resume/analyzer/service/ResumeService.java
```

Do **not** change `ResumeController` for this segment.

---

## `ResumeService.java` (extraction stage)

Recommended style: try-with-resources for stream and document cleanup.

```java
package com.resume.analyzer.service;

import java.io.InputStream;

import org.apache.pdfbox.pdmodel.PDDocument;
import org.apache.pdfbox.text.PDFTextStripper;
import org.springframework.stereotype.Service;
import org.springframework.web.multipart.MultipartFile;

@Service
public class ResumeService {

    public String processResume(MultipartFile file, String role) {

        try (InputStream inputStream = file.getInputStream();
             PDDocument document = PDDocument.load(inputStream)) {

            PDFTextStripper pdfStripper = new PDFTextStripper();
            String text = pdfStripper.getText(document);

            // Verification response for this segment only.
            // Later: run extractSkills(text), calculateScore, etc., then labeled output.
            return "Extracted Text:\n" + text;

        } catch (Exception e) {
            e.printStackTrace();
            return "Error while processing resume ❌";
        }
    }
}
```

`role` is unused on purpose at this stage — signature stays stable for later matching.

---

## Alternative: match reference close pattern

If you prefer explicit close without try-with-resources (mirrors common tutorial style):

```java
public String processResume(MultipartFile file, String role) {

    try {
        InputStream inputStream = file.getInputStream();
        PDDocument document = PDDocument.load(inputStream);

        PDFTextStripper pdfStripper = new PDFTextStripper();
        String text = pdfStripper.getText(document);

        document.close();

        return "Extracted Text:\n" + text;

    } catch (Exception e) {
        e.printStackTrace();
        return "Error while processing resume ❌";
    }
}
```

**Caution:** if `getText` throws after load, `close()` may be skipped. Prefer try-with-resources for production-minded scaffolding.

---

## Optional preview return (large resumes)

```java
String preview = text.length() <= 2000
        ? text
        : text.substring(0, 2000) + "\n...[truncated]";
return "Extracted characters: " + text.length() + "\n\n" + preview;
```

Useful in Postman when the full resume dump is huge.

---

## Controller (unchanged)

```java
@PostMapping("/upload")
public ResponseEntity<String> uploadResume(
        @RequestParam("file") MultipartFile file,
        @RequestParam("role") String role) {

    String result = resumeService.processResume(file, role);
    return ResponseEntity.ok(result);
}
```

Same URL, same params, same `ResponseEntity.ok`.

---

## Build and run

```bash
./mvnw clean compile
./mvnw spring-boot:run
```

### Smoke curl

```bash
curl -X POST "http://localhost:8080/resume/upload" \
  -F "file=@/path/to/resume.pdf" \
  -F "role=Java Developer"
```

Expected (success):

- HTTP `200`
- Body starts with `Extracted Text:`
- Readable resume words appear (name, skills, education, etc.)

Expected (corrupt file):

- HTTP `200`
- Body: `Error while processing resume ❌`

---

## Implementation checklist

- [ ] Stub return string removed
- [ ] `PDDocument` + `PDFTextStripper` imports present
- [ ] Text assigned from `getText(document)`
- [ ] Document closed (try-with-resources or explicit)
- [ ] Catch returns clear error string
- [ ] Controller untouched
- [ ] Compiles and Postman shows extracted content

---

## Evolution note

When scoring starts, keep the extraction block and **append analysis** instead of returning raw text:

```java
String text = pdfStripper.getText(document);
List<String> skills = extractSkills(text);
// ... build labeled response from text + skills
```

Do not drop try/catch or close logic when that lands.

---

## Common errors

| Problem | Fix |
|---------|-----|
| `PDDocument cannot be resolved` | Add PDFBox dependency; Maven refresh |
| Empty body after `Extracted Text:` | Scanned/image PDF — need text-based sample |
| Error on every file | Wrong file selected; or encrypted PDF |
| Port already in use | Stop previous Boot process |

---

## Link to prior context

Next: [error-handling.md](./error-handling.md) — corrupt PDF, empty extraction, and resource-leak avoidance in depth.
