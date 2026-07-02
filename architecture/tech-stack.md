# Tech Stack

## Stack at a glance

| Layer | Technology | Version (target) |
|-------|------------|------------------|
| Language | Java | 17 |
| Framework | Spring Boot | 3.x |
| Web | Spring MVC (`spring-boot-starter-web`) | via Boot parent |
| Build | Maven | via `mvnw` wrapper |
| PDF parsing | Apache PDFBox | 2.0.30 |
| Frontend markup | HTML5 | — |
| Frontend style | CSS3 | — |
| Frontend logic | JavaScript (vanilla) | ES5+ |
| Charts | Chart.js | CDN (latest minor) |
| Optional ORM | Spring Data JPA | via Boot starter |
| Optional DB | MySQL | 8.x (`mysql-connector-j`) |
| Dev ergonomics | Lombok, Spring DevTools | optional |
| Health / ops | Spring Actuator | included |
| Validation | Spring Validation | included |
| Testing | JUnit via `spring-boot-starter-test` | test scope |

---

## Backend

### Java 17

| Why chosen | Detail |
|------------|--------|
| LTS baseline | Stable language features for teaching and hiring familiarity |
| Spring Boot 3 requirement | Boot 3.x targets Jakarta EE and Java 17+ |
| Ecosystem | Strong PDF, web, and IDE support |

### Spring Boot 3

| Why chosen | Detail |
|------------|--------|
| Rapid API + static hosting | One process serves REST endpoint and `static/` files |
| Convention over configuration | Auto-config for web, optional JPA, actuator |
| Portfolio standard | Common stack for Java backend demos |

**Parent POM:** `spring-boot-starter-parent` (3.5.x in reference build).

**Entry point:** `@SpringBootApplication` main class (e.g. `BackendApplication`).

### Spring MVC

Delivered by `spring-boot-starter-web`.

| Use in ResumeDoc | Component |
|------------------|-----------|
| REST endpoint | `@RestController`, `@PostMapping` |
| File upload | `@RequestParam MultipartFile` |
| Response | `ResponseEntity<String>` |

No separate Spring WebFlux or reactive stack — synchronous request handling is sufficient for PDF parse + keyword analysis.

### Apache PDFBox 2.0.30

| Why chosen | Detail |
|------------|--------|
| Purpose-built | Industry-standard Java PDF text extraction |
| No external service | Keeps v1 offline-capable on localhost |
| Stable API | `PDDocument.load`, `PDFTextStripper.getText` |

```xml
<dependency>
  <groupId>org.apache.pdfbox</groupId>
  <artifactId>pdfbox</artifactId>
  <version>2.0.30</version>
</dependency>
```

All downstream analysis (skills, ATS, sections) consumes the **plain string** PDFBox returns.

---

## Build & project tooling

### Maven

| Item | Detail |
|------|--------|
| Wrapper | `mvnw` / `mvnw.cmd` for reproducible builds without global Maven install |
| Packaging | Executable JAR via `spring-boot-maven-plugin` |
| Coordinates (reference) | `groupId` + `artifactId` + `0.0.1-SNAPSHOT` |

**Common commands:**

```bash
./mvnw clean package    # compile + test + JAR
./mvnw spring-boot:run  # run dev server
```

### Lombok (optional convenience)

| Use | Scope |
|-----|-------|
| Reduce boilerplate on JPA entities | `provided` |
| Annotation processing | Configured in `maven-compiler-plugin` |

Not required for controller/service logic if written without entities.

### Spring Boot DevTools

| Use | Detail |
|-----|--------|
| Hot reload during dev | `runtime`, `optional` |
| Faster iteration | Restart on classpath change |

---

## Spring starters (backend dependencies)

| Dependency | Role in ResumeDoc |
|------------|------------------|
| `spring-boot-starter-web` | **Required** — HTTP API + embedded Tomcat |
| `spring-boot-starter-validation` | Bean validation if extended later |
| `spring-boot-starter-actuator` | Health/metrics endpoints for ops readiness |
| `spring-boot-starter-data-jpa` | **Optional** — entity/repository scaffold |
| `spring-boot-starter-test` | Context-load and unit tests |
| `mysql-connector-j` | **Optional** — JDBC driver when MySQL enabled |
| `spring-boot-devtools` | Dev-only convenience |

