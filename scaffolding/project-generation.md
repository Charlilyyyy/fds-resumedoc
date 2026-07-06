# Project Generation

## Goal

Create a new Maven + Spring Boot 3 project with base package `com.resume.analyzer` and main class `BackendApplication`. At this step the app only needs to compile and boot — no resume logic yet.

Three supported paths below. Pick one; all must produce the same structural outcome described in [overview.md](./overview.md).

---

## Path A — Spring Initializr (recommended for speed)

### Step 1 — Open generator

Go to [https://start.spring.io/](https://start.spring.io/)

### Step 2 — Project metadata

| Field | Value |
|-------|-------|
| Project | Maven |
| Language | Java |
| Spring Boot | 3.5.x (or latest stable 3.x) |
| Group | `com.resume.analyzer` |
| Artifact | `backend` (or `resumedoc-backend`) |
| Name | `backend` |
| Description | Resume feedback API (optional) |
| Package name | `com.resume.analyzer` |
| Packaging | Jar |
| Java | 17 |

### Step 3 — Dependencies to select in Initializr UI

| Dependency | Initializr label |
|------------|------------------|
| Spring Web | `Spring Web` |
| Spring Data JPA | `Spring Data JPA` |
| Validation | `Validation` |
| Spring Boot Actuator | `Actuator` |
| MySQL Driver | `MySQL Driver` |
| Lombok | `Lombok` |
| Spring Boot DevTools | `Developer Tools` |

**Not in Initializr UI — add manually to `pom.xml` after unzip:**

- `org.apache.pdfbox:pdfbox:2.0.30`

(See [dependencies.md](./dependencies.md).)

### Step 4 — Generate and unzip

1. Click **Generate**
2. Unzip to your workspace directory
3. Open terminal in project root

### Step 5 — Rename main class (if needed)

Initializr may create `BackendApplication` automatically from artifact name. If it created a different name (e.g. `BackendApplication` vs `Application`), rename to:

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

### Step 6 — Post-Initializr checklist

- [ ] Add PDFBox dependency to `pom.xml`
- [ ] Create empty packages: `controller`, `service`, `model`, `repository`
- [ ] Create empty `src/main/resources/static/` folder
- [ ] Proceed to [configuration.md](./configuration.md)

---

## Path B — IntelliJ IDEA

### Step 1 — New project

1. **File → New → Project**
2. Select **Spring Initializr** (left sidebar)
3. Click **Next**

### Step 2 — Metadata

Match Path A table: Group `com.resume.analyzer`, Java 17, Jar, Maven.

### Step 3 — Dependencies

Add the same starters as Path A (Web, JPA, Validation, Actuator, MySQL, Lombok, DevTools).

### Step 4 — Finish and adjust

1. Wait for Maven import/index
2. Add PDFBox to `pom.xml`
3. Under `src/main/java/com/resume/analyzer/`, create packages:
   - `controller`
   - `service`
   - `model`
   - `repository`
4. Ensure `BackendApplication.java` exists at package root

### Step 5 — Run configuration

1. Right-click `BackendApplication` → **Run**
2. Confirm console shows Tomcat started on port 8080 (after configuration)

---

## Path C — Eclipse / Spring Tools Suite

### Step 1 — New Spring Starter Project

1. **File → New → Spring Starter Project**
2. Name: `backend`
3. Type: **Maven**, Java 17
4. Group: `com.resume.analyzer`, Artifact: `backend`

### Step 2 — Dependencies tab

Select: Web, JPA, Validation, Actuator, MySQL, Lombok, DevTools.

### Step 3 — Finish

1. Add PDFBox to `pom.xml`
2. Create layer packages under `com.resume.analyzer`
3. **Run As → Spring Boot App** on `BackendApplication`

---

## Path D — Manual from empty folder

Use when not using generators.

### Step 1 — Create root structure

```bash
mkdir -p backend/src/main/java/com/resume/analyzer
mkdir -p backend/src/main/resources/static
mkdir -p backend/src/test/java/com/resume/analyzer
mkdir -p backend/.mvn/wrapper
```

### Step 2 — Add Maven wrapper

Copy `mvnw`, `mvnw.cmd`, and `.mvn/wrapper/` from an existing Spring Boot project, or generate once via Initializr and reuse wrapper files.

### Step 3 — Write `pom.xml`

Full dependency list in [dependencies.md](./dependencies.md). Minimum parent:

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>3.5.6</version>
    <relativePath/>
</parent>

<groupId>com.resume.analyzer</groupId>
<artifactId>backend</artifactId>
<version>0.0.1-SNAPSHOT</version>

<properties>
    <java.version>17</java.version>
</properties>
```

### Step 4 — Add `BackendApplication.java`

Same source as Path A Step 5.

### Step 5 — Add minimal test class

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

### Step 6 — First compile

```bash
cd backend
./mvnw clean compile
```

Fix any JDK or dependency errors before continuing.

---

## Coordinates reference

| Maven / Java | Value |
|--------------|-------|
| `groupId` | `com.resume.analyzer` |
| `artifactId` | `backend` (flexible) |
| `version` | `0.0.1-SNAPSHOT` |
| Base package | `com.resume.analyzer` |
| Main class | `com.resume.analyzer.BackendApplication` |

Using a consistent `groupId` matching the package avoids confusion when adding controllers and services later.

---

## What not to add yet

| Item | When |
|------|------|
| `ResumeController` | Next implementation segment |
| `ResumeService` | After controller stub |
| `index.html` | Frontend segment |
| `Resume` entity | Optional — can scaffold empty package now, class later |
| Analysis logic | Requirements segments 5–10 |

---

## Verification after generation

| Check | Command / action |
|-------|------------------|
| Project structure exists | Compare to [overview.md](./overview.md) layout |
| Compiles | `./mvnw clean compile` |
| Main class present | `BackendApplication.java` in correct package |
| Layer folders exist | `controller`, `service` at minimum |

Full boot verification: [verify-and-wrapup.md](./verify-and-wrapup.md).

---

## Common issues

| Problem | Fix |
|---------|-----|
| Wrong Java version | Set IDE project SDK and `java.version` to 17 |
| Package not `com.resume.analyzer` | Refactor package in IDE or regenerate |
| Initializr nested package | Ensure package name field is `com.resume.analyzer`, not `com.resume.analyzer.backend` |
| Maven not found | Use `./mvnw` in project root |

---

## Link to prior context

Next: [dependencies.md](./dependencies.md) — complete `pom.xml` entries, versions, and plugin configuration.
