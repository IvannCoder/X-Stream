# Software Requirements Specification (SRS) - X-Streme Platform

## 1. System Overview & User Roles

### 1.1 System Overview
X-Streme is a unified football ecosystem incorporating three core sub-modules:
1. **STM Football Merchandise Shop:** E-commerce catalog, shopping cart, and inventory tracking.
2. **PitchMaster FTMS:** Team operations, roster management, match tactics, and medical tracking.
3. **OmniPitch:** Live match streaming with multi-language commentary tracks and match analytics.

### 1.2 User Roles & Access Hierarchy
* **Admin:** Full permissions across user management, system settings, inventory, and global configurations.
* **Manager / Coach:** Access to team rosters, tactical formations (e.g., 4-3-3), session scheduling, and player stats[cite: 1].
* **Medical Staff:** Exclusive HIPAA/GDPR-compliant access to player medical histories, injury logs, and fitness tracking[cite: 1].
* **Player / Customer:** Access to stream viewing, daily wellness assessments, merchandise purchasing, and profile settings[cite: 1].

---

## 2. Functional Requirements (FRs)

### 2.1 Sub-Module 1: Merchandise Shop (STM)
* **FR-STM-01 (High):** Customer registration and role-based login[cite: 1].
* **FR-STM-02 (High):** Product catalog with search, team filtering, stock tracking, and shopping cart checkout[cite: 1].
* **FR-STM-03 (Medium):** Automatic total calculation, PDF receipt generation, and discount code application[cite: 1].

### 2.2 Sub-Module 2: Team Management (PitchMaster)
* **FR-FTMS-01 (High):** Roster management tracking demographic profiles, contracts, and jersey numbers[cite: 1].
* **FR-FTMS-02 (High):** Interactive fixture calendar with drag-and-drop tactical pitch formation builder[cite: 1].
* **FR-FTMS-03 (High):** Medical injury log with automated availability tags (Fit, Doubtful, Injured)[cite: 1].

### 2.3 Sub-Module 3: Live Streaming (OmniPitch)
* **FR-OMNI-01 (Critical):** Live stream delivery in 720p, 1080p, and 4K with adaptive bitrate streaming (ABR)[cite: 1].
* **FR-OMNI-02 (Critical):** Real-time audio track switching across minimum 10 commentary languages without stream interruption[cite: 1].
* **FR-OMNI-03 (High):** Subscription tier enforcement (Free, Premium, Pro) integrated with payment gateways[cite: 1].

---

## 3. Non-Functional Requirements (NFRs)

* **NFR-SEC-01 (Data Encryption):** Data in transit encrypted via TLS 1.3; sensitive identifiers and medical data at rest encrypted via AES-256[cite: 1].
* **NFR-SEC-02 (Medical Segregation):** Medical diagnostic details strictly restricted to Medical Staff roles[cite: 1].
* **NFR-PRF-01 (Latency):** Live stream latency `< 5` seconds; standard UI queries render in `< 1.0` second[cite: 1].
* **NFR-REL-01 (Availability):** Operational threshold of 99.9% uptime during active competition seasons[cite: 1].