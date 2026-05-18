# ⬡ ArcKit

> The fastest way to build on [Circle's Arc blockchain](https://arc.network).

ArcKit is a CLI scaffold tool that gets you from zero to a deployed Arc smart contract in minutes — with pre-built contract templates, a full test suite, and Arc testnet config baked in.

```bash
npx arckit init my-payment-app
```

---

## Why ArcKit?

Arc is a brand new L1 blockchain purpose-built for stablecoin finance. That means the existing EVM tooling ecosystem hasn't caught up yet — no boilerplates, no Arc-specific testing utilities, no quickstart templates.

ArcKit fills that gap.

- **Zero config** — Arc testnet RPC, chain ID, and USDC address pre-configured
- **Contract templates** — production-ready Solidity contracts for common Arc use cases
- **Full test suites** — every template ships with comprehensive Hardhat tests
- **One command deploy** — scaffold → compile → test → deploy in under 5 minutes

---

## Installation

```bash
npm install -g arckit
```

Or use without installing:

```bash
npx arckit init my-app
```

---

## Quick Start

```bash
# 1. Scaffold a new project
arckit init my-escrow-app

# 2. Go into the project
cd my-escrow-app

# 3. Install dependencies
npm install

# 4. Set up environment
cp .env.example .env
# Add your wallet private key to .env

# 5. Compile
npx hardhat compile

# 6. Test locally
npx hardhat test

# 7. Deploy to Arc Testnet
npx hardhat run scripts/deploy.js --network arc-testnet
```

That's it. Your contract is live on Arc Testnet.

---

## Commands

### `arckit init [project-name]`

Scaffold a new Arc project interactively.

```bash
arckit init my-app
arckit init my-app --template escrow
arckit init my-app --template payment
arckit init my-app --template subscription
```

**Options:**

| Flag | Description | Default |
|------|-------------|---------|
| `-t, --template` | Contract template to use | `escrow` |

---

### `arckit network`

Print Arc Testnet network config for adding to MetaMask or any EVM wallet.

```bash
arckit network

# Output:
# Network Name:   Arc Testnet
# RPC URL:        https://rpc.testnet.arc.network
# Chain ID:       5042002
# Currency:       USDC (native gas)
# Explorer:       https://testnet.arcscan.app
```

---

### `arckit faucet`

Open the Arc testnet USDC faucet in your browser.

```bash
arckit faucet
```

---

## Contract Templates

### 🔐 Escrow (`--template escrow`)

Trustless milestone-based escrow denominated in USDC.

**Use cases:** Freelance payments, project milestones, dispute resolution, trade finance.

**Features:**
- Multiple milestones per escrow
- Arbiter-controlled release and refund
- Depositor self-release after configurable timeout
- ReentrancyGuard protected
- Custom error types for gas efficiency

```solidity
// Create a 3-milestone escrow
await escrow.createEscrow(
  beneficiary,           // who receives payment
  arbiter,               // trusted third party
  [100e6, 200e6, 300e6], // milestone amounts in USDC (6 decimals)
  7 * 24 * 60 * 60       // 7-day timeout for self-release
);

// Arbiter releases milestone 0
await escrow.releaseMilestone(escrowId, 0);

// Arbiter refunds remaining balance
await escrow.refund(escrowId);
```

**Tests included:** 7 tests covering create, release, double-release prevention, refund, non-arbiter access control, and timeout self-release.

---

### 💸 Payment (`--template payment`) — Coming in v0.2

USDC payment splitter for multi-recipient distributions.

**Use cases:** Revenue sharing, team payroll, royalty splits, DAO distributions.

---

### 🔁 Subscription (`--template subscription`) — Coming in v0.2

Recurring USDC subscription billing with on-chain enforcement.

**Use cases:** SaaS billing, membership fees, recurring donations.

---

## Project Structure

When you run `arckit init my-app`, you get:

```
my-app/
├── contracts/
│   ├── ArcEscrow.sol        # Main contract
│   └── MockUSDC.sol         # Local test USDC
├── scripts/
│   └── deploy.js            # Deployment script
├── test/
│   └── ArcEscrow.test.js    # Full test suite
├── hardhat.config.js        # Arc testnet pre-configured
├── .env.example             # Environment template
└── README.md
```

---

## Arc Testnet

| Resource | URL |
|----------|-----|
| Explorer | https://testnet.arcscan.app |
| USDC Faucet | https://faucet.circle.com |
| Docs | https://docs.arc.network |
| Circle Developer | https://circle.com/developer |

**Network Config:**

| Setting | Value |
|---------|-------|
| Network Name | Arc Testnet |
| RPC URL | https://rpc.testnet.arc.network |
| Chain ID | 5042002 |
| Native Gas Token | USDC |
| Explorer | https://testnet.arcscan.app |

---

## Environment Variables

```env
# Required
PRIVATE_KEY=0x...                          # Your wallet private key

# Optional (defaults provided)
ARC_RPC_URL=https://rpc.testnet.arc.network
USDC_ADDRESS=0x1c7D4B196Cb0C7B01d743Fbc6116a902379C7238
ARC_EXPLORER_API_KEY=                      # For contract verification
```

---

## Requirements

- Node.js 18+
- npm or yarn
- A wallet with Arc Testnet USDC (get from [faucet.circle.com](https://faucet.circle.com))

---

## Live Deployment

ArcKit was used to deploy a live `ArcEscrow` contract on Arc Testnet:

**Contract:** [`0x90c4A75CE69693E404cdD92f36dFcfd0fa1A3d32`](https://testnet.arcscan.app/address/0x90c4A75CE69693E404cdD92f36dFcfd0fa1A3d32)

---

## Roadmap

- [x] v0.1 — CLI scaffold, ArcEscrow template, 7 tests, Arc testnet deploy
- [ ] v0.2 — Payment splitter template, Subscription template
- [ ] v0.3 — npm publish, interactive faucet integration
- [ ] v0.4 — Arc mainnet support, contract verification

---

## Contributing

PRs welcome! To add a new template:

1. Add your contract to `src/templates/contract-templates.js`
2. Add tests to `src/templates/script-templates.js`
3. Register the template name in `src/index.js`

---

## License

MIT © [jbnikky13](https://github.com/jbnikky13)

---

<p align="center">Built for the Arc ecosystem ⬡</p>