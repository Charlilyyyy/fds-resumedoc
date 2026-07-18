# Testing & Integration — Overview

## Purpose

The application is feature-complete. This `testing/` folder documents how to verify it works end-to-end: the automated context test, a manual test matrix, API testing, and edge cases across the full upload → analysis → dashboard flow.

Goal: a repeatable checklist that confirms the whole system behaves per the acceptance criteria.

---

## What this segment delivers

| Deliverable | Description |
|-------------|-------------|
| Context test | `contextLoads` verifies the app boots |
| Manual test matrix | Feature-by-feature checks |
| API testing | Postman/cURL cases for the endpoint |
| Edge cases | Corrupt/empty/encrypted PDFs, unknown role, no keywords |
| Integration walkthrough | One full happy-path run |

**Scope:** verification and integration only — no new features.

---

## Test layers

```
Automated:  contextLoads (JUnit)
Manual API: Postman / cURL against /resume/upload
Manual UI:  browser dashboard at /
Edge:       malformed inputs and boundary conditions
```

---

## Feature mapping

| Requirement | This segment |
|-------------|--------------|
| F13 Testing & integration | All documents |

Cross-references every acceptance file:
[acceptance-upload-and-parsing.md](../requirements/acceptance-upload-and-parsing.md),
[acceptance-scoring-and-matching.md](../requirements/acceptance-scoring-and-matching.md),
[acceptance-dashboard-and-wrapup.md](../requirements/acceptance-dashboard-and-wrapup.md).

---

## Documents in this segment

| Document | Contents |
|----------|----------|
| [overview.md](./overview.md) | This file — scope |
| context-test.md | Automated boot test |
| manual-test-matrix.md | Feature checklist |
| api-testing.md | Postman/cURL cases |
| edge-cases.md | Malformed inputs |
| verify-and-wrapup.md | Integration run and close |

This segment is split into **6 commits** (one document per commit).

---

## Prerequisites

- [ ] Full app builds and runs
- [ ] Sample PDFs (rich, sparse, non-PDF, encrypted)
- [ ] Postman or cURL available

---

## Success criteria

- [ ] Context test passes
- [ ] Manual matrix all green
- [ ] API cases return expected bodies
- [ ] Edge cases handled without crash
- [ ] One documented full happy-path run

---

## Next document

**context-test.md** — the automated Spring Boot context load test.
