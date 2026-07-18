# Section Detection

## Purpose

Define `detectSections(text)` — check whether standard resume section headings appear in the text and report Found/Missing for each.

Implements requirement **F8**.

---

## Method signature

```java
private String detectSections(String text)
```

| Input | Output |
|-------|--------|
| Full resume text | Labeled block with three section statuses |

---

## Detection rules

Lowercase the text, then check for each heading keyword:

| Section | Keyword | Status |
|---------|---------|--------|
| Skills | `skills` | Found ✅ / Missing ❌ |
| Education | `education` | Found ✅ / Missing ❌ |
| Experience | `experience` | Found ✅ / Missing ❌ |

---

## Reference logic

```java
private String detectSections(String text) {
    String lowerText = text.toLowerCase();

    boolean hasSkills = lowerText.contains("skills");
    boolean hasEducation = lowerText.contains("education");
    boolean hasExperience = lowerText.contains("experience");

    String result = "Resume Sections:\n";
    result += "Skills Section: " + (hasSkills ? "Found ✅" : "Missing ❌") + "\n";
    result += "Education Section: " + (hasEducation ? "Found ✅" : "Missing ❌") + "\n";
    result += "Experience Section: " + (hasExperience ? "Found ✅" : "Missing ❌");

    return result;
}
```

---

## Output format

```text
Resume Sections:
Skills Section: Found ✅
Education Section: Found ✅
Experience Section: Missing ❌
```

Frontend parses the block with `/Resume Sections:\n([\s\S]*?)\n\n/`.

---

## Examples

| Text contains | Result |
|---------------|--------|
| skills, education, experience | All Found ✅ |
| skills only | Skills ✅, Education ❌, Experience ❌ |
| none | All Missing ❌ |

---

## Design notes

| Topic | Note |
|-------|------|
| Case-insensitive | Text lowercased |
| Substring match | `experience` matches within `work experience` |
| Block label | Starts with `Resume Sections:` (required by UI parser) |
| Emoji markers | ✅ / ❌ shown in the dashboard sections card |

---

## Checklist

- [ ] Three section checks (skills, education, experience)
- [ ] Found/Missing emoji markers
- [ ] Block begins `Resume Sections:`
- [ ] Three status lines

---

## Link to prior context

Overview: [overview.md](./overview.md).  
Next: [career-roadmap.md](./career-roadmap.md) — role-based improvement guidance.
