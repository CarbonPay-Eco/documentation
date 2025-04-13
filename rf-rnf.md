# 1 - Software Requirements Specification

## 1.1. Functional Requirements (FR)

Functional requirements specify the behavior of the system in response to inputs and its interaction with users and external systems.

### FR1 – Tokenization of Carbon Credits
The system shall allow authenticated admin users to tokenize certified carbon credits via smart contracts. Each token must embed metadata including: certification entity (e.g., Verra, Gold Standard), project geolocation, volume of emissions offset (in tCO₂), issuance date, expiration (if any), and a unique identifier.

### FR2 – Project Registry & Listing
Users shall be able to access a catalog of tokenized carbon credit projects. Each project entry must contain a project summary, certification body, amount of credits available, retirement status, and pricing per ton.

### FR3 – On-Chain Purchase Mechanism
The system shall integrate with Solana wallets (Phantom, Backpack) to enable the purchase of carbon credit tokens using USDC or SOL. Smart contracts shall ensure atomic settlement and delivery (instant ownership transfer).

### FR4 – Token Retirement Workflow
Users shall be able to retire tokens through a “burn” operation. Once burned, the token must become non-transferable and marked as “retired” with a timestamp. The system must update the project registry accordingly.

### FR5 – Automated Offsetting (Premium Feature)
The system shall allow users to configure recurring emission offsets based on estimated GHG profiles. This includes rules for frequency, token type preference, and automated wallet debiting.

### FR6 – Public Traceability & Verification Page
Each user or business entity shall have access to a public-facing traceability page, which lists retired credits, project sources, and retirement proofs via blockchain explorer links.

### FR7 – Organization Dashboard
Authorized businesses shall access a corporate portal with analytics (e.g., monthly offset totals), offset strategies, token history, and downloadable environmental impact reports.

### FR8 – Wallet Onboarding and Connection
The system shall guide new users through the process of connecting a Solana-compatible wallet (Phantom, Backpack, Solflare). On first login, the system must verify wallet signature, create an associated user profile, and store wallet metadata in a secure off-chain database for dashboard association.

### FR9 – Admin Portal for Token Issuance and Audit
Admin users shall have access to a secure portal to upload documentation (PDFs, Verra links, retirement proofs), initiate token issuance, and manage lifecycle operations. The portal must include version control for documents and full audit logs for every action performed.

### FR10 – Smart Contract Interaction Layer
The system shall abstract interactions with Solana smart contracts via a secure backend layer (e.g., using Anchor IDL bindings), exposing APIs for mint, transfer, burn, and metadata querying operations. Rate-limiting and error-handling mechanisms must be implemented to protect from misuse.

### FR11 – API for External Carbon Credit Registries
The backend shall provide a standardized API to allow integration with external registries (e.g., Verra, Gold Standard). This API must support project import, status verification (active, retired, pending), and documentation retrieval for token eligibility validation.

### FR12 – Compliance Audit Trail Export
Users and organizations shall be able to generate compliance audit files (CSV, JSON) containing token retirement history, wallet addresses, timestamps, and hash proofs of transaction finality. These files must be digitally signed by the platform and verifiable on-chain.

---

## 1.2. Non-Functional Requirements (NFR)

Non-functional requirements define quality attributes of the software, as per ISO/IEC 25010:2011 (ISO, 2011). Each attribute below maps to a corresponding ISO quality category.

### NFR1 – Performance Efficiency
- **Response Time**: 95% of all dashboard and project page requests must resolve in under 1200 ms (ISO 25010: Time Behaviour).
- **Blockchain Operations**: Minting, transfer, and retirement of tokens shall execute in under 3 seconds on the Solana blockchain.

### NFR2 – Availability
- **Uptime Guarantee**: The platform shall maintain a minimum uptime of 99.5% monthly, excluding scheduled maintenance windows (ISO 25010: Reliability – Availability).

### NFR3 – Security
- All sensitive operations (e.g., admin login, token minting) must enforce two-factor authentication and RBAC (role-based access control).
- Token metadata must be immutable post-minting.
- Smart contracts must undergo security audit before deployment (ISO 25010: Security – Integrity, Confidentiality).

### NFR4 – Maintainability
- Codebase must follow modular architecture (React + Solana/Anchor), be covered by unit/integration tests, and maintain at least 80% test coverage.
- CI/CD pipelines (GitHub Actions) must validate codebase integrity before each release (ISO 25010: Maintainability – Modularity, Analyzability).

