# Response Assembly

## Purpose

Define the final, complete response string that `processResume` returns — all analysis blocks in the fixed order the dashboard parses.

Implements requirement **F12** (unified response).

---

## Block order

| # | Block | Source |
|---|-------|--------|
| 1 | `Skills: [...]` | extractSkills |
| 2 | `Score: N/100` | calculateScore |
| 3 | `Suggestions: [...]` | getSuggestions |
| 4 | *(blank line)* | separator |
| 5 | Role match block | matchJobRole |
| 6 | *(blank line)* | separator |
| 7 | ATS block | checkATSScore |
| 8 | *(blank line)* | separator |
| 9 | Resume sections block | detectSections |
| 10 | *(blank line)* | separator |
| 11 | Career roadmap block | getJobSuggestions |

---

## Assembly code

```java
return "Skills: " + skills
     + "\nScore: " + score + "/100"
     + "\nSuggestions: " + suggestions
     + "\n\n" + jobMatch
     + "\n\n" + atsResult
     + "\n\n" + sectionResult
     + "\n\n" + jobSuggestions;
```

- `\n` separates lines within the first block.
- `\n\n` (blank line) separates major blocks — required by the UI section parser.

---

## Full sample response

```text
Skills: [Java, SQL, HTML]
Score: 50/100
Suggestions: [Create a REST API project using Spring Boot, Work on frontend using JavaScript and build projects, Improve frontend skills using HTML, CSS and responsive design]

Role: Java Developer
Match Score: 66%
Missing Skills: [Spring Boot]

ATS Score: 42%
Missing Keywords: [spring boot, rest api, microservices, hibernate, docker, kubernetes, aws, javascript, css]

Resume Sections:
Skills Section: Found ✅
Education Section: Found ✅
Experience Section: Missing ❌

Career Roadmap for Java Developer:
[Learn Spring Boot and build REST APIs, Build a full backend project using Spring Boot]
```

---

## Parser alignment

| UI field | Regex |
|----------|-------|
| Skills | `/Skills: \[(.*?)\]/` |
| Score | `/Score: (\d+)/` |
| Suggestions | `/Suggestions: \[(.*?)\]/` |
| ATS score | `/ATS Score: (\d+)%/` |
| ATS missing | `/Missing Keywords: \[(.*?)\]/` |
| Sections | `/Resume Sections:\n([\s\S]*?)\n\n/` |
| Roadmap | `/Career Roadmap[\s\S]*/` |

The blank line after the sections block is what makes the sections regex terminate correctly — keep the `\n\n` separators.

---

## Stability rule

Renaming any label (e.g. `Score:` → `Total:`) breaks the frontend parser. Changes to labels require a coordinated UI update. See [api-contract.md](../requirements/api-contract.md).

---

## Checklist

- [ ] Blocks in contract order
- [ ] `\n\n` between major blocks
- [ ] Sections block followed by blank line before roadmap
- [ ] Labels unchanged from contract

---

## Link to prior context

Roadmap: [career-roadmap.md](./career-roadmap.md).  
Next: [implementation.md](./implementation.md) — complete final `ResumeService`.
