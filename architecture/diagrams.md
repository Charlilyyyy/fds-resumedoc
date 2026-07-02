# Architecture Diagrams

## Purpose

Visual reference for ResumeDoc v1. Use alongside [overview.md](./overview.md), [layers-and-packages.md](./layers-and-packages.md), and [request-flow.md](./request-flow.md).

API field details remain in [api-contract.md](../requirements/api-contract.md).

---

## 1. System context (C4-style)

Who interacts with the system and what external systems exist.

```mermaid
flowchart LR
    subgraph users [Users]
        JS[Job seeker browser]
    end

    subgraph resumedoc [ResumeDoc System]
        APP[Spring Boot monolith]
    end

    subgraph external [External - optional]
        CDN[Chart.js CDN]
        DB[(MySQL)]
    end

    JS <-->|HTTP :8080| APP
    JS -.->|script load| CDN
    APP -.->|JPA optional| DB
```

| Actor / system | Relationship |
|----------------|--------------|
| Job seeker | Uploads PDF, reads dashboard |
| Spring Boot app | Hosts UI + API + analysis |
| Chart.js CDN | Client-side charts only |
| MySQL | Optional; not on v1 hot path |

---

## 2. Container view (single deployable)

Everything that ships in one JAR/process.

```mermaid
flowchart TB
    subgraph jar [Spring Boot JAR :8080]
        STATIC[static/index.html]
        TOMCAT[Embedded Tomcat]
        CTRL[ResumeController]
        SVC[ResumeService]
        PDFB[PDFBox library]
        JPA[JPA + Repository scaffold]
    end

    STATIC --> TOMCAT
    TOMCAT --> CTRL
    CTRL --> SVC
    SVC --> PDFB
    SVC -.-> JPA
```

---

## 3. Application layers

```mermaid
flowchart TB
    subgraph presentation [Presentation Layer]
        HTML[HTML structure]
        CSS[CSS theme]
        JS[JavaScript + fetch]
        CHART[Chart.js]
    end

    subgraph web [Web Layer]
        RC[ResumeController]
    end

    subgraph business [Business Layer]
        RS[ResumeService]
        subgraph helpers [Private methods]
            SK[extractSkills]
            SC[calculateScore]
            SG[getSuggestions]
            JM[matchJobRole]
            AT[checkATSScore]
            SEC[detectSections]
            RM[getJobSuggestions]
        end
    end

    subgraph data [Data Layer - optional]
        REPO[ResumeRepository]
        MODEL[Resume entity]
    end

    JS -->|POST multipart| RC
    RC --> RS
    RS --> SK
    RS --> SC
    RS --> SG
    RS --> JM
    RS --> AT
    RS --> SEC
    RS --> RM
    REPO --> MODEL
```

---

## 4. Analysis pipeline (service internals)

Order of execution inside `processResume`:

```mermaid
flowchart TD
    START([MultipartFile + role]) --> OPEN[PDFBox: load PDF]
    OPEN --> TEXT[Extract plain text]
    TEXT --> SK[extractSkills]
    SK --> SC[calculateScore]
    SK --> SG[getSuggestions]
    SK --> JM[matchJobRole]
    TEXT --> AT[checkATSScore]
    TEXT --> SEC[detectSections]
    SK --> RM[getJobSuggestions]
    SC --> BUILD[Concatenate response string]
    SG --> BUILD
    JM --> BUILD
    AT --> BUILD
    SEC --> BUILD
    RM --> BUILD
    BUILD --> OUT([Plain-text HTTP body])
    OPEN -->|error| ERR[Error message string]
```

---

## 5. Request/response sequence

Detailed interaction for one analyze action.

```mermaid
sequenceDiagram
    autonumber
    participant U as User
    participant UI as index.html
    participant DS as DispatcherServlet
    participant RC as ResumeController
    participant RS as ResumeService
    participant PB as PDFBox

    U->>UI: Submit PDF + role
    UI->>UI: Loader on, hero off
    UI->>DS: POST /resume/upload
    DS->>RC: uploadResume(file, role)
    RC->>RS: processResume(file, role)
    RS->>PB: load + strip text
    PB-->>RS: resume text
    RS->>RS: skills, score, ATS, sections, match, roadmap
    RS-->>RC: result string
    RC-->>UI: 200 OK
    UI->>UI: Parse regex, render dashboard
    UI-->>U: Score, charts, cards
```

