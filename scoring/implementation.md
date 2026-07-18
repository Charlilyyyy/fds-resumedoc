# Implementation

## Purpose

Combine PDF extraction with skill detection, scoring, and suggestions inside `ResumeService`. At this stage the response includes skills, score, and suggestions. ATS, role match, sections, and roadmap are added in later segments.

Design: [skill-extraction.md](./skill-extraction.md), [scoring-logic.md](./scoring-logic.md), [suggestions.md](./suggestions.md).

---

## File to update

```
src/main/java/com/resume/analyzer/service/ResumeService.java
```

Controller unchanged.

---

## `ResumeService.java` (scoring stage)

```java
package com.resume.analyzer.service;

import java.io.InputStream;
import java.util.ArrayList;
import java.util.List;

import org.apache.pdfbox.pdmodel.PDDocument;
import org.apache.pdfbox.text.PDFTextStripper;
import org.springframework.stereotype.Service;
import org.springframework.web.multipart.MultipartFile;

@Service
public class ResumeService {

    public String processResume(MultipartFile file, String role) {

        try (InputStream inputStream = file.getInputStream();
             PDDocument document = PDDocument.load(inputStream)) {

            String text = new PDFTextStripper().getText(document);

            List<String> skills = extractSkills(text);
            int score = calculateScore(skills);
            List<String> suggestions = getSuggestions(skills);

            return "Skills: " + skills
                 + "\nScore: " + score + "/100"
                 + "\nSuggestions: " + suggestions;

        } catch (Exception e) {
            e.printStackTrace();
            return "Error while processing resume ❌";
        }
    }

    private List<String> extractSkills(String text) {
        List<String> skillList = new ArrayList<>();
        String lowerText = text.toLowerCase();

        if (lowerText.contains("java"))       skillList.add("Java");
        if (lowerText.contains("spring"))     skillList.add("Spring Boot");
        if (lowerText.contains("mysql") || lowerText.contains("sql")) skillList.add("SQL");
        if (lowerText.contains("html"))       skillList.add("HTML");
        if (lowerText.contains("css"))        skillList.add("CSS");
        if (lowerText.contains("javascript")) skillList.add("JavaScript");

        return skillList;
    }

    private int calculateScore(List<String> skills) {
        int score = 0;
        if (skills.contains("Java"))        score += 20;
        if (skills.contains("Spring Boot")) score += 20;
        if (skills.contains("SQL"))         score += 20;
        if (skills.contains("HTML"))        score += 10;
        if (skills.contains("CSS"))         score += 10;
        if (skills.contains("JavaScript")) score += 20;
        return score;
    }

    private List<String> getSuggestions(List<String> skills) {
        List<String> suggestions = new ArrayList<>();

        if (!skills.contains("Java"))
            suggestions.add("Build strong foundation in Java with OOP concepts");
        if (!skills.contains("Spring Boot"))
            suggestions.add("Create a REST API project using Spring Boot");
        if (!skills.contains("SQL"))
            suggestions.add("Practice database queries and learn MySQL deeply");
        if (!skills.contains("JavaScript"))
            suggestions.add("Work on frontend using JavaScript and build projects");
        if (!skills.contains("HTML") || !skills.contains("CSS"))
            suggestions.add("Improve frontend skills using HTML, CSS and responsive design");

        if (skills.size() < 3)
            suggestions.add("Add at least 2 real-world projects to strengthen your resume");

        return suggestions;
    }
}
```

---

## Build and test

```bash
./mvnw clean compile
./mvnw spring-boot:run
```

```bash
curl -X POST "http://localhost:8080/resume/upload" \
  -F "file=@/path/to/resume.pdf" \
  -F "role=Java Developer"
```

Example body:

```text
Skills: [Java, SQL, HTML]
Score: 50/100
Suggestions: [Create a REST API project using Spring Boot, Work on frontend using JavaScript and build projects, Improve frontend skills using HTML, CSS and responsive design]
```

---

## Checklist

- [ ] `extractSkills`, `calculateScore`, `getSuggestions` present
- [ ] Response includes Skills, Score, Suggestions labels
- [ ] PDF extraction and try/catch retained
- [ ] Controller unchanged
- [ ] Compiles and returns expected body

---

## Evolution note

Later segments append more blocks after suggestions (role match, ATS, sections, roadmap). Keep the extraction + scoring core intact.

---

## Link to prior context

Next: [verify-and-wrapup.md](./verify-and-wrapup.md) — acceptance checks and segment close.
