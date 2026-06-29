# Market Context & Pain Points

## Why resume feedback matters

Most hiring pipelines today begin with an Applicant Tracking System (ATS) before a human recruiter ever opens a file. Candidates upload a PDF, the system parses text, scores keyword overlap against a job description, and filters the long tail of applicants. A strong resume on paper can still fail silently when formatting breaks parsing, sections are mislabeled, or the vocabulary does not match the role.

Job seekers—especially students and early-career developers—rarely get actionable feedback at the moment they need it. They might ask a friend, run a generic spell-check, or submit and hope. None of that answers the questions that actually determine whether they advance:

- Will an ATS read my PDF correctly?
- Do my listed skills match what recruiters search for?
- Am I aligned with the role I am targeting?
- What is missing from my structure or content?

## Who feels this pain most

| Audience | Typical situation | What goes wrong |
|----------|-------------------|-----------------|
| University students | First or second internship search | Thin experience sections, unclear skill lists, no role-specific tailoring |
| Bootcamp graduates | Pivoting into tech quickly | Resume reads like coursework, not job-ready keywords |
| Junior developers | Competing on similar stacks | Same buzzwords as peers; hard to stand out or spot gaps |
| Career switchers | Translating non-tech background | ATS misses transferable skills; wrong terminology for target role |

These groups often lack access to career coaches or paid review services. They need fast, specific guidance—not a generic template download.

## Common failure modes

### ATS and formatting

- Multi-column layouts or graphics that parsers strip or scramble
- Skills buried in prose instead of scannable lists
- Section headers that machines do not recognize (`"What I know"` vs `"Skills"`)
- Missing standard blocks (education, experience) that scoring rules expect

### Keyword and role misalignment

- Resume written for `"software engineer"` but application is `"Java Developer"` with different keyword sets
- Listing tools without the phrases ATS and job posts actually use (e.g. `"REST API"` vs `"built APIs"`)
- No visibility into which expected terms are absent

### Subjective guesswork

- Candidates cannot estimate how competitive their resume is before applying
- No single place to see score, skill coverage, section health, and next steps together
- Repeated applications without learning what to fix

## The gap this project addresses

There is room for a lightweight tool that:

1. Accepts a resume as a PDF—the format most applicants already have
2. Lets the user pick a target job role for contextual analysis
3. Returns structured, immediate feedback instead of a black-box rejection weeks later

The goal is not to replace human recruiters or guarantee offers. It is to give job seekers a clear mirror: what was detected, what scored well, what is missing, and what to improve next—before they hit submit.

## Context for later work

Everything that follows in this documentation builds on this problem space: turning an opaque application step into a guided, measurable feedback loop for people who are learning how to present themselves in a competitive, automated hiring market.
