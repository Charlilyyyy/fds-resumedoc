# ATS Scoring Logic

## Purpose

Define `checkATSScore(text)` — count how many ATS keywords appear in the resume text, compute a percentage, and list missing keywords.

Implements requirement **F6**.

---

## Method signature

```java
private String checkATSScore(String text)
```

| Input | Output |
|-------|--------|
| Full resume text | Labeled block: ATS score % + missing keywords |

---

## Algorithm

1. Get keyword list from `getATSKeywords()` (14 terms).
2. Lowercase the resume text.
3. For each keyword, check `contains`; count matches, collect misses.
4. `score = (matchCount * 100) / totalKeywords` (integer division).
5. Return formatted block.

---

## Reference logic

```java
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

## Percentage examples

| Matches (of 14) | Calculation | ATS Score |
|-----------------|-------------|-----------|
| 14 | 1400 / 14 | 100% |
| 7 | 700 / 14 | 50% |
| 3 | 300 / 14 | 21% (integer) |
| 0 | 0 / 14 | 0% |

Integer division truncates (e.g. 3/14 → 21%, not 21.4%).

---

## Output format

```text
ATS Score: 42%
Missing Keywords: [spring boot, rest api, microservices, hibernate, docker, kubernetes, aws, javascript, css]
```

Frontend parses `/ATS Score: (\d+)%/` and `/Missing Keywords: \[(.*?)\]/`.

---

## Behavior notes

| Topic | Note |
|-------|------|
| Case-insensitive | Text lowercased before matching |
| Substring match | `spring` matches within `springboot`; `sql` matches within `mysql` |
| Multi-word | `spring boot` and `rest api` require the phrase to appear |
| Independent of skills | Uses raw text, not the F4 skills list |

---

## Checklist

- [ ] Uses `getATSKeywords()` for denominator
- [ ] Text lowercased once
- [ ] Match count and missing list built together
- [ ] Integer percentage
- [ ] Returns ATS Score + Missing Keywords block

---

## Link to prior context

Keyword list: [keyword-set.md](./keyword-set.md).  
Next: [implementation.md](./implementation.md) — wiring ATS into the service response.
