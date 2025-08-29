# Game of Telephone ("Ryktet går")

## Overview

An online, fast-paced multiplayer drawing and guessing game inspired by *Ryktet går* (a.k.a. "Telephone"). Designed to be vibe-coded in 1–2 days and playable on mobile devices or browsers. Ideal for a booth demo or casual play.

---

## Gameplay

1. **Players & Setup**

   * 5–8 players join a game via QR code or link.
   * Each round lasts \~60 seconds.

2. **Round Flow**

   1. **Start:** Each player receives a random word or phrase.
   2. **Draw:** Players sketch the word on their device, then press *Done*.
   3. **Pass:** Each drawing is sent to another player in the “circle.”
   4. **Guess:** The receiving player writes what they think the drawing represents.
   5. **Repeat:** Guess → Drawing → Guess until the round ends.
   6. **Reveal:** The final chain of word/drawing pairs is shown to all.
   7. **AI Guess (optional):** An AI model attempts to guess the final drawing and compare with the original word.

3. **End of Game**

   * Display the evolution chain: Word → Drawing → Guess → Drawing → … → Final.
   * Showcase funny mismatches.

---

## Features

* **Drawing Board:** Mobile-friendly canvas for sketches.
* **Text Input:** Simple field for guesses.
* **Real-Time Sync:** Game state passed in sequence via backend/WebSocket.
* **Reveal Mode:** Gallery showing all chains with step-by-step progression.
* **Optional Enhancements:**

  * AI guesses between rounds.
  * Live voting for funniest/best drawings.
  * Leaderboard of most accurate/funny players.

---

## Technical Requirements

* **Frontend:** React/Next.js with Canvas drawing, responsive design.
* **Backend:** Node.js + Socket.io for real-time state management.
* **Database:** Lightweight (SQLite/Postgres) for game sessions.
* **AI (optional):** Simple image classifier (e.g., CLIP) for guessing drawings.
* **Hosting:** Vercel (frontend), Fly.io/Heroku (backend).

---

## Constraints

* Must be playable within **60 seconds per round**.
* Support at least **8 players per session**.
* Easy onboarding (QR code → join link).
* Fun to watch: public reveal mode for big screen demo.

---

## Goals

* **Fun & Hilarious:** Highlight funny mismatches in the chain.
* **Low-Barrier:** Works on any mobile browser.
* **Demo-Friendly:** Quick games (2 per coffee break).
