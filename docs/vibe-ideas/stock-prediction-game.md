# Functional Requirements Document: OBX Stock Prediction Betting Game

## 1. Purpose

An interactive game for conference participants where humans and an AI compete to predict stock/index price movements (OBX/OSEBX/QQQ). Players use virtual money to place bets on short-term (5-minute) and medium-term (60-minute) price movements. The system tracks predictions, bets, and outcomes, and displays live leaderboards. The game will also serve as a showcase of data, AI/ML, and real-time visualization capabilities. The product objectives and success metrics are defined in the PRD and must be supported by functional tracking.

---

## 2. Core Gameplay Requirements

1. **Virtual Currency**

   * Each user starts with **1,000,000 virtual "money"**.
   * Balances update after each bet is settled.

2. **Betting Rounds**

   * Each participant can place:

     * **10 bets** on 5-minute intervals.
     * **4 bets** on 60-minute intervals.
   * Bets are either **long** (expecting price to rise) or **short** (expecting price to fall).

3. **AI Player**

   * Runs under the same rules (10 × 5-minute bets, 4 × 60-minute bets).
   * AI always predicts every interval, but only commits bets when strategy decides.

4. **Scoring**

   * If bet direction is correct: portfolio increases proportionally to movement.
   * If wrong: portfolio decreases accordingly.
   * Partial correctness (e.g., very small change) results in minimal P/L.

5. **Leaderboard**

   * Public scoreboard visible to all attendees, displayed on a dedicated screen at the booth.
   * Displays top balances of humans and AI.
   * Updates live as bets settle.
   * Must always remain visible during the event to meet engagement objectives.

6. **Feedback**

   * Visual and/or sound cues highlight winning and losing bets to increase engagement and align with fun/interactive UX principles.

---

## 3. Data & Market Feed

1. **Live Feed**

   * Use OBX, OSEBX or QQQ index real-time prices (via WebSocket) (they have 20+ minutes delay)
       * Providers:
           * Yahoo Finance with [yfinance](https://ranaroussi.github.io/yfinance/)
           * maybe others?

   * Aggregate into **candlesticks**:

       * 5-minute OHLC (Open, High, Low, Close)
       * 60-minute OHLC
   * Display current live value of the index.

2. **Historical Data**

   * Used to train AI models before the event.
       * Providers:
           * barchart.com have historical data (Runar has access)
   * Must support intraday granularity (5m, 60m).

3. **Visualization**

   * Candlestick charts:

     * Configurable to 5m or 60m view.
     * Show current value, price change indicators.
   * Overlay markers for bets (long/short) and AI predictions.

---

## 4. User Management

1. **Registration & Login**

   * User registers with email.
   * Backend generates unique **hash/UUID**.
   * Email sent with magic link.
   * Hash is stored and linked to user profile.
   * New hash request invalidates old session.
   * **QR code login path** provided at booth for easy onboarding.

2. **User State**

   * Stored in DB: email, hash, balance, bets placed, outcomes.
   * Each user limited to 10 × 5m bets and 4 × 60m bets.

---

## 5. Bets & Outcomes

1. **Placing Bets**

   * User selects interval (5m or 60m), direction (long/short).
   * System records bet with timestamp and amount (default stake).

2. **Settlement**

   * After interval completes, compare predicted vs actual.
   * Update balance and record P/L in DB.
   * Update leaderboard.

3. **Bet Visibility**

   * Show active bets on chart (markers).
   * Show historical bets in user profile.

---

## 6. AI Predictions & Strategy

1. **Prediction Models**

   * AI predicts next candle’s direction (5m and 60m).
   * Models: EMA, ARIMA, linear regression, or ensemble.

2. **Betting Strategy**

   * AI places bets only 10 × 5m and 4 × 60m times.
   * Strategy may choose bets with strongest confidence.

3. **Tracking Accuracy**

   * Record AI predictions vs actual outcomes.
   * Display AI accuracy rate for fun.

---

## 7. Leaderboard & Visualization

1. **Leaderboard**

   * Display top human users and AI balances.
   * Updates in real time.
   * Highlight if AI is leading.
   * Must remain visible throughout the conference to attract attention and drive booth traffic.

2. **Charts**

   * Candlestick chart with real-time updates.
   * Markers for bets (user + AI).
   * Overlay predicted values (optional).

---

## 8. Admin & Operations

1. **Admin Controls**

   * Reset game (wipe balances, bets).
   * Manually correct a bet if system error occurs.
   * Monitor logs of all bets placed.

2. **Audit Logs**

   * Immutable log of all bets, outcomes, balance changes.

3. **Analytics Tracking**

   * System must track basic engagement metrics (bets placed per user, participation counts).
   * Must support reporting to measure PRD success metrics (e.g., engagement, retention).

---

## 9. Security Requirements

1. **Authentication**

   * Magic link login only, no passwords.
   * JWT tokens for session handling.

2. **Database Safety**

   * Row Level Security by user.
   * Prevent SQL injection via ORM (Prisma/Drizzle).

3. **Public Safety**

   * Backend behind Cloudflare WAF & TLS.
   * Rate limits on API endpoints (auth, bets).

---

## 10. Non-Functional Requirements

1. **Performance**

   * System must handle 100–200 concurrent users.
   * Bet placement latency < 500ms.

2. **Availability**

   * Must run reliably for 2 full conference days.
   * Recovery plan: redeploy container in minutes if crash.

3. **Observability**

   * Logs of all bets, errors, predictions.
   * Healthcheck endpoints.

4. **Extendability**

   * Future: multiple stocks, more bet types, different indices.

5. **Explicit Exclusions**

   * No real money betting or payouts.
   * No persistent accounts beyond the event.
   * No complex financial instruments beyond long/short bets.
   * Out of scope items defined in PRD (multi-day persistence, real payouts, advanced strategies).
