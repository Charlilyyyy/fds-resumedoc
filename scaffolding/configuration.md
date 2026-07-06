# Configuration

## Purpose

Configure `src/main/resources/application.properties` so ResumeDoc boots on **port 8080** with a clear application name and optional MySQL/JPA settings. At scaffolding time no custom business properties are required.

---

## File location

```
src/main/resources/application.properties
```

Spring Boot loads this file automatically at startup. YAML (`application.yml`) is an alternative — properties format is used here to match the reference layout.

---

## Minimum configuration (boot-first)

Use this when you want the app to start **without MySQL running** and JPA on the classpath:

```properties
spring.application.name=backend

server.port=8080
```

| Property | Value | Purpose |
|----------|-------|---------|
| `spring.application.name` | `backend` | Service name in logs/actuator |
| `server.port` | `8080` | HTTP port (default anyway; explicit is clearer) |

If JPA auto-configuration fails without a database, see **Boot without MySQL** below.

---

## Full configuration (with MySQL + JPA)

When MySQL is installed and `resume_db` exists:

```properties
spring.application.name=backend

server.port=8080

# --- DataSource (MySQL) ---
spring.datasource.url=jdbc:mysql://localhost:3306/resume_db
spring.datasource.username=root
spring.datasource.password=root

# --- JPA / Hibernate ---
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```

### Property reference

| Property | Description |
|----------|-------------|
| `spring.datasource.url` | JDBC URL; database `resume_db` must exist |
| `spring.datasource.username` | DB user (local dev often `root`) |
| `spring.datasource.password` | DB password — **do not commit production secrets** |
| `spring.jpa.hibernate.ddl-auto` | `update` creates/updates tables from entities |
| `spring.jpa.show-sql` | Log SQL in console (dev only) |

### Create database (one-time)

```sql
CREATE DATABASE IF NOT EXISTS resume_db;
```

Run in MySQL client before first boot with full JPA config.

---

## Boot without MySQL

`spring-boot-starter-data-jpa` on the classpath triggers datasource auto-config. If MySQL is not running, startup may fail with connection errors.

### Option A — Use minimum config only (recommended for first smoke test)

Temporarily **comment out** datasource and JPA lines; keep only:

```properties
spring.application.name=backend
server.port=8080
```

Add MySQL block back when DB is ready.

### Option B — Exclude datasource auto-config

On `BackendApplication`:

```java
@SpringBootApplication(exclude = {
    org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration.class,
    org.springframework.boot.autoconfigure.orm.jpa.HibernateJpaAutoConfiguration.class
})
public class BackendApplication { ... }
```

Use when JPA is on classpath but DB is intentionally disabled for v1 demo.

### Option C — H2 for local-only (not default stack)

H2 is listed as out-of-scope in dependencies doc; prefer Option A or B for ResumeDoc.

---

## Server & Tomcat

| Setting | Default | ResumeDoc target |
|---------|---------|------------------|
| `server.port` | 8080 | 8080 |
| Context path | `/` | `/` |
| Static resources | `/static` classpath | Ready for future `index.html` |

Verify after boot:

```text
http://localhost:8080/
```

Empty static folder may return 404 until `index.html` is added — **Tomcat started** in logs is the scaffold success signal.

---

## Multipart upload (future)

When `ResumeController` is added, default Spring multipart limits usually suffice for PDF resumes. Optional explicit settings:

```properties
# Optional — add when implementing upload endpoint
spring.servlet.multipart.enabled=true
spring.servlet.multipart.max-file-size=10MB
spring.servlet.multipart.max-request-size=10MB
```

Not required for empty scaffold boot.

---

## Actuator (optional exposure)

Actuator is on the classpath via `spring-boot-starter-actuator`. Defaults expose limited endpoints.

Dev-only health check:

```properties
management.endpoints.web.exposure.include=health
management.endpoint.health.show-details=when-authorized
```

Test: `http://localhost:8080/actuator/health`

---

## Logging (optional)

```properties
logging.level.com.resume.analyzer=DEBUG
logging.level.org.springframework.web=INFO
```

Useful when debugging controller/service in later segments.

---

## Profiles (optional pattern)

| File | Use |
|------|-----|
| `application.properties` | Shared defaults |
| `application-dev.properties` | Local MySQL credentials |
| `application-prod.properties` | Deploy settings (future) |

Activate:

```bash
./mvnw spring-boot:run -Dspring-boot.run.profiles=dev
```

Or in IDE run configuration: **Active profiles** = `dev`.

---

## Secrets hygiene

| Do | Don't |
|----|-------|
| Use env vars for prod passwords | Commit real production credentials |
| Keep `root`/`root` as local-only example | Push `.env` with secrets to public repos |
| Document placeholder values | Hardcode API keys in properties |

Example override via environment:

```properties
spring.datasource.password=${DB_PASSWORD:root}
```

---

## Configuration checklist

- [ ] `application.properties` in `src/main/resources/`
- [ ] `spring.application.name` set
- [ ] `server.port=8080` (explicit or default)
- [ ] MySQL block added only when DB is available OR autoconfig excluded
- [ ] No resume-specific API keys required at scaffold

---

## Expected startup log signals

Successful scaffold boot shows lines similar to:

```text
Tomcat started on port 8080 (http)
Started BackendApplication in X.XXX seconds
```

Failure signals:

| Log / error | Likely fix |
|-------------|------------|
| `Communications link failure` | MySQL not running — use boot-without-MySQL option |
| `Port 8080 was already in use` | Stop other process or change `server.port` |
| `java.version` mismatch | Use JDK 17 |

---

## Link to prior context

Package layout: [package-structure.md](./package-structure.md).  
Next: [git-and-tooling.md](./git-and-tooling.md) — `.gitignore`, Maven wrapper, IDE import.
