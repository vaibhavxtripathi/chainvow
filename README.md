# ChainVow ✦

> **Eternal commitments, sealed on the Stellar blockchain**

Two wallets. One vow. Immutable forever.

ChainVow lets any two Stellar wallet holders write a mutual commitment on-chain. The proposer writes the vow text and sets the partner's address. The partner seals it — a two-signature ceremony that locks the text to both addresses permanently in a Soroban smart contract.

---

## Live Links

| | |
|---|---|
| **Frontend** | `https://chainvow.vercel.app` _(update after deploy)_ |
| **GitHub Repo** | `https://github.com/YOUR_USERNAME/chainvow` |
| **Contract on Stellar Expert** | `https://stellar.expert/explorer/testnet/contract/CONTRACT_ID_HERE` |
| **Proof Transaction** | `https://stellar.expert/explorer/testnet/tx/TX_HASH_HERE` |

---

## Tech Stack

| Layer | Tech |
|---|---|
| Smart Contract | Rust + Soroban SDK v22 |
| Network | Stellar Testnet |
| Frontend | React 18 + Vite |
| Wallet | Freighter via `@stellar/freighter-api` |
| RPC | Soroban RPC (`soroban-testnet.stellar.org`) |
| Hosting | Vercel |

---

## Contract Functions

```rust
// Propose a vow — proposer signs immediately, partner receives vow ID
propose_vow(proposer: Address, partner: Address, vow_text: String) -> u64

// Partner accepts and seals the vow — immutable after this
seal_vow(vow_id: u64, signer: Address)

// Read any vow from chain
get_vow(vow_id: u64) -> Vow

// All vow IDs associated with a wallet
get_wallet_vows(wallet: Address) -> Vec<u64>

// Global vow count
vow_count() -> u64
```

---

## Run Locally

### Prerequisites
- [Rust](https://rustup.rs/) + `wasm32-unknown-unknown` target
- [Stellar CLI](https://github.com/stellar/stellar-cli) v25+
- [Node.js](https://nodejs.org/) 20+
- [Freighter wallet](https://freighter.app/) browser extension

### Deploy Contract + Run Frontend

```bash
# 1. Deploy contract to testnet (takes ~2 min)
chmod +x scripts/deploy.sh && ./scripts/deploy.sh

# 2. Install and run frontend
cd frontend
npm install
npm run dev
# → http://localhost:5173
```

### Publish to GitHub + Vercel

```bash
chmod +x scripts/publish.sh && ./scripts/publish.sh
```

---

## Architecture

```
User (Freighter Wallet)
       │
       ▼
React Frontend (Vite)
       │  @stellar/freighter-api  ← signs transactions
       │  @stellar/stellar-sdk   ← builds + submits txs
       ▼
Soroban RPC (testnet)
       │
       ▼
ChainVow Contract (WASM on Stellar testnet)
       │
       ├── propose_vow()  → stores Vow struct, marks proposer_signed=true
       └── seal_vow()     → verifies partner auth, sets sealed=true
```

---

## Project #1 of 30 — Stellar Hackathon MOU

Built as part of a 30-project full-stack Soroban hackathon submission.
