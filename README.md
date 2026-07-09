# PVUT ERP

**Prince Victor University of Technology Enterprise Resource Planning System**

[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-005571?style=for-the-badge&logo=fastapi)](https://fastapi.tiangolo.com/)
[![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/)
[![Architecture First](https://img.shields.io/badge/Architecture-First-blue?style=for-the-badge)](https://github.com/princeville45/PVUT-ERP/blob/main/docs/architecture-decisions/Architecture_Decisions.md)

# PVUT-ERP

**Prince Victor University of Technology — Enterprise Resource Planning System**

> *Knowledge • Innovation • Excellence*

A production-grade University Enterprise Resource Planning system, designed the way a real enterprise system is designed: architecture first, code second. Designed using enterprise software engineering principles before implementation to ensure correctness, scalability, and maintainability. This is not a tutorial project or a CRUD demonstration — every table in this system exists because a business rule required it, and every business rule was stress-tested against real university operations before a single line of SQL was written.

---

## About Prince Victor University of Technology

| | |
|---|---|
| **Established** | 1998 |
| **Type** | Public University |
| **Students** | 30,000+ |
| **Lecturers** | 700+ |
| **Faculties** | 6 |
| **Departments** | 42 |
| **Campuses** | 1 |

**Faculties:** Computing and Mathematical Sciences · Engineering · Natural Sciences · Social Sciences · Management Sciences · Agriculture

PVUT is a fictional institution created specifically as the subject of this engineering project — but every module, capacity constraint, and workflow is modeled as if it belonged to a real university, so that the resulting architecture is genuinely production-viable, not a toy simplification.

---

## Project Purpose

Build a University ERP that models how a university *actually operates* — not how a database schema is conventionally taught. The system follows enterprise architecture principles, Domain-Driven Design, transaction safety, normalization, single-owner data governance, failure isolation, role-based access control, full auditability, and workload-driven optimization.

The full architecture — every module, entity, invariant, precondition, event, and the reasoning behind each design decision — is documented in `docs/architecture-decisions/`, the design document this README summarizes. Nothing in this system was arrived at by convention; every decision has a recorded "why."

---

## Design Philosophy

**1. Single Source of Truth**
Every business fact has exactly one owner. Finance owns payments. Library owns lending. Academic Records own results. Graduation owns the graduation decision. No module writes into another module's data.

**2. Transactions Protect Business Truth, Not Convenience**
Only facts that would be *contradictory* if left half-complete belong in the same transaction (a receipt with no matching payment; a book marked lost with no corresponding charge). Notifications, printing, email, SMS, and reconciliation are downstream reactions — never part of the transaction they react to.

**3. Failure Isolation**
An external or unrelated system's failure must never destroy completed business truth. A jammed printer cannot undo a graduation. An SMS gateway outage cannot undo a payment. Finance being offline cannot undo a library penalty already raised.

**4. Domain Ownership, Event-Driven Communication**
Modules never reach into another module's data directly. They communicate through events and generic, source-agnostic contracts (`ChargeExpected`, `CLEARANCE_UPDATED`) — so a new department can plug into Finance or Graduation next year without either module's code changing.

**5. Normalization First**
Store a fact once; reference it everywhere else. Redundant storage is only introduced as a deliberate, documented trade-off — never a default.

**6. Workload-Driven Optimization**
Indexes, caches, and denormalization exist only when real workload justifies them. The default is the correct, normalized model; optimization is a measured response to evidence, not an anticipated convenience.

**7. Configuration Over Hard-Coding**
University policy — carry-over limits, registration windows, progression thresholds, fine rates — is configurable, not hard-coded, so the same architecture can serve institutions with different rules.

---

## Core Modules

### Academic Domain
Students · Lecturers · Courses & Course Offerings · Registration · Timetable · Attendance · Exams · Academic Records · Graduation · Alumni

### Library Domain
Catalog · Assets · Lending · Reservations · Penalties · Library Registry · Defaulters

### Finance Domain
Expected Charges · Payments · Receipts · Refunds

### Student Services
Hostels · Medicals · Secretary

### Infrastructure
Authentication · Staff · Audit · Configuration · Database · Main (composition root)

---

## Technology Stack

| Layer | Technology |
|---|---|
| Database | PostgreSQL |
| Database Tooling | pgAdmin 4 |
| Backend Language | Python |
| Backend Framework | FastAPI |
| Frontend | Web Portal |
| Version Control | Git / GitHub |
| Automation | Zapier |
| Documentation | Markdown |

---

## Project Structure

```
PVUT-ERP/
├── database/
│   ├── schema.sql
│   ├── tables.sql
│   ├── constraints.sql
│   ├── functions.sql
│   ├── procedures.sql
│   ├── triggers.sql
│   ├── views.sql
│   └── roles.sql
│
├── backend/
│   └── (FastAPI application, connecting to PostgreSQL)
│
├── docs/
│   ├── architecture-decisions/
│   │   └── Architecture_Decisions.md
│   ├── erd/
│       └── (Entity-Relationship diagrams)
│
└── README.md
```

---

## Portals

The finished system supports role-specific portals, each showing only what that role is authorized to see:

- **Student Portal** — biodata, registration, results, fees, hostel allocation
- **Lecturer Portal** — assigned course offerings, attendance, result entry
- **Secretary Portal** — department-scoped student administration
- **Library Portal** — circulation, penalties, reservations
- **Finance Portal** — charges, payments, receipts, refunds
- **Administration Portal** — system-wide oversight (System Administrator / ICT Administrator)

---

## Project Status

- ✅ Architecture Phase (Sprint 0–2) — **Complete.** Module boundaries, ownership map, ERD, and a full-day production stress simulation all signed off.
- 🚧 Implementation Phase — PostgreSQL schema, constraints, functions, triggers, views, roles, indexing, and backup strategy.
- ⏳ Backend Phase — FastAPI, authentication, CRUD, dependency injection, background tasks, testing.
- ⏳ Automation Phase — Event-driven workflows on top of the completed ERP.
- ⏳ AI Phase — AI assistance grounded in the ERP's own data, never owning business truth.

Full module-by-module detail, every stress-test finding, and the reasoning behind each architectural decision live in `docs/architecture-decisions/`.

---

## Why this project exists

This project is part of my transition into Backend Engineering. Rather than building isolated CRUD applications, I wanted to understand how real enterprise software is designed before writing production code. Every architectural decision documented here represents months of iterative design, review, and refinement.

---

*This project was built by designing the organization before designing the software — because a database schema is only ever as correct as the understanding of the business it represents.*
