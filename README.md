# 🚗 Tesla IoT Dashboard + BARK Protocol

A real-time Tesla dashboard with Solana blockchain integration — combining $AI6 and $BARK token utilities, NFT skin rewards, wallet tracking, staking, APRS/WX packet transmission, SMS alerts, supply chain validation with Pyth, and payments with Solana Pay.

![Architecture Diagram](./docs/architecture-diagram.png)

---

## 🚀 Key Features

### 🚘 Tesla Vehicle Dashboard

- Real-time vehicle state: driving, charging, parked
- GPS trail rendering and interactive trip replay
- Tire pressure, battery %, media playback, climate info
- Cached vehicle state + offline fallback
- API logs and dynamic polling intervals

### 🔐 Solana Blockchain Integration

- $BARK SPL Token tracking (BTC, SOL, USDC included)
- Solana wallet connect (Phantom, Backpack, Solflare)
- XP system + Anchor staking contract
- NFT minting via **Metaplex**
- NFT skin selection + Leaderboard
- On-chain triggers using **Solana Actions** and **Dialect Blinks**
- **Solana Pay** QR payment support for NFT skins and dashboard features

### 🛰 APRS-IS Support

- Configurable call sign, WX mode, passcode
- EU APRS-IS packet transmission (position + temperature)
- 30s while driving, 10min idle minimum update interval

### 📲 Infobip SMS Notifications

- Real-time driver alerts (optional)
- Restrict to “Driving only” or “Always”
- Message sender ID and API key configurable
- All SMS stored to `data/sms.log`

### 📦 Pyth Oracle Integration

- Real-time Solana-based oracle pricing for:
  - $AI6 / $BARK / SOL / USDC tokens
  - Vehicle-linked supply chain assets (energy, battery usage)
- Cross-check pricing with on-chain Pyth feeds
- Plug-in ready for AI-driven logistics forecasting

---

## 🛠 Architecture

### 🧩 Frontend: Next.js + Tailwind + TypeScript

| Route      | Description                                   |
| ---------- | --------------------------------------------- |
| `/`        | Tesla dashboard: vehicle state, XP, portfolio |
| `/config`  | APRS, polling, SMS setup, caching             |
| `/sms`     | SMS message logs                              |
| `/history` | CSV trip logs + interactive replay map        |
| `/vehicle` | Full raw vehicle snapshot                     |

Core components include:

- `WalletConnect`
- `VehicleStatusCard`
- `TripMap`
- `TokenPortfolio`
- `MediaPlayer`
- `Leaderboard`
- `NFTSkinSelector`

### ⚙️ Backend: Node.js (Express) + SSE + Solana SDKs

| Endpoint              | Description                               |
| --------------------- | ----------------------------------------- |
| `/api/state`          | Current vehicle state (live or cached)    |
| `/api/occupant`       | Wake vehicle on occupant detection        |
| `/api/config`         | Save/load APRS, SMS, polling, settings    |
| `/stream/:vehicle_id` | Live SSE stream of vehicle data           |
| `/apiliste`           | List of latest Tesla API variables/values |

Backend services:

- Tesla Poller: dynamic intervals, offline caching
- Infobip SMS Service
- APRS Packet Sender
- CSV Trip Logger
- Token Metadata Fetcher
- Pyth Oracle Reader
- NFT + Solana Actions + Metaplex Handler
- Anchor staking pool client

---

## 🔗 Solana Integration Stack

| Component                 | Role                                                  |
| ------------------------- | ----------------------------------------------------- |
| `@solana/web3.js`         | Blockchain interaction (transactions, token balances) |
| `@project-serum/anchor`   | XP staking via Anchor contracts                       |
| `@metaplex-foundation/js` | NFT minting and metadata via Metaplex                 |
| `@solana/spl-token`       | SPL Token management for $BARK, etc.                 |
| `@solana/pay`             | Solana Pay QR code generator and validator            |
| `@pythnetwork/client`     | Real-time oracle feeds for supply chain validation    |
| `@dialectlabs/sdk`        | On-chain event + alert triggers via Blinks            |

---

