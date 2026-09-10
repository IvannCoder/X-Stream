# X-Streme ⚽
"One Platform. Total Football."

An all-in-one digital football ecosystem unifying retail merchandising, team operations, and live multi-language match streaming into a single, scalable web application.

---

## 📌 Table of Contents
- [Overview](#-overview)
- [Problem & Vision](#-problem--vision)
- [Key Features](#-key-features)
- [Agile Documentation](#-agile-documentation)
- [Architecture & Tech Stack](#-architecture--tech-stack)
- [Data Model & Database Design](#-data-model--database-design)
- [Project Milestones](#-project-milestones)
- [Project Structure](#-project-structure)
- [Team Members](#-team-members)
- [License](#-license)

---

## 📖 Overview
**X-Streme** is an integrated software engineering project developed for university coursework. 

Most digital sports applications force users, coaches, and fans to fragment their experience across different apps—using one platform for official merchandise, another for squad tactics, and a third for watching matches. **X-Streme** flips this script by integrating three specialized sports sub-modules into a single cohesive ecosystem:

1. **STM Football Merchandise Shop:** Official e-commerce store with catalog search, cart management, and automated stock processing.
2. **PitchMaster FTMS:** Professional team administration, drag-and-drop tactical lineups, fixture planning, and HIPAA/GDPR-compliant medical logs.
3. **OmniPitch:** European football live streaming platform featuring real-time multi-language commentary track switching.

---

## 💡 Problem & Vision

### The Problem
Football fans, club administrators, and players face a fragmented digital environment:
* Fans must juggle separate applications for buying merchandise, tracking stats, and streaming games.
* Coaches struggle with manual tools for pitch tactics, session scheduling, and player availability tracking.
* Live streaming services lack real-time localized audio commentary tracks for global audiences.

### The Vision
Deliver a unified, accessible, and high-performance sports workspace that allows users to manage team operations, order gear, and stream live European matches with instant commentary switching in under 3 clicks.

---

## ✨ Key Features

| Category | Description |
| :--- | :--- |
| 🛍️ **STM Merchandise Shop** | Browse official merchandise, filter by price/team, manage shopping carts, and generate downloadable PDF receipts. |
| 📋 **Roster & Tactical Board** | Manage squad demographic profiles, assign jersey numbers, and drag-and-drop players into 4-3-3 tactical pitch formations. |
| 🩺 **Medical & Fitness Logs** | Exclusive role-gated injury tracking, return-to-play timelines, and automated operational availability tags (`Fit`, `Doubtful`, `Injured`). |
| 📺 **Multi-Language Streaming** | Live match streaming in 720p/1080p/4K with real-time switching across 10+ commentary languages without video buffering. |
| 💳 **Subscription Management** | Flexible 3-tier access control (Free, Premium, Pro) integrated with secure payment gateways. |
| 🔒 **Role-Based Isolation** | Strict access control isolating user roles (Admin, Manager, Medical, Player, Customer) for privacy and compliance. |

---

## 📂 Agile Documentation
Detailed engineering and product requirements are maintained within the `docs/` directory:

* 📋 [Project Charter](./docs/PROJECT_CHARTER.md): Project purpose, team roles, module scopes, business objectives, and milestone constraints.
* 📝 [Requirements Specification](./docs/REQUIREMENTS.md): Complete functional and non-functional requirements prioritized using the MoSCoW framework.
* ✅ [Acceptance Criteria](./docs/ACCEPTANCE_CRITERIA.md): Observable, testable behavior defined in BDD (Given-When-Then) format for core workflows.
* 🗄️ [Database Design](./docs/DATABASE_DESIGN.md): Logical schema, ERD, normalized tables, foreign key constraints, and privacy retention rules.

---

## 🏗️ Architecture & Tech Stack

### Proposed Stack
* **Frontend:** Responsive Web Application (React / Vue.js with TypeScript)
* **Backend:** Scalable REST / GraphQL Web API (Node.js Express / Python FastAPI)
* **Persistence:**
  * **Database:** PostgreSQL 12+ (relational metadata, foreign key constraints, `timestamptz` UTC)
  * **Caching:** Redis for high-throughput session management
* **Media & Infrastructure:**
  * HLS / DASH adaptive bitrate video streaming
  * CloudFront / CloudFlare CDN for multi-region media distribution

---

## 🗃️ Data Model & Database Design
The relational schema uses PostgreSQL with UUID primary keys and strict user ownership isolation (`users.user_id` foreign keys).

For full details, review [Database Design Documentation](./docs/DATABASE_DESIGN.md).

---

## 🚀 Project Milestones
* **Sprint 1 — Discovery & Scope Validation:** Combined requirements specification, acceptance criteria, and project charter baseline.
* **Sprint 2 — UX Prototype & Architecture Baseline:** UI/UX wireframing, component design, database migrations, and project skeleton.
* **Sprint 3 — Auth & E-Commerce Core:** User auth flows, product catalog, cart checkout, and inventory tracking.
* **Sprint 4 — Team Operations & Medical Logs:** Tactical board drag-and-drop, session calendars, and HIPAA/GDPR medical logs.
* **Sprint 5 — Multi-Language Streaming Engine:** HLS/DASH player, real-time commentary audio switching, and subscription gateways.
* **Sprint 6 — End-to-End Testing & Release:** Pilot deployment, load validation, security audits, and academic review.

---

## 📁 Project Structure
```text
.
├── docs/                               # Agile Project Documentation
│   ├── ACCEPTANCE_CRITERIA.md          # Given-When-Then BDD acceptance scenarios
│   ├── DATABASE_DESIGN.md              # Relational schema, ERD & data dictionary
│   ├── PROJECT_CHARTER.md              # Scope, objectives, risks, & team governance
│   └── REQUIREMENTS.md                 # MoSCoW functional & non-functional specs
└── README.md                           # Main repository overview & documentation guide
## 👥 Team Members

| Name | Student ID | Nickname | Role |
| :--- | :--- | :--- | :--- |
| **Hein Thura Naung** | `6705140056` | Ivan | Full-Stack Lead / Systems Architect |
| **Aung Myo Hlaing** | `6705140040` | Olary | Database Specialist / Backend Engineer |
| **Hein Zaw Linn** | `6705140038` | Hubert | UI/UX Designer / Frontend Engineer |