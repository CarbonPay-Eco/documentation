# 🧱 Database Schema – CarbonPay

This document describes the off-chain database structure of the CarbonPay platform. The system uses **wallet-based authentication only** (no username/password), and all core entities are tied to connected Solana wallets. Each certified carbon offset project is tokenized as a **single SPL token**, so **each project = one token**.

---

## 🧾 Table: `wallets`

Stores information about connected Solana wallets.

| Column          | Type       | Description                                 |
|-----------------|------------|---------------------------------------------|
| `id`            | UUID (PK)  | Unique wallet identifier                    |
| `wallet_address`| TEXT       | Solana public key (base58)                  |
| `provider`      | TEXT       | Wallet provider (Phantom, Backpack, etc.)   |
| `role`          | TEXT       | Optional role: `organization`, `admin`, etc.|
| `created_at`    | TIMESTAMP  | First connection timestamp                  |

---

## 🏢 Table: `organizations`

Stores onboarding data for registered companies.

| Column                           | Type       | Description |
|----------------------------------|------------|-------------|
| `id`                             | UUID (PK)  | Organization ID |
| `wallet_id`                      | UUID (FK)  | References `wallets.id` |
| `company_name`                   | TEXT       | Legal company name |
| `country`                        | TEXT       | Country of registration |
| `registration_number`           | TEXT       | Tax ID (e.g., CNPJ) |
| `industry_type`                  | TEXT       | Industry sector |
| `company_size`                   | TEXT       | Size of the company |
| `description`                    | TEXT       | Business activity overview |
| `tracks_emissions`              | BOOLEAN    | Does the company track its emissions? |
| `emission_sources`              | TEXT[]     | Main sources of emissions |
| `sustainability_certifications` | TEXT[]     | Relevant certifications or programs |
| `prior_offsetting`              | BOOLEAN    | Previously participated in offsetting? |
| `contact_email`                 | TEXT       | Contact email (optional) |
| `website_url`                   | TEXT       | Company website (optional) |
| `accepted_terms`                | BOOLEAN    | Terms of service accepted |
| `created_at`                    | TIMESTAMP  | Registration timestamp |

---

## 🪙 Table: `tokenized_projects`

Each entry represents a certified environmental project that has been tokenized as a unique SPL token on the Solana blockchain.

| Column              | Type       | Description |
|---------------------|------------|-------------|
| `id`                | UUID (PK)  | Unique identifier for the tokenized project |
| `token_id`          | TEXT       | SPL Token ID (e.g., CP-ATL-2022) |
| `project_name`      | TEXT       | Name of the environmental project |
| `location`          | TEXT       | Geographic location (e.g., São Paulo, Brazil) |
| `description`       | TEXT       | Detailed project description |
| `certification_body`| TEXT       | Certifying authority (e.g., Verra) |
| `project_ref_id`    | TEXT       | Registry reference (e.g., VCS/3449) |
| `methodology`       | TEXT       | Methodology used (e.g., VM0015) |
| `verifier_name`     | TEXT       | Verifying entity name |
| `vintage_year`      | INT        | Issuance year |
| `total_issued`      | INT        | Total amount of CO₂ credits issued (in tCO₂) |
| `available`         | INT        | Available credits remaining |
| `price_per_ton`     | NUMERIC    | Price per tonne (USD) |
| `ipfs_hash`         | TEXT       | IPFS hash of certification documents |
| `documentation_url` | TEXT       | Optional public documentation link |
| `on_chain_mint_tx`  | TEXT       | Solana mint transaction hash |
| `status`            | TEXT       | Enum: `available`, `sold_out`, `retired` |
| `project_image_url` | TEXT       | Optional image URL |
| `tags`              | TEXT[]     | Optional tags (e.g., rainforest, REDD+) |
| `created_at`        | TIMESTAMP  | Timestamp of token creation |

---

## 🔥 Table: `retirements`

Tracks carbon credit retirement events (offsets) performed by organizations.

| Column                   | Type       | Description |
|--------------------------|------------|-------------|
| `id`                     | UUID (PK)  | Retirement record ID |
| `wallet_id`              | UUID (FK)  | Wallet that performed the offset |
| `tokenized_project_id`   | UUID (FK)  | Tokenized project used for retirement |
| `quantity`               | INT        | Amount of CO₂ retired (in tCO₂) |
| `retirement_date`        | TIMESTAMP  | Timestamp of retirement |
| `tx_hash`                | TEXT       | Solana burn transaction hash |
| `proof_url`              | TEXT       | URL to public verification/explorer |
| `auto_offset`            | BOOLEAN    | Was the offset automated? |
| `reporting_period_start` | DATE       | Start date of reporting period |
| `reporting_period_end`   | DATE       | End date of reporting period |

---

## 🛡️ Table: `audit_logs`

Tracks all sensitive actions and interactions for security and compliance purposes.

| Column        | Type       | Description |
|---------------|------------|-------------|
| `id`          | UUID (PK)  | Log entry ID |
| `wallet_id`   | UUID (FK)  | Actor wallet address |
| `action`      | TEXT       | Action performed (e.g., `mint_credit`) |
| `entity_type` | TEXT       | Entity affected (e.g., `tokenized_project`) |
| `entity_id`   | UUID       | Affected entity ID |
| `metadata`    | JSONB      | Optional context (before/after data) |
| `timestamp`   | TIMESTAMP  | Action timestamp |

---

## 🔄 Updated Entity Relationships

```plaintext
wallets (1) ─── (1) organizations  
wallets (1) ─── (N) retirements  
wallets (1) ─── (N) audit_logs  
tokenized_projects (1) ─── (N) retirements  
