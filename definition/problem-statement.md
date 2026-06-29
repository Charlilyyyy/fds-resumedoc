# Problem Statement

## One-line summary

Job seekers cannot easily tell whether their resume will survive automated screening or match a specific role before they apply.

## The problem

Candidates preparing for technical roles upload PDF resumes into employer portals every day. Behind those uploads, Applicant Tracking Systems parse text, score keyword overlap, and rank applicants long before a recruiter reviews anyone. The candidate receives no immediate signal about how their document performed.

Without timely feedback, applicants repeat the same mistakes: weak keyword coverage, missing standard sections, skills listed in ways parsers miss, and resumes tailored to a generic title instead of the role they want. Students and junior developers are hit hardest—they have limited industry exposure, little budget for professional resume reviews, and no reliable way to practice improvement between applications.

The core problem is **information asymmetry at submit time**. Employers know what their ATS and job descriptions expect; candidates guess.

## Problem in concrete terms

| Dimension | What happens today | What candidates need |
|-----------|-------------------|----------------------|
| ATS readiness | Unknown until rejection or silence | A read on keyword coverage and parse-friendly structure |
| Role fit | Resume is one-size-fits-all | Comparison against skills expected for a chosen role |
| Skill visibility | Self-reported lists feel complete | Detection of what is actually present in the document |
| Structure | Easy to omit or mislabel sections | Clear pass/fail on standard blocks (skills, education, experience) |
| Next steps | Vague advice from blogs or peers | Actionable suggestions tied to gaps found in *their* file |

## Who is affected

**Primary:** Students and junior developers applying to entry-level and early-career software roles.

**Secondary:** Anyone submitting a PDF resume into an automated pipeline who wants a quick sanity check before applying—bootcamp graduates, career switchers, and self-taught builders entering the job market.

## Current workarounds and why they fall short

- **Generic resume templates** — Do not reflect the candidate's actual content or target role.
- **Peer review** — Subjective, inconsistent, and rarely grounded in ATS keyword logic.
- **Paid coaching** — Effective but costly and not available on demand at 11 p.m. before a deadline.
- **Apply and wait** — Slow feedback loop; each failed application costs time and morale.

None of these combine **instant**, **document-specific**, and **role-aware** analysis in one place.

## Proposed direction (high level)

Build a lightweight web application where a user:

1. Uploads a resume PDF
2. Selects a target job role
3. Receives structured feedback: overall score, detected skills, ATS keyword coverage, section checklist, role match gaps, and improvement suggestions

This does not promise employment outcomes. It closes the feedback gap between writing a resume and submitting it blind.

## Success criteria for solving this problem

The problem is meaningfully addressed when a user can, in a single session:

- Understand how well their resume text aligns with common ATS keyword expectations
- See which skills were detected and which are missing for their chosen role
- Know whether standard resume sections are present
- Leave with specific ideas for what to add or rewrite—without waiting for an employer response

## Scope boundaries

**In scope:** PDF upload, text-based analysis, role-selected matching, immediate dashboard-style results.

**Out of scope (for now):** Guaranteed job placement, replacing human recruiters, storing long-term user accounts, or rewriting the resume automatically.

## Link to prior context

This statement formalizes the pain points described in [market-context.md](./market-context.md). The next documents define the value delivered to users, who they are in detail, and how they move through the product.