### NFR5 – Compatibility
- The frontend must support latest versions of Chrome, Safari, Edge and Firefox.
- Wallet integrations shall follow Solana wallet adapter standards and support Phantom, Backpack, Solflare (ISO 25010: Compatibility – Interoperability).

### NFR6 – Usability
- The onboarding process (wallet connection, project browsing, and first purchase) must be fully completable by new users in under 3 minutes.
- Platform must support accessibility compliance (WCAG 2.1 AA) where applicable.

### NFR7 – Scalability
- The backend and smart contract infrastructure must support a minimum of 10,000 concurrent users without service degradation.
- Load testing must confirm horizontal scaling capabilities under simulated peak load (ISO 25010: Performance Efficiency – Resource Utilization).

### NFR8 – Recoverability
- In case of system failure, all smart contract states must remain verifiable on-chain and the platform must recover full functionality within 10 minutes using automated deployment strategies (ISO 25010: Reliability – Recoverability).

### NFR9 – Auditability
- All user and system activities related to token generation, transfers, and offsetting must be logged with timestamp, actor ID, and action metadata.
- Logs must be queryable via an internal audit tool with filtering capabilities (ISO 25010: Security – Accountability, Traceability).

### NFR10 – Legal & Compliance
- All tokenized credits must store verifiable references to original registry documentation, including certification and retirement proofs, ensuring alignment with Verra, Gold Standard, and OCPC 10 (ISO 25010: Security – Legal Compliance).

### NFR11 – Localization & Internationalization
- The platform must support full English and Portuguese language options, and structure must allow easy extension to other locales.
- All dates, units, and currency formats must be adapted to user's selected region (ISO 25010: Usability – Operability).

### NFR12 – Portability
- The application must be deployable across cloud providers (Vercel, AWS, or GCP) with containerization (Docker) and infrastructure-as-code (Terraform) to facilitate migration or disaster recovery (ISO 25010: Portability – Installability, Adaptability).

---

## 1.3. Testing Approach

### Unit Testing
- Smart contracts (Anchor) must include unit tests for token minting, transfer, and burning logic.
- React components shall be tested with Jest/RTL, ensuring rendering, form validation, and wallet connection.

### Integration Testing
- End-to-end tests with Cypress will validate the flow from token listing to retirement.
- Backend APIs will be tested for data integrity and latency.

### Performance Testing
- Load testing using k6 to simulate 500 concurrent users and validate adherence to response time and throughput requirements.

### Security Testing
- Static analysis of smart contracts using tools like Solana Analyzer or Sec3.
- Manual code reviews for backend + frontend logic.

---

## 1.4. Coverage Goals

- **Functional Coverage**: 100% of core user stories implemented and tested.
- **Test Coverage**: ≥ 80% automated test coverage across backend and frontend codebases.
- **Performance Compliance**: ≤ 1200ms avg latency under load, ≤ 3s on-chain ops.
- **Availability**: 99.5% uptime via monitoring tools like Vercel Health + UptimeRobot.

# 2 – Test Scenario Planning

This document presents a comprehensive test scenario planning framework for the CarbonPay platform, focused on verifying the functional requirements of the application through clearly defined use cases. Each scenario outlines the objective, initial conditions, procedural steps, expected results, and postconditions. The intent is to ensure system reliability, functional correctness, data integrity, and user experience consistency throughout the platform.

---

## Test Scenario 1 – Tokenization of Certified Carbon Credits (FR1)

**Objective**  
To validate the process through which authenticated administrative users can tokenize verified carbon credits via smart contracts on the Solana blockchain.

**Preconditions**  
- The administrator is authenticated in the system.
- Valid certification documentation has been submitted and validated.
- The connected wallet has authorization to execute smart contract operations.

**Procedure**  
1. Access the administrative dashboard and initiate the "Tokenize Credit" workflow.  
2. Input all mandatory metadata: certification standard, geographic origin, project ID, emission volume (tCO₂), issuance date, and optional expiration.  
3. Execute the token mint operation.  
4. Approve the transaction using a connected wallet (e.g., Phantom).

**Expected Results**  
- A new SPL token is minted and recorded on-chain.  
- Token metadata is stored in the registry.  
- A transaction hash is generated and retrievable.

**Postconditions**  
- The token is listed as active and available for trading.  
- The tokenized project becomes visible in the user interface and available for future purchases.

---

## Test Scenario 2 – Display and Interaction with Tokenized Project Registry (FR2)

