# ClinicCare Mini EMR

### Problem: Clinics record diagnosis code and treatment notes for every consultation.

### The goal is to create a minimal, secure, and intuitive tool to manage these notes.
#### Goal: Build a small ClinicCare web app that allows doctors to:

bash```
1. Search for ICD-10 diagnostic codes (use any 100 codes from site
https://www.icd10data.com/ICD10CM/Codes and add into the sql file to
populate database’s table).

2. Record a simple patient consultation note with selected diagnosis codes.

3. List past consultation notes.
```

bash```
- Project Requirements :
    Backend – using FastAPI
        1. Endpoints :
            a. GET /diagnosis?search=<term> → Search diagnosis codes (from local table or static dataset).
            b. POST /consultation → Save a consultation note.
            c. GET /consultation → List all consultation notes.
        2. Data :
            a. Use a lightweight SQL DB (SQLite/PostgreSQL)
        3. Expected Features
            a. Input validation with Pydantic.
            b. Basic error handling.
    Frontend – using either Vue 3 or Nuxt 3
        a. A page to for Consultations list: Table of past consultations
        b. A page for New Consultation Form.
    Optional:
        • Add JWT authentication for doctors and a login page to login
```