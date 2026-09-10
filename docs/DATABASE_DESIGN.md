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
-- PostgreSQL Schema for X-Streme Platform

CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

-- 1. Identity, Authentication & Subscriptions
CREATE TABLE users (
    user_id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    email_normalized VARCHAR(320) NOT NULL UNIQUE,
    password_hash TEXT NOT NULL,
    role VARCHAR(20) NOT NULL CHECK (role IN ('admin', 'manager', 'medical', 'player', 'customer')),
    subscription_tier VARCHAR(20) NOT NULL DEFAULT 'free' CHECK (subscription_tier IN ('free', 'premium', 'pro')),
    is_active BOOLEAN NOT NULL DEFAULT TRUE,
    created_at TIMESTAMPTZ NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMPTZ NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE user_profiles (
    user_id UUID PRIMARY KEY REFERENCES users(user_id) ON DELETE CASCADE,
    first_name VARCHAR(80) NOT NULL,
    last_name VARCHAR(80) NOT NULL,
    phone_number VARCHAR(30),
    preferred_language VARCHAR(10) NOT NULL DEFAULT 'en',
    avatar_object_key TEXT,
    updated_at TIMESTAMPTZ NOT NULL DEFAULT CURRENT_TIMESTAMP
);

-- 2. STM Merchandise Shop
CREATE TABLE shop_products (
    product_id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    sku VARCHAR(50) NOT NULL UNIQUE,
    name VARCHAR(120) NOT NULL,
    description TEXT,
    category VARCHAR(50) NOT NULL,
    price NUMERIC(10, 2) NOT NULL CHECK (price > 0),
    stock_quantity INTEGER NOT NULL DEFAULT 0 CHECK (stock_quantity >= 0),
    image_object_key TEXT NOT NULL,
    is_active BOOLEAN NOT NULL DEFAULT TRUE,
    created_at TIMESTAMPTZ NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE orders (
    order_id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    user_id UUID NOT NULL REFERENCES users(user_id),
    order_status VARCHAR(20) NOT NULL CHECK (order_status IN ('pending', 'paid', 'shipped', 'cancelled')),
    total_amount NUMERIC(10, 2) NOT NULL CHECK (total_amount >= 0),
    shipping_address TEXT NOT NULL,
    created_at TIMESTAMPTZ NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE order_items (
    order_id UUID REFERENCES orders(order_id) ON DELETE CASCADE,
    product_id UUID REFERENCES shop_products(product_id),
    unit_price NUMERIC(10, 2) NOT NULL CHECK (unit_price >= 0),
    quantity INTEGER NOT NULL CHECK (quantity > 0),
    PRIMARY KEY (order_id, product_id)
);

-- 3. PitchMaster FTMS (Roster & Medical)
CREATE TABLE player_profiles (
    player_id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    user_id UUID REFERENCES users(user_id) ON DELETE SET NULL,
    jersey_number SMALLINT NOT NULL CHECK (jersey_number BETWEEN 1 AND 99),
    position VARCHAR(30) NOT NULL,
    preferred_foot VARCHAR(10) CHECK (preferred_foot IN ('left', 'right', 'both')),
    availability_status VARCHAR(20) NOT NULL DEFAULT 'Fit' CHECK (availability_status IN ('Fit', 'Doubtful', 'Injured')),
    created_at TIMESTAMPTZ NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE medical_records (
    record_id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    player_id UUID NOT NULL REFERENCES player_profiles(player_id) ON DELETE CASCADE,
    diagnosis TEXT NOT NULL,
    treatment_plan TEXT,
    estimated_return_date DATE,
    confidentiality_level VARCHAR(20) NOT NULL DEFAULT 'HIPAA_STRICT',
    created_at TIMESTAMPTZ NOT NULL DEFAULT CURRENT_TIMESTAMP
);

-- 4. OmniPitch Live Streaming & Ticketing
CREATE TABLE matches (
    match_id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    home_team VARCHAR(80) NOT NULL,
    away_team VARCHAR(80) NOT NULL,
    kickoff_time TIMESTAMPTZ NOT NULL,
    venue VARCHAR(120) NOT NULL,
    stream_url_hls TEXT,
    status VARCHAR(20) CHECK (status IN ('scheduled', 'live', 'completed'))
);

CREATE TABLE tickets (
    ticket_id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    match_id UUID NOT NULL REFERENCES matches(match_id) ON DELETE CASCADE,
    seat_section VARCHAR(30) NOT NULL,
    price NUMERIC(10, 2) NOT NULL CHECK (price >= 0),
    available_quantity INTEGER NOT NULL CHECK (available_quantity >= 0)
);

CREATE TABLE ticket_reservations (
    reservation_id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    user_id UUID NOT NULL REFERENCES users(user_id) ON DELETE CASCADE,
    ticket_id UUID NOT NULL REFERENCES tickets(ticket_id) ON DELETE CASCADE,
    reserved_at TIMESTAMPTZ NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE match_statistics (
    match_id UUID REFERENCES matches(match_id) ON DELETE CASCADE,
    player_id UUID REFERENCES player_profiles(player_id) ON DELETE CASCADE,
    goals SMALLINT NOT NULL DEFAULT 0 CHECK (goals >= 0),
    assists SMALLINT NOT NULL DEFAULT 0 CHECK (assists >= 0),
    minutes_played SMALLINT NOT NULL DEFAULT 0 CHECK (minutes_played >= 0),
    PRIMARY KEY (match_id, player_id)
);
