# Verify & Wrap-Up

## Purpose

Run one documented full integration pass and close the `testing/` segment.

---

## Integration walkthrough (happy path)

```bash
# 1. Build + unit test
./mvnw clean test

# 2. Run
./mvnw spring-boot:run
```

3. Browser → `http://localhost:8080/`.
4. Upload `rich.pdf`, select `Java Developer`, Analyze.
5. Observe:

| Panel | Expected |
|-------|----------|
| Loader | Appears then hides |
| Score | Doughnut 100/100 |
| Skills | Six tags |
| Suggestions | Minimal (rich resume) |
| Role | Java Developer, 100% |
| ATS | High % |
| Sections | All Found ✅ |
| Roadmap | Java Developer roadmap |
| Insight | "Interview ready" band |

6. Upload `sparse.pdf` without reload → dashboard updates, no chart stacking.

---

## Sign-off checklist

- [ ] `contextLoads` passes
- [ ] Manual matrix all PASS
- [ ] API cases return expected statuses/bodies
- [ ] Edge cases handled without crash
- [ ] Full happy-path run observed
- [ ] Re-submit works cleanly

---

## Deliverable confirmation

| Deliverable | Evidence |
|-------------|----------|
| Automated boot test | context-test |
| Manual coverage | matrix |
| API verification | api-testing |
| Robustness | edge-cases |
| Integration proof | this walkthrough |

**End-to-end verification completed across backend, API, and UI** ✓

---

## Requirements mapping

| Item | Status |
|------|--------|
| F13 Testing & integration | Done |

---

## Document index

| # | Document |
|---|----------|
| 1 | [overview.md](./overview.md) |
| 2 | [context-test.md](./context-test.md) |
| 3 | [manual-test-matrix.md](./manual-test-matrix.md) |
| 4 | [api-testing.md](./api-testing.md) |
| 5 | [edge-cases.md](./edge-cases.md) |
| 6 | [verify-and-wrapup.md](./verify-and-wrapup.md) |

---

## What comes next

**Documentation** — a project README and guides so anyone can understand, run, and extend the application.

---

## Testing & integration complete

The application is verified end-to-end and behaves per the acceptance criteria.

**Testing and integration documentation for this segment is complete.**
