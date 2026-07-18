# README Content

## Purpose

The body for the project README — the first thing a visitor reads. Copy this into `README.md` at the project root.

---

## Suggested README

```markdown
# ResumeDoc

Analyze your resume instantly. Upload a PDF, pick a target role, and get a
score, detected skills, ATS keyword coverage, role match, section checklist,
and a personalized career roadmap — all in a single dashboard.

## Features

- PDF resume upload
- Automatic text extraction (Apache PDFBox)
- Skill detection and 0–100 resume score
- ATS keyword coverage with missing-keyword list
- Job role matching (Java Developer, Full Stack Developer, Data Analyst)
- Resume section detection (Skills / Education / Experience)
- Smart suggestions and a role-based career roadmap
- Interactive dashboard with charts (Chart.js)

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Language | Java 17 |
| Framework | Spring Boot 3 |
| Build | Maven |
| PDF parsing | Apache PDFBox |
| Frontend | HTML, CSS, JavaScript, Chart.js |

## Quick Start

```bash
git clone <repo-url>
cd <project>
./mvnw spring-boot:run
```

Open http://localhost:8080/ and upload a resume.

## API

`POST /resume/upload` — multipart form with `file` (PDF) and `role` (text).
Returns a labeled plain-text analysis.

## License

For educational/portfolio use.
```

---

## Section rationale

| Section | Why |
|---------|-----|
| One-line pitch | Instant understanding |
| Features | Scannable capability list |
| Tech stack | Signals stack fit to evaluators |
| Quick start | Lowers barrier to run |
| API | Points integrators to the contract |

---

## Naming consistency

Use **ResumeDoc** consistently across README, UI title, and docs. See [vision-summary.md](../definition/vision-summary.md).

---

## Checklist

- [ ] Pitch line present
- [ ] Feature list matches [must-have-features.md](../requirements/must-have-features.md)
- [ ] Tech stack matches [tech-stack.md](../architecture/tech-stack.md)
- [ ] Quick start runs from clean clone
- [ ] API line matches [api-contract.md](../requirements/api-contract.md)

---

## Link to prior context

Overview: [overview.md](./overview.md).  
Next: [how-it-works.md](./how-it-works.md) — architecture and flow for developers.
