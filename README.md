

---

# 🏝️ Gangsta Island / Chase 101 - Digital Game Platform

> **A custom-built, cross-platform digital board game engine with Discord integration, real-money economy, and mobile app.**

---

## 📋 Table of Contents

1. [Project Overview](#project-overview)
2. [Tech Stack](#tech-stack)
3. [System Architecture](#system-architecture)
4. [Features (Full Scope)](#features-full-scope)
5. [Prerequisites](#prerequisites)
6. [Installation & Setup](#installation--setup)
7. [Environment Variables](#environment-variables)
8. [Database Schema](#database-schema)
9. [API Documentation](#api-documentation)
10. [Discord Bot Commands](#discord-bot-commands)
11. [Game Logic](#game-logic)
12. [Milestone Breakdown](#milestone-breakdown)
13. [Folder Structure](#folder-structure)
14. [Deployment](#deployment)
15. [Testing](#testing)
16. [Troubleshooting](#troubleshooting)
17. [Contributing](#contributing)
18. [License](#license)

---

## 🎯 Project Overview

**Gangsta Island / Chase 101** is a digital adaptation of a physical urban-themed board game. The game features:

- **101 unique squares** (17th Polk, 5th Low End, Meth House, etc.)
- **Special mechanics**: Teleports, Jail, Hospital, Cemetery, Life Cards
- **Unique characters**: King Cervus, Meth Man, Officer Friendly, Rev
- **Multiplayer**: 2-4 players per game
- **Dual economy**: Play Money (practice) + Real Money (stakes)

### Platforms Supported

| Platform | Status | Description |
|----------|--------|-------------|
| Discord Bot | ✅ MVP | Headless version with `/commands` |
| Mobile App | 🔄 Phase 2 | React Native iOS/Android |
| Web Dashboard | 🔄 Phase 3 | Admin panel for management |

---

## 🛠 Tech Stack

### Backend
| Technology | Purpose |
|------------|---------|
| **Node.js (v20+)** | Runtime environment |
| **Express.js** | REST API framework |
| **Socket.io** | Real-time multiplayer sync |
| **MongoDB** | Main database |
| **Redis** | Session management & caching |
| **JWT** | Authentication |

### Discord Bot
| Technology | Purpose |
|------------|---------|
| **Discord.js (v14+)** | Discord API wrapper |
| **Canvas** | Dynamic board image generation |
| **Command Handler** | Slash commands (`/roll`, `/board`) |

### Frontend (Mobile App)
| Technology | Purpose |
|------------|---------|
| **React Native** | Cross-platform mobile |
| **Redux Toolkit** | State management |
| **React Navigation** | Screen routing |

### Payments & Infrastructure
| Technology | Purpose |
|------------|---------|
| **Stripe** | Fiat payments (credit card, PayPal) |
| **AWS S3** | Image/assets storage |
| **AWS EC2 / Render** | Hosting |
| **MongoDB Atlas** | Cloud database |

---

## 🏗 System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                         CLIENTS                              │
├──────────────┬──────────────┬──────────────┬────────────────┤
│ Discord Bot  │  Mobile App  │  Web Admin   │    API Users   │
│ (Discord.js) │ (React Native)│ (React)      │    (REST)      │
└──────┬───────┴──────┬───────┴──────┬───────┴───────┬────────┘
       │              │              │               │
       ▼              ▼              ▼               ▼
┌──────────────────────────────────────────────────────────────┐
│                      API GATEWAY (Express)                    │
│                    Port: 5000 (HTTP/WebSocket)                │
└──────────────────────────────┬───────────────────────────────┘
                               │
       ┌───────────────────────┼───────────────────────┐
       │                       │                       │
       ▼                       ▼                       ▼
┌─────────────┐     ┌─────────────────┐     ┌─────────────┐
│   MongoDB   │     │      Redis      │     │    Stripe   │
│  (Primary)  │     │   (Sessions)    │     │ (Payments)  │
└─────────────┘     └─────────────────┘     └─────────────┘
```

### Data Flow (Game Move)
```
1. User types /roll in Discord
2. Bot sends API request → Backend
3. Backend calculates new position
4. Checks for teleport/jail/hospital
5. Updates database
6. Sends WebSocket event to mobile app (if open)
7. Bot generates new board image
8. User sees result
```

---

## ✨ Features (Full Scope)

### Phase 1 - MVP (Discord Bot + Core Logic)
- [x] User registration (Discord OAuth)
- [x] Create/join game lobbies (2-4 players)
- [x] Dice roll mechanism (`/roll`)
- [x] Movement on 101-square board
- [x] Teleport logic (square → square mapping)
- [x] Jail/Hospital/Cemetery mechanics
- [x] Life Cards (basic draw & use)
- [x] Play Money system (practice mode)
- [x] Dynamic board image generation
- [x] Turn-based multiplayer

### Phase 2 - Full Production
- [ ] Mobile app (React Native)
- [ ] Real-money wallet (Stripe)
- [ ] Withdrawals & deposits
- [ ] Admin dashboard
- [ ] Tournament system
- [ ] In-game purchases (Life Card packs)
- [ ] Push notifications
- [ ] Friend invites

### Phase 3 - Advanced
- [ ] Character skins
- [ ] VIP subscription tiers
- [ ] Analytics dashboard
- [ ] Affiliate/referral system
- [ ] Leaderboards
- [ ] Replay system

---

## 📦 Prerequisites

Before you begin, ensure you have:

| Requirement | Version | Check Command |
|-------------|---------|---------------|
| Node.js | v20+ | `node --version` |
| npm | v9+ | `npm --version` |
| MongoDB | v6+ | `mongod --version` |
| Redis | v7+ | `redis-server --version` |
| Git | latest | `git --version` |

### Required Accounts
- [Discord Developer Portal](https://discord.com/developers/applications) - Bot token
- [MongoDB Atlas](https://www.mongodb.com/atlas) - Cloud database (or local)
- [Stripe](https://stripe.com) - Payment processing (Phase 2)
- [AWS](https://aws.amazon.com) - S3 storage (optional)

---

## 🚀 Installation & Setup

### 1. Clone Repository

```bash
git clone https://github.com/yourusername/gangsta-island.git
cd gangsta-island
```

### 2. Install Dependencies

```bash
# Backend dependencies
cd backend
npm install

# Discord bot dependencies
cd ../discord-bot
npm install

# Mobile app dependencies (Phase 2)
cd ../mobile-app
npm install
```

### 3. Environment Configuration

Create `.env` files in each directory (see [Environment Variables](#environment-variables))

### 4. Start MongoDB & Redis

```bash
# Start MongoDB (local)
mongod --dbpath /data/db

# Start Redis
redis-server

# OR use Docker
docker-compose up -d
```

### 5. Run Database Migrations

```bash
cd backend
npm run migrate
npm run seed   # Optional: seed test data
```

### 6. Start Backend Server

```bash
cd backend
npm run dev
# Server runs on http://localhost:5000
```

### 7. Start Discord Bot

```bash
cd discord-bot
npm run start
# Bot should show "Online" in Discord
```

### 8. Invite Bot to Your Server

Use this URL pattern:
```
https://discord.com/api/oauth2/authorize?client_id=YOUR_BOT_ID&permissions=8&scope=bot%20applications.commands
```

---

## 🔐 Environment Variables

### Backend `.env`

```env
# Server
PORT=5000
NODE_ENV=development

# Database
MONGODB_URI=mongodb://localhost:27017/gangsta_island
MONGODB_URI_PROD=mongodb+srv://username:password@cluster.mongodb.net/gangsta_island

# Redis
REDIS_HOST=localhost
REDIS_PORT=6379
REDIS_PASSWORD=

# JWT
JWT_SECRET=your_super_secret_key_change_this
JWT_EXPIRES_IN=7d

# Discord
DISCORD_CLIENT_ID=123456789012345678
DISCORD_CLIENT_SECRET=abcdefghijklmnopqrstuvwxyz
DISCORD_BOT_TOKEN=your_bot_token_here

# Stripe (Phase 2)
STRIPE_SECRET_KEY=sk_live_xxxxxxxxxxxxx
STRIPE_WEBHOOK_SECRET=whsec_xxxxxxxxxxxxx

# AWS (Phase 2)
AWS_ACCESS_KEY_ID=AKIAXXXXXXXXXXXXXX
AWS_SECRET_ACCESS_KEY=xxxxxxxxxxxxxxxxxxxx
AWS_S3_BUCKET=gangsta-island-assets
AWS_REGION=us-east-1

# Admin
ADMIN_EMAIL=admin@gangstaisland.com
ADMIN_PASSWORD_HASH=$2a$10$xxxxxxxxxxxxxxxxx
```

### Discord Bot `.env`

```env
DISCORD_BOT_TOKEN=your_bot_token_here
DISCORD_CLIENT_ID=123456789012345678
API_BASE_URL=http://localhost:5000/api
```

### Mobile App `.env` (Phase 2)

```env
API_URL=http://localhost:5000/api
WS_URL=http://localhost:5000
STRIPE_PUBLIC_KEY=pk_live_xxxxxxxxxxxxx
```

---

## 🗄 Database Schema

### Users Collection

```javascript
{
  _id: ObjectId,
  discordId: "123456789012345678",
  username: "player1",
  email: "player1@example.com",
  avatar: "https://cdn.discordapp.com/...",
  wallets: {
    playMoney: 10000,      // Practice currency
    realMoney: 0.00        // Real USD balance
  },
  stats: {
    gamesPlayed: 0,
    gamesWon: 0,
    totalEarnings: 0,
    totalWagered: 0
  },
  currentGame: ObjectId,   // Active game ID
  isBanned: false,
  role: "user",            // user, admin, vip
  createdAt: ISODate,
  lastActive: ISODate
}
```

### Games Collection

```javascript
{
  _id: ObjectId,
  gameCode: "ABCD1234",
  status: "waiting",       // waiting, active, completed, abandoned
  players: [
    {
      userId: ObjectId,
      position: 0,
      lifeCards: ["card1", "card2"],
      isInJail: false,
      jailTurnsLeft: 0,
      isInHospital: false,
      hospitalTurnsLeft: 0,
      order: 0             // Turn order (0-3)
    }
  ],
  currentTurn: 0,          // Index of player whose turn it is
  board: {
    teleports: {...},
    specialSquares: {...}
  },
  potAmount: 0,            // Real money pot
  entryFee: 0,             // Entry fee per player
  isRealMoney: false,
  winner: ObjectId,
  createdAt: ISODate,
  updatedAt: ISODate
}
```

### LifeCards Collection

```javascript
{
  _id: ObjectId,
  name: "Get Out of Jail Free",
  description: "Instantly escape jail",
  effect: "escape_jail",
  rarity: "common",        // common, rare, epic, legendary
  imageUrl: "/cards/jail_free.png"
}
```

### Transactions Collection (Phase 2)

```javascript
{
  _id: ObjectId,
  userId: ObjectId,
  type: "deposit",         // deposit, withdrawal, bet, win, rake
  amount: 50.00,
  currency: "usd",
  stripePaymentIntentId: "pi_xxxxxxxxx",
  status: "completed",     // pending, completed, failed
  gameId: ObjectId,
  createdAt: ISODate
}
```

---

## 📡 API Documentation

### Base URL
```
Development: http://localhost:5000/api
Production:  https://api.gangstaisland.com/api
```

### Authentication
All endpoints (except `/auth/*`) require Bearer token:
```
Authorization: Bearer <jwt_token>
```

### Endpoints

#### Auth

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/auth/discord` | Login with Discord OAuth |
| GET | `/auth/verify` | Verify JWT token |
| POST | `/auth/logout` | Logout user |

#### Users

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/users/me` | Get current user profile |
| PUT | `/users/me` | Update user profile |
| GET | `/users/me/stats` | Get user statistics |
| GET | `/users/leaderboard` | Get top players |

#### Games

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/games/create` | Create new game lobby |
| POST | `/games/join/:code` | Join game by code |
| GET | `/games/:id` | Get game details |
| POST | `/games/:id/roll` | Roll dice and move |
| POST | `/games/:id/use-card` | Use a Life Card |
| POST | `/games/:id/leave` | Leave game |

#### Wallets

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/wallets/balance` | Get both wallet balances |
| POST | `/wallets/play-money/add` | Add play money (admin only) |

#### Admin

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/admin/users` | List all users |
| PUT | `/admin/users/:id/ban` | Ban user |
| PUT | `/admin/users/:id/role` | Change user role |
| GET | `/admin/games` | List all games |
| GET | `/admin/stats` | Platform statistics |

### Example API Call

```javascript
// Create a game
fetch('http://localhost:5000/api/games/create', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer YOUR_JWT_TOKEN',
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    entryFee: 10,
    isRealMoney: true,
    maxPlayers: 4
  })
})
.then(res => res.json())
.then(data => console.log(data));
```

---

## 🤖 Discord Bot Commands

### Player Commands

| Command | Description | Example |
|---------|-------------|---------|
| `/register` | Register account | `/register` |
| `/profile` | View your profile | `/profile` |
| `/create` | Create new game lobby | `/create` |
| `/join [code]` | Join game by code | `/join ABCD1234` |
| `/roll` | Roll dice (in active game) | `/roll` |
| `/board` | Show current board | `/board` |
| `/cards` | Show your Life Cards | `/cards` |
| `/use-card [card]` | Use a Life Card | `/use-card "Get Out of Jail"` |
| `/leave` | Leave current game | `/leave` |
| `/leaderboard` | Show top players | `/leaderboard` |
| `/help` | Show all commands | `/help` |

### Admin Commands

| Command | Description |
|---------|-------------|
| `/add-money @user 1000` | Add play money to user |
| `/ban @user` | Ban user |
| `/unban @user` | Unban user |
| `/stats` | Show platform stats |
| `/reset-game [code]` | Force reset game |

### Bot Response Example

When user types `/roll`:

```
🎲 **Player1 rolled a 6!**

📍 Moved from Square #42 (Low End) to Square #48 (Meth House)

⚠️ **Special Square!** You landed on Meth House
- Effect: Lose 1 turn
- Current balance: 2,500 Play Money

📊 **Current Board:**
[1] [2] [3] ... [42] 👤 ... [48] 🏚️ ... [101]

🃏 **Next turn: Player2**
```

---

## 🎮 Game Logic

### Board Configuration (`board.config.js`)

```javascript
// 101 Squares definition
const squares = [
  { id: 0, name: "Start", type: "start" },
  { id: 1, name: "17th Polk", type: "normal" },
  { id: 2, name: "5th Low End", type: "normal" },
  // ... up to 101
  { id: 48, name: "Meth House", type: "penalty", effect: "lose_turn" },
  { id: 52, name: "Teleport", type: "teleport", destination: 67 },
  { id: 67, name: "Teleport Destination", type: "normal" },
  { id: 75, name: "Jail", type: "jail", turnsToStay: 2 },
  { id: 82, name: "Hospital", type: "hospital", turnsToStay: 1 },
  { id: 99, name: "Cemetery", type: "cemetery", effect: "reset_to_start" }
];

// Teleport mapping
const teleports = {
  52: 67,   // Square 52 → 67
  33: 89,   // Square 33 → 89
  14: 45,   // Square 14 → 45
  // Add all teleport pairs
};
```

### Game Flow

```javascript
// Simplified game engine logic
class GameEngine {
  
  rollDice() {
    return Math.floor(Math.random() * 6) + 1;
  }
  
  movePlayer(currentPos, diceRoll) {
    let newPos = currentPos + diceRoll;
    
    // Check win condition
    if (newPos >= 101) {
      return { position: 101, won: true };
    }
    
    // Check teleport
    if (teleports[newPos]) {
      newPos = teleports[newPos];
    }
    
    // Check special squares
    const square = squares[newPos];
    switch(square.type) {
      case 'jail':
        return { position: newPos, jail: true, turns: 2 };
      case 'hospital':
        return { position: newPos, hospital: true, turns: 1 };
      case 'penalty':
        return { position: newPos, loseTurn: true };
      default:
        return { position: newPos };
    }
  }
  
  // Life Card effects
  useLifeCard(cardType, player) {
    switch(cardType) {
      case 'escape_jail':
        player.isInJail = false;
        return "You escaped jail!";
      case 'extra_turn':
        return "You get an extra turn!";
      case 'teleport_anywhere':
        player.position = 95;
        return "Teleported to square 95!";
      // Add more card effects
    }
  }
}
```

### House Rake Calculation

```javascript
// For real-money games
function calculatePayout(potAmount, houseRakePercentage = 7.5) {
  const houseCut = potAmount * (houseRakePercentage / 100);
  const winnerPayout = potAmount - houseCut;
  
  return {
    totalPot: potAmount,
    houseRake: houseCut,
    winnerGets: winnerPayout
  };
}
// Example: $100 pot → $7.50 house → $92.50 to winner
```

---

## 📊 Milestone Breakdown

| Milestone | Deliverables | Timeline | Payment |
|-----------|--------------|----------|---------|
| **M1** | Discord bot + core board logic + play money + multiplayer (2-4 players) | 4-5 weeks | $1,000 |
| **M2** | Currency system + backend + real-money wallet (Stripe) + database | 2-3 weeks | $1,500 |
| **M3** | Mobile app (React Native) + UI + board sync with Discord | 3-4 weeks | $1,500 |
| **M4** | Admin panel + withdrawals + tournaments + final polish | 2-3 weeks | $1,500 |
| **TOTAL** | | **10-12 weeks** | **$5,500** |

### Minimum Launch Version (Discord Only)
- **Price:** $3,500
- **Timeline:** 4-5 weeks
- **Includes:** M1 only (no mobile app, no real payments)

---

## 📁 Folder Structure

```
gangsta-island/
│
├── backend/
│   ├── src/
│   │   ├── controllers/
│   │   │   ├── auth.controller.js
│   │   │   ├── game.controller.js
│   │   │   ├── user.controller.js
│   │   │   └── wallet.controller.js
│   │   ├── models/
│   │   │   ├── User.model.js
│   │   │   ├── Game.model.js
│   │   │   ├── LifeCard.model.js
│   │   │   └── Transaction.model.js
│   │   ├── routes/
│   │   │   ├── auth.routes.js
│   │   │   ├── game.routes.js
│   │   │   ├── user.routes.js
│   │   │   └── admin.routes.js
│   │   ├── middleware/
│   │   │   ├── auth.middleware.js
│   │   │   ├── error.middleware.js
│   │   │   └── rateLimit.middleware.js
│   │   ├── services/
│   │   │   ├── gameEngine.service.js
│   │   │   ├── payment.service.js
│   │   │   └── discord.service.js
│   │   ├── utils/
│   │   │   ├── logger.js
│   │   │   └── helpers.js
│   │   ├── config/
│   │   │   ├── database.js
│   │   │   └── redis.js
│   │   └── app.js
│   ├── .env
│   ├── package.json
│   └── server.js
│
├── discord-bot/
│   ├── src/
│   │   ├── commands/
│   │   │   ├── roll.js
│   │   │   ├── board.js
│   │   │   ├── join.js
│   │   │   ├── create.js
│   │   │   ├── profile.js
│   │   │   └── admin.js
│   │   ├── events/
│   │   │   ├── ready.js
│   │   │   └── interactionCreate.js
│   │   ├── utils/
│   │   │   ├── boardRenderer.js  (Canvas image generation)
│   │   │   └── api.js
│   │   └── index.js
│   ├── .env
│   ├── package.json
│   └── deploy-commands.js
│
├── mobile-app/ (Phase 2)
│   ├── src/
│   │   ├── screens/
│   │   │   ├── LoginScreen.js
│   │   │   ├── GameScreen.js
│   │   │   ├── BoardScreen.js
│   │   │   ├── ProfileScreen.js
│   │   │   └── WalletScreen.js
│   │   ├── components/
│   │   │   ├── Dice.js
│   │   │   ├── Board.js
│   │   │   ├── PlayerCard.js
│   │   │   └── LifeCard.js
│   │   ├── store/
│   │   │   ├── gameSlice.js
│   │   │   └── userSlice.js
│   │   ├── services/
│   │   │   ├── api.js
│   │   │   └── websocket.js
│   │   └── App.js
│   ├── .env
│   └── package.json
│
├── admin-panel/ (Phase 2)
│   ├── src/
│   │   ├── pages/
│   │   │   ├── Dashboard.js
│   │   │   ├── Users.js
│   │   │   ├── Games.js
│   │   │   ├── Transactions.js
│   │   │   └── Settings.js
│   │   ├── components/
│   │   └── App.js
│   └── package.json
│
├── docker-compose.yml
├── README.md
└── .gitignore
```

---

## 🚢 Deployment

### Deploy Backend to Render

```bash
# 1. Push code to GitHub
git push origin main

# 2. On Render.com
# New Web Service → Connect GitHub → Select repo
# Environment Variables → Add all from .env
# Build Command: npm install
# Start Command: npm start
```

### Deploy Discord Bot to Railway

```bash
# 1. Install Railway CLI
npm install -g @railway/cli

# 2. Login
railway login

# 3. Deploy
cd discord-bot
railway up

# 4. Add environment variables in Railway dashboard
```

### Deploy MongoDB Atlas

```bash
# 1. Create cluster on MongoDB Atlas
# 2. Get connection string
# 3. Add to .env: MONGODB_URI=your_atlas_uri
```

### Docker Deployment

```yaml
# docker-compose.yml
version: '3.8'
services:
  mongodb:
    image: mongo:6
    ports:
      - "27017:27017"
    volumes:
      - mongo_data:/data/db
  
  redis:
    image: redis:7
    ports:
      - "6379:6379"
  
  backend:
    build: ./backend
    ports:
      - "5000:5000"
    depends_on:
      - mongodb
      - redis
    env_file:
      - ./backend/.env
  
  discord-bot:
    build: ./discord-bot
    depends_on:
      - backend
    env_file:
      - ./discord-bot/.env

volumes:
  mongo_data:
```

Run with:
```bash
docker-compose up -d
```

---

## 🧪 Testing

### Run Backend Tests

```bash
cd backend
npm run test          # Unit tests
npm run test:integration  # Integration tests
npm run test:coverage     # Coverage report
```

### Example Test

```javascript
// gameEngine.test.js
const GameEngine = require('./gameEngine');

describe('Game Engine', () => {
  test('rollDice returns number between 1-6', () => {
    const roll = GameEngine.rollDice();
    expect(roll).toBeGreaterThanOrEqual(1);
    expect(roll).toBeLessThanOrEqual(6);
  });
  
  test('teleport works correctly', () => {
    const result = GameEngine.movePlayer(51, 1); // 51+1=52 (teleport to 67)
    expect(result.position).toBe(67);
  });
  
  test('jail applies penalty', () => {
    const result = GameEngine.movePlayer(74, 1); // 74+1=75 (jail)
    expect(result.jail).toBe(true);
    expect(result.turns).toBe(2);
  });
});
```

---

## 🔧 Troubleshooting

### Common Issues & Solutions

| Issue | Solution |
|-------|----------|
| Discord bot not responding | Check bot token in `.env`. Regenerate token in Discord Developer Portal |
| MongoDB connection failed | Verify `MONGODB_URI`. Check IP whitelist in MongoDB Atlas |
| Port 5000 already in use | Kill process: `npx kill-port 5000` OR change PORT in `.env` |
| Bot can't join voice channel | Bot needs "Connect" and "Speak" permissions |
| WebSocket connection drops | Check firewall. Increase timeout in `socket.io` config |
| Stripe webhook failing | Use Stripe CLI for local testing: `stripe listen --forward-to localhost:5000/webhook` |

### Logs

```bash
# Backend logs
cd backend
npm run logs

# Discord bot logs
cd discord-bot
npm run logs

# MongoDB logs
mongod --logpath /var/log/mongodb.log
```

---

## 👥 Contributing

### For Developers

1. Fork the repository
2. Create feature branch: `git checkout -b feature/amazing-feature`
3. Commit changes: `git commit -m 'Add amazing feature'`
4. Push: `git push origin feature/amazing-feature`
5. Open Pull Request

### Code Style

- Use ESLint: `npm run lint`
- Format with Prettier: `npm run format`
- Follow Conventional Commits

---

## 📝 License

This project is proprietary and confidential.

**© 2024 Gangsta Island / Chase 101. All rights reserved.**

Unauthorized copying, distribution, or modification of this software is strictly prohibited.

---

## 📞 Support & Contact

| Role | Contact |
|------|---------|
| Developer | Zahid (Level 2 Fiverr Seller) |
| Client | [Client Name] |
| Project Status | Active |

**Emergency Contact:** Discord DM or Fiverr inbox

---

## ✅ Quick Start Checklist for New Developer

```bash
# 1. Clone repo
git clone [repo-url]

# 2. Install dependencies
cd backend && npm install
cd ../discord-bot && npm install

# 3. Copy .env files (ask Zahid for values)
cp backend/.env.example backend/.env
cp discord-bot/.env.example discord-bot/.env

# 4. Start MongoDB (local or Docker)
docker-compose up -d mongodb redis

# 5. Run migrations
cd backend && npm run migrate

# 6. Start backend
npm run dev

# 7. Start bot (new terminal)
cd discord-bot && npm start

# 8. Test with `/roll` in Discord
```

---

## 📚 Additional Documentation

| Document | Location |
|----------|----------|
| Board Layout Map | `/docs/board-layout.json` |
| Life Cards List | `/docs/life-cards.json` |
| Character Profiles | `/docs/characters.json` |
| API Postman Collection | `/docs/GangstaIsland.postman_collection.json` |
| Database ER Diagram | `/docs/database-diagram.png` |

---

**End of README**

---
