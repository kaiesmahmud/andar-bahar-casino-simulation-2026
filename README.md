# andar-bahar-casino-simulation-2026
# 🎴 Andar Bahar — Casino Simulation

> A fully self-contained, single-file browser simulation of the classic Indian card game **Andar Bahar**, built with vanilla HTML, CSS, and JavaScript. No frameworks. No server. No dependencies.

---

## 📌 What Is This?

This is a **demo/simulation** of a live Andar Bahar casino game — the kind typically powered by a real game server with WebSockets, a dealer camera feed, and an operator backend. This file simulates all of that logic client-side, making it perfect for:

- Operator demos and pitches
- UI/UX prototyping
- Game logic testing before backend integration
- Understanding the house economy model

---

## 🎮 Game Flow

```
Loading Screen (3s)
    ↓
Card Shuffle Animation (3s)
    ↓
Center Card Revealed
    ↓
10-Second Betting Window ← Fake live bets added each second
    ↓
Cards Deal (Andar ↔ Bahar, alternating)
    ↓
Matching Rank Found → Round Ends
    ↓
Win / Loss Result Shown (5s)
    ↓
Loop
```

---

## 🃏 Core Game Rules

- A standard 52-card deck is used.
- One **center card** is revealed face-up.
- Cards are dealt alternately to **Andar** (left) and **Bahar** (right).
- The round ends the moment a dealt card matches the **rank** of the center card — **suit does not matter**.
  - Example: Center card is **10♠** → game ends when **10♥**, **10♦**, or **10♣** appears.
- Players bet on:

| Bet Box | Description | Payout |
|---|---|---|
| **Andar** | Matching card lands on Andar side | 2x |
| **Bahar** | Matching card lands on Bahar side | 2x |
| **Andar 1–9** | Match found at Andar's 1st–9th odd position | 2x |
| **Andar 11–19** | Match found at Andar's 11th–19th odd position | 2x |
| **Bahar 2–10** | Match found at Bahar's 2nd–10th even position | 2x |
| **Bahar 12–20** | Match found at Bahar's 12th–20th even position | 2x |

---

## 🧠 House Algorithm

This simulation includes a **profit-protecting algorithm** that decides the outcome before dealing begins:

1. **Win-side selection** — After bets close, the algorithm compares total bets on Andar vs. Bahar and routes the win to whichever side has **less total bet value** (minimizes payout).
2. **Serial position selection** — For the 4 position-range boxes, the algorithm picks the range where the **user has placed fewer chips**, further reducing exposure.
3. **Giveaway reserve check** — If the accumulated giveaway holding balance is ≥ 2× the round's total bet pool, the algorithm may occasionally let the higher-bet side win, creating the *illusion of fairness* for players.
4. **Deck rigging** — The 51-card dealing deck is constructed so:
   - Non-matching cards fill positions before the winner.
   - The winner card (same rank, different suit) is placed at the algorithm's chosen position.
   - Other same-rank cards appear only in the tail (after the win), preventing early accidental matches.

---

## 💰 Economy Model

Every round, the house collects the difference between total bets placed and total payout made. This profit is split:

| Slice | Allocation |
|---|---|
| **30%** | Giveaway Holding (reserve for future player wins) |
| **50%** | Platform Profit |
| **20%** | Game Provider Fee |

All figures persist across sessions via **localStorage**.

---

## 📊 Live Stats Panel

Visible to all players (even those who didn't bet):

- **Total Bets** — cumulative chips wagered by all players (real + simulated) across all rounds
- **Users Get** — cumulative chips paid out to winners
- **Your Bets / Wins / Loss** — personal player ledger
- **Giveaway Holding** — reserve balance for high-payout rounds
- **Total Profit** — house net profit
- **Provider Fee** — game provider's share

---

## 🎭 Fake Live Bets (Crowd Simulation)

To simulate a real multi-player live game environment:

- At round start, all bet pools reset to **0**.
- Each second of the 10-second betting window, random bet amounts are added to the pools:
  - **Andar / Bahar**: 200–1800 per second
  - **Serial boxes**: 50–800 per second (60% chance each)
- This creates the appearance of live player activity and **makes it difficult for the real user to predict which box will have the lowest pool** at betting close — since the final totals are only locked after the last tick.

---

## 🔧 Technical Details

| Property | Value |
|---|---|
| **File type** | Single `.html` file |
| **Dependencies** | None (Google Fonts CDN only) |
| **Framework** | Vanilla JS + CSS |
| **Persistence** | `localStorage` |
| **Mobile** | Fully responsive, 390px mobile-first |
| **Card deck** | Fisher-Yates shuffle, 52 cards |
| **Deck build** | Rigged post-bet — outcome decided before dealing |

---

## 🚀 How to Use

1. Open `andar-bahar.html` in any modern browser.
2. Watch the loading screen and card shuffle.
3. When the center card appears, note its **rank** — any card of that rank ends the round.
4. Select a chip (50 / 100 / 200 / 500 / 1000) and click any bet box within 10 seconds.
5. Watch cards deal to Andar and Bahar until a rank match is found.
6. Collect winnings or wait for the next round — it loops automatically.

To **reset** all data (balance, stats, history, localStorage): scroll to the bottom of the stats panel and click **⟳ RESET ALL DATA**.

---

## 📐 Architecture Notes (For Backend Integration)

This simulation is designed to mirror the architecture of a real live game system:

```
[Real System]                    [This Simulation]
─────────────────────────────    ──────────────────────────────
Game Server (WebSocket)      →   phaseBetting() + timer loop
Operator Backend (DB)        →   localStorage
Shuffle RNG (server-side)    →   buildRiggedDeck() (client-side)
Live player bets (real)      →   addRandomTickBets() (fake)
Dealer camera feed           →   CSS card animations
Win/loss push notification   →   phaseResult() banner
```

Replacing the client-side logic with WebSocket calls to a real game server (e.g., a Node.js vsplay server) requires changes only to:
- `phaseBettingEnd()` — send bet payload via WebSocket
- `phaseResult()` — receive win/loss event from server
- `addRandomTickBets()` — replace with real-time bet feed from server

---

## 📬 Need a Custom Live Game Built?

This simulation was built as a demo. If you need a **production-ready live casino game** — with real WebSocket game servers, operator dashboards, multi-role user management, white-label API, and real-money payment integration — get in touch:

**✉️ kaiesmahmudnehal@gmail.com**

Services available:
- Live casino game development (Andar Bahar, Call Break, Roulette, etc.)
- Game server architecture (Node.js / WebSocket)
- Operator platform & agent management systems
- White-label gaming API for multiple operators
- USDT / crypto payment gateway integration
- SvelteKit / React / Next.js frontend development

---

*Built with ❤️ — Single file. No dependencies. Full casino logic.*