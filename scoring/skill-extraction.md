# Skill Extraction

## Purpose

Define `extractSkills(text)` — scan the extracted resume text for known technical keywords and return a list of normalized skill names. This drives scoring, suggestions, and (later) role matching.

Implements requirement **F4**.

---

## Method signature

```java
private List<String> extractSkills(String text)
```

| Input | Output |
|-------|--------|
| Full resume text | `List<String>` of matched skills (may be empty) |

---

## Detection rules

Lowercase the text once, then substring-match each keyword.

| Keyword in text (lowercased) | Normalized skill |
|------------------------------|------------------|
| `java` | Java |
| `spring` | Spring Boot |
| `mysql` or `sql` | SQL |
| `html` | HTML |
| `css` | CSS |
| `javascript` | JavaScript |

Notes:

- Matching is **case-insensitive** (text is lowercased first).
- `sql` is matched from either `mysql` or `sql` and added **once**.
- `spring` maps to the display name `Spring Boot`.

---

## Reference logic

```java
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
```

---

## Behavior examples

| Resume text contains | Detected skills |
|----------------------|-----------------|
| "Java, MySQL, HTML" | [Java, SQL, HTML] |
| "JAVASCRIPT and css" | [CSS, JavaScript] |
| "Python data analysis" | [] |
| "Spring Boot, SQL" | [Spring Boot, SQL] |

---

## Known characteristics (v1)

| Characteristic | Note |
|----------------|------|
| Substring match | `javascript` also contains `java`, so JavaScript-only resumes also flag Java |
| No word boundaries | Simple `contains`; acceptable for v1 keyword scan |
| Fixed keyword set | Six skills; expand in future scope |
| Python/Excel not detected | Relevant later for Data Analyst matching gaps |

These are intentional v1 simplifications, not defects.

---

## Imports needed

```java
import java.util.ArrayList;
import java.util.List;
```

---

## Checklist

- [ ] Text lowercased before scanning
- [ ] Six keyword rules implemented
- [ ] SQL added once from mysql/sql
- [ ] Returns list (empty allowed)

---

## Link to prior context

Overview: [overview.md](./overview.md).  
Next: [scoring-logic.md](./scoring-logic.md) — weighted score from the skills list.
