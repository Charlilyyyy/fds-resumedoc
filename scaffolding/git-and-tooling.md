# Git & Tooling

## Purpose

Set up version control ignores, Maven wrapper usage, and IDE import so the ResumeDoc project stays clean in Git and builds consistently across machines.

Scaffolding deliverable includes: **`.gitignore`**, working **`mvnw`**, and verified IDE project import.

---

## Maven wrapper

### Files required

```
project-root/
├── mvnw
├── mvnw.cmd
└── .mvn/wrapper/
    ├── maven-wrapper.properties
    └── maven-wrapper.jar    ← binary; often gitignored
```

### Why use the wrapper

| Benefit | Detail |
|---------|--------|
| No global Maven install | `./mvnw` downloads correct Maven version |
| CI-friendly | Same build command everywhere |
| Team consistency | Matches Spring Initializr output |

### Common commands

```bash
chmod +x mvnw                    # macOS/Linux once

./mvnw clean compile             # compile
./mvnw test                      # run tests
./mvnw spring-boot:run           # start app
./mvnw clean package             # build JAR
```

Windows:

```cmd
mvnw.cmd spring-boot:run
```

### `maven-wrapper.properties` (reference)

```properties
wrapperVersion=3.3.2
distributionType=only-script
distributionUrl=https://repo.maven.apache.org/maven2/org/apache/maven/apache-maven/3.9.9/apache-maven-3.9.9-bin.zip
```

Versions may differ by generator; do not change unless upgrading wrapper intentionally.

---

## `.gitignore`

Create at project root. Based on Spring Boot + common IDEs.

```gitignore
HELP.md
target/
.mvn/wrapper/maven-wrapper.jar
!**/src/main/**/target/
!**/src/test/**/target/

### STS ###
.apt_generated
.classpath
.factorypath
.project
.settings
.springBeans
.sts4-cache

### IntelliJ IDEA ###
.idea
*.iws
*.iml
*.ipr

### NetBeans ###
/nbproject/private/
/nbbuild/
/dist/
/nbdist/
/.nb-gradle/
build/
!**/src/main/**/build/
!**/src/test/**/build/

### VS Code ###
.vscode/

### Local secrets & OS ###
.env
*.log
.DS_Store
```

### What must never be committed

| Pattern | Reason |
|---------|--------|
| `target/` | Maven build output |
| `.idea/`, `*.iml` | IDE-specific settings |
| `.env` | Local passwords, API keys |
| `application-local.properties` | If used for personal DB passwords |

### What should be committed

| Path | Reason |
|------|--------|
| `pom.xml` | Dependency definition |
| `mvnw`, `mvnw.cmd` | Reproducible builds |
| `.mvn/wrapper/maven-wrapper.properties` | Wrapper config |
| `src/` | Source code |
| `.gitignore` | Ignore rules |

---

## `.gitattributes` (optional)

Normalizes line endings for cross-platform teams:

```gitattributes
/mvnw text eol=lf
*.cmd text eol=crlf
```

Add if collaborators use Windows and macOS/Linux together.

---

## Initial Git setup

From project root (code repo, not necessarily `fds-resumedoc` docs repo):

```bash
git init
git add .
git status                       # verify target/ not staged
git commit -m "init: Spring Boot scaffold for ResumeDoc"
```

Before first `git add`, confirm:

```bash
git check-ignore -v target/
```

Should show a rule matching `target/`.

---

## IntelliJ IDEA import

1. **File → Open** → select folder containing `pom.xml`
2. Trust project if prompted
3. Wait for Maven import (bottom progress bar)
4. **File → Project Structure → Project SDK** → Java 17
5. Enable annotation processing: **Settings → Build → Compiler → Annotation Processors → Enable** (for Lombok)
6. Install Lombok plugin if IDE does not recognize annotations
7. Run `BackendApplication` via green gutter icon

### Run configuration

| Field | Value |
|-------|-------|
| Main class | `com.resume.analyzer.BackendApplication` |
| Module | `backend` (or artifact name) |
| JRE | 17 |

---

## Eclipse / STS import

1. **File → Import → Maven → Existing Maven Projects**
2. Select project root
3. Finish; wait for classpath build
4. **Project → Properties → Java Compiler** → compliance 17
5. **Run As → Spring Boot App** on `BackendApplication`

---

## VS Code (optional)

1. Install **Extension Pack for Java**
2. Open folder with `pom.xml`
3. Trust Maven project
4. Run main from `BackendApplication.java`

`.vscode/` is gitignored; each developer configures locally or uses shared `extensions.json` in team repos (optional).

---

## JDK setup

| Check | Command |
|-------|---------|
| Java version | `java -version` → 17.x |
| JAVA_HOME | Points to JDK 17 install |

IntelliJ and Maven must use the same JDK 17.

---

## Tooling checklist

- [ ] `mvnw` executable on Unix
- [ ] `./mvnw clean compile` succeeds
- [ ] `.gitignore` present; `target/` ignored
- [ ] IDE opens project without dependency errors
- [ ] Lombok annotation processing enabled (if entity added later)
- [ ] No secrets in staged files (`git diff --cached`)

---

## Troubleshooting

| Issue | Fix |
|-------|-----|
| `mvnw: permission denied` | `chmod +x mvnw` |
| IDE shows red `pom.xml` | Reimport Maven project |
| Lombok getters not found | Enable annotation processing + Lombok plugin |
| Wrapper JAR missing | Regenerate from Initializr or run `mvn wrapper:wrapper` if Maven installed |

---

## Link to prior context

Configuration: [configuration.md](./configuration.md).  
Next: [verify-and-wrapup.md](./verify-and-wrapup.md) — boot verification on `localhost:8080` and segment close.
