<div align="center">

# Equinox Finance

**Bridging Real Estate & DeFi on Stellar**

[![Monorepo CI](https://github.com/Equinox-Finance/equinox-dapp/actions/workflows/monorepo-ci.yml/badge.svg)](https://github.com/Equinox-Finance/equinox-dapp/actions/workflows/monorepo-ci.yml)
[![API CI](https://github.com/Equinox-Finance/equinox-dapp/actions/workflows/api-ci.yml/badge.svg)](https://github.com/Equinox-Finance/equinox-dapp/actions/workflows/api-ci.yml)
[![Webapp CI](https://github.com/Equinox-Finance/equinox-dapp/actions/workflows/webapp-ci.yml/badge.svg)](https://github.com/Equinox-Finance/equinox-dapp/actions/workflows/webapp-ci.yml)
[![Contracts CI](https://github.com/Equinox-Finance/equinox-dapp/actions/workflows/contracts-ci.yml/badge.svg)](https://github.com/Equinox-Finance/equinox-dapp/actions/workflows/contracts-ci.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

**Equinox Finance** is an institutional-grade platform bridging traditional real estate with decentralized finance (DeFi) on Stellar. Property owners tokenize real-world assets (RWA) into on-chain shares, and investors can then use those shares as collateral to access DeFi lending pools — all on Stellar's high-throughput, low-cost network.

[Getting Started](#getting-started) · [Architecture](#architecture) · [Tech Stack](#tech-stack) · [Contributing](CONTRIBUTING.md) · [Documentation](docs/)

</div>

---

## Project Overview

Equinox Finance addresses two tightly coupled structural problems at the intersection of real estate and decentralized finance:

1. **Real Estate Illiquidity.** Tokenizing property into fractional on-chain shares transforms real-world assets into programmable, tradeable, and transferable blockchain tokens — unlocking liquidity in one of the world's largest asset classes.

2. **Collateral Scarcity in DeFi.** By accepting tokenized real estate as collateral, Equinox expands DeFi lending capacity beyond volatile crypto-native assets, anchoring it to tangible, regulated property with intrinsic value.

The platform is purpose-built for institutional compliance — KYC/AML enforcement, role-based access control, and tamper-evident audit trails — while remaining composable and permissionless for DeFi participants.

---

## Key Features

### Real Estate Tokenization

- **Fractional ownership** of individual properties, tracked entirely on-chain via Soroban smart contracts
- **KYC/AML compliance** enforced at the application and contract layer
- **Controlled minting and burning** with role-gated administrative operations
- **Immutable property metadata** with verifiable audit history

### DeFi Lending Protocol

- **Collateralized borrowing** using tokenized real estate shares as pledged assets
- **Privacy-configurable lending pools** designed for institutional participants
- **Automated interest accrual** and liquidation mechanisms governed by smart contracts
- **Oracle-integrated asset valuation** ensuring accurate collateralization ratios

### Compliance & Security

- **Wallet-based authentication** via Stellar Ed25519 signatures — no passwords, no centralized identity provider
- **Role-based access control** spanning admin, operator, and user tiers
- **Webhook signature verification** for all external integrations
- **Rate limiting**, input sanitization, and structured audit logging throughout the API layer

---

## Architecture

### System Overview

```
┌──────────────────────────────────────────────────────────────────────┐
│                        Equinox Finance Platform                      │
│                                                                      │
│  ┌─────────────────┐   ┌─────────────────┐   ┌───────────────────┐  │
│  │   Web Frontend  │   │   Backend API    │   │  Smart Contracts  │  │
│  │  Next.js + React│◄──►│  Elysia / Bun   │◄──►│  Soroban / Rust   │  │
│  │  localhost:3000 │   │  localhost:3001  │   │  Stellar Network  │  │
│  └────────┬────────┘   └────────┬────────┘   └───────────────────┘  │
│           │                     │                                    │
│           └──────────┬──────────┘                                    │
│                      ▼                                               │
│            ┌──────────────────┐                                      │
│            │  Shared Library  │                                      │
│            │  Types · Schemas │                                      │
│            │  Utils · SDK     │                                      │
│            └──────────────────┘                                      │
└──────────────────────────────────────────────────────────────────────┘
```

The repository is a **Bun monorepo** with four workspaces:

| Workspace          | Path              | Role                                                               |
| ------------------ | ----------------- | ------------------------------------------------------------------ |
| `@equinox/webapp`  | `apps/webapp`     | Next.js 16 frontend with React 19                                  |
| `@equinox/api`     | `apps/api`        | Elysia REST API running on Bun                                     |
| `@equinox/shared`  | `apps/shared`     | Types, schemas, utilities, and Stellar SDK helpers                 |
| Contracts          | `apps/contracts`  | Soroban smart contracts written in Rust                            |
| `@equinox/land`    | `apps/equinox-land` | Interactive real estate metaverse experience                    |

### Tokenization Flow

```
Property Owner → Submits Property → Frontend Validates → API Verifies KYC
→ Soroban Contract Mints Shares → On-Chain Event Emitted → API Indexes
→ Frontend Reflects Updated Portfolio
```

### Lending Flow

```
Investor → Requests Loan → Frontend Calculates Available Collateral
→ API Verifies On-Chain Share Balance → Contract Validates Collateral Ratio
→ Contract Disburses Funds → Frontend Updates Lending Position
```

---

## Tech Stack

| Layer                  | Technology                                                      |
| ---------------------- | --------------------------------------------------------------- |
| **Frontend**           | Next.js 16, React 19, TypeScript, Tailwind CSS 4, Zustand, Zod  |
| **Backend API**        | Elysia, Bun runtime, TypeScript, Drizzle ORM, Zod               |
| **Database**           | PostgreSQL (Drizzle migrations), Redis (optional caching)       |
| **Smart Contracts**    | Rust, Soroban SDK 25, WASM compilation target                   |
| **Blockchain**         | Stellar (Testnet / Mainnet), Horizon REST API, Soroban RPC      |
| **Wallet Integration** | `@creit.tech/stellar-wallets-kit`                               |
| **Testing**            | `bun test`, `@testing-library/react`                            |
| **CI/CD**              | GitHub Actions                                                  |
| **Tooling**            | Bun 1.2+, ESLint 9, Prettier                                    |

---

## Getting Started

### Prerequisites

- [Bun](https://bun.sh) ≥ 1.0
- Docker (for PostgreSQL and Redis)

### Installation

```bash
git clone https://github.com/Equinox-Finance/equinox-dapp.git
cd equinox
bun install
```

### Running Locally

```bash
# Copy environment variables
cp apps/api/.env.example apps/api/.env
cp apps/webapp/.env.example apps/webapp/.env.local

# Start infrastructure services
docker compose -f docker-compose.dev.yml up -d

# Launch all applications
bun run dev
```

For detailed setup instructions, see [docs/local-setup.md](docs/local-setup.md).

---

## Available Scripts

Run from the repository root:

| Script                     | Description                                    |
| -------------------------- | ---------------------------------------------- |
| `bun run dev`              | Start frontend, API, and land app concurrently |
| `bun run build`            | Build all workspaces                           |
| `bun run test`             | Run all workspace test suites                  |
| `bun run lint`             | Lint all workspaces                            |
| `bun run typecheck`        | Type-check all workspaces                      |
| `bun run format`           | Format all files with Prettier                 |
| `bun run clean`            | Remove all build artifacts and `node_modules`  |
| `bun run build:contracts`  | Build Soroban contracts via `scripts/build.sh` |
| `bun run deploy:contracts` | Deploy contracts via `scripts/deploy.sh`       |

---

## Environment Variables

All required environment variables are documented in [`docs/deployment/environment-variables.md`](docs/deployment/environment-variables.md). The canonical source is `apps/api/.env.example`.

Key categories:

- **Database** — PostgreSQL connection string and pool settings
- **API Server** — Port, environment, log level
- **Security** — Webhook secret, operations credential, allowed admin wallets
- **Stellar / Soroban** — Horizon URL, RPC URL, network passphrase, admin keypair, contract IDs
- **KYC** — Upload directory for compliance documents

> **Security note:** Never commit `.env` files. `STELLAR_ADMIN_SECRET` is a root credential — treat it as a private key and load it from a secrets manager in production.

---

## CI/CD

Equinox Finance runs five independent GitHub Actions workflows on every push and pull request to `main` and `develop`:

| Workflow  | File               | Checks                                                                                                |
| --------- | ------------------ | ----------------------------------------------------------------------------------------------------- |
| Monorepo  | `monorepo-ci.yml`  | Workspace integrity, dependency audit, bundle sizes, cross-workspace integration, security compliance |
| API       | `api-ci.yml`       | Lint, type-check, unit tests, build                                                                   |
| Webapp    | `webapp-ci.yml`    | Lint, type-check, unit tests, build                                                                   |
| Shared    | `shared-ci.yml`    | Lint, type-check, build                                                                               |
| Contracts | `contracts-ci.yml` | Rust format, Clippy, unit tests, WASM build                                                           |

All five workflows must pass before any pull request is merged.

---

## Project Structure

```
equinox/
├── apps/
│   ├── api/               # Elysia/Bun backend API
│   │   ├── src/
│   │   │   ├── controllers/
│   │   │   ├── services/
│   │   │   ├── repositories/
│   │   │   ├── routes/
│   │   │   ├── middleware/
│   │   │   ├── db/
│   │   │   └── workers/
│   │   └── drizzle/       # Database migrations
│   ├── webapp/            # Next.js frontend
│   │   └── src/
│   │       ├── app/       # App Router pages and layouts
│   │       ├── components/
│   │       ├── hooks/
│   │       ├── services/
│   │       └── types/
│   ├── equinox-land/      # Interactive metaverse experience
│   ├── contracts/         # Soroban smart contracts (Rust)
│   │   └── contracts/
│   │       └── defi-rwa/
│   └── shared/            # Shared TypeScript library
├── docs/
│   ├── api/               # API endpoint documentation
│   ├── architecture/      # System design documents
│   ├── deployment/        # Deployment and environment guides
│   ├── guides/            # Developer getting-started guides
│   ├── operations/        # Runbooks for production operations
│   └── testing/           # Testing strategy and smoke tests
├── scripts/               # Build and deployment shell scripts
└── .github/
    └── workflows/         # GitHub Actions CI definitions
```

---

## Documentation

| Document                                                                               | Description                             |
| -------------------------------------------------------------------------------------- | --------------------------------------- |
| [`docs/guides/getting-started.md`](docs/guides/getting-started.md)                     | Full local setup walkthrough            |
| [`docs/architecture/system-architecture.md`](docs/architecture/system-architecture.md) | System design and component breakdown   |
| [`docs/deployment/environment-variables.md`](docs/deployment/environment-variables.md) | Complete environment variable reference |
| [`docs/deployment/deploy-contracts.md`](docs/deployment/deploy-contracts.md)           | Contract deployment to Stellar networks |
| [`docs/api/overview.md`](docs/api/overview.md)                                         | API overview and authentication         |
| [`docs/api/minting-workflow.md`](docs/api/minting-workflow.md)                         | Property tokenization API flow          |
| [`docs/api/kyc-workflow.md`](docs/api/kyc-workflow.md)                                 | KYC verification API flow               |
| [`CONTRIBUTING.md`](CONTRIBUTING.md)                                                   | Contribution workflow and standards     |

---

## Contributing

We welcome contributions. Equinox Finance uses a **fork-based workflow** — all changes must come through a fork and pull request. No direct pushes to `main` or `develop`.

Read the full contribution guide before submitting your first pull request: **[CONTRIBUTING.md](CONTRIBUTING.md)**

---

## License

[MIT](LICENSE) — Acachete Labs
