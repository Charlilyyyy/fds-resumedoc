# Vision Summary

## Product name

**ResumeDoc**

Short for *resume document intelligence*—a name that reflects what the product does (reads a resume document) without overpromising full artificial intelligence. It is memorable, easy to say in a README, and fits a portfolio repo focused on resume feedback documentation.

| Criterion | How ResumeDoc fits |
|-----------|-------------------|
| Clarity | Users know it relates to resumes |
| Scope | Analysis and feedback, not job placement |
| Tone | Professional enough for GitHub, approachable for students |
| Distinct | Not tied to a specific framework or employer brand |

Working title in UI copy may use a light tagline (e.g. “ResumeDoc — instant resume feedback”) while the technical codebase uses neutral package naming.

---

## Vision statement

ResumeDoc gives every job seeker—especially students and junior developers—a fair preview of how their resume might perform in an automated hiring pipeline **before** they click submit.

The vision is a single-session feedback loop: upload, select a role, see the truth in the text, improve, repeat. No black box, no week-long silence, no guesswork.

---

## Mission (what we are building toward)

| Goal | Measure of success |
|------|-------------------|
| Close the pre-submit feedback gap | User gets score, skills, ATS %, sections, and suggestions in one visit |
| Make role targeting explicit | Every analysis is anchored to a chosen job role |
| Keep friction near zero | PDF + dropdown + dashboard; no account for v1 |
| Teach through results | Suggestions and roadmap turn gaps into next actions |
| Stay honest | Tool informs improvement; it does not guarantee offers |

---

## Idea recap — the five definition documents

This folder captures the full **idea and problem definition** for ResumeDoc. Read in order:

| Document | Question it answers |
|----------|---------------------|
| [market-context.md](./market-context.md) | Why does resume feedback matter now? |
| [problem-statement.md](./problem-statement.md) | What exact problem are we solving? |
| [value-proposition.md](./value-proposition.md) | What does the user get out of it? |
| [user-personas.md](./user-personas.md) | Who are we building for? |
| [user-flow.md](./user-flow.md) | How do they move through the product? |
| **vision-summary.md** (this file) | What is the product called, and what is the north star? |

Together they form the one-page problem statement plus persona deliverable expanded into a coherent definition set.

---

## One-page problem + persona (consolidated)

**Problem:** Job seekers cannot tell if their PDF resume is ATS-friendly or aligned with a target role until after they apply—and often not even then.

**Solution shape:** A web app that accepts a PDF and a role, then returns structured feedback: numeric score, detected skills, ATS keyword coverage, section checklist, role match gaps, suggestions, and a career roadmap on a dashboard.

**Primary users:** Alex (CS student) and Jordan (junior developer)—people with limited coaching budget who need fast, document-specific, role-aware guidance.

**Core journey:** Upload PDF → pick role → view dashboard → revise offline → re-run.

---

## Principles that guide later work

1. **User document first** — Every metric comes from the uploaded file, not a template.
2. **Role-aware** — Matching logic changes when the selected role changes.
3. **Immediate** — Same-session results; no async email flow in v1.
4. **Transparent** — Show what was found, what was missing, and why it matters.
5. **Incremental scope** — Ship the core upload-analyze-display loop before auth, persistence, or external AI APIs.

---

## Explicit non-goals (v1)

- Guaranteed interviews or job offers
- Replacing recruiters or human resume writers
- Auto-rewriting the candidate’s resume
- User accounts and upload history (future consideration)
- Support for non-PDF formats in the first release

---

## What comes next

With the why, who, what, and how defined here, the next documentation work moves into **feature planning and requirements**: a concrete checklist of must-have capabilities, inputs and outputs, and acceptance criteria for each feature before any code is written.

That work will translate this vision into buildable units—PDF upload, skill extraction, scoring, ATS analysis, role matching, section detection, suggestions, and the dashboard UI.

---

## Definition complete

ResumeDoc exists to turn resume submission from a blind bet into an informed step. The market context, problem, value, personas, and user flow all point to the same outcome: **actionable clarity at the moment it matters most.**

Definition documentation for this segment is complete.