---

## Optional persistence

### Spring Data JPA + MySQL

The reference codebase may include a `Resume` entity and `ResumeRepository`. **v1 analysis does not require persistence** — upload → analyze → respond is stateless.

| Config key (when used) | Example |
|------------------------|---------|
| `spring.datasource.url` | `jdbc:mysql://localhost:3306/resume_db` |
| `spring.datasource.username` | `root` |
| `spring.datasource.password` | (local secret) |
| `spring.jpa.hibernate.ddl-auto` | `update` |
| `spring.jpa.show-sql` | `true` (dev) |

**Architecture decision:** JPA is scaffold-ready; core demo runs even if DB is not configured (may need profile or autoconfig exclusion if MySQL absent — implementation detail for scaffolding segment).

---

## Frontend

### HTML5 + CSS3 + JavaScript

| Property | Choice |
|----------|--------|
| Location | `src/main/resources/static/index.html` |
| Build step | None — no Webpack, Vite, or npm for v1 |
| Served by | Spring Boot default static resource handler at `/` |

**Why no React (v1):** Requirements lock a single static page with Chart.js; React is deferred in [future-scope.md](../requirements/future-scope.md).

### Chart.js

| Property | Detail |
|----------|--------|
| Load | CDN script in `<head>` |
| Charts used | Doughnut (score), bar (skill analysis) |
| Lifecycle | Destroy previous chart instance before redraw on re-submit |

### UI characteristics

- Dark theme, gradient background, glassmorphism-style cards
- Form: file input + role `<select>` + submit button
- Dashboard: grid of cards populated from parsed API text
- Client-side insight messages derived from score bands (not from API)

---

## API & integration surface

| Concern | v1 choice |
|---------|-----------|
| Protocol | HTTP/1.1 |
| Upload format | `multipart/form-data` |
| Response format | Plain text (parseable labels) |
| CORS | Same-origin when UI and API both on `localhost:8080` |
| Auth | None |

Full contract: [api-contract.md](../requirements/api-contract.md).

---

## Development & test tools

| Tool | Purpose |
|------|---------|
| IntelliJ IDEA / Eclipse | Run main class, debug service logic |
| Postman / cURL | Test `POST /resume/upload` without browser |
| Browser DevTools | Network tab, console for fetch/parse issues |
| Git | Version control; `.gitignore` excludes `target/`, IDE files |

---

## Runtime topology (local)

```
Developer machine
├── JDK 17
├── (Optional) MySQL 8 on :3306
└── Spring Boot app on :8080
      ├── Tomcat serves static/index.html  →  GET /
      └── ResumeController                 →  POST /resume/upload
```

| Port | Service |
|------|---------|
| 8080 | ResumeDoc app (default) |
| 3306 | MySQL (optional) |

---

## What we explicitly did not choose (v1)

| Alternative | Why not (v1) |
|-------------|--------------|
| Node.js / Express backend | Requirements align with Java/Spring learning path |
| Python + FastAPI | Same — stack consistency with Spring Boot demo |
| Separate React SPA repo | Static page meets F11; less build complexity |
| OpenAI API | Deterministic keyword logic; no API keys |
| PostgreSQL | MySQL matches reference scaffold; either works for optional JPA |
| Docker | Local IDE run sufficient for portfolio (deploy in future-scope) |

---

## Version pinning strategy

| Category | Strategy |
|----------|----------|
| Spring Boot | Parent BOM manages most Spring artifact versions |
| PDFBox | Explicit `2.0.30` in POM |
| Lombok | Explicit `1.18.30` in reference POM |
| Chart.js | CDN — pin URL to specific version in production deploy |
| Java | `java.version` property `17` |

---

## Link to prior context

Stack choices support requirements F1–F12 and the monolithic style in [overview.md](./overview.md). Next: **layers-and-packages.md** — where each technology sits in the codebase structure.