## 🗄️ Monorepo & Supabase Setup

This project follows a **monorepo** architecture for better modularity and shared dependencies, separating frontend, backend, and shared libraries into dedicated packages:

```

/apps
/frontend       # Next.js Tesla Dashboard UI
/backend        # Node.js Express API & Services
/libs
/common         # Shared utilities, types, and constants
/solana         # Solana blockchain helpers & hooks
/ui             # Reusable UI components & styles

````

### Why Monorepo?

- Simplifies dependency management
- Encourages code reuse between frontend & backend
- Easier versioning and testing workflows
- Streamlined CI/CD pipeline

We use **Yarn Workspaces** or **npm Workspaces** to manage this structure.

---

### Supabase Integration

The project uses **Supabase** as a backend-as-a-service for:

- User authentication and sessions
- Real-time syncing of vehicle & token data
- Persistent storage for app configurations
- SMS logs and notifications tracking

#### Setup Supabase

1. Create a new project on [Supabase.io](https://supabase.io)
2. Get your **Project URL** and **Anon Key** from the Supabase dashboard
3. Add the following to your `.env.local` (frontend & backend):

```env
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key  # Backend only, keep secret
````

4. Initialize Supabase clients in your apps:

* **Frontend:**

```ts
import { createClient } from '@supabase/supabase-js'

const supabase = createClient(
  process.env.NEXT_PUBLIC_SUPABASE_URL!,
  process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!
)

export default supabase
```

* **Backend:**

```ts
import { createClient } from '@supabase/supabase-js'

const supabaseAdmin = createClient(
  process.env.NEXT_PUBLIC_SUPABASE_URL!,
  process.env.SUPABASE_SERVICE_ROLE_KEY!
)

export default supabaseAdmin
```

---

## ⚙️ Environment Variables

### `.env.local` (Frontend & Backend)

```env
# Supabase
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key  # Backend only

# Solana
NEXT_PUBLIC_SOLANA_NETWORK=devnet
NEXT_PUBLIC_RPC_URL=https://api.devnet.solana.com
NEXT_PUBLIC_MINT_API_URL=https://api.actions.barkprotocol.net/mint
NEXT_PUBLIC_WALLET_ADDRESS=YOUR_WALLET_ADDRESS

# Tesla API
TESLA_API_KEY=your-tesla-api-key

# Infobip SMS
INFOBIP_API_KEY=your-infobip-key

# Pyth Oracle
PYTH_PROGRAM_ID=your-pyth-program
```

---

## 🧪 Local Development

### Install dependencies and run

```bash
# Install all dependencies across the monorepo
yarn install

# Start backend server (default port 8013)
yarn workspace backend dev

# Start frontend app (default port 3000)
yarn workspace frontend dev
```

Open your browser to:

* [http://localhost:3000](http://localhost:3000) → React Tesla Dashboard
* [http://localhost:8013/config](http://localhost:8013/config) → Backend config UI

---

## 🎨 Theming & Styling

* Brand colors: whites, blacks, light grays, icons in `#d4c89d`
* Fonts: Inter and Geist, with BARK logo using Inter SemiBold uppercase
* Tailwind CSS with dark/light mode toggling
* Theme provider with React Context to switch between modes

---

## 🧪 Storybook Component Preview

To develop UI components in isolation, run:

```bash
cd apps/frontend
npm run storybook
```

Storybook showcases the reusable UI components with the brand style applied.

---

## 📜 License

MIT License – See `LICENSE.md`

---

## 🤝 Contributing

PRs and feedback welcome! This project bridges the physical and decentralized world — contributions from Web3, IoT, and Tesla communities encouraged.

---

## 📚 Docs

Detailed markdown guides live in `/docs`:

* [01-theming.md](./docs/01-theming.md) — Theming and styling setup
* [02-storybook.md](./docs/02-storybook.md) — Storybook UI component previews
* [03-deployment.md](./docs/03-deployment.md) — Deployment instructions
* [04-component-examples.md](./docs/04-component-examples.md) — Brand-styled component templates

---

Thank you for exploring the Tesla IoT + BARK Protocol project!
