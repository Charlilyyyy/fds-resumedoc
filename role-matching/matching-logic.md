# Matching Logic

## Purpose

Define `matchJobRole(skills, role)` — compare detected skills against the selected role's required skills, compute a match percentage, and list missing skills.

Implements requirement **F7**.

---

## Method signature

```java
private String matchJobRole(List<String> skills, String role)
```

| Input | Output |
|-------|--------|
| Detected skills + selected role | Labeled block: role, match %, missing skills |

---

## Algorithm

1. Look up required skills for `role` from `jobRoles()`.
2. For each required skill, check if it is in the detected skills list.
3. Count matches; collect misses.
4. `matchScore = (matchCount * 100) / requiredSkills.size()` (integer).
5. Return formatted block.

---

## Reference logic

```java
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

## Examples

| Detected skills | Role | Match | Missing |
|-----------------|------|-------|---------|
| [Java, Spring Boot, SQL] | Java Developer | 100% | [] |
| [Java, SQL] | Java Developer | 66% | [Spring Boot] |
| [Java, HTML, CSS] | Full Stack Developer | 75% | [JavaScript] |
| [SQL] | Data Analyst | 33% | [Python, Excel] |

Integer division: 2/3 → 66%, 1/3 → 33%.

---

## Output format

```text
Role: Java Developer
Match Score: 66%
Missing Skills: [Spring Boot]
```

---

## Edge case — unknown role

| Situation | v1 behavior |
|-----------|-------------|
| `role` not in `jobRoles()` map | `jobRoles().get(role)` returns `null` → NullPointerException in the loop |
| Result | Caught by `processResume` try/catch → error message string |

The v1 UI restricts `role` to the three valid options, so this does not occur in normal use. Hardening (validate role, return friendly message) is future scope. Documented in AC-R06 / AC-M note.

---

## Design notes

| Topic | Note |
|-------|------|
| Uses detected skills | Not raw text; consistent with F4 |
| Case-sensitive skill compare | `skills.contains("Spring Boot")` exact |
| Role-dependent | Different role → different required list and result |

---

## Checklist

- [ ] Required skills fetched from `jobRoles()`
- [ ] Match count and missing built together
- [ ] Integer percentage
- [ ] Returns Role / Match Score / Missing Skills block
- [ ] Unknown role behavior understood (caught upstream)

---

## Link to prior context

Roles: [role-definitions.md](./role-definitions.md).  
Next: [implementation.md](./implementation.md) — wiring role matching into the response.
