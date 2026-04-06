
---

# 🏝️ GANGSTA ISLAND / CHASE 101 - Digital Board Game Platform

> **A complete digital adaptation of the official "Game of Chase" tabletop board game with Discord bot, mobile app, and real-money economy.**

---


## 📋 Table of Contents

1. [Project Overview](#project-overview)
2. [Game Rules Summary](#game-rules-summary)
3. [Characters & Teams](#characters--teams)
4. [Tech Stack](#tech-stack)
5. [System Architecture](#system-architecture)
6. [Features by Phase](#features-by-phase)
7. [Prerequisites](#prerequisites)
8. [Installation & Setup](#installation--setup)
9. [Environment Variables](#environment-variables)
10. [Database Schema](#database-schema)
11. [API Documentation](#api-documentation)
12. [Discord Bot Commands](#discord-bot-commands)
13. [Core Game Logic](#core-game-logic)
14. [Special Squares & Effects](#special-squares--effects)
15. [Card System](#card-system)
16. [Money & Banking System](#money--banking-system)
17. [Milestone Breakdown](#milestone-breakdown)
18. [Folder Structure](#folder-structure)
19. [Deployment](#deployment)
20. [Testing](#testing)
21. [Troubleshooting](#troubleshooting)
22. [Quick Start for Developers](#quick-start-for-developers)

---

## 🎯 Project Overview

**Gangsta Island / Chase 101** is a digital adaptation of the official **"Game of Chase"** tabletop board game. This project transforms the physical board game into a cross-platform digital experience.

### Official Game Information

| Aspect | Details |
|--------|---------|
| **Game Name** | Game of Chase / Gangsta Island |
| **Publisher** | Bittersweet Publishing |
| **Max Players** | Up to 14 players |
| **Age Rating** | 12+ |
| **Win Condition** | First to **$1,000,000** (One Million Dollars) |
| **Good Guys Win** | Team collectively reaches **$2,000,000** |

### Game Theme
> *"The Game of Chase is an action game, a tabletop board game that portrays the real-life robbery, killings, bullies in the inner city, the everyday trials and tribulations of the street."*

### Platforms Supported

| Platform | Status | Description |
|----------|--------|-------------|
| **Discord Bot** | ✅ Phase 1 (MVP) | Headless version with slash commands |
| **Mobile App** | 🔄 Phase 2 | React Native (iOS & Android) |
| **Admin Panel** | 🔄 Phase 3 | Web dashboard for management |

---

## 📖 Game Rules Summary

### Core Concept
- Players move around the board using **6 dice**
- Land on squares to earn money, take opponents, or face penalties
- **3 types of players** with different abilities and win conditions
- First to reach **$1,000,000** wins (Good Guys need $2,000,000 as a team)

### Turn Order
```
1. BAD HUSTLAS (move first - bring financial commerce to streets)
2. GOOD GUYS (move second - capture BAD HUSTLAS money)
3. UGLY TAKERS (move last - hunt everyone)
```

### Movement Rules
- Only **one direction** (no backward movement)
- **30-second time limit** per turn
- **Corners** can be advanced straight or diagonally
- Once piece is placed, you cannot change position

### Dice System

| Roll | Effect |
|------|--------|
| `1-2-3-4-5-6` | **ATMBF** - Awards $1,000 |
| `1-1-1-2-6-6` (3 craps) | **SNITCH** - Go directly to JAIL |
| **6 of a kind** | **$5,000,000 LOTTERY** (instant win, except LAMA) |

---

## 👥 Characters & Teams

### Total Characters: 14

### THE GOOD GUYS (Team 1 - Rebuild Community)

| # | Character Name | Starting Location |
|---|----------------|-------------------|
| 1 | **KING CERVUS** | Drug Free Zone |
| 2 | **REVEREND** | Church |
| 3 | **OFFICER FRIENDLY** | Breakfast Shop |
| 4 | **COUNSELOR MOM** | Bus Stop |

**Team Win Condition:** Collectively raise **$2,000,000** by taking illegally earned money from BAD HUSTLAS to rebuild the community.

**Abilities:**
- Cannot HUSTLE
- Work as a team
- Take BAD HUSTLAS money (except ATM funds)

---

### THE BAD HUSTLAS (Team 2 - Move First)

| # | Character Name | Starting Location |
|---|----------------|-------------------|
| 1 | **COKALINA** | Crack House |
| 2 | **MR. H** | Dog Pound |
| 3 | **METH MAN** | Meth House |
| 4 | **RASTA** | Smoke House |
| 5 | **PROFESSOR X** | Scripts |

**Team Win Condition:** Hustle up **$1,000,000** and retire.

**Abilities:**
- Can HUSTLE (special dice rolling mechanic)
- Leave house poor, only selling candy
- Dream of retiring from the game

---

### THE UGLY TAKERS (Team 3 - Move Last)

| # | Character Name | Starting Location | Special Ability |
|---|----------------|-------------------|-----------------|
| 1 | **5-FINGERS** | Shelter | Robs/stabs → Hospital |
| 2 | **BULLY D. WILLIAMS** | Arcade | Beats up/robs → Hospital |
| 3 | **MRS. SEXY** | Lap Dance Club | Infects with STI → Hospital |
| 4 | **CROOKED COP** | Fed Sub-Holding | Arrests all (except LAMA) → Jail |
| 5 | **LAMA / LAMAX** | Unda-Boss Corner | **BOARD KILLER** → Cemetery |

**Team Goal:** Eliminate all other players and cause mayhem.

**Important:** Crooked Cop and LAMA **cannot be defended against** in a Struggle.

---

## 🛠 Tech Stack

### Backend
| Technology | Version | Purpose |
|------------|---------|---------|
| Node.js | v20+ | Runtime environment |
| Express.js | v4.18+ | REST API framework |
| Socket.io | v4.5+ | Real-time multiplayer sync |
| MongoDB | v6+ | Primary database |
| Redis | v7+ | Session management & caching |
| JWT | v9+ | Authentication |

### Discord Bot
| Technology | Version | Purpose |
|------------|---------|---------|
| Discord.js | v14+ | Discord API wrapper |
| Canvas | v2.11+ | Dynamic board image generation |
| Axios | v1.4+ | HTTP requests to backend |

### Frontend (Mobile App - Phase 2)
| Technology | Version | Purpose |
|------------|---------|---------|
| React Native | v0.72+ | Cross-platform mobile |
| Redux Toolkit | v1.9+ | State management |
| React Navigation | v6+ | Screen routing |

### Payments & Infrastructure
| Technology | Purpose |
|------------|---------|
| Stripe | Fiat payments (credit card, PayPal) |
| AWS S3 | Image/assets storage |
| Render / Railway | Hosting |
| MongoDB Atlas | Cloud database |

---

## 🏗 System Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                           CLIENTS                                │
├──────────────┬──────────────┬──────────────┬───────────────────┤
│ Discord Bot  │  Mobile App  │  Web Admin   │     API Users     │
│ (Discord.js) │(React Native)│   (React)    │      (REST)       │
└──────┬───────┴──────┬───────┴──────┬───────┴───────┬───────────┘
       │              │              │               │
       ▼              ▼              ▼               ▼
┌─────────────────────────────────────────────────────────────────┐
│                      API GATEWAY (Express)                       │
│                    Port: 5000 (HTTP/WebSocket)                   │
└──────────────────────────────┬──────────────────────────────────┘
                               │
       ┌───────────────────────┼───────────────────────┐
       │                       │                       │
       ▼                       ▼                       ▼
┌─────────────┐     ┌─────────────────┐     ┌─────────────┐
│   MongoDB   │     │      Redis      │     │   Stripe    │
│  (Primary)  │     │   (Sessions)    │     │ (Payments)  │
└─────────────┘     └─────────────────┘     └─────────────┘
```

### Data Flow (Player Move Example)

```
1. User types /roll in Discord
2. Bot sends API request → Backend
3. Backend calculates new position
4. Checks for special squares (Jail/Hospital/Cemetery)
5. Checks for TAKES (landing on other players)
6. Updates database with new positions and money
7. Sends WebSocket event to mobile app (if open)
8. Bot generates new board image via Canvas
9. User sees result in Discord
```

---

## ✨ Features by Phase

### Phase 1 - MVP (Discord Bot + Core Logic)
**Timeline: 4-5 weeks | Price: $3,500**

- [x] Discord bot with slash commands
- [x] User registration (Discord OAuth)
- [x] Create/join game lobbies (2-14 players)
- [x] 6 dice roll mechanism
- [x] Movement on board
- [x] 3 team system (Good Guys, Bad Hustlas, Ugly Takers)
- [x] Turn order management
- [x] HUSTLE system for BAD HUSTLAS
- [x] TAKE system (landing on opponents)
- [x] STRUGGLE system (defense against TAKERS)
- [x] STACKING bonuses
- [x] Special squares (Jail, Hospital, Cemetery)
- [x] Basic Life Cards
- [x] Play Money system (practice mode)
- [x] Dynamic board image generation
- [x] Win condition checking

### Phase 2 - Full Production
**Timeline: 10-12 weeks | Price: $5,500 (total including Phase 1)**

- [ ] Mobile app (React Native)
- [ ] Real-money wallet (Stripe)
- [ ] Withdrawals & deposits
- [ ] Admin dashboard
- [ ] Tournament system
- [ ] Bond Out Cards purchase
- [ ] Beneficiary Cards purchase
- [ ] ATM system with BANK RUN
- [ ] ATMBF (Armored Truck Money Bag Fall)
- [ ] MY MONEY call-out system
- [ ] Push notifications
- [ ] Friend invites

### Phase 3 - Advanced
- [ ] Character skins
- [ ] VIP subscription tiers
- [ ] Analytics dashboard
- [ ] Affiliate/referral system
- [ ] Leaderboards
- [ ] Replay system
- [ ] Custom dice skins

---

## 📦 Prerequisites

### Software Requirements

| Requirement | Version | Check Command |
|-------------|---------|---------------|
| Node.js | v20+ | `node --version` |
| npm | v9+ | `npm --version` |
| MongoDB | v6+ | `mongod --version` |
| Redis | v7+ | `redis-server --version` |
| Git | latest | `git --version` |

### Required Accounts

| Service | Purpose | Cost |
|---------|---------|------|
| [Discord Developer Portal](https://discord.com/developers/applications) | Bot token | Free |
| [MongoDB Atlas](https://www.mongodb.com/atlas) | Cloud database | Free tier available |
| [Stripe](https://stripe.com) | Payment processing (Phase 2) | Free to start |
| [Render](https://render.com) | Hosting (optional) | Free tier available |

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

# OR use Docker (recommended)
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

Use this URL pattern (replace `YOUR_BOT_ID`):
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
  team: "good_guys",        // good_guys, bad_hustlas, ugly_takers
  character: "KING CERVUS",  // Character name from list
  wallets: {
    playMoney: 10000,        // Practice currency
    realMoney: 0.00          // Real USD balance
  },
  lifeCards: 3,               // Current life cards count (max 3)
  stats: {
    gamesPlayed: 0,
    gamesWon: 0,
    totalEarnings: 0,
    totalWagered: 0,
    moneyHustled: 0,
    playersTaken: 0
  },
  currentGame: ObjectId,     // Active game ID
  isBanned: false,
  role: "user",              // user, admin, vip
  createdAt: ISODate,
  lastActive: ISODate
}
```

### Games Collection

```javascript
{
  _id: ObjectId,
  gameCode: "ABCD1234",
  status: "waiting",         // waiting, active, completed, abandoned
  maxPlayers: 14,
  
  players: [
    {
      userId: ObjectId,
      character: "METH MAN",
      team: "bad_hustlas",
      position: 0,
      money: 0,
      atmFunds: 0,
      lifeCards: 3,
      bondOutCards: 0,        // Max 3
      beneficiaryCards: 0,    // Max 3
      isInJail: false,
      jailTurnsLeft: 0,
      isInHospital: false,
      hospitalTurnsLeft: 0,
      isInCemetery: false,
      order: 0                // Turn order
    }
  ],
  
  currentTurn: 0,             // Index of player whose turn it is
  turnOrder: ["bad_hustlas", "good_guys", "ugly_takers"],
  
  // Game state
  at mbf: 0,                  // Armored Truck Money Bag Fall total
  lottery: 5000000,           // $5,000,000 lottery
  snitchActive: false,
  
  potAmount: 0,               // Real money pot
  entryFee: 0,                // Entry fee per player
  isRealMoney: false,
  
  winner: ObjectId,
  createdAt: ISODate,
  updatedAt: ISODate
}
```

### Cards Collection

```javascript
// Life Cards
{
  _id: ObjectId,
  name: "Get Out of Jail Free",
  type: "jail_card",
  description: "Instantly escape jail",
  effect: "escape_jail",
  rarity: "common"
}

// Bond Out Card
{
  _id: ObjectId,
  name: "Bond Out Card",
  type: "bond_out",
  price: 10000,
  maxOwn: 3,
  effect: "Protects ATM funds in jail"
}

// Beneficiary Card
{
  _id: ObjectId,
  name: "Beneficiary Card",
  type: "beneficiary",
  price: 50000,
  maxOwn: 3,
  effect: "Protects ATM funds in cemetery, inherit funds"
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
| POST | `/games/:id/hustle` | BAD HUSTLAS: Get your hustle on |
| POST | `/games/:id/struggle` | Defend against TAKER |
| POST | `/games/:id/use-card` | Use a Life Card |
| POST | `/games/:id/bank-run` | Call BANK RUN to deposit |
| POST | `/games/:id/my-money` | Call MY MONEY on violation |
| POST | `/games/:id/leave` | Leave game |

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
| `/hustle` | BAD HUSTLAS: Hustle after landing on entry | `/hustle` |
| `/struggle` | Defend against TAKER | `/struggle` |
| `/board` | Show current board | `/board` |
| `/cards` | Show your cards | `/cards` |
| `/use-card [card]` | Use a Life Card | `/use-card "Get Out of Jail"` |
| `/bank-run` | Call BANK RUN to deposit money | `/bank-run` |
| `/my-money` | Call MY MONEY on violation | `/my-money` |
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

### Bot Response Examples

#### When user types `/roll`:

```
🎲 **Player1 (METH MAN - BAD HUSTLA) rolled a 7!**

📍 Moved from Square #42 to Square #49

⚠️ **LANDED ON:** METH HOUSE
- Effect: Penalty square
- Lose 1 turn
- Pay $5,000 to community

💰 **Current Money:** $12,500
💳 **Life Cards:** 3
🏦 **ATM Funds:** $50,000

📊 **Next turn: Player2 (OFFICER FRIENDLY - GOOD GUY)**
```

#### When BAD HUSTLA uses `/hustle`:

```
💰 **METH MAN is GETTING THEIR HUSTLE ON!**

🎲 Rolled: 6, 6, 6, 3, 2, 1
✅ Three 6's! Score: $6,000

📊 Continue hustling? (yes/no)
Risk: Lose it all if next roll fails!

💰 **Current Hustle Pot:** $6,000
```

#### When TAKER takes a player:

```
⚠️ **TAKE!** LAMA landed on KING CERVUS!

💀 LAMA (BOARD KILLER) attacks!
- KING CERVUS loses 1 LIFE CARD
- Sent to CEMETERY
- All money lost (including ATM)

🛡️ Can't defend against LAMA!

💀 **KING CERVUS has 2 LIFE CARDS remaining**
```

---

## 🎮 Core Game Logic

### Dice System Implementation

```javascript
// Dice roll logic
class DiceSystem {
  static rollDice(count = 6) {
    const rolls = [];
    for (let i = 0; i < count; i++) {
      rolls.push(Math.floor(Math.random() * 6) + 1);
    }
    return rolls;
  }
  
  static checkSpecialRolls(rolls) {
    const sorted = [...rolls].sort();
    
    // Check for 1-2-3-4-5-6 (ATMBF)
    if (sorted.join(',') === '1,2,3,4,5,6') {
      return { type: 'ATMBF', reward: 1000 };
    }
    
    // Check for 1,1,1,2,6,6 (SNITCH - 3 craps)
    const counts = {};
    rolls.forEach(r => counts[r] = (counts[r] || 0) + 1);
    if (counts[1] === 3 && counts[2] === 1 && counts[6] === 2) {
      return { type: 'SNITCH', penalty: 'JAIL' };
    }
    
    // Check for 6 of a kind (LOTTERY)
    if (rolls.every(r => r === rolls[0])) {
      return { type: 'LOTTERY', reward: 5000000 };
    }
    
    return null;
  }
  
  static checkHustleScore(rolls) {
    const counts = {};
    rolls.forEach(r => counts[r] = (counts[r] || 0) + 1);
    
    // Check for three of a kind
    for (let face = 1; face <= 6; face++) {
      if (counts[face] >= 3) {
        let multiplier = 1000;
        if (counts[face] === 4) multiplier = 10000;
        if (counts[face] === 5) multiplier = 100000;
        if (counts[face] === 6) multiplier = 1000000;
        
        return { score: face * multiplier, threeOfAKind: true };
      }
    }
    
    // Check for 3 doubles
    const doubles = Object.values(counts).filter(c => c === 2).length;
    if (doubles === 3) {
      return { score: 50000, threeDoubles: true };
    }
    
    return { score: 0, threeOfAKind: false };
  }
}
```

### HUSTLE System (BAD HUSTLAS only)

```javascript
class HustleSystem {
  constructor(game, playerId) {
    this.game = game;
    this.player = game.players.find(p => p.userId === playerId);
    this.currentPot = 0;
  }
  
  canHustle() {
    // Must be BAD HUSTLA and landed on entry spot
    return this.player.team === 'bad_hustlas' && this.isOnEntrySpot();
  }
  
  isOnEntrySpot() {
    const entrySpots = [/* Entry spot numbers */];
    return entrySpots.includes(this.player.position);
  }
  
  async hustle(rolls) {
    const result = DiceSystem.checkHustleScore(rolls);
    
    if (result.score === 0) {
      // Failed - lose everything
      this.currentPot = 0;
      return { success: false, message: "You lost everything!" };
    }
    
    // Check for double/triple spots
    let multiplier = 1;
    if (this.isOnLakefrontCondos()) multiplier = 2;
    if (this.isOnDaCarter()) multiplier = 3;
    
    const earned = result.score * multiplier;
    this.currentPot += earned;
    
    return {
      success: true,
      earned: earned,
      currentPot: this.currentPot,
      canContinue: true,
      message: `You earned $${earned.toLocaleString()}! Pot: $${this.currentPot.toLocaleString()}`
    };
  }
  
  takeMoney() {
    this.player.money += this.currentPot;
    return this.currentPot;
  }
}
```

### TAKE System (Landing on Opponents)

```javascript
class TakeSystem {
  constructor(game, attackerId, defenderId) {
    this.game = game;
    this.attacker = game.players.find(p => p.userId === attackerId);
    this.defender = game.players.find(p => p.userId === defenderId);
  }
  
  execute() {
    const attackerTeam = this.attacker.team;
    const defenderTeam = this.defender.team;
    
    // GOOD GUY takes BAD HUSTLA
    if (attackerTeam === 'good_guys' && defenderTeam === 'bad_hustlas') {
      const takenMoney = this.defender.money;
      this.attacker.money += takenMoney;
      this.defender.money = 0;
      return { 
        taken: takenMoney, 
        message: `${this.attacker.character} took $${takenMoney} from ${this.defender.character}!`
      };
    }
    
    // GOOD GUY lands on BAD GUY or vulnerable TAKER
    if (attackerTeam === 'good_guys' && 
        (defenderTeam === 'bad_hustlas' || defenderTeam === 'ugly_takers')) {
      const bonus = 50000;
      this.attacker.money += bonus;
      return { bonus: bonus, message: `${this.attacker.character} got $50,000 bonus!` };
    }
    
    // UGLY TAKER takes any player
    if (attackerTeam === 'ugly_takers') {
      return this.takerTake();
    }
    
    return null;
  }
  
  takerTake() {
    const isInvincible = ['CROOKED COP', 'LAMA'].includes(this.attacker.character);
    
    // Take LIFE CARD
    this.defender.lifeCards -= 1;
    
    // Take money (except ATM if protected)
    let takenMoney = this.defender.money;
    if (this.defender.bondOutCards > 0 && this.attacker.character === 'CROOKED COP') {
      takenMoney = 0; // Bond Out Card protects
    }
    if (this.defender.beneficiaryCards > 0 && this.attacker.character === 'LAMA') {
      takenMoney = 0; // Beneficiary Card protects
    }
    
    this.attacker.money += takenMoney;
    this.defender.money = 0;
    
    // Send to appropriate place
    let destination = '';
    if (this.attacker.character === 'CROOKED COP') destination = 'JAIL';
    else if (this.attacker.character === 'LAMA') destination = 'CEMETERY';
    else destination = 'HOSPITAL';
    
    return {
      lifeCardLost: true,
      moneyTaken: takenMoney,
      destination: destination,
      canStruggle: !isInvincible,
      message: `${this.attacker.character} took ${this.defender.character}! Sent to ${destination}.`
    };
  }
}
```

### STRUGGLE System (Defense)

```javascript
class StruggleSystem {
  constructor(game, defenderId, takerId) {
    this.game = game;
    this.defender = game.players.find(p => p.userId === defenderId);
    this.taker = game.players.find(p => p.userId === takerId);
  }
  
  canStruggle() {
    // Cannot struggle against Crooked Cop or LAMA
    const invincibleTakers = ['CROOKED COP', 'LAMA'];
    return !invincibleTakers.includes(this.taker.character);
  }
  
  async struggle(defenseRolls) {
    // Must roll 3 of a kind to win struggle
    const counts = {};
    defenseRolls.forEach(r => counts[r] = (counts[r] || 0) + 1);
    
    let isThreeOfAKind = false;
    for (let face = 1; face <= 6; face++) {
      if (counts[face] >= 3) {
        isThreeOfAKind = true;
        break;
      }
    }
    
    if (!isThreeOfAKind) {
      // Lose struggle - lose extra LIFE CARD
      this.defender.lifeCards -= 1;
      return { 
        success: false, 
        message: `Struggle failed! ${this.defender.character} lost an extra LIFE CARD!`,
        lifeCardsRemaining: this.defender.lifeCards
      };
    }
    
    // Win struggle - regain money and LIFE CARD
    // Then roll 2 dice to escape or send taker to cemetery
    const escapeRoll = DiceSystem.rollDice(2);
    const isPair = escapeRoll[0] === escapeRoll[1];
    
    if (isPair) {
      // Taker goes to cemetery
      this.taker.position = this.getCemeterySquare();
      return {
        success: true,
        takerGoesToCemetery: true,
        message: `Struggle won! ${this.taker.character} goes to CEMETERY!`
      };
    }
    
    return {
      success: true,
      escaped: true,
      message: `Struggle won! ${this.defender.character} escaped with ${escapeRoll[0]}, ${escapeRoll[1]}!`
    };
  }
}
```

### STACKING Bonuses

```javascript
class StackingSystem {
  static calculateBonus(playersOnSquare, newPlayerIndex) {
    // $100,000 per extra player already on square
    const extraPlayers = playersOnSquare.length;
    return extraPlayers * 100000;
  }
  
  static handleUglyTakerOnStack(taker, stackedPlayers) {
    // UGLY TAKER takes EVERY player on stack
    const results = [];
    for (const player of stackedPlayers) {
      results.push({
        player: player.character,
        lifeCardLost: true,
        moneyTaken: player.money,
        destination: this.getDestination(taker)
      });
    }
    
    // Taker gets $100,000 per player
    const bonus = stackedPlayers.length * 100000;
    taker.money += bonus;
    
    return {
      playersTaken: stackedPlayers.length,
      bonusEarned: bonus,
      results: results
    };
  }
}
```

---

## 🏥 Special Squares & Effects

### Square Types and Effects

| Square | Effect | Implementation |
|--------|--------|----------------|
| **JAIL** | Player misses turns, ATM funds released | `isInJail: true, jailTurnsLeft: 2` |
| **HOSPITAL** | After losing LIFE CARD to vulnerable TAKER | `isInHospital: true, hospitalTurnsLeft: 1` |
| **CEMETERY** | Lose LIFE CARD + ALL money + ATM funds | `isInCemetery: true, money: 0, atmFunds: 0` |
| **LAKEFRONT CONDOS** | Double hustle money | `hustleMultiplier: 2` |
| **DA CARTER** | Triple hustle money | `hustleMultiplier: 3` |
| **BAIL BONDS** | Purchase Bond Out Cards | `canPurchase: 'bond_out'` |
| **LAWYER'S OFFICE** | Purchase Bond Out Cards | `canPurchase: 'bond_out'` |
| **BITTERSWEET LIFE** | Purchase Beneficiary Cards | `canPurchase: 'beneficiary'` |
| **ENTRY SPOTS** | Trigger HUSTLE for BAD HUSTLAS | `isEntryPoint: true` |

### Special Square Implementation

```javascript
class SpecialSquareHandler {
  static handleLanding(player, squareId, game) {
    const square = squares[squareId];
    
    switch(square.type) {
      case 'jail':
        player.isInJail = true;
        player.jailTurnsLeft = 2;
        // ATM funds released
        player.money += player.atmFunds;
        player.atmFunds = 0;
        return { message: "You're in JAIL! Roll to get out." };
        
      case 'hospital':
        player.isInHospital = true;
        player.hospitalTurnsLeft = 1;
        return { message: "You're in HOSPITAL! Pull a Hospital Card." };
        
      case 'cemetery':
        player.lifeCards -= 1;
        player.money = 0;
        if (!player.beneficiaryCards > 0) {
          player.atmFunds = 0;
        }
        return { message: "You're in CEMETERY! Lost a LIFE CARD and all money." };
        
      case 'entry':
        if (player.team === 'bad_hustlas') {
          return { canHustle: true, message: "Entry spot! Get your HUSTLE ON!" };
        }
        break;
    }
    
    return null;
  }
}
```

---

## 🃏 Card System

### Card Types Summary

| Card Type | Price | Max Own | Where to Buy | Effect |
|-----------|-------|---------|--------------|--------|
| **Life Card** | Starting: 3 | 3 | Cryo Gene Clinic ($250k) | Extra life |
| **Bond Out Card** | $10,000 | 3 | Bail Bonds / Lawyer's Office | Protects ATM in jail |
| **Beneficiary Card** | $50,000 | 3 | Bittersweet Life | Protects ATM in cemetery |
| **Jail Card** | N/A | N/A | Pulled in jail | Various jail effects |
| **Hospital Card** | N/A | N/A | Pulled in hospital | Various hospital effects |
| **Cemetery Card** | N/A | N/A | Pulled in cemetery | Various cemetery effects |

### Card Implementation

```javascript
class CardSystem {
  static cardEffects = {
    'Get Out of Jail Free': {
      type: 'jail_card',
      effect: (player) => {
        player.isInJail = false;
        return "Escaped jail!";
      }
    },
    
    'Bond Out Card': {
      type: 'bond_out',
      price: 10000,
      maxOwn: 3,
      effect: (player) => {
        // Protects ATM funds in jail
        return "ATM funds protected in jail!";
      }
    },
    
    'Beneficiary Card': {
      type: 'beneficiary',
      price: 50000,
      maxOwn: 3,
      effect: (player) => {
        // Protects ATM funds in cemetery
        return "ATM funds protected in cemetery!";
      }
    }
  };
  
  static purchaseCard(player, cardType, game) {
    const card = this.cardEffects[cardType];
    if (!card) return { success: false, message: "Card not found" };
    
    if (player[`${cardType.toLowerCase().replace(/ /g, '_')}s`] >= card.maxOwn) {
      return { success: false, message: `Maximum ${card.maxOwn} ${cardType}s owned` };
    }
    
    if (player.money < card.price) {
      return { success: false, message: `Need $${card.price.toLocaleString()}` };
    }
    
    player.money -= card.price;
    player[`${cardType.toLowerCase().replace(/ /g, '_')}s`]++;
    
    return { success: true, message: `Purchased ${cardType}!` };
  }
}
```

---

## 💰 Money & Banking System

### ATM System

```javascript
class ATMSystem {
  static bankRun(player, amount, game) {
    if (amount > player.money) {
      return { success: false, message: "Insufficient funds" };
    }
    
    // Call BANK RUN alerts other players
    game.bankRunAlert = true;
    
    player.money -= amount;
    player.atmFunds += amount;
    
    return {
      success: true,
      message: `BANK RUN! Deposited $${amount.toLocaleString()} to ATM. Funds are now SAFE!`,
      atmFunds: player.atmFunds
    };
  }
  
  static myMoney(caller, violator, game) {
    // Caller gets all of violator's money (except ATM)
    const takenMoney = violator.money;
    caller.money += takenMoney;
    violator.money = 0;
    
    // Escort to jail
    violator.isInJail = true;
    violator.jailTurnsLeft = 2;
    
    return {
      success: true,
      message: `${caller.character} called MY MONEY! Took $${takenMoney.toLocaleString()} and sent ${violator.character} to JAIL!`
    };
  }
}
```

### ATMBF (Armored Truck Money Bag Fall)

```javascript
class ATMBFSystem {
  constructor() {
    this.total = 0;
  }
  
  addToPot(amount) {
    this.total += amount;
  }
  
  awardToPlayer(player) {
    const awarded = this.total;
    player.money += awarded;
    this.total = 0;
    return {
      awarded: awarded,
      message: `ATMBF! ${player.character} won $${awarded.toLocaleString()}!`
    };
  }
}
```

---

## 📊 Milestone Breakdown

| Milestone | Deliverables | Timeline | Payment |
|-----------|--------------|----------|---------|
| **M1** | Discord bot + all core game logic (14 characters, 3 teams, HUSTLE, TAKE, STRUGGLE, STACKING, special squares, basic Life Cards, Play Money) | 4-5 weeks | $1,000 |
| **M2** | Currency system + backend + real-money wallet (Stripe) + database + ATM/BANK RUN/MY MONEY system | 2-3 weeks | $1,500 |
| **M3** | Mobile app (React Native) + UI + board sync with Discord + Card purchase system | 3-4 weeks | $1,500 |
| **M4** | Admin panel + withdrawals + tournaments + final polish + Bond Out/Beneficiary Cards | 2-3 weeks | $1,500 |
| **TOTAL** | | **10-12 weeks** | **$5,500** |

### Minimum Launch Version (Discord Only)
- **Price:** $3,500
- **Timeline:** 4-5 weeks
- **Includes:** M1 only (no mobile app, no real payments, no ATM system)
- **Good for:** Testing gameplay, building community, practice mode

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
│   │   │   ├── Card.model.js
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
│   │   │   ├── dice.service.js
│   │   │   ├── hustle.service.js
│   │   │   ├── take.service.js
│   │   │   ├── struggle.service.js
│   │   │   ├── stacking.service.js
│   │   │   ├── atm.service.js
│   │   │   ├── payment.service.js
│   │   │   └── discord.service.js
│   │   ├── utils/
│   │   │   ├── logger.js
│   │   │   ├── constants.js  (Characters, squares, cards)
│   │   │   └── helpers.js
│   │   ├── config/
│   │   │   ├── database.js
│   │   │   ├── redis.js
│   │   │   └── stripe.js
│   │   └── app.js
│   ├── .env
│   ├── package.json
│   └── server.js
│
├── discord-bot/
│   ├── src/
│   │   ├── commands/
│   │   │   ├── roll.js
│   │   │   ├── hustle.js
│   │   │   ├── struggle.js
│   │   │   ├── board.js
│   │   │   ├── join.js
│   │   │   ├── create.js
│   │   │   ├── profile.js
│   │   │   ├── cards.js
│   │   │   ├── use-card.js
│   │   │   ├── bank-run.js
│   │   │   ├── my-money.js
│   │   │   └── admin.js
│   │   ├── events/
│   │   │   ├── ready.js
│   │   │   └── interactionCreate.js
│   │   ├── utils/
│   │   │   ├── boardRenderer.js  (Canvas image generation)
│   │   │   ├── embeds.js
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
│   │   └── App.js
│   └── package.json
│
├── constants/
│   ├── characters.js      (All 14 characters with teams and starting positions)
│   ├── squares.js         (Board squares 1-101 with types)
│   ├── cards.js           (All card definitions)
│   └── rules.js           (Game constants: win amounts, turn orders, etc.)
│
├── docker-compose.yml
├── README.md
└── .gitignore
```

### Constants File Example (`constants/characters.js`)

```javascript
// All 14 characters with their teams and starting positions
const CHARACTERS = {
  goodGuys: [
    { name: "KING CERVUS", start: "Drug Free Zone", startPosition: 1 },
    { name: "REVEREND", start: "Church", startPosition: 2 },
    { name: "OFFICER FRIENDLY", start: "Breakfast Shop", startPosition: 3 },
    { name: "COUNSELOR MOM", start: "Bus Stop", startPosition: 4 }
  ],
  badHustlas: [
    { name: "COKALINA", start: "Crack House", startPosition: 5 },
    { name: "MR. H", start: "Dog Pound", startPosition: 6 },
    { name: "METH MAN", start: "Meth House", startPosition: 7 },
    { name: "RASTA", start: "Smoke House", startPosition: 8 },
    { name: "PROFESSOR X", start: "Scripts", startPosition: 9 }
  ],
  uglyTakers: [
    { name: "5-FINGERS", start: "Shelter", startPosition: 10, vulnerable: true },
    { name: "BULLY D. WILLIAMS", start: "Arcade", startPosition: 11, vulnerable: true },
    { name: "MRS. SEXY", start: "Lap Dance Club", startPosition: 12, vulnerable: true },
    { name: "CROOKED COP", start: "Fed Sub-Holding", startPosition: 13, invincible: true },
    { name: "LAMA", start: "Unda-Boss Corner", startPosition: 14, invincible: true }
  ]
};

const TEAM_ORDER = ['bad_hustlas', 'good_guys', 'ugly_takers'];
const WIN_AMOUNTS = {
  goodGuys: 2000000,  // Team total $2M
  badHustlas: 1000000,
  uglyTakers: 1000000
};
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

### Run Tests

```bash
cd backend
npm run test          # Unit tests
npm run test:integration  # Integration tests
npm run test:coverage     # Coverage report
```

### Example Test

```javascript
// dice.test.js
const DiceSystem = require('./dice.service');

describe('Dice System', () => {
  test('rollDice returns 6 numbers between 1-6', () => {
    const rolls = DiceSystem.rollDice(6);
    expect(rolls).toHaveLength(6);
    rolls.forEach(r => {
      expect(r).toBeGreaterThanOrEqual(1);
      expect(r).toBeLessThanOrEqual(6);
    });
  });
  
  test('ATMBF detection works', () => {
    const rolls = [1,2,3,4,5,6];
    const result = DiceSystem.checkSpecialRolls(rolls);
    expect(result.type).toBe('ATMBF');
    expect(result.reward).toBe(1000);
  });
  
  test('SNITCH detection works', () => {
    const rolls = [1,1,1,2,6,6];
    const result = DiceSystem.checkSpecialRolls(rolls);
    expect(result.type).toBe('SNITCH');
    expect(result.penalty).toBe('JAIL');
  });
  
  test('Hustle scoring works', () => {
    const rolls = [6,6,6,3,2,1];
    const result = DiceSystem.checkHustleScore(rolls);
    expect(result.score).toBe(6000);
    expect(result.threeOfAKind).toBe(true);
  });
});
```

---

## 🔧 Troubleshooting

| Issue | Solution |
|-------|----------|
| Discord bot not responding | Check bot token in `.env`. Regenerate token in Discord Developer Portal |
| MongoDB connection failed | Verify `MONGODB_URI`. Check IP whitelist in MongoDB Atlas |
| Port 5000 already in use | Kill process: `npx kill-port 5000` OR change PORT in `.env` |
| Bot can't register slash commands | Run `node deploy-commands.js` |
| WebSocket connection drops | Check firewall. Increase timeout in `socket.io` config |
| HUSTLE not working | Verify player is BAD HUSTLA and on entry spot |
| STRUGGLE not triggering | Check if TAKER is vulnerable (not Crooked Cop or LAMA) |

---

## ✅ Quick Start for Developers

```bash
# 1. Clone repo
git clone [repo-url]

# 2. Install dependencies
cd backend && npm install
cd ../discord-bot && npm install

# 3. Copy .env files (ask Zahid for values)
cp backend/.env.example backend/.env
cp discord-bot/.env.example discord-bot/.env

# 4. Start MongoDB and Redis
docker-compose up -d mongodb redis

# 5. Run migrations
cd backend && npm run migrate

# 6. Start backend
npm run dev

# 7. Start bot (new terminal)
cd discord-bot && npm start

# 8. Test in Discord
/register
/create
/join [code]
/roll
```

---

## 📞 Support & Contact

| Role | Contact |
|------|---------|
| Lead Developer | Zahid Ghotia (Full Stack Developer) |
| Project Owner | [Client Name] |
| Official Game Site | https://gameofchase.com |

---

## 📝 License

This project is proprietary and confidential.

**© 2026 Game of Chase / Bittersweet Publishing. All rights reserved.**

---

**End of README**

---
