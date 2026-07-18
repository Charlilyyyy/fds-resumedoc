# ATS Keyword Set

## Purpose

Define `getATSKeywords()` — the fixed list of common applicant-tracking keywords ResumeDoc scans for. ATS scoring measures resume coverage against this list.

---

## Method signature

```java
private List<String> getATSKeywords()
```

Returns an immutable-style list of lowercase keyword strings.

---

## The 14 keywords (v1)

| # | Keyword |
|---|---------|
| 1 | java |
| 2 | spring |
| 3 | spring boot |
| 4 | rest api |
| 5 | microservices |
| 6 | mysql |
| 7 | sql |
| 8 | hibernate |
| 9 | docker |
| 10 | kubernetes |
| 11 | aws |
| 12 | javascript |
| 13 | html |
| 14 | css |

Total = **14**. This count is used as the denominator for the ATS percentage.

---

## Reference logic

```java
private List<String> getATSKeywords() {
    return Arrays.asList(
        "java", "spring", "spring boot", "rest api", "microservices",
        "mysql", "sql", "hibernate", "docker", "kubernetes",
        "aws", "javascript", "html", "css"
    );
}
```

---

## Rationale

| Group | Keywords | Why |
|-------|----------|-----|
| Core backend | java, spring, spring boot, rest api, microservices | Common in Java backend job posts |
| Data | mysql, sql, hibernate | Persistence layer expectations |
| DevOps / cloud | docker, kubernetes, aws | Frequently listed "nice to have" |
| Frontend | javascript, html, css | Full-stack coverage |

---

## Relationship to skill extraction

| Aspect | Skill list (F4) | ATS keyword list (F6) |
|--------|-----------------|------------------------|
| Size | 6 skills | 14 keywords |
| Purpose | Score + role match | Coverage vs hiring vocabulary |
| Overlap | java, sql, html, css, javascript, spring | Yes, but ATS is broader |
| Extra terms | — | rest api, microservices, hibernate, docker, kubernetes, aws |

The two lists are **separate on purpose** — a resume can score ATS points for `docker` even though Docker is not a scored skill.

---

## Imports needed

```java
import java.util.Arrays;
import java.util.List;
```

---

## Checklist

- [ ] Exactly 14 keywords
- [ ] All lowercase
- [ ] Includes multi-word terms (`spring boot`, `rest api`)

---

## Link to prior context

Overview: [overview.md](./overview.md).  
Next: [scoring-logic.md](./scoring-logic.md) — match counting and percentage.
