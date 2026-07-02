# Acceptance Criteria — Scoring & Matching

## Scope

This document defines **pass/fail conditions** for:

- **F4** — Skill extraction from resume text  
- **F5** — Resume score (0–100)  
- **F6** — ATS keyword analysis  
- **F7** — Job role matching  

Upload, role param, and PDF parsing are in [acceptance-upload-and-parsing.md](./acceptance-upload-and-parsing.md).  
Sections, suggestions, dashboard UI, and segment wrap-up are in [acceptance-dashboard-and-wrapup.md](./acceptance-dashboard-and-wrapup.md).

Reference: [must-have-features.md](./must-have-features.md), [api-contract.md](./api-contract.md).

---

## Test resume fixtures (conceptual)

Use small PDFs or controlled text content for repeatable checks:

| Fixture | Text content (summary) | Purpose |
|---------|------------------------|---------|
| **R-full** | java, spring, mysql, html, css, javascript | All six detectable skills |
| **R-java-only** | java, skills, education, experience | Partial skills + all sections |
| **R-minimal** | hello world (no tech keywords) | Empty skills, zero scores |
| **R-ats-rich** | java, spring boot, rest api, docker, aws, sql | High ATS coverage |
| **R-fs** | java, javascript, html, css | Full Stack role fit |

---

## F4 — Skill extraction

### AC-S01 — Detect Java

| | |
|---|---|
| **Given** | PDF text contains `java` (any casing) |
| **When** | Analyzed |
| **Then** | `Skills:` list includes `Java` |

### AC-S02 — Detect Spring Boot from "spring"

| | |
|---|---|
| **Given** | PDF text contains `spring` but not necessarily `Spring Boot` phrase |
| **When** | Analyzed |
| **Then** | `Skills:` includes `Spring Boot` |

### AC-S03 — Detect SQL from mysql or sql

| | |
|---|---|
| **Given** | PDF text contains `mysql` OR `sql` |
| **When** | Analyzed |
| **Then** | `Skills:` includes `SQL` (once, not duplicated) |

### AC-S04 — Detect HTML, CSS, JavaScript

| | |
|---|---|
| **Given** | PDF text contains `html`, `css`, `javascript` |
| **When** | Analyzed |
| **Then** | `Skills:` includes `HTML`, `CSS`, `JavaScript` |

### AC-S05 — No false positives from unrelated words

| | |
|---|---|
| **Given** | PDF with no tech keywords (fixture **R-minimal**) |
| **When** | Analyzed |
| **Then** | `Skills: []` or empty list equivalent |

### AC-S06 — Case insensitivity

| | |
|---|---|
| **Given** | PDF text: `JAVA`, `JavaScript`, `SQL` |
| **When** | Analyzed |
| **Then** | Skills detected same as lowercase equivalents |

### AC-S07 — Substring matching

| | |
|---|---|
| **Given** | PDF word `javascript` embedded in sentence |
| **When** | Analyzed |
| **Then** | `JavaScript` detected (substring match on full text) |

---

## F5 — Resume score

### AC-C01 — Maximum score 100

| | |
|---|---|
| **Given** | Fixture **R-full** (all six skills present) |
| **When** | Analyzed |
| **Then** | `Score: 100/100` |

### AC-C02 — Weighted partial score

| | |
|---|---|
| **Given** | PDF with only `java` and `html` in text |
| **When** | Analyzed |
| **Then** | `Score: 30/100` (Java 20 + HTML 10) |

### AC-C03 — Zero score

| | |
|---|---|
| **Given** | Fixture **R-minimal** |
| **When** | Analyzed |
| **Then** | `Score: 0/100` |

### AC-C04 — Score formula consistency

| | |
|---|---|
| **Given** | Known skill set in response |
| **When** | Score manually recomputed from weights table |
| **Then** | Matches API score |

| Skill | Points |
|-------|--------|
| Java | 20 |
| Spring Boot | 20 |
| SQL | 20 |
| JavaScript | 20 |
| HTML | 10 |
| CSS | 10 |

### AC-C05 — Score label format

| | |
|---|---|
| **Given** | Any successful analysis |
| **When** | Body inspected |
| **Then** | Line matches pattern `Score: {integer}/100` |

---

## F6 — ATS keyword analysis

### AC-A01 — ATS score present

| | |
|---|---|
| **Given** | Any valid PDF |
| **When** | Analyzed |
| **Then** | Body contains `ATS Score: {N}%` where N is integer 0–100 |

### AC-A02 — ATS keyword set size

| | |
|---|---|
| **Given** | Implementation review |
| **When** | Keyword list counted |
| **Then** | Exactly **14** keywords: java, spring, spring boot, rest api, microservices, mysql, sql, hibernate, docker, kubernetes, aws, javascript, html, css |

### AC-A03 — Full keyword coverage

| | |
|---|---|
| **Given** | Fixture **R-ats-rich** containing all 14 keywords in text |
| **When** | Analyzed |
| **Then** | `ATS Score: 100%`; `Missing Keywords: []` or empty |

### AC-A04 — Zero ATS coverage