**Objective**  
To confirm the correct retrieval and rendering of tokenized carbon credit projects in the user interface.

**Preconditions**  
- At least one tokenized project exists in the registry.  
- The front-end is integrated with the indexer service or RPC node for data querying.

**Procedure**  
1. Access the platform’s marketplace or registry view.  
2. Optionally apply filters (e.g., by certification standard or location).  
3. Select a project to view its full details.

**Expected Results**  
- The project details are correctly loaded and rendered, including certification body, available credit volume, pricing, and retirement status.

**Postconditions**  
- Users are able to make informed purchasing decisions based on the available data.

---

## Test Scenario 3 – On-Chain Purchase of Carbon Credit Tokens (FR3)

**Objective**  
To verify the purchase and settlement of carbon credit tokens using a Solana-compatible wallet.

**Preconditions**  
- The user has a connected wallet (e.g., Phantom) and a sufficient balance in USDC or SOL.  
- The selected project has tokens available for purchase.

**Procedure**  
1. Initiate a token purchase from the project view.  
2. Enter the quantity of credits to acquire.  
3. Confirm the transaction via the wallet interface.  
4. Wait for transaction confirmation.

**Expected Results**  
- The smart contract finalizes the token transfer.  
- Ownership is updated, and the user’s wallet receives the tokens.  
- A transaction hash is displayed for traceability.

**Postconditions**  
- Tokens are now in the user’s possession and visible in their portfolio.

---

## Test Scenario 4 – Token Retirement Operation (FR4)

**Objective**  
To validate the correct execution of the token burn operation that retires credits permanently on-chain.

**Preconditions**  
- The user holds one or more unretired tokens.  
- The associated project has not expired or been fully retired.

**Procedure**  
1. Navigate to the user portfolio.  
2. Select a token to retire.  
3. Confirm the burn transaction via the wallet.  
4. Observe UI confirmation and transaction finality.

**Expected Results**  
- Token status is updated to "Retired".  
- The transaction is logged and referenced in the registry.  
- The retirement timestamp is recorded.

**Postconditions**  
- The token is rendered non-transferable.  
- The system reflects a reduced project credit balance.

---

## Test Scenario 5 – Automated Offset Configuration (FR5)

**Objective**  
To test the configuration of recurring emissions offset schedules and their correct execution.

**Preconditions**  
- The user must be an organization with verified emissions data.  
- The wallet must have pre-approved the automation contract for recurring deductions.

**Procedure**  
1. Access the automation panel.  
2. Set offset rules: periodicity, token types, and wallet parameters.  
3. Activate the automation.

**Expected Results**  
- The system initiates periodic token retirements based on the defined schedule.  
- Notifications are sent to the user via email or platform alert.  
- Offset history is updated accordingly.

**Postconditions**  
- Emission offsets occur without user intervention at defined intervals.  
- All transactions are auditable and stored on-chain.

---

## Test Scenario 6 – Public Traceability Portal Access (FR6)

**Objective**  
To verify that any external user can consult the traceability page of a registered organization.

**Preconditions**  
- The organization has retired credits on-chain.  
- The traceability page is generated and published.

**Procedure**  
1. Access the public URL corresponding to a business entity.  
2. Browse offset history, project origins, and associated blockchain transactions.

**Expected Results**  
- The public page displays accurate and real-time traceability data.  
- Links to blockchain explorers confirm each token retirement.

**Postconditions**  
- Third parties can independently verify offset commitments and transaction validity.

---

## Test Scenario 7 – Enterprise Dashboard Features (FR7)

**Objective**  
To validate the enterprise dashboard’s ability to present operational analytics, manage tokens, and export compliance reports.

**Preconditions**  
- The user is authenticated as an enterprise account.  
- The account has executed previous token transactions (purchases or retirements).

**Procedure**  
1. Navigate to the enterprise dashboard.  
2. View graphs related to total offsets, project distribution, and token usage.  
3. Export reports in supported formats.

**Expected Results**  
- Data visualization is rendered correctly and dynamically.  
- Downloaded reports contain all required metrics and identifiers.  
- Offset strategy parameters are editable and saved correctly.

**Postconditions**  
- Enterprise stakeholders can use data for ESG reporting and compliance audits.

# 3 - Preliminary Implementations Overview

This document outlines the current state of CarbonPay’s technical implementations and their alignment with the twelve functional requirements defined for the platform. The goal is to demonstrate architectural decisions, partial or completed feature delivery, and next steps for full MVP readiness.

## Frontend Stack

- **Framework**: Next.js (App Router) with TypeScript
- **UI Layer**: TailwindCSS + shadcn/ui components
- **Wallet Adapter**: Solana Wallet Adapter integration (Phantom, Backpack, Solflare)
- **Testing**: Unit testing with Vitest, linting (ESLint), formatting (Prettier)

## Smart Contracts (On-Chain Stack)

- **Language**: Rust
- **Framework**: Anchor (Solana IDL-compatible)
- **Program Accounts**: Program Derived Addresses (PDAs) to store credit data securely
- **Blockchain Network**: Solana Devnet (test environment)

---

## 3.1 Functional Requirements Implementation Status

### FR1 – Tokenization of Carbon Credits
✅ **Partially Implemented**  
Initial smart contract for token minting has been deployed on Solana Devnet using Anchor. Tokens contain structured metadata fields (project name, CO₂ offset volume, certification entity, issuance date). Admins can call the `mint_credit` instruction. Integration with file-backed metadata (e.g. Verra documents) is in planning.

### FR2 – Project Registry & Listing
✅ **Frontend Implemented (Static)**  
Project cards are displayed dynamically on the frontend. Each card includes summary information, certification label, price per ton, and availability. Backend integration to serve on-chain data is under development.

### FR3 – On-Chain Purchase Mechanism
🔄 **Prototype Stage**  
Wallet connection and basic mock purchase flow are operational. Solana Wallet Adapter integrated. Smart contract interaction layer being extended to ensure real on-chain transaction with USDC or SOL using CPI (Cross-Program Invocation).

### FR4 – Token Retirement Workflow
✅ **Implemented**  
Burn operation functional via smart contract. Tokens are marked as retired and made non-transferable. Timestamped and stored in the project registry PDA.

### FR5 – Automated Offsetting (Premium Feature)
🕓 **Not Started**  
Feature currently in backlog. Design specification defined: recurrence logic and user-side scheduling via CRON-like pattern to automate emissions offset via wallet debits.

### FR6 – Public Traceability & Verification Page
🛠 **Planned**  
Design prototype completed. The system will expose a public-facing page with a list of retired credits, explorer links, and environmental impact visualizations. On-chain proof via tx hash verification is planned using Solana Explorer API.

### FR7 – Organization Dashboard
✅ **Partially Implemented**  
Corporate dashboard with token history and monthly offset preview is in place. Charts and downloadable reports are pending backend support.

### FR8 – Wallet Onboarding and Connection
✅ **Implemented**  
Users are prompted to connect their wallet. Upon connection, the app verifies wallet signature and stores metadata securely in a Firestore database for later profile retrieval and dashboard association.

### FR9 – Admin Portal for Token Issuance and Audit
🛠 **Initial UI in place**  
Basic admin-only dashboard with project upload UI (mock data). Document upload and audit logging backend in design phase. Final version will support uploading Verra files, validation proofs, and version control.

### FR10 – Smart Contract Interaction Layer
✅ **In Progress**  
Anchor IDL bindings are being connected via a backend layer (Node.js service). Exposes internal API for frontend to call `mint`, `burn`, and `fetch` instructions. Rate-limiting and retry logic to be enforced before production.

### FR11 – API for External Carbon Credit Registries
🔲 **Not Started**  
Design stage only. Will follow OpenAPI 3.0 format for registry integration (e.g., Verra, Gold Standard). Functions will include fetching project metadata, verifying retirement status, and downloading original documentation.

### FR12 – Compliance Audit Trail Export
🛠 **Planned (Spec Ready)**  
CSV and JSON export design complete. Will include tx hashes, retirement proofs, wallet addresses, and credit identifiers. Files will be signed with CarbonPay’s backend key and linked to IPFS hashes or on-chain signature verification.

---

## 3.2 Summary & Next Steps

- 7 of 12 functional requirements have partial or full implementations.
- Smart contract maturity on Devnet allows for minting, burning, and tracking tokens.
- Major frontend structures (Landing Page, Wallet Connect, Dashboard) are complete.
- Remaining efforts will focus on:
  - Project registry on-chain integration
  - Admin audit flow
  - Full Solana token purchase flow
  - Traceability interface and premium automations

CarbonPay is technically positioned for MVP testing, with a clear roadmap for completing high-impact features in the upcoming sprints.

---

**References**

ISO/IEC 25010:2011 – Systems and software engineering — Systems and software Quality Requirements and Evaluation (SQuaRE) — System and software quality models. ISO, Geneva, 2011.
