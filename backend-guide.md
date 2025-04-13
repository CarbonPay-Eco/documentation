# CarbonPay – Backend Overview

The CarbonPay backend is built with Node.js and TypeScript, providing RESTful APIs for carbon credit tokenization, retirement, and traceability. Authentication is entirely wallet-based using the Solana blockchain.

---

## Authentication

- Wallet connection only (no email/password)
- Verification flow:
  1. Frontend signs a message using the wallet
  2. Backend verifies signature with `@solana/web3.js`
  3. On success, access is granted

---

## API Endpoints

### Auth & Wallet

| Method | Endpoint             | Description                          |
|--------|----------------------|--------------------------------------|
| POST   | `/auth/verify`       | Verify wallet signature              |
| GET    | `/wallet/:address`   | Get wallet metadata                  |

---

### Organization

| Method | Endpoint               | Description                        |
|--------|------------------------|------------------------------------|
| POST   | `/organization`        | Register organization info         |
| GET    | `/organization/me`     | Get organization details           |

---

### Tokenized Projects

| Method | Endpoint                   | Description                           |
|--------|----------------------------|---------------------------------------|
| POST   | `/projects`                | Admin creates and mints new project token |
| GET    | `/projects`                | List all available projects           |
| GET    | `/projects/:token_id`      | Get project details                   |

---

### Retirement (Carbon Offsetting)

| Method | Endpoint                  | Description                        |
|--------|---------------------------|------------------------------------|
| POST   | `/retire`                 | Burn token to retire carbon credits |
| GET    | `/retirements`            | List retirements by authenticated wallet |
| GET    | `/public/:walletAddress`  | Public view of an org’s retirements |

---

### Admin

| Method | Endpoint                | Description                    |
|--------|-------------------------|--------------------------------|
| GET    | `/admin/audit-logs`     | View platform activity logs    |
| GET    | `/admin/projects`       | View all tokenized projects    |

---

## Web3 Integration

### Smart Contract (Solana + Anchor)

- Uses `@project-serum/anchor` with precompiled IDL
- Key instructions:
  - `mint_credit()` – Tokenize certified carbon credits
  - `burn_credit()` – Retire credits (offset)
  - `get_credit_data()` – Read token/project info

```ts
const provider = new AnchorProvider(connection, wallet, options);
const program = new Program(idl, programId, provider);

await program.methods.mintCredit(...)
  .accounts({ ... })
  .rpc();