---

## 6. Local deployment topology

```mermaid
flowchart TB
    subgraph devmachine [Developer machine]
        JDK[JDK 17]
        IDE[IntelliJ / Eclipse]
        subgraph process [Process :8080]
            BOOT[BackendApplication]
        end
        subgraph optional [Optional]
            MYSQL[(MySQL :3306 resume_db)]
        end
    end

    BROWSER[Chrome / Firefox]
    POSTMAN[Postman]

    IDE -->|run main| BOOT
    BROWSER <-->|GET / POST /resume/upload| BOOT
    POSTMAN -->|POST /resume/upload| BOOT
    BOOT -.-> MYSQL
```

---

## 7. Static asset & API routing

How Tomcat maps URLs in v1.

| Request | Handler | Result |
|---------|---------|--------|
| `GET /` | ResourceHttpRequestHandler | `static/index.html` |
| `GET /index.html` | Static resource | Same page |
| `POST /resume/upload` | `ResumeController` | Analysis text |
| `GET /actuator/health` | Actuator (if exposed) | Health JSON |

```mermaid
flowchart LR
    REQ[HTTP Request]
    REQ -->|GET /| STATIC[static/]
    REQ -->|POST /resume/upload| API[ResumeController]
```

---

## 8. Data flow — inputs to dashboard fields

```mermaid
flowchart LR
    subgraph inputs [Inputs]
        PDF[PDF file]
        ROLE[role string]
    end

    subgraph backend [Backend outputs in text body]
        SKL[Skills list]
        SCR[Score]
        SUG[Suggestions]
        MAT[Match %]
        ATS[ATS %]
        SEC[Sections]
        RDM[Roadmap]
    end

    subgraph ui [Dashboard cards]
        C1[Score chart]
        C2[Skill tags]
        C3[Suggestions list]
        C4[ATS card]
        C5[Sections card]
        C6[Roadmap card]
        C7[AI insight]
    end

    PDF --> SKL
    PDF --> ATS
    PDF --> SEC
    ROLE --> MAT
    ROLE --> RDM
    SKL --> SCR
    SKL --> SUG
    SKL --> MAT

    SCR --> C1
    SCR --> C7
    SKL --> C2
    SUG --> C3
    ATS --> C4
    SEC --> C5
    RDM --> C6
```

---

## 9. Optional persistence (future path)

Dotted path not active in reference v1 `processResume`:

```mermaid
flowchart LR
    SVC[ResumeService]
    REPO[ResumeRepository]
    DB[(MySQL)]
    ENT[Resume entity]

    SVC -.->|save metadata| REPO
    REPO --> ENT
    REPO --> DB
```

Enable when upload history feature ships ([future-scope.md](../requirements/future-scope.md)).

---

## 10. ASCII — monolith bird's eye

```
                    ┌─────────────────────────────────────┐
                    │         ResumeDoc (one JAR)         │
  Browser ─────────►│  Tomcat                             │
  GET  /            │    └─► static/index.html            │
  POST /resume/upload│    └─► ResumeController            │
                    │           └─► ResumeService         │
                    │                  ├─► PDFBox          │
                    │                  └─► keyword logic   │
                    │  [optional] JPA ──► MySQL           │
                    └─────────────────────────────────────┘
                              ▲
                              │ script tag
                         Chart.js CDN
```

---

## Diagram index

| # | Diagram | Use when |
|---|---------|----------|
| 1 | System context | Explaining scope to non-developers |
| 2 | Container / JAR | Deployment and repo structure |
| 3 | Application layers | Code organization |
| 4 | Analysis pipeline | Implementing or testing service |
| 5 | Sequence | Debugging request lifecycle |
| 6 | Local deployment | Environment setup |
| 7 | URL routing | 404 vs API path issues |
| 8 | Data flow | UI ↔ API field mapping |
| 9 | Optional persistence | Planning history feature |
| 10 | ASCII monolith | README or quick slide |

---

## Link to prior context

Next: **summary-wrapup.md** — consolidated architecture summary and close of this documentation segment.
