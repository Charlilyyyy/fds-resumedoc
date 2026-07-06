# Scaffolding Overview

## Purpose

Architecture in `architecture/` defines **how** ResumeDoc is structured. This `scaffolding/` folder documents **how to create the empty runnable project** before any resume analysis logic is written.

The goal at the end of this segment: a Spring Boot application that **starts cleanly** on port 8080 with the correct packages, dependencies, and configuration — but no upload endpoint or PDF processing yet (those come in later implementation segments).

---

## What “scaffolding” means here

| In scope | Out of scope (later segments) |
|----------|-------------------------------|
| Generate or create Spring Boot project | `ResumeController` upload endpoint |
| `pom.xml` with required dependencies | `ResumeService` analysis logic |
| Package folders under `com.resume.analyzer` | Static dashboard UI |
| `BackendApplication` main class | PDF text extraction |
| `application.properties` baseline | Postman upload tests with real PDF |
| `.gitignore`, Maven wrapper | Feature acceptance testing |

Deliverable: **empty Spring Boot app that boots without errors.**

---

## Prerequisites

| Requirement | Version / notes |
|-------------|-----------------|
| JDK | 17 (LTS) |
| IDE | IntelliJ IDEA or Eclipse (optional but recommended) |
| Maven | Via project `mvnw` wrapper — global install not required |
| MySQL | Optional for JPA scaffold; core boot does not require DB running for first smoke test* |
| Git | For version control and `.gitignore` |

\*If JPA auto-config fails without MySQL, use a dev profile or exclude datasource autoconfig for initial boot — detailed in `configuration.md`.

---

## Scaffolding workflow (high level)

```
Choose creation path  →  Set coordinates & dependencies  →  Create packages
        →  Configure properties  →  Add .gitignore  →  Run & verify boot
```

### Creation paths

| Path | When to use |
|------|-------------|
| [Spring Initializr](https://start.spring.io/) | Quick download of starter ZIP |
| IDE wizard (IntelliJ / Eclipse) | Integrated project creation |
| Manual `pom.xml` + folders | Full control; matches reference layout |

All paths must converge on the same package name, dependencies, and folder layout documented in this segment.

---

## Target project coordinates

| Property | Value |
|----------|-------|
| Base package | `com.resume.analyzer` |
| Main class | `BackendApplication` |
| Java version | 17 |
| Spring Boot | 3.x |
| Build tool | Maven |
| Default port | 8080 |

Artifact name can be `backend` or portfolio-specific; package name matters more for code organization.

---

## Target directory layout (preview)

```
project-root/
├── .mvn/
│   └── wrapper/
├── src/
│   ├── main/
│   │   ├── java/com/resume/analyzer/
│   │   │   ├── BackendApplication.java
│   │   │   ├── controller/      (empty or .gitkeep)
│   │   │   ├── service/         (empty)
│   │   │   ├── model/           (optional)
│   │   │   └── repository/      (optional)
│   │   └── resources/
│   │       ├── application.properties
│   │       └── static/          (empty for now)
│   └── test/java/
│       └── .../BackendApplicationTests.java
├── .gitignore
├── mvnw
├── mvnw.cmd
└── pom.xml
```

Full package rules: `package-structure.md`.

---

## Required dependencies (preview)

| Dependency | Purpose |
|------------|---------|
| `spring-boot-starter-web` | Embedded Tomcat + Spring MVC |
| `spring-boot-starter-validation` | Future request validation |
| `spring-boot-starter-actuator` | Health endpoints |
| `spring-boot-starter-data-jpa` | Optional persistence scaffold |
| `pdfbox` 2.0.30 | PDF parsing (added now, used later) |
| `lombok` | Entity boilerplate reduction |
| `mysql-connector-j` | JDBC driver when DB enabled |
| `spring-boot-devtools` | Dev reload |
| `spring-boot-starter-test` | Context load test |

Full POM detail: `dependencies.md`.

---

## Scaffolding documents (planned)

| Document | Contents |
|----------|----------|
| [overview.md](./overview.md) | This file — scope, workflow, layout preview |
| project-generation.md | Initializr / IDE / manual creation steps |
| dependencies.md | Complete `pom.xml` dependency guide |
| package-structure.md | Packages, main class, empty layer folders |
| configuration.md | `application.properties`, port, optional DB |
| git-and-tooling.md | `.gitignore`, Maven wrapper, IDE import |
| verify-and-wrapup.md | Boot commands, smoke checks, segment close |

This segment is split into **7 commits** (one document per commit).

---

## Success criteria for this segment

Scaffolding is complete when:

- [ ] Project compiles with `./mvnw clean compile`
- [ ] `BackendApplication` starts without fatal errors
- [ ] App listens on `http://localhost:8080`
- [ ] Package tree matches `com.resume.analyzer` with controller/service/(model/repository)
- [ ] All listed dependencies resolve in `pom.xml`
- [ ] `.gitignore` excludes `target/`, IDE files, and local secrets
- [ ] Context load test passes (or is created empty)

---

## Traceability

| Prior doc | Scaffolding response |
|-----------|---------------------|
| [tech-stack.md](../architecture/tech-stack.md) | Dependency list and versions |
| [layers-and-packages.md](../architecture/layers-and-packages.md) | Folder layout under `com.resume.analyzer` |
| [summary-wrapup.md](../architecture/summary-wrapup.md) | Readiness gate before this segment |

---

## What comes after scaffolding

Once the empty app boots:

1. **REST API & controller layer** — upload endpoint with stub service  
2. **PDF upload & text extraction** — PDFBox in service  
3. Further analysis features and dashboard UI per requirements  

---

## Next document

**project-generation.md** — step-by-step project creation via Spring Initializr, IDE wizard, and manual setup checklist.
