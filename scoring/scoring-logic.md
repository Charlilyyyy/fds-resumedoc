# Scoring Logic

## Purpose

Define `calculateScore(skills)` — convert the detected skills list into a single 0–100 resume score using fixed weights.

Implements requirement **F5**.

---

## Method signature

```java
private int calculateScore(List<String> skills)
```

| Input | Output |
|-------|--------|
| Detected skills list | Integer score 0–100 |

---

## Weights

| Skill | Points |
|-------|--------|
| Java | 20 |
| Spring Boot | 20 |
| SQL | 20 |
| JavaScript | 20 |
| HTML | 10 |
| CSS | 10 |
| **Maximum** | **100** |

Four "core" skills at 20 points each (80) plus two frontend basics at 10 each (20) sum to 100.

---

## Reference logic

```java
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
```

---

## Examples

| Skills | Calculation | Score |
|--------|-------------|-------|
| [Java, Spring Boot, SQL, JavaScript, HTML, CSS] | 20+20+20+20+10+10 | 100 |
| [Java, SQL, HTML] | 20+20+10 | 50 |
| [Java, HTML] | 20+10 | 30 |
| [] | 0 | 0 |

---

## Output format

The score is rendered in the response as:

```text
Score: 50/100
```

Frontend later parses this with `/Score: (\d+)/`.

---

## Design notes

| Topic | Note |
|-------|------|
| Deterministic | Same skills always yield same score |
| No partial credit | Skill is present or absent |
| Order-independent | Uses `contains`, not position |
| Depends on F4 | Only counts skills `extractSkills` can detect |

Score is a **relative signal for improvement**, not a guarantee of interview success (see [vision-summary.md](../definition/vision-summary.md)).

---

## Checklist

- [ ] Weights match table (20/20/20/20/10/10)
- [ ] Max achievable is exactly 100
- [ ] Empty skills → 0
- [ ] Returns `int`

---

## Link to prior context

Skill source: [skill-extraction.md](./skill-extraction.md).  
Next: [suggestions.md](./suggestions.md) — improvement tips from missing skills.
