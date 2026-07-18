# Run Guide

## Purpose

Reproducible steps to build, run, and use the application from a clean checkout.

Consolidates [scaffolding/](../scaffolding/) setup docs.

---

## Prerequisites

| Tool | Version | Check |
|------|---------|-------|
| JDK | 17+ | `java -version` |
| Maven | 3.9+ (or use wrapper) | `./mvnw -v` |
| Git | any | `git --version` |
| Browser | modern | Chrome/Firefox/Edge |

MySQL is **not** required for v1 analysis (see [configuration.md](../scaffolding/configuration.md)).

---

## Build

```bash
git clone <repo-url>
cd <project>
./mvnw clean compile
```

---

## Run

```bash
./mvnw spring-boot:run
```

Or run the packaged jar:

```bash
./mvnw clean package
java -jar target/*.jar
```

Application starts on `http://localhost:8080`.

---

## Use (browser)

1. Open `http://localhost:8080/`.
2. Choose a PDF resume.
3. Select a role: Java Developer, Full Stack Developer, or Data Analyst.
4. Click **Analyze Now**.
5. View the dashboard: score, skills, ATS, sections, roadmap, insight.

---

## Use (API)

```bash
curl -X POST "http://localhost:8080/resume/upload" \
  -F "file=@resume.pdf" \
  -F "role=Java Developer"
```

Returns the labeled analysis text. See [api-testing.md](../testing/api-testing.md).

---

## Troubleshooting

| Symptom | Likely cause | Fix |
|---------|--------------|-----|
| Port 8080 in use | Another process | Change `server.port` or free the port |
| Context fails on startup | JPA/MySQL with no DB | Exclude datasource or start MySQL ([configuration.md](../scaffolding/configuration.md)) |
| `Error while processing resume ❌` | Non-PDF/corrupt/encrypted file | Upload a valid text-based PDF |
| Blank dashboard | Error body parsed | Check the response text / server logs |
| Charts stacked | Old build | Ensure `destroy()` guard present ([charts-and-cards.md](../frontend/charts-and-cards.md)) |

---

## Verify install

```bash
./mvnw test
```

Expect `contextLoads` to pass. See [context-test.md](../testing/context-test.md).

---

## Checklist

- [ ] Prerequisites installed
- [ ] Builds from clean clone
- [ ] Runs on 8080
- [ ] Browser flow works
- [ ] API call works
- [ ] Troubleshooting covered

---

## Link to prior context

How it works: [how-it-works.md](./how-it-works.md).  
Next: [future-and-credits.md](./future-and-credits.md) — roadmap and acknowledgements.
