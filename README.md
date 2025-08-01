# 🚗 Tesla IoT Dashboard + BARK Protocol

A real-time Tesla dashboard with Solana blockchain integration — combining \$AI6 and \$BARK token utilities, NFT skin rewards, wallet tracking, staking, APRS/WX packet transmission, SMS alerts, supply chain validation with Pyth, and payments with Solana Pay.

![Architecture Diagram](./docs/architecture-diagram.png)

---

## 🚀 Key Features

### 🚘 Tesla Vehicle Dashboard

* Real-time vehicle state: driving, charging, parked
* GPS trail rendering and interactive trip replay
* Tire pressure, battery %, media playback, climate info
* Cached vehicle state + offline fallback
* API logs and dynamic polling intervals

### 🔐 Solana Blockchain Integration

* \$BARK SPL Token tracking (BTC, SOL, USDC included)
* Solana wallet connect (Phantom, Backpack, Solflare)
* XP system + Anchor staking contract
* NFT minting via **Metaplex**
* NFT skin selection + Leaderboard
* On-chain triggers using **Solana Actions** and **Dialect Blinks**
* **Solana Pay** QR payment support for NFT skins and dashboard features

### 🛰 APRS-IS Support

* Configurable call sign, WX mode, passcode
* EU APRS-IS packet transmission (position + temperature)
* 30s while driving, 10min idle minimum update interval

### 📲 Infobip SMS Notifications

* Real-time driver alerts (optional)
* Restrict to “Driving only” or “Always”
* Message sender ID and API key configurable
* All SMS stored to `data/sms.log`

### 📦 Pyth Oracle Integration

* Real-time Solana-based oracle pricing for:

  * \$AI6 / \$BARK / SOL / USDC tokens
  * Vehicle-linked supply chain assets (energy, battery usage)
* Cross-check pricing with on-chain Pyth feeds
* Plug-in ready for AI-driven logistics forecasting

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

* `WalletConnect`
* `VehicleStatusCard`
* `TripMap`
* `TokenPortfolio`
* `MediaPlayer`
* `Leaderboard`
* `NFTSkinSelector`

### ⚙️ Backend: Node.js (Express) + SSE + Solana SDKs

| Endpoint              | Description                               |
| --------------------- | ----------------------------------------- |
| `/api/state`          | Current vehicle state (live or cached)    |
| `/api/occupant`       | Wake vehicle on occupant detection        |
| `/api/config`         | Save/load APRS, SMS, polling, settings    |
| `/stream/:vehicle_id` | Live SSE stream of vehicle data           |
| `/apiliste`           | List of latest Tesla API variables/values |

Services:

* Tesla Poller: dynamic intervals, offline caching
* Infobip SMS Service
* APRS Packet Sender
* CSV Trip Logger
* Token Metadata Fetcher
* Pyth Oracle Reader
* NFT + Solana Actions + Metaplex Handler
* Anchor staking pool client

---

## 🔗 Solana Integration Stack

| Component                 | Role                                                  |
| ------------------------- | ----------------------------------------------------- |
| `@solana/web3.js`         | Blockchain interaction (transactions, token balances) |
| `@project-serum/anchor`   | XP staking via Anchor contracts                       |
| `@metaplex-foundation/js` | NFT minting and metadata via Metaplex                 |
| `@solana/spl-token`       | SPL Token management for \$BARK, etc.                 |
| `@solana/pay`             | Solana Pay QR code generator and validator            |
| `@pythnetwork/client`     | Real-time oracle feeds for supply chain validation    |
| `@dialectlabs/sdk`        | On-chain event + alert triggers via Blinks            |

---

## 📦 Project Structure (Monorepo)

```
/apps
├── frontend
│   ├── components/
│   ├── pages/
│   ├── lib/
│   └── public/
├── backend
│   ├── routes/
│   ├── services/
│   │   ├── teslaPoller.ts
│   │   ├── infobip.ts
│   │   ├── aprs.ts
│   │   └── solana/
│   │       ├── wallet.ts
│   │       ├── metaplex.ts
│   │       ├── pyth.ts
│   │       └── actions.ts
│   └── data/
│       ├── <vehicle_id>/cache.json
│       └── trips/
/packages
├── ui
├── utils
└── sdk
/docs
└── architecture-diagram.png
```

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

## 🧪 Local Development

### Backend (Port 8013)

```bash
cd apps/backend
npm run dev
```

### Frontend (Port 3000)

```bash
cd apps/frontend
npm run dev
```

Open:

* [http://localhost:3000](http://localhost:3000) → React dashboard
* [http://localhost:8013/config](http://localhost:8013/config) → backend UI

---

## 📜 License

MIT License – See `LICENSE.md`

---

## 🤝 Contributing

PRs and feedback are welcome. This project bridges the physical and decentralized world — contributions from Web3, IoT, and Tesla communities encouraged.
