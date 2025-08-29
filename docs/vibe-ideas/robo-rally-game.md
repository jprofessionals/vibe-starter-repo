# Simplified Robo Rally

## Overview

An online, simplified version of the tabletop game *Robo Rally*. Players program robots to move on a grid-based board, trying to reach checkpoints while avoiding obstacles. The design should be simple enough to vibe-code in 1–2 days, but extensible to allow more rules and features later.

---

## Gameplay

1. **Players & Setup**

   * 2–6 players join a game via QR code or link.
   * Each player controls one robot starting from a base position on the grid.

2. **Round Flow**

   1. **Programming Phase (30–60 sec):** Each player selects a sequence of 3–5 movement commands (e.g., move forward, turn left, turn right, back up).
   2. **Execution Phase:** All robots execute their commands simultaneously in order.
   3. **Collisions:** Robots that attempt to move into the same square push each other or block movement.
   4. **Round End:** Check if any player has reached the next checkpoint.

3. **Victory Conditions**

   * First player to reach all checkpoints in order wins.
   * If time is limited (e.g., booth demo), victory can also be whoever reaches the furthest checkpoint within the session.

---

## Features

* **Game Board:** Simple grid layout (e.g., 10×10), with:

  * Empty squares
  * Checkpoints (goal markers)
  * Walls/obstacles

* **Robot Controls:** Basic commands:

  * Forward 1 square
  * Turn left / right
  * Back up 1 square

* **Simultaneous Execution:** Commands resolved step-by-step in sequence.
* **Visualization:** Robots animate across the grid each round.
* **Optional Enhancements (future):**

  * Conveyor belts, lasers, pits
  * Multiple lives/respawns
  * Damage system limiting number of commands
  * Random card decks for command distribution

---

## Technical Requirements

* **Frontend:** React/Next.js with grid-based visualization (CSS grid or Canvas).
* **Backend:** Node.js + Socket.io for real-time state management.
* **Database:** Lightweight session storage (SQLite/Postgres) for games in progress.
* **Hosting:** Vercel (frontend), Fly.io/Heroku (backend).
* **Extensibility:** Clear data model for board tiles and robot states to allow adding new rules later.

---

## Constraints

* Must support **2–6 players per session**.
* Must run on mobile and desktop browsers.
* Each round must resolve quickly (≤5 sec execution).
* Onboarding via QR code → join link.
* Fun and watchable on a shared screen.

---

## Goals

* **Competitive & Strategic:** Players must plan moves ahead.
* **Simple Core:** Easy to understand in 1–2 minutes.
* **Extensible:** Game can grow with more board features and rules later.
* **Demo-Friendly:** Rounds are quick (1–2 min) to keep audience engaged.
