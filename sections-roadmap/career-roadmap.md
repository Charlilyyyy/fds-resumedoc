# Career Roadmap

## Purpose

Define `getJobSuggestions(skills, role)` — role-specific learning and project guidance beyond the general suggestions from the scoring segment.

Implements requirement **F10**.

---

## Method signature

```java
private String getJobSuggestions(List<String> skills, String role)
```

| Input | Output |
|-------|--------|
| Detected skills + selected role | Labeled block: `Career Roadmap for {role}:` + tips |

---

## Rules by role

### Java Developer

| Condition | Tip |
|-----------|-----|
| Missing Spring Boot | Learn Spring Boot and build REST APIs |
| Missing SQL | Practice MySQL and database design |
| Missing Java | Strengthen Java fundamentals and OOP concepts |
| Always | Build a full backend project using Spring Boot |

### Full Stack Developer

| Condition | Tip |
|-----------|-----|
| Missing JavaScript | Learn JavaScript and build dynamic web apps |
| Missing HTML or CSS | Improve frontend skills using HTML, CSS |
| Always | Build a full stack project (frontend + backend) |

### Data Analyst

| Condition | Tip |
|-----------|-----|
| Always | Learn Python and data analysis libraries |
| Always | Practice SQL queries and data visualization |
| Always | Work on real datasets and dashboards |

---

## Reference logic

```java
private String getJobSuggestions(List<String> skills, String role) {
    List<String> suggestions = new ArrayList<>();

    if (role.equalsIgnoreCase("Java Developer")) {
        if (!skills.contains("Spring Boot"))
            suggestions.add("Learn Spring Boot and build REST APIs");
        if (!skills.contains("SQL"))
            suggestions.add("Practice MySQL and database design");
        if (!skills.contains("Java"))
            suggestions.add("Strengthen Java fundamentals and OOP concepts");
        suggestions.add("Build a full backend project using Spring Boot");

    } else if (role.equalsIgnoreCase("Full Stack Developer")) {
        if (!skills.contains("JavaScript"))
            suggestions.add("Learn JavaScript and build dynamic web apps");
        if (!skills.contains("HTML") || !skills.contains("CSS"))
            suggestions.add("Improve frontend skills using HTML, CSS");
        suggestions.add("Build a full stack project (frontend + backend)");

    } else if (role.equalsIgnoreCase("Data Analyst")) {
        suggestions.add("Learn Python and data analysis libraries");
        suggestions.add("Practice SQL queries and data visualization");
        suggestions.add("Work on real datasets and dashboards");
    }

    return "Career Roadmap for " + role + ":\n" + suggestions;
}
```

---

## Output format

```text
Career Roadmap for Java Developer:
[Learn Spring Boot and build REST APIs, Build a full backend project using Spring Boot]
```

Frontend parses with `/Career Roadmap[\s\S]*/`.

---

## Notes

| Topic | Note |
|-------|------|
| Case-insensitive role | Uses `equalsIgnoreCase` |
| Conditional + always tips | Some tips always added per role |
| Unknown role | No branch matches → header + empty list `[]` |
| vs general suggestions | This is role-aware; F9 suggestions are skill-only |

Unlike `matchJobRole`, an unknown role here does **not** throw — it simply produces an empty roadmap list.

---

## Checklist

- [ ] Three role branches
- [ ] Conditional tips for missing skills
- [ ] Always-on project tips
- [ ] Header `Career Roadmap for {role}:`

---

## Link to prior context

Sections: [section-detection.md](./section-detection.md).  
Next: [response-assembly.md](./response-assembly.md) — full labeled response order.
