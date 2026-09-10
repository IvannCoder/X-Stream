# Database Design — X-Streme Platform

## 1. Schema Overview
The X-Streme database utilizes **PostgreSQL 12+** with relational schema structures to support cross-module data flow[cite: 1].

---

## 2. Core Entity Tables

### Users Table (`users`)
| Column Name | Data Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `user_id` | `UUID` | `PRIMARY KEY` | Unique account identifier |
| `email` | `VARCHAR(255)` | `UNIQUE, NOT NULL` | System login address[cite: 1] |
| `password_hash` | `VARCHAR(255)` | `NOT NULL` | Encrypted authentication credentials[cite: 1] |
| `role` | `VARCHAR(50)` | `NOT NULL` | Admin, Manager, Medical, Player, Customer[cite: 1] |

### Products Table (`shop_products`)
| Column Name | Data Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `product_id` | `UUID` | `PRIMARY KEY` | Product identifier |
| `name` | `VARCHAR(255)` | `NOT NULL` | Merchandise item name[cite: 1] |
| `price` | `DECIMAL(10,2)` | `NOT NULL` | Unit price[cite: 1] |
| `stock_qty` | `INT` | `DEFAULT 0` | Current inventory level[cite: 1] |

### Player Profiles Table (`players`)
| Column Name | Data Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `player_id` | `UUID` | `PRIMARY KEY` | Unique player record |
| `jersey_number` | `INT` | `NOT NULL` | Squad jersey number assignment[cite: 1] |
| `fitness_tag` | `VARCHAR(50)` | `DEFAULT 'Fit'` | Status tag (Fit, Doubtful, Injured)[cite: 1] |

### Matches Table (`matches`)
| Column Name | Data Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `match_id` | `UUID` | `PRIMARY KEY` | Unique match event ID |
| `home_team` | `VARCHAR(255)` | `NOT NULL` | Home squad name[cite: 1] |
| `away_team` | `VARCHAR(255)` | `NOT NULL` | Away squad name[cite: 1] |
| `stream_url` | `VARCHAR(500)` | `NOT NULL` | CDN video stream source link[cite: 1] |