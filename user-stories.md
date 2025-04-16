# CarbonPay – Decentralized Carbon Offsetting

## Project Name: CarbonPay

### Value Proposition:
CarbonPay is a decentralized platform for tokenizing, trading, and retiring certified carbon credits on the Solana blockchain. It enables organizations to offset their carbon footprint through verifiable, auditable, and transparent smart contracts — with metadata and certification securely stored via IPFS.

### Product-Market Fit:
The voluntary carbon market is fragmented, opaque, and lacks technical infrastructure for traceability and proof. CarbonPay addresses these challenges by offering an end-to-end solution with smart contract logic, decentralized document storage (IPFS), and wallet-native onboarding, reducing fraud and double counting, and creating a seamless offsetting experience.

---

## Target User Profiles

### The Sustainability Officer (Corporate ESG Lead)
- **Demographics**: 30–50 years old, working in large and medium-sized enterprises with sustainability mandates.
- **Interests**: Corporate responsibility, carbon tracking, ESG reporting, sustainability tools.
- **Motivations**: Seeking reliable ways to meet offset goals, reduce manual tracking, and generate transparent audit reports for stakeholders.

### The Carbon Project Developer
- **Demographics**: 35–60 years old, operates a conservation or reforestation project certified by Verra or Gold Standard.
- **Interests**: Forest preservation, carbon methodology compliance, getting better liquidity and visibility.
- **Motivations**: Wants to sell their certified carbon credits globally with fewer barriers and with end-to-end traceability. Tokenization is the solution that enables that.

### The Web3 Native Climate Advocate
- **Demographics**: 20–35 years old, active in blockchain communities, DAOs, or impact investing.
- **Interests**: Crypto, sustainability, NFTs, tokenization, proof-of-impact.
- **Motivations**: Desires to offset their digital life, support verified climate projects, and own collectible proof of their impact.

---

## User Stories

### User Story ID: CP-001a
**Priority**: High  
**User Persona**: Carbon Project Developer  
**Goal**: Sell certified carbon credits globally with fewer barriers  
**User Story**: As a project developer, I want to sell my certified carbon credits globally with low friction and full traceability.  
**Functionality**:
- Tokenize carbon credits (SPL mint)
- Attach certification metadata
- Define price and amount available  
**Attributes**:
- Certification body, verifier, project location, methodology, IPFS hash  
**User Interaction**:
- Upload documents
- Confirm issuance with wallet

---

### User Story ID: CP-001b  
**Priority**: High  
**User Persona**: Project Developer  
**Goal**: Track project credit availability  
**User Story**: As a project developer, I want to track how many credits are still available or retired, so I can monitor my project's lifecycle.  
**Functionality**:
- View issued, available, and retired amounts per project  
**Attributes**:
- token_mint, total_issued, total_retired  
**User Interaction**:
- View dashboard linked to connected wallet

---

### User Story ID: CP-002a  
**Priority**: High  
**User Persona**: Corporate ESG Manager  
**Goal**: Offset emissions transparently  
**User Story**: As an ESG manager, I want to purchase and retire carbon credits so that I can transparently offset my company’s emissions.  
**Functionality**:
- Buy tokens
- Receive NFT receipt
- Retire (burn) part of the credits  
**Attributes**:
- Purchase NFT with metadata
- Retirement transaction stored on-chain  
**User Interaction**:
- Burn part of the tokens
- Receive updated NFT if credits remain

---

### User Story ID: CP-002b  
**Priority**: Medium  
**User Persona**: ESG Manager  
**Goal**: Generate audit-ready reports  
**User Story**: As an ESG manager, I want to download a digitally signed report of my retired credits for auditors.  
**Functionality**:
- Generate exportable CSV/JSON  
**Attributes**:
- tx_hash, quantity, project, timestamp  
**User Interaction**:
- Click "Download Report" in dashboard

---

### User Story ID: CP-003a  
**Priority**: High  
**User Persona**: Carbon Project Developer  
**Goal**: Receive payment for sold credits  
**User Story**: As a project developer, I want to automatically receive payment whenever credits are sold, minus CarbonPay’s commission.  
**Functionality**:
- Smart contract distributes payment
- Commission deducted automatically  
**Attributes**:
- Configurable % commission  
**User Interaction**:
- View payout history linked to wallet

---

### User Story ID: CP-003b  
**Priority**: Medium  
**User Persona**: Project Developer  
**Goal**: Update non-critical metadata  
**User Story**: As a project developer, I want to update my project description or image (not the certified data) to keep the project page engaging.  
**Functionality**:
- Update fields like image URL or description  
**Attributes**:
- Only `authority` wallet can edit  
**User Interaction**:
- Edit in UI > confirm with wallet

---

### User Story ID: CP-004a  
**Priority**: Medium  
**User Persona**: Web3 Native  
**Goal**: Offset personal carbon footprint  
**User Story**: As a Web3 user, I want to buy small amounts of certified credits and get a collectible NFT as proof of my contribution.  
**Functionality**:
- Buy 1–100 credits
- NFT minted to wallet  
**Attributes**:
- Metadata: project, quantity, date  
**User Interaction**:
- Click “Offset Now”
- View/share NFT

---

### User Story ID: CP-004b  
**Priority**: Low  
**User Persona**: Web3 Native  
**Goal**: View personal offset history  
**User Story**: As a Web3 user, I want to see all my past offsets and NFTs in one place to track my impact over time.  
**Functionality**:
- Personal offset dashboard  
**Attributes**:
- List of projects + total retired  
**User Interaction**:
- Connect wallet → View “My Offsets”