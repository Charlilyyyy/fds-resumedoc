# Core Value Proposition

## Value in one sentence

Upload a PDF resume, pick a target job role, and get instant, structured feedback—score, skills, keyword gaps, section health, and career suggestions—in one dashboard view.

## What users get

The product turns a static PDF into a readable report within seconds. Every analysis is tied to the file the user actually plans to submit, not a generic checklist pulled from a blog post.

| Output | What it tells the user | Why it matters |
|--------|------------------------|----------------|
| **Resume score (0–100)** | Weighted strength based on detected technical skills | A single number to track improvement between revisions |
| **Detected skills** | Skills found in the document text (e.g. Java, SQL, HTML) | Confirms what parsers and recruiters are likely to see |
| **ATS keyword coverage** | Percentage match against common hiring keywords + list of missing terms | Surfaces vocabulary gaps before automated screening |
| **Role match score** | How well detected skills align with the selected job role | Connects the resume to a specific application, not a vague title |
| **Section checklist** | Found or missing: Skills, Education, Experience | Catches structural issues that hurt both ATS and human skim-reading |
| **Smart suggestions** | Learning and content tips based on what is absent | Gives a concrete next step instead of “rewrite your resume” |

## How value is delivered

```
PDF upload  +  job role selection  →  analysis pipeline  →  dashboard results
```

1. **Input** — User provides a resume PDF and chooses a role (e.g. Java Developer, Full Stack Developer, Data Analyst).
2. **Processing** — Text is extracted from the PDF and run through skill detection, scoring, ATS keyword checks, role matching, and section scanning.
3. **Output** — Results appear together on an interactive dashboard with charts and cards so the user can scan strengths and gaps at a glance.

No account required for the core loop. No waiting days for recruiter silence. Feedback is immediate and repeatable—upload a revised PDF and compare.

## Who benefits and how

| User type | Primary gain |
|-----------|--------------|
| Student / intern applicant | Learns what “job-ready” looks like for a chosen role before the first real application |
| Junior developer | Spots missing stack keywords and thin sections before mass-applying |
| Career switcher | Sees whether their resume language matches the role they are targeting |

## Differentiators vs. common alternatives

| Alternative | Limitation | This product |
|-------------|------------|--------------|
| Static templates | One-size-fits-all layout | Analysis of *your* PDF content |
| Generic spell-check | Grammar only, no hiring logic | ATS keywords, role skills, section rules |
| Peer review | Subjective, slow to schedule | Consistent, on-demand, document-specific |
| Apply first, learn later | Feedback arrives too late | Feedback *before* submit |

## Value principles

1. **Instant** — Results in the same session as the upload.
2. **Specific** — Every metric derives from the user’s file and selected role.
3. **Actionable** — Missing keywords, skills, and sections map to suggestions, not just scores.
4. **Honest** — Scores inform improvement; they do not guarantee interviews or offers.
5. **Low friction** — PDF + role dropdown + dashboard; no complex setup.

## What “good” looks like for the user

After one session, a user should be able to answer:

- “How strong is this version of my resume?” → **Score**
- “What skills did the system actually find?” → **Skills list**
- “Would typical ATS keyword filters see enough signal?” → **ATS %**
- “Am I a fit for the role I care about?” → **Match % + missing skills**
- “Is my resume structurally complete?” → **Section checklist**
- “What should I work on next?” → **Suggestions & career roadmap**

## Non-goals (value boundaries)

The product does **not**:

- Write or auto-edit the resume for the user
- Replace a human recruiter’s judgment
- Store application history without explicit future auth work
- Promise employment outcomes

Staying within these boundaries keeps the value proposition credible: **a fast mirror and coach**, not a magic job offer.

## Link to prior context

This document defines *what* the user receives, building on the problem framed in [problem-statement.md](./problem-statement.md). Next: detailed user personas and the step-by-step journey through the product.
