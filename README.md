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

Components include:

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

Services:

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

## 📦 Project Structure (Monorepo)

```

/apps
├── frontend
│   ├── components/
│   ├── hooks/                  ← 🧠 Custom React hooks (useVehicleState, useWalletBalance, etc.)
│   ├── types/                  ← 📚 TypeScript shared types (Vehicle, Trip, NFTMetadata, etc.)
│   ├── lib/
│   │   ├── supabase.ts         ← 📦 Supabase client for trip/session logs
│   │   └── solana.ts           ← 🔗 Solana connection, wallet utils
│   ├── api/                    ← 🔌 Optional API routes for frontend
│   └── migrations/            ← 📜 DB schema changes, if using Supabase CLI
├── backend
│   ├── routes/
│   │   ├── trip.ts             ← 🚗 POST/GET trip log data
│   │   └── mint.ts             ← 🪙 Trigger NFT mint from trip stats
│   ├── lib/
│   │   ├── supabase.ts         ← 🔐 Supabase server SDK
│   │   └── types.ts            ← 🔄 Shared backend types
│   ├── services/
│   ├── db/
│   │   └── schema.sql          ← 🧩 Supabase SQL migrations
│   └── cron/
│       └── pythPoller.ts       ← ⏰ Poll Pyth oracle prices periodically
/packages
├── sdk
│   ├── nft/
│   ├── staking/
│   └── actions/
└── utils
├── energy.ts              ← ⚡ Normalize Tesla energy usage to on-chain logic
├── token.ts               ← 💰 Format SPL token balances
└── analytics.ts           ← 📈 Drive stats → NFT metadata or DAO proposal input

````

---

## 📦 Supabase (Postgres) Example

```sql
create table trips (
  id uuid primary key default uuid_generate_v4(),
  wallet text not null,
  distance_km float,
  duration_minutes int,
  co2_saved_kg float,
  metadata jsonb,
  created_at timestamp default now()
);

create table payments (
  id uuid primary key default uuid_generate_v4(),
  wallet text not null,
  amount float,
  tx_signature text,
  created_at timestamp default now()
);
````

---

## ⚙️ Environment Variables

### `.env` (Frontend)

```env
NEXT_PUBLIC_SOLANA_NETWORK=devnet
NEXT_PUBLIC_RPC_URL=https://api.devnet.solana.com
NEXT_PUBLIC_MINT_API_URL=https://api.actions.barkprotocol.net/mint
NEXT_PUBLIC_WALLET_ADDRESS=YOUR_WALLET
```

### `.env` (Backend)

```env
PORT=8013
TESLA_API_KEY=mock-or-real-key
SOLANA_NETWORK=devnet
TOKEN_PROGRAM_ID=Tokenkeg...
NFT_PROGRAM_ID=gEb7n...
INFOBIP_API_KEY=your-infobip-key
PYTH_PROGRAM_ID=your-pyth-program
```

---

## 🔗 Backend Integrations

### API Endpoints

| Method | Endpoint      | Description                                                 |
| ------ | ------------- | ----------------------------------------------------------- |
| GET    | `/api/trips`  | Fetch user driving history                                  |
| POST   | `/api/mint`   | Mint NFTs with drive-linked metadata                        |
| GET    | `/api/prices` | Read real-time \$BARK / \$AI6 / SOL prices from Pyth oracle |

---

### React Hooks

* `useTripHistory()` — Fetch and manage user trip logs.
* `useMintNFT()` — Mint NFTs with metadata based on driving stats.
* `usePythPrices()` — Subscribe to real-time token price updates.

---

## 🧪 Local Development

> **Prerequisites:** Node.js >=16, npm/yarn, Supabase CLI (optional)

### Backend

```bash
cd apps/backend
npm install
npm run dev
```

Runs backend API on [http://localhost:8013](http://localhost:8013)

### Frontend

```bash
cd apps/frontend
npm install
npm run dev
```

Runs frontend app on [http://localhost:3000](http://localhost:3000)

---

## 🧑‍💻 Development Setup Prompt

Run this command in your terminal to set up everything quickly:

```bash
# Clone repo, install deps and start all services concurrently
git clone https://github.com/your-org/tesla-bark.git
cd tesla-bark

# Install all packages in monorepo
npm install

# Start frontend and backend concurrently (using `concurrently` or `turbo`)
npm run dev
```

You can customize your `.env` files as needed:

* `.env.local` for frontend config
* `.env` for backend config

---

## 📜 License

MIT License — see [LICENSE.md](LICENSE.md)

---

## 🤝 Contributing

Contributions welcome! PRs, issues, and feedback help improve this bridge between Tesla IoT, Solana blockchain, and real-world sustainability.

---

## 📫 Contact

BARK Protocol Team — [contact@barkprotocol.net](mailto:contact@barkprotocol.net)
