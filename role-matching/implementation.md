# Implementation

## Purpose

Add role matching to `ResumeService` and append its result to the response after the ATS block.

Design: [role-definitions.md](./role-definitions.md), [matching-logic.md](./matching-logic.md).

---

## File to update

```
src/main/java/com/resume/analyzer/service/ResumeService.java
```

Controller unchanged.

---

## New methods

```java
private Map<String, List<String>> jobRoles() {
    Map<String, List<String>> roles = new HashMap<>();
    roles.put("Java Developer", Arrays.asList("Java", "Spring Boot", "SQL"));
    roles.put("Full Stack Developer", Arrays.asList("Java", "JavaScript", "HTML", "CSS"));
    roles.put("Data Analyst", Arrays.asList("SQL", "Python", "Excel"));
    return roles;
}

private String matchJobRole(List<String> skills, String role) {
    List<String> requiredSkills = jobRoles().get(role);

    int matchCount = 0;
    List<String> missing = new ArrayList<>();

    for (String req : requiredSkills) {
        if (skills.contains(req)) {
            matchCount++;
        } else {
            missing.add(req);
        }
    }

    int matchScore = (matchCount * 100) / requiredSkills.size();

    return "Role: " + role
         + "\nMatch Score: " + matchScore + "%"
         + "\nMissing Skills: " + missing;
}
```

---

## Wiring into `processResume`

```java
List<String> skills = extractSkills(text);
int score = calculateScore(skills);
List<String> suggestions = getSuggestions(skills);

String jobMatch = matchJobRole(skills, role);
String atsResult = checkATSScore(text);

return "Skills: " + skills
     + "\nScore: " + score + "/100"
     + "\nSuggestions: " + suggestions
     + "\n\n" + jobMatch
     + "\n\n" + atsResult;
```

Block order follows [api-contract.md](../requirements/api-contract.md): skills/score/suggestions, then role match, then ATS.

---

## Required imports

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
```

`HashMap` and `Map` are newly required.

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
Suggestions: [...]

Role: Java Developer
Match Score: 66%
Missing Skills: [Spring Boot]

ATS Score: 42%
Missing Keywords: [...]
```

---

## Checklist

- [ ] `jobRoles` map with three roles
- [ ] `matchJobRole` computes % and missing
- [ ] Role block appended before ATS block
- [ ] `HashMap`/`Map` imported
- [ ] Compiles; role reflected in output

---

## Link to prior context

Next: [verify-and-wrapup.md](./verify-and-wrapup.md) — role match acceptance and close.
