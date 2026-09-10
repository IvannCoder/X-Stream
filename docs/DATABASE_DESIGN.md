# X-Streme Database Design

## 1. Overview
The proposed MVP persistence layer for the **X-Streme** football platform uses **PostgreSQL 12+** for relational data and private object storage for media assets (product images, medical attachments, stream media). It supports unified account management, sports merchandise catalog & order processing, squad roster management, HIPAA/GDPR-compliant medical records, match ticketing, and live match streaming statistics without storing large binary media files directly in the database.

This is a logical database design. The exact PostgreSQL version, identity provider, object storage platform, migration framework, hosting region, and compliance retention periods remain implementation decisions.

---

## 2. Design Principles
* **Account Isolation & Authorization:** Every user-owned entity is anchored to `users.user_id` and authorized server-side.
* **Externalized Media:** Images, streaming segments, and medical documents live in private object storage; only non-guessable object keys and metadata are stored in PostgreSQL.
* **Controlled Vocabularies:** Machine codes (roles, order status, medical status) use constrained values or lookup tables to prevent data corruption.
* **Audit & Traceability:** All records maintain precise UTC timestamps using `timestamptz`.
* **UUID Primary Keys:** Primary keys use UUIDs (`uuid_generate_v4()`) to avoid exposing sequential numeric identifiers in public API routes.
* **Data Privacy Compliance:** Sensitive medical logs are soft-archived and restricted via strict role-based access control (RBAC).

---

## 3. Entity Relationship Diagram

```mermaid
erDiagram
    USERS ||--o{ ORDERS : "places"
    USERS ||--o{ TICKET_RESERVATIONS : "books"
    USERS ||--o{ PLAYER_PROFILES : "manages/belongs to"

    SHOP_PRODUCTS ||--o{ ORDER_ITEMS : "contains"
    ORDERS ||--o{ ORDER_ITEMS : "includes"

    MATCHES ||--o{ TICKETS : "sells"
    TICKETS ||--o{ TICKET_RESERVATIONS : "reserved in"

    PLAYER_PROFILES ||--o{ MEDICAL_RECORDS : "has"
    PLAYER_PROFILES ||--o{ MATCH_STATISTICS : "records"
    MATCHES ||--o{ MATCH_STATISTICS : "tracks"
