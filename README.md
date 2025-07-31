# 🚗 Tesla IoT Dashboard + BARK Protocol Integration

A real-time Tesla vehicle dashboard integrated with the Solana blockchain, featuring $BARK token utilities, NFT customization, wallet integration, and XP-based rewards for driving, staking, and more.

![Architecture Diagram](./docs/architecture-diagram.png)

---

## 📌 Features

- 🔌 **Live Tesla Vehicle Telemetry** (Driving, Parked, Charging)
- 📡 **WebSocket + SSE Streaming** of vehicle data
- 🪙 **Solana Wallet Integration** (Phantom, Backpack, Solflare)
- 💰 **Real-Time Token Portfolio** ( $BTC, $BARK, SOL, USDC)
- 🎖️ **NFT Skin Selector** and XP leaderboard
- 🗺️ **Trip History Replay** with CSV logging and map overlays
- 🏁 **Anchor-based Staking Contract** (for XP, skins, and rewards)
- 🧾 **Solana Pay** support for NFT skin purchases and premium features

---

## ⚙️ Tech Stack

### 🖥 Frontend
- React + Tailwind CSS + TypeScript
- Solana Wallet Adapter
- WebSocket + Server-Sent Events (SSE)
- Leaflet / Mapbox (for trip maps)
- Solana Pay QR Code Generator
- XP / NFT / Token Portfolio Display

### 🛠 Backend
- Node.js + Express + TypeScript
- Mock Tesla Telemetry API
- WebSocket and SSE server
- CSV logger for trip data
- Token metadata fetcher
- Solana integration (via `@solana/web3.js`, `@coral-xyz/anchor`)

### 🔗 Blockchain
- Solana Devnet / Mainnet
- $BARK SPL Tokens
- Anchor staking contract
- Metaplex NFT minting
- Dialect Blinks for on-chain triggers
- Solana Pay transactions

---

## 🚀 Getting Started

### 1. Clone the Repository
```bash
git clone https://github.com/bark-protocol/tesla-dashboard.git
cd tesla-dashboard
````

### 2. Install Dependencies

```bash
# Backend
cd backend
npm install

# Frontend
cd ../frontend
npm install
```

### 3. Setup Environment Variables

#### `.env` (Frontend)

```env
NEXT_PUBLIC_SOLANA_NETWORK=devnet
NEXT_PUBLIC_RPC_URL=https://api.devnet.solana.com
NEXT_PUBLIC_MINT_API_URL=https://api.actions.barkprotocol.net/mint
NEXT_PUBLIC_WALLET_ADDRESS=YOUR_WALLET_ADDRESS
```

#### `.env` (Backend)

```env
PORT=4000
TESLA_API_KEY=mock-or-real-key
SOLANA_NETWORK=devnet
TOKEN_PROGRAM_ID=Tokenkeg...
NFT_PROGRAM_ID=gEb7n...
```

### 4. Run the App

#### Backend

```bash
cd backend
npm run dev
```

#### Frontend

```bash
cd frontend
npm run dev
```

Access the dashboard at: [http://localhost:3000](http://localhost:3000)

---

## 📂 Project Structure

```
/backend
  └── api/tesla.mock.ts
  └── websocket/
  └── solana/
  └── utils/
  └── data/trips/
/frontend
  └── components/
  └── pages/
  └── lib/
  └── public/
/docs
  └── architecture-diagram.png
```

---

## 📈 Telemetry + Logging

* Real-time streaming: `/stream/<vehicle_id>`
* API Explorer: `/apiliste` + `api-liste.json`
* Trips logged to: `data/<vehicle_id>/trips/YYYY-MM-DD.csv`

---

## 📬 Contact & Credits

* Powered by **BARK Protocol**
* Solana Integration
* Tesla data: configurable via JSON or real API

---

## 📜 License

MIT License. See `LICENSE.md` for details.
