# Context Test

## Purpose

Document the automated JUnit test that verifies the Spring Boot application context loads — the baseline "does it boot" check generated with the project.

Implements the automated part of requirement **F13**.

---

## File

```
src/test/java/com/resume/analyzer/BackendApplicationTests.java
```

---

## Test source

```java
package com.resume.analyzer;

import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
class BackendApplicationTests {

    @Test
    void contextLoads() {
    }
}
```

---

## What it proves

| Verified | Why it matters |
|----------|----------------|
| Context starts | All beans (`ResumeController`, `ResumeService`) wire without error |
| Config valid | `application.properties` loads without failure |
| Dependencies resolve | PDFBox, web, etc. present on classpath |
| No circular DI | Injection graph is sound |

An empty body is expected — the test passes if context startup does not throw.

---

## Run

```bash
./mvnw test
```

Expected:

```text
Tests run: 1, Failures: 0, Errors: 0, Skipped: 0
BUILD SUCCESS
```

---

## Boot without a database

If MySQL/JPA is on the classpath but no database is running, context load can fail. Options (see [configuration.md](../scaffolding/configuration.md)):

| Option | Effect |
|--------|--------|
| Comment out JPA/MySQL deps | No datasource needed |
| `spring.autoconfigure.exclude=...DataSourceAutoConfiguration` | Skip datasource wiring |
| Run local MySQL | Real datasource available |

v1 analysis does not require persistence, so excluding the datasource is acceptable for tests.

---

## Checklist

- [ ] `BackendApplicationTests` present
- [ ] `@SpringBootTest` + `contextLoads`
- [ ] `./mvnw test` passes
- [ ] Datasource issue resolved if present

---

## Link to prior context

Overview: [overview.md](./overview.md).  
Next: [manual-test-matrix.md](./manual-test-matrix.md) — feature-by-feature manual checks.
