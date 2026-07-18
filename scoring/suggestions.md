# Smart Suggestions

## Purpose

Define `getSuggestions(skills)` — general, non-role-specific improvement tips based on skills the resume is **missing**, plus a bonus tip for thin resumes.

Implements requirement **F9** (general suggestions). Role-specific career roadmap is a separate later method.

---

## Method signature

```java
private List<String> getSuggestions(List<String> skills)
```

| Input | Output |
|-------|--------|
| Detected skills list | `List<String>` of suggestion messages |

---

## Rules

| Condition | Suggestion added |
|-----------|------------------|
| Java missing | Build strong foundation in Java with OOP concepts |
| Spring Boot missing | Create a REST API project using Spring Boot |
| SQL missing | Practice database queries and learn MySQL deeply |
| JavaScript missing | Work on frontend using JavaScript and build projects |
| HTML **or** CSS missing | Improve frontend skills using HTML, CSS and responsive design |
| Fewer than 3 skills total | Add at least 2 real-world projects to strengthen your resume |

The thin-resume rule stacks on top of the missing-skill rules.

---

## Reference logic

```java
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
```

---

## Examples

| Skills | Suggestions |
|--------|-------------|
| [] | All five missing-skill tips + projects tip |
| [Java, SQL] | Spring Boot, JavaScript, HTML/CSS tips + projects tip (size < 3) |
| [Java, Spring Boot, SQL, JavaScript, HTML, CSS] | None (all present, size ≥ 3) |

---

## Output format

Rendered in response as:

```text
Suggestions: [Create a REST API project using Spring Boot, Work on frontend using JavaScript and build projects]
```

Frontend parses with `/Suggestions: \[(.*?)\]/` and splits on commas.

---

## Design notes

| Topic | Note |
|-------|------|
| Missing-driven | Suggestions target gaps, not present skills |
| General vs role | This is skill-based; role roadmap comes later |
| Comma caution | Suggestion text contains commas; UI split-on-comma is approximate in v1 |

---

## Checklist

- [ ] Five missing-skill rules implemented
- [ ] HTML/CSS uses OR condition
- [ ] Thin-resume rule for size < 3
- [ ] Returns list (empty when all present)

---

## Link to prior context

Score: [scoring-logic.md](./scoring-logic.md).  
Next: [implementation.md](./implementation.md) — combined service source for skills, score, suggestions.
