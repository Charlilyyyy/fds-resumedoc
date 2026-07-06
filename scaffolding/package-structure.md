# Package Structure

## Goal

Create the Java package tree under `com.resume.analyzer` with `BackendApplication` at the root and empty layer folders ready for later classes. At scaffolding time **no controller or service classes are required** — only folders and the main class.

Aligns with [layers-and-packages.md](../architecture/layers-and-packages.md).

---

## Directory tree (scaffolding state)

```
src/
├── main/
│   ├── java/com/resume/analyzer/
│   │   ├── BackendApplication.java
│   │   ├── controller/          ← empty (no .java yet)
│   │   ├── service/             ← empty
│   │   ├── model/               ← optional, empty
│   │   └── repository/          ← optional, empty
│   └── resources/
│       ├── application.properties
│       └── static/              ← empty (no index.html yet)
└── test/
    └── java/com/resume/analyzer/
        └── BackendApplicationTests.java
```

---

## Base package rule

| Rule | Value |
|------|-------|
| Root package | `com.resume.analyzer` |
| Path segment | `src/main/java/com/resume/analyzer/` |
| Main class package | `com.resume.analyzer` (same as root, not a subpackage) |
| Layer packages | `com.resume.analyzer.controller`, `.service`, `.model`, `.repository` |

Spring Boot component scan starts at `BackendApplication` package and scans **all subpackages** automatically.

---

## `BackendApplication.java`

Location: `src/main/java/com/resume/analyzer/BackendApplication.java`

```java
package com.resume.analyzer;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class BackendApplication {

    public static void main(String[] args) {
        SpringApplication.run(BackendApplication.class, args);
    }
}
```

| Annotation | Effect |
|------------|--------|
| `@SpringBootApplication` | Enables auto-config, component scan, configuration |

Optional startup log (reference style):

```java
System.out.println("Application Has Started");
```

---

## Layer packages (empty at scaffold)

Create folders so later segments only add files — no package refactoring.

### `controller/`

| Future class | Role |
|--------------|------|
| `ResumeController` | `POST /resume/upload` |

**Scaffold:** folder exists; zero `.java` files or a `.gitkeep` only.

### `service/`

| Future class | Role |
|--------------|------|
| `ResumeService` | PDF + analysis pipeline |

**Scaffold:** empty folder.

### `model/` (optional)

| Future class | Role |
|--------------|------|
| `Resume` | JPA entity (`id`, `fileName`) |

Can remain empty until persistence segment; creating the folder now documents intent.

### `repository/` (optional)

| Future interface | Role |
|------------------|------|
| `ResumeRepository` | `JpaRepository<Resume, Long>` |

**Scaffold:** empty folder.

---

## Package ↔ layer mapping

| Package | Layer | Scaffold content |
|---------|-------|-------------------|
| `com.resume.analyzer` | Application entry | `BackendApplication` |
| `.controller` | Web | Empty |
| `.service` | Business | Empty |
| `.model` | Data (entity) | Empty |
| `.repository` | Data (access) | Empty |

Presentation layer (`static/`) is not a Java package — it lives under `resources/static/`.

---

## Resources layout

```
src/main/resources/
├── application.properties    # Required at scaffold
└── static/                   # Empty folder for future index.html
```

| Path | Scaffold state |
|------|----------------|
| `application.properties` | Server name, port, optional DB keys |
| `static/` | Exists but no `index.html` yet |

Spring Boot serves `static/` at web root when files are added later.

---

## Test package

**Important:** Test class must use the **same base package** as the main app so `@SpringBootTest` loads the correct context.

Location: `src/test/java/com/resume/analyzer/BackendApplicationTests.java`

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

| Anti-pattern | Fix |
|--------------|-----|
| Test in `com.example.demo` | Move to `com.resume.analyzer` |
| Test class name mismatch | Keep `BackendApplicationTests` convention |

Run:

```bash
./mvnw test
```

Context load should pass with empty layer packages.

---

## IDE package creation

### IntelliJ

1. Right-click `com.resume.analyzer` → **New → Package**
2. Name: `controller`, then repeat for `service`, `model`, `repository`

### Eclipse

1. Right-click `src/main/java` → **New → Package**
2. Name: `com.resume.analyzer.controller` (full name)

### Manual

```bash
mkdir -p src/main/java/com/resume/analyzer/{controller,service,model,repository}
mkdir -p src/main/resources/static
mkdir -p src/test/java/com/resume/analyzer
```

---

## Component scan preview (later)

When classes are added, Spring picks them up without extra config:

| Class | Annotation | Package |
|-------|------------|---------|
| `ResumeController` | `@RestController` | `controller` |
| `ResumeService` | `@Service` | `service` |
| `ResumeRepository` | extends `JpaRepository` | `repository` |

All must stay under `com.resume.analyzer.*`.

---

## Scaffolding checklist

- [ ] `BackendApplication.java` in `com.resume.analyzer`
- [ ] Packages `controller`, `service` exist (minimum)
- [ ] Packages `model`, `repository` exist (if using JPA scaffold)
- [ ] `src/main/resources/application.properties` present
- [ ] `src/main/resources/static/` folder exists
- [ ] `BackendApplicationTests` in `com.resume.analyzer` test package
- [ ] No business classes in controller/service yet

---

## What gets added in later segments

| Segment | Package | New file |
|---------|---------|----------|
| REST API | `controller` | `ResumeController.java` |
| REST API | `service` | `ResumeService.java` (stub) |
| Optional JPA | `model`, `repository` | `Resume.java`, `ResumeRepository.java` |
| Frontend | `resources/static` | `index.html` |

---

## Link to prior context

Dependencies: [dependencies.md](./dependencies.md).  
Next: [configuration.md](./configuration.md) — `application.properties`, port 8080, optional MySQL.
