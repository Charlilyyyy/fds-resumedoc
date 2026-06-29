# User Personas

## Overview

The product is built for people who submit PDF resumes into automated hiring pipelines and need fast, specific feedback before they apply. Two primary personas anchor design decisions; two secondary personas share the same core needs with different backgrounds.

| Persona | Priority | Typical goal |
|---------|----------|--------------|
| Alex — Computer Science Student | Primary | Land first internship or graduate role |
| Jordan — Junior Developer | Primary | Improve hit rate on early-career applications |
| Sam — Bootcamp Graduate | Secondary | Prove job-readiness after a career pivot |
| Riley — Career Switcher | Secondary | Reframe non-tech experience for a tech role |

---

## Primary persona 1: Alex — Computer Science Student

### Profile

| Attribute | Detail |
|-----------|--------|
| Age | 20–23 |
| Background | Final-year CS student or recent graduate |
| Experience | 1–2 academic projects, maybe one internship |
| Target roles | Java Developer, Full Stack Developer, junior backend roles |
| Tech comfort | Comfortable with web apps; uses GitHub and LinkedIn regularly |

### Context

Alex has a resume exported from a university career-center template. It lists coursework and group projects but reads thin on industry keywords. Alex applies to dozens of postings and hears little back, unsure whether the resume, the market, or the role fit is the issue.

### Goals

- Understand if the resume looks credible to recruiters and ATS filters
- Tailor applications to specific role types without rewriting from scratch each time
- Learn what skills or sections to add before graduation recruiting season

### Frustrations

- Career office advice is generic and not tied to Alex’s actual PDF
- Friends give conflicting opinions (“add more buzzwords” vs “keep it simple”)
- No budget for paid resume coaching
- Cannot tell if a low response rate means bad luck or a fixable resume problem

### How this product helps Alex

- Uploads the current PDF and picks **Java Developer** or **Full Stack Developer**
- Sees a numeric score and which skills were actually detected from project descriptions
- Gets ATS keyword gaps (e.g. missing “REST API”, “Spring Boot”)
- Learns that **Experience** is missing or weak and receives suggestions to add a real-world project block

### Success moment

Alex revises the resume, re-uploads, and watches the score and role match percentage rise—confidence to apply before the next career fair.

---

## Primary persona 2: Jordan — Junior Developer

### Profile

| Attribute | Detail |
|-----------|--------|
| Age | 23–27 |
| Background | 1–2 years professional or contract dev work |
| Experience | Small team shipping features; stack similar to peers |
| Target roles | Java Developer, Full Stack Developer, mid-junior backend |
| Tech comfort | Daily coder; expects tools to be fast and precise |

### Context

Jordan has been applying for six months with mixed results. The resume lists Java and SQL but Jordan suspects it does not emphasize the right phrases for ATS or the specific role family Jordan wants next.

### Goals

- Benchmark the resume against common hiring keyword sets
- Compare fit for **Java Developer** vs **Full Stack Developer** before choosing where to focus
- Iterate quickly between versions without waiting for recruiter feedback

### Frustrations

- Resume “feels fine” but applications stall at automated screening
- Hard to see which missing skills matter for the *selected* role vs generic advice
- Reformatting in Word does not answer whether the content is strong

### How this product helps Jordan

- Runs the same PDF against different role selections to compare match scores
- Surfaces missing ATS terms Jordan thought were implied in job bullet points
- Section checklist flags a buried skills block or missing education formatting cues
- Career roadmap suggestions point to concrete upskilling (e.g. Spring Boot depth, portfolio project)

### Success moment

Jordan picks the role with the higher match, tightens bullet wording for missing keywords, and uses the dashboard as a regression check before each batch of applications.

---

## Secondary persona: Sam — Bootcamp Graduate

| Attribute | Detail |
|-----------|--------|
| Background | 12-week intensive program; prior career in retail or admin |
| Pain | Resume reads like training exercises, not production work |
| Need | Keyword alignment and section structure that signal employability |

Sam uses the same upload → role → dashboard flow. Suggestions to add projects and role-specific skills matter more than a high initial score.

---

## Secondary persona: Riley — Career Switcher

| Attribute | Detail |
|-----------|--------|
| Background | Self-taught or transitioning from adjacent field |
| Pain | Transferable skills use wrong vocabulary for tech ATS |
| Need | Visibility into which target-role skills are still absent |

Riley benefits from role match breakdowns and missing-skill lists when reframing experience for **Data Analyst** or developer tracks.

---

## Shared needs across personas

All four personas value:

1. **Speed** — Feedback in one sitting, not days later
2. **Specificity** — Results from their PDF, not a template
3. **Role context** — Analysis changes when the selected job role changes
4. **Actionable output** — Scores plus clear “add this / fix that” guidance
5. **Low setup** — PDF file and a role dropdown; no account for the core path

## Design implications

| Persona need | Product response |
|--------------|------------------|
| Unsure about ATS | ATS keyword % and missing list |
| Unsure about fit | Role match % and missing skills for chosen role |
| Thin experience | Section detection + project suggestions |
| Iterating versions | Re-upload and compare scores on the dashboard |
| Non-expert in hiring | Plain-language cards and charts, not raw API text |

## Anti-personas (not the main focus)

- **Executive hiring managers** — They evaluate candidates, not their own resumes
- **Professional resume writers** — They want white-label tools, not a student-oriented dashboard
- **Users needing LinkedIn-only or DOCX-only workflows** — Initial scope is PDF upload

## Link to prior context

Personas ground the value described in [value-proposition.md](./value-proposition.md). The next document maps how Alex, Jordan, and others move through the product step by step.
