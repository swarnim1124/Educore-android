# EduCore ERP Backend

![Python](https://img.shields.io/badge/Python-3.12-3776AB?logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?logo=fastapi&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?logo=postgresql&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-ready-2496ED?logo=docker&logoColor=white)
![Architecture](https://img.shields.io/badge/Architecture-Modular%20Monolith-blue)

A modular education ERP backend built with **FastAPI** and **PostgreSQL**. It covers authentication and RBAC, student profiles, curriculum, timetables, attendance, exams and fees. All of it runs behind a **single dynamic-dispatch endpoint** instead of hundreds of hand-registered routes.

Android client: [educore-android](https://github.com/<your-username>/educore-android)

## Screenshots

### Student Dashboard
![Student Dashboard](images/dashboard.png)

### Examination Hall Ticket
![Examination Hall Ticket](images/hall-ticket.png)

### Exams
![Exams](images/exams.png)

### Manage Pinned Modules
![Manage Pinned Modules](images/pinned-modules.png)

## Highlights

- **Dispatcher architecture.** One `POST /` endpoint routes requests to `module → submodule → action` by naming convention. Adding a feature means adding a file, not editing a router.
- **Modular monolith.** 8 domain modules, each with its own manifest, schema, data layer and submodules, loaded dynamically by a module loader.
- **Security built in.** JWT access and refresh tokens, RBAC, MFA, a policy engine and session tracking (`m_AAA`). Production mode refuses to start without a strong JWT secret.
- **Observability.** Every request gets a UUID and a milestone timeline (`REQ001`, `AUTH001`, ...) written as structured JSON logs.
- **Integrations.** Razorpay payments, Twilio and MSG91 SMS, Firebase push, reCAPTCHA, and PDF/Excel/Word document generation.
- **Documented.** Per-module API contracts, engineering standards, ER diagrams and release-certification docs are included.
- **CI/CD.** Dockerfile and Jenkins pipeline included.

## Tech stack

| Area | Tools |
|---|---|
| Language / framework | Python 3.12, FastAPI, Uvicorn, Pydantic v2 |
| Database | PostgreSQL, SQLAlchemy, psycopg / asyncpg |
| Auth | PyJWT, bcrypt, pyotp (MFA) |
| Integrations | Razorpay, Twilio, MSG91, Firebase Admin, reCAPTCHA |
| Documents | WeasyPrint (PDF), openpyxl (Excel), python-docx (Word) |
| DevOps | Docker, Jenkins |

## Architecture

```text
POST /   { "mod", "subMod", "action", "actionType", "data" }
   │
   ▼
index.py ── middleware: request ID, ExecutionContext, JWT validation
   │
   ▼
ActionDispatcher ── resolves module → submodule → handler
   │
   ▼
modules/m_*/m_*_sm_*.py ── business logic → m_*_data.py ── PostgreSQL
   │
   ▼
Standard JSON response + structured log
```

Example request:

```json
{
  "mod": "m_exam",
  "subMod": "sm_schedule",
  "action": "create_schedule",
  "actionType": "add",
  "data": { "academic_year_id": "AY-2026", "exam_type": "REGULAR" }
}
```

## Modules

| Module | Responsibility |
|---|---|
| `m_AAA` | Authentication, RBAC, MFA, policies, audit |
| `m_profile` | Personal, family, academic and medical profiles |
| `m_curriculum` | Programmes, courses, curriculum, enrollment |
| `m_timetable` | Scheduling, allocation, substitution, approvals |
| `m_attendance` | Records, leave, exceptions, shortage tracking |
| `m_exam` | Schedules, seating, hall tickets, marks, results, revaluation |
| `m_fees` | Fee structures, invoices, payments, concessions, refunds |
| `m_super_admin` | Module/action registry, config, master data management |

## Project structure

```text
index.py / main.py     FastAPI app and middleware pipeline
modules/               Domain modules (m_* → sm_* submodules)
xsc_lib/               Framework: dispatcher, container, loader, logger, DB, providers
xsc_conf/              Configuration
contracts/             API contract per module
docs/ guidelines/      Architecture, engineering standards, ER diagrams
seeder/                Seed and verification scripts
Dockerfile, Jenkinsfile
```

## Getting started

**Requirements:** Python 3.10+, PostgreSQL 13+.

```bash
git clone https://github.com/<your-username>/educore-erp-backend.git
cd educore-erp-backend

python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt

cp .env.example .env
cp xsc_conf/xsc_conf_pgrs.example.json xsc_conf/xsc_conf_pgrs.json
# edit both files with your own values

uvicorn main:app --reload --port 8000
```

Interactive API docs: <http://localhost:8000/docs>

### Docker

```bash
docker build -t educore-backend .
docker run -p 8000:8000 --env-file .env educore-backend
```

### Configuration

| File | Purpose |
|---|---|
| `.env` | JWT settings, environment, optional Razorpay / Twilio / reCAPTCHA keys |
| `xsc_conf/xsc_conf_pgrs.json` | PostgreSQL connection details |
| `xsc_conf/firebase_credentials.json` | Optional, for push notifications (gitignored) |

Set `ENV=production` to enforce a JWT secret of at least 32 characters.

## Documentation

- [Backend architecture](guidelines/01_Backend_Architecture.md)
- [Routing specification](guidelines/02_Routing_Specification.md)
- [Module and submodule conventions](guidelines/03_Module_Submodule_Convention.md)
- [API contracts](contracts/)
- [Engineering standard](docs/engineering/OneApp_Engineering_Standard_v1.md)

## My contributions

> Replace with your real work.

- Built the _<m_profile / m_exam ...>_ modules (submodules, data layer, API contracts)
- _<Dispatcher / API design, seeding scripts, bug fixes, performance work>_

## Credits

Developed collaboratively. Original framework authors: Siddharth Amdal, Akshay Basutkar, Swarnim Deshpande.

## License

See [LICENSE](LICENSE).
