# Implementation

## Purpose

Add ATS keyword analysis to `ResumeService` and append its result to the response after skills/score/suggestions.

Design: [keyword-set.md](./keyword-set.md), [scoring-logic.md](./scoring-logic.md).

---

## File to update

```
src/main/java/com/resume/analyzer/service/ResumeService.java
```

Controller unchanged.

---

## New methods

```java
private List<String> getATSKeywords() {
    return Arrays.asList(
        "java", "spring", "spring boot", "rest api", "microservices",
        "mysql", "sql", "hibernate", "docker", "kubernetes",
        "aws", "javascript", "html", "css"
    );
}

private String checkATSScore(String text) {
    List<String> keywords = getATSKeywords();
    List<String> missing = new ArrayList<>();
    String lowerText = text.toLowerCase();
    int matchCount = 0;

    for (String key : keywords) {
        if (lowerText.contains(key)) {
            matchCount++;
        } else {
            missing.add(key);
        }
    }

    int score = (matchCount * 100) / keywords.size();
    return "ATS Score: " + score + "%"
         + "\nMissing Keywords: " + missing;
}
```

---

## Wiring into `processResume`

```java
String text = new PDFTextStripper().getText(document);

List<String> skills = extractSkills(text);
int score = calculateScore(skills);
List<String> suggestions = getSuggestions(skills);

String atsResult = checkATSScore(text);

return "Skills: " + skills
     + "\nScore: " + score + "/100"
     + "\nSuggestions: " + suggestions
     + "\n\n" + atsResult;
```

The ATS block is separated from the skills block by a blank line (`\n\n`), matching the response format for frontend parsing.

---

## Required imports

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
```

`Arrays` is newly required for the keyword list.

---

## Build and test

```bash
./mvnw clean compile
./mvnw spring-boot:run
```

Example body:

```text
Skills: [Java, SQL, HTML]
Score: 50/100
Suggestions: [Create a REST API project using Spring Boot, ...]

ATS Score: 42%
Missing Keywords: [spring boot, rest api, microservices, hibernate, docker, kubernetes, aws, javascript, css]
```

---

## Checklist

- [ ] `getATSKeywords` returns 14 terms
- [ ] `checkATSScore` computes % and missing list
- [ ] ATS block appended after suggestions with blank line
- [ ] `Arrays` imported
- [ ] Compiles and returns ATS block

---

## Link to prior context

Next: [verify-and-wrapup.md](./verify-and-wrapup.md) — ATS acceptance checks and close.