| | |
|---|---|
| **Given** | Fixture **R-minimal** |
| **When** | Analyzed |
| **Then** | `ATS Score: 0%`; missing list contains all keywords |

### AC-A05 — Partial match calculation

| | |
|---|---|
| **Given** | PDF contains exactly 7 of 14 keywords |
| **When** | Analyzed |
| **Then** | `ATS Score: 50%` (7 ÷ 14 × 100, integer division) |

### AC-A06 — Missing keywords listed

| | |
|---|---|
| **Given** | PDF missing `docker` and `kubernetes` |
| **When** | Analyzed |
| **Then** | `Missing Keywords:` includes `docker` and `kubernetes` |

### AC-A07 — Case-insensitive ATS match

| | |
|---|---|
| **Given** | PDF contains `REST API` and `Docker` |
| **When** | Analyzed |
| **Then** | `rest api` and `docker` counted as matched |

### AC-A08 — spring vs spring boot

| | |
|---|---|
| **Given** | PDF contains only word `spring` |
| **When** | Analyzed |
| **Then** | Both `spring` and `spring boot` keywords may match if `spring boot` substring logic applies to `spring` in list — verify: `spring` matches; `spring boot` as separate keyword requires phrase `spring boot` in text |

*Expected v1 behavior:* `spring` keyword matches on `spring`; `spring boot` keyword matches only when full phrase appears in lowercased text.

---

## F7 — Job role matching

### AC-M01 — Java Developer full match

| | |
|---|---|
| **Given** | Skills detected: Java, Spring Boot, SQL |
| **When** | `role=Java Developer` |
| **Then** | `Match Score: 100%`; `Missing Skills: []` |

### AC-M02 — Java Developer partial match

| | |
|---|---|
| **Given** | Skills detected: Java, SQL only (no Spring Boot) |
| **When** | `role=Java Developer` |
| **Then** | `Match Score: 66%` (2 of 3); `Missing Skills: [Spring Boot]` |

### AC-M03 — Full Stack Developer full match

| | |
|---|---|
| **Given** | Fixture **R-fs** |
| **When** | `role=Full Stack Developer` |
| **Then** | `Match Score: 100%` |

### AC-M04 — Full Stack Developer missing JavaScript

| | |
|---|---|
| **Given** | Skills: Java, HTML, CSS only |
| **When** | `role=Full Stack Developer` |
| **Then** | `Match Score: 75%` (3 of 4); missing includes `JavaScript` |

### AC-M05 — Data Analyst — Python and Excel not in skill extractor

| | |
|---|---|
| **Given** | PDF with only `sql` detected |
| **When** | `role=Data Analyst` |
| **Then** | `Match Score: 33%` (1 of 3); `Missing Skills` includes `Python` and `Excel` |

*Note:* v1 skill extractor does not detect Python/Excel from text; Data Analyst match is expected to be low unless PDF text is extended manually for testing.

### AC-M06 — Role line echoes request

| | |
|---|---|
| **Given** | Any role |
| **When** | Analyzed |
| **Then** | `Role: {role}` matches submitted `role` string exactly |

### AC-M07 — Match uses detected skills only

| | |
|---|---|
| **Given** | PDF mentions `python` in prose but Python not in F4 skill list |
| **When** | `role=Data Analyst` |
| **Then** | Python still listed in `Missing Skills` (matching uses `extractSkills` output, not raw ATS keywords) |

---

## Cross-feature integration

### AC-X01 — Skills drive score and role match

| | |
|---|---|
| **Given** | Single upload |
| **When** | Skills parsed from response |
| **Then** | Score equals weighted sum of those skills; match % consistent with same skill list |

### AC-X02 — ATS independent of skill list

| | |
|---|---|
| **Given** | PDF with `docker` in text but docker not in F4 skill names |
| **When** | Analyzed |
| **Then** | ATS score reflects `docker` keyword; `Skills:` may not include Docker |

### AC-X03 — Response segment order

| | |
|---|---|
| **Given** | Successful analysis |
| **When** | Body inspected |
| **Then** | `Skills:` and `Score:` appear before `ATS Score:`; role block appears before ATS block per [api-contract.md](./api-contract.md) |

---

## Sign-off checklist

Scoring & matching is **accepted** when:

- [ ] AC-S01 through AC-S07 — PASS  
- [ ] AC-C01 through AC-C05 — PASS  
- [ ] AC-A01 through AC-A08 — PASS (document A08 actual behavior)  
- [ ] AC-M01 through AC-M07 — PASS  
- [ ] AC-X01 through AC-X03 — PASS  

---

## Known v1 behaviors (not bugs)

| Behavior | Explanation |
|----------|-------------|
| Data Analyst match often low | Python/Excel not extracted by F4 |
| Integer division on percentages | Match and ATS use integer math |
| Skill list ≠ ATS keyword list | Different purposes and vocabularies |

---

## Link to prior context

Next: [acceptance-dashboard-and-wrapup.md](./acceptance-dashboard-and-wrapup.md) — sections, suggestions, career roadmap, dashboard UI, and requirements segment close.
