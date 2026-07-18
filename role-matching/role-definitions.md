# Role Definitions

## Purpose

Define `jobRoles()` — a map from each supported job role to its required skills. Role matching compares detected skills against these lists.

---

## Method signature

```java
private Map<String, List<String>> jobRoles()
```

---

## The three roles (v1)

| Role | Required skills |
|------|-----------------|
| Java Developer | Java, Spring Boot, SQL |
| Full Stack Developer | Java, JavaScript, HTML, CSS |
| Data Analyst | SQL, Python, Excel |

---

## Reference logic

```java
private Map<String, List<String>> jobRoles() {
    Map<String, List<String>> roles = new HashMap<>();
    roles.put("Java Developer", Arrays.asList("Java", "Spring Boot", "SQL"));
    roles.put("Full Stack Developer", Arrays.asList("Java", "JavaScript", "HTML", "CSS"));
    roles.put("Data Analyst", Arrays.asList("SQL", "Python", "Excel"));
    return roles;
}
```

---

## Role notes

| Role | Required count | Notes |
|------|----------------|-------|
| Java Developer | 3 | All three are detectable by `extractSkills` |
| Full Stack Developer | 4 | All four detectable |
| Data Analyst | 3 | `Python` and `Excel` are **not** detected by v1 skill extractor |

### Data Analyst characteristic

Because `extractSkills` does not detect Python or Excel, a Data Analyst match is typically capped at `SQL` only (33%). This is a known v1 characteristic documented in acceptance AC-M05 — not a bug. Expanding the skill extractor is future scope.

---

## Keys must match request values

The `role` string sent from the UI/Postman must **exactly** match a map key:

| Sent `role` | Matches |
|-------------|---------|
| `Java Developer` | Yes |
| `java developer` | No (case-sensitive map lookup) |
| `Full Stack Developer` | Yes |
| `Data Analyst` | Yes |

The v1 UI dropdown sends exact strings, so this is consistent in normal use. Unknown keys return `null` from the map — see matching-logic edge cases.

---

## Imports needed

```java
import java.util.Arrays;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
```

---

## Checklist

- [ ] Three roles present
- [ ] Required skills match table
- [ ] Keys match UI dropdown option text exactly

---

## Link to prior context

Overview: [overview.md](./overview.md).  
Next: [matching-logic.md](./matching-logic.md) — computing match % and missing skills.
