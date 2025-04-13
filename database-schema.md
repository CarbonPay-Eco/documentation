# 🧱 Database Schema – CarbonPay

This document outlines the structure of CarbonPay's off-chain database tables, including field types, relationships, and usage. The platform uses **wallet-based authentication only** (no login/password), with all entities tied to the connected Solana wallet.

---

## 🧾 Table: `wallets`

Stores information about connected wallets.

| Column          | Type       | Description                                 |
|-----------------|------------|---------------------------------------------|
| `id`            | UUID (PK)  | Unique identifier for the wallet            |
| `wallet_address`| TEXT       | Solana wallet address (base58)              |
| `provider`      | TEXT       | Wallet provider (Phantom, Backpack, etc.)   |
| `role`          | TEXT       | Optional role: `organization`, `admin`, etc.|
| `created_at`    | TIMESTAMP  | First connection timestamp                  |

---

## 🏢 Table: `organizations`

Stores onboarding data from connected companies.

| Column                           | Type       | Description |
|----------------------------------|------------|-------------|
| `id`                             | UUID (PK)  | Unique organization ID |
| `wallet_id`                      | UUID (FK)  | References `wallets.id` |
| `company_name`                   | TEXT       | Legal company name |
| `country`                        | TEXT       | Country of operation |
| `registration_number`           | TEXT       | Company registration number (e.g., CNPJ) |
| `industry_type`                  | TEXT       | Sector (e.g., Energy, Agriculture) |
| `company_size`                   | TEXT       | Size of the company |
| `description`                    | TEXT       | Brief description of company activities |
| `tracks_emissions`              | BOOLEAN    | Whether the company tracks its emissions |
| `emission_sources`              | TEXT[]     | Main sources of emissions |
| `sustainability_certifications` | TEXT[]     | Environmental certifications held |
| `prior_offsetting`              | BOOLEAN    | Has the company offset emissions before? |
| `contact_email`                 | TEXT       | Optional contact email |
| `website_url`                   | TEXT       | Optional website URL |
| `accepted_terms`                | BOOLEAN    | Whether T&Cs were accepted |
| `created_at`                    | TIMESTAMP  | Registration date |

---

## 🌱 Table: `carbon_projects`

Represents registered environmental projects.

| Column               | Type       | Description |
|----------------------|------------|-------------|
| `id`                 | UUID (PK)  | Project ID |
| `name`               | TEXT       | Project name |
| `location`           | TEXT       | Location (e.g., São Paulo, Brazil) |
| `description`        | TEXT       | Project description |
| `certification_body` | TEXT       | Certification authority (e.g., Verra) |
| `methodology`        | TEXT       | Methodology code (e.g., VM0015) |
| `verifier_name`      | TEXT       | Entity that verified the project |
| `tags`               | TEXT[]     | Related tags (e.g., forest, renewable) |
| `project_image_url`  | TEXT       | Optional project image URL |
| `created_at`         | TIMESTAMP  | Registration date |

---

## 🪙 Table: `carbon_credits`

Represents each tokenized issuance of a project (SPL token).

| Column              | Type       | Description |
|---------------------|------------|-------------|
| `id`                | UUID (PK)  | Unique credit issuance ID |
| `project_id`        | UUID (FK)  | References `carbon_projects.id` |
| `token_id`          | TEXT       | Token identifier (e.g., CP-ATL-2022) |
| `project_ref_id`    | TEXT       | Registry code (e.g., VCS/3449) |
| `vintage_year`      | INT        | Year of issuance |
| `total_issued`      | INT        | Total credits issued (tCO₂) |
| `available`         | INT        | Remaining credits available |
| `price_per_ton`     | NUMERIC    | Current price per tonne (USD) |
| `ipfs_hash`         | TEXT       | IPFS hash for certification documents |
| `documentation_url` | TEXT       | Optional direct doc URL |
| `on_chain_mint_tx`  | TEXT       | On-chain mint transaction hash |
| `status`            | TEXT       | Status: `available`, `sold_out`, `retired` |
| `created_at`        | TIMESTAMP  | Timestamp of tokenization |

---

## 🔥 Table: `retirements`

Tracks carbon credit retirements (offsets) by organizations.

| Column                   | Type       | Description |
|--------------------------|------------|-------------|
| `id`                     | UUID (PK)  | Unique retirement ID |
| `wallet_id`              | UUID (FK)  | Wallet that performed the offset |
| `credit_id`              | UUID (FK)  | Token/lote used |
| `quantity`               | INT        | Amount of CO₂ offset (tCO₂) |
| `retirement_date`        | TIMESTAMP  | Date of the offset |
| `tx_hash`                | TEXT       | On-chain burn transaction hash |
| `proof_url`              | TEXT       | Public link to explorer or verification |
| `auto_offset`            | BOOLEAN    | Whether it was an automated offset |
| `reporting_period_start` | DATE       | Period start for which offset applies |
| `reporting_period_end`   | DATE       | Period end for which offset applies |

---

## 🛡️ Table: `audit_logs`

Tracks key actions on the platform for compliance and transparency.

| Column        | Type       | Description |
|---------------|------------|-------------|
| `id`          | UUID (PK)  | Log entry ID |
| `wallet_id`   | UUID (FK)  | Wallet that performed the action |
| `action`      | TEXT       | Action type (e.g., `mint_credit`) |
| `entity_type` | TEXT       | Affected entity type (e.g., `carbon_credit`) |
| `entity_id`   | UUID       | Affected entity ID |
| `metadata`    | JSONB      | Additional data (before/after) |
| `timestamp`   | TIMESTAMP  | Action timestamp |

---

## 🔄 Main Relationships

```plaintext
wallets (1) ─── (1) organizations  
wallets (1) ─── (N) retirements  
wallets (1) ─── (N) audit_logs  
carbon_projects (1) ─── (N) carbon_credits  
carbon_credits (1) ─── (N) retirements