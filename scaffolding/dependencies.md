# Dependencies (`pom.xml`)

## Purpose

This document is the authoritative dependency guide for ResumeDoc scaffolding. Every library listed here belongs in `pom.xml` before the first boot — even PDFBox, which is not used until later segments.

Reference stack: [tech-stack.md](../architecture/tech-stack.md).

---

## Parent POM

Spring Boot parent manages versions for most starters.

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>3.5.6</version>
    <relativePath/>
</parent>
```

| Property | Value |
|----------|-------|
| `groupId` | `com.resume.analyzer` |
| `artifactId` | `backend` |
| `version` | `0.0.1-SNAPSHOT` |
| `java.version` | `17` |

```xml
<properties>
    <java.version>17</java.version>
</properties>
```

---

## Dependency checklist

| # | Artifact | Required | Scope | Used when |
|---|----------|----------|-------|-----------|
| 1 | `spring-boot-starter-web` | Yes | compile | Boot, REST API, static files |
| 2 | `spring-boot-starter-validation` | Yes | compile | Future `@Valid` on uploads |
| 3 | `spring-boot-starter-actuator` | Yes | compile | Health/metrics |
| 4 | `spring-boot-starter-data-jpa` | Yes* | compile | Optional entity scaffold |
| 5 | `mysql-connector-j` | Yes* | runtime | JDBC when MySQL enabled |
| 6 | `lombok` | Yes | provided | Entity boilerplate |
| 7 | `spring-boot-devtools` | Yes | runtime, optional | Dev reload |
| 8 | `pdfbox` 2.0.30 | Yes | compile | PDF parsing (later segment) |
| 9 | `spring-boot-starter-test` | Yes | test | Context load test |

\*JPA + MySQL are scaffold dependencies; app can boot without live DB if configured (see [configuration.md](./configuration.md)).

---

## Full `<dependencies>` block

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-validation</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
        <scope>runtime</scope>
        <optional>true</optional>
    </dependency>
    <dependency>
        <groupId>com.mysql</groupId>
        <artifactId>mysql-connector-j</artifactId>
        <scope>runtime</scope>
    </dependency>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.30</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.apache.pdfbox</groupId>
        <artifactId>pdfbox</artifactId>
        <version>2.0.30</version>
    </dependency>
</dependencies>
```

---

## Per-dependency notes

### `spring-boot-starter-web`

| Brings in | Use in ResumeDoc |
|-----------|------------------|
| Spring MVC | `@RestController`, `@PostMapping` |
| Embedded Tomcat | Serves `:8080` |
| Jackson (JSON) | Available if JSON API added later |
| `spring-web` multipart | `MultipartFile` upload |

**Without this:** No HTTP server, no API.

### `spring-boot-starter-validation`

| Use | When |
|-----|------|
| Jakarta Bean Validation | Optional constraints on request DTOs later |

Included at scaffold time to avoid POM churn later.

### `spring-boot-starter-actuator`

| Endpoint (default) | Purpose |
|--------------------|---------|
| `/actuator/health` | Liveness check |

Production exposure rules are out of v1 scope; dependency is included per stack design.

### `spring-boot-starter-data-jpa`

| Brings in | Use |
|-----------|-----|
| Hibernate | ORM |
| Spring Data | `JpaRepository` interfaces |

Core analyze flow does not require DB; entity scaffold is optional learning path.

### `mysql-connector-j`

JDBC driver for MySQL 8+. Version managed by Boot parent BOM.

### `lombok`

| Annotation (future) | On |
|---------------------|-----|
| `@Getter` / `@Setter` | `Resume` entity |

Requires annotation processor config in compiler plugin (below).

### `spring-boot-devtools`

Restart on classpath change during development. Not packaged for production use as a hard dependency.

### `pdfbox` 2.0.30

| Class (later) | Role |
|---------------|------|
| `PDDocument` | Load PDF |
| `PDFTextStripper` | Extract text |

**Pin version explicitly** — not in Spring BOM.

### `spring-boot-starter-test`

| Includes | Use |
|----------|-----|
| JUnit 5 | `@Test` |
| `@SpringBootTest` | Context loads smoke test |

---

## Build plugins

### `spring-boot-maven-plugin`

Produces executable fat JAR; excludes Lombok from repackaged artifact.

```xml
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
    <configuration>
        <excludes>
            <exclude>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
            </exclude>
        </excludes>
    </configuration>
</plugin>
```

### `maven-compiler-plugin` + Lombok

Ensures Lombok annotation processing during compile:

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <configuration>
        <annotationProcessorPaths>
            <path>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
            </path>
        </annotationProcessorPaths>
    </configuration>
</plugin>
```

---

## Verify dependencies resolve

```bash
./mvnw dependency:tree
```

Expected top-level artifacts include:

- `spring-boot-starter-web`
- `spring-boot-starter-data-jpa`
- `pdfbox:jar:2.0.30`

```bash
./mvnw clean compile
```

Build should succeed with only `BackendApplication` and test class — no business code required.

---

## What not to add at scaffold time

| Dependency | Reason to defer |
|------------|-----------------|
| `spring-boot-starter-security` | No auth in v1 |
| OpenAI / HTTP client SDKs | Future-scope |
| React / frontend Maven plugins | Static HTML, no build |
| H2 database | Stack targets MySQL for JPA scaffold |

---

## Transitive highlights (informational)

`spring-boot-starter-web` transitively provides:

- `tomcat-embed-core`
- `spring-webmvc`
- `jackson-databind`

`spring-boot-starter-data-jpa` transitively provides:

- `hibernate-core`
- `spring-data-jpa`

No need to declare these explicitly.

---

## Link to prior context

Project creation steps: [project-generation.md](./project-generation.md).  
Next: [package-structure.md](./package-structure.md) — Java packages and `BackendApplication` placement.
