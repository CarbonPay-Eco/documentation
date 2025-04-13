# CarbonPay Backend – Developer Guide

This document outlines the structure, architecture, and key modules of the CarbonPay backend. The backend is implemented using **Node.js with TypeScript**, and provides a RESTful API that integrates with the Solana blockchain, IPFS, and PostgreSQL.

---

## 1. Overview

The backend serves as the bridge between the client application (Next.js frontend), the on-chain programs (Anchor smart contracts on Solana), and off-chain data (PostgreSQL + IPFS).

### Responsibilities

- Handle authenticated wallet interactions via Solana Wallet Adapter.
- Expose REST APIs to support tokenization, retirement, automation, and traceability.
- Integrate with IPFS to store certification documents.
- Interact with Anchor smart contracts for minting, burning, and querying tokens.
- Maintain PostgreSQL records for organizations, projects, retirements, and logs.
- Enforce RBAC and validation on critical routes (admin access).

...

## 13. Future Enhancements

- Add webhook support for token retirements
- Support multichain (future compatibility with other L1s)
- Add audit log export (CSV, JSON with signature)
- Implement offset automation engine (scheduled offsets)
