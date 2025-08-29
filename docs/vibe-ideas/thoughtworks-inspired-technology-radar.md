# Norwegian Tech Radar – Vibe-Coding Requirements

## Overview

A web application inspired by the *Thoughtworks Technology Radar*, but tailored for the Norwegian market. It allows the company to add, categorize, and rate technologies, platforms, tools, techniques, frameworks, and trends. Each entry includes ratings, descriptions, links, and optional discussions. The goal is to create a simple but extensible MVP that can be vibe-coded in 1–2 days.

---

## Core Features

1. **Technology Entries**

   * Each entry has:

     * Name
     * Category (e.g., Language/Framework, Tool, Technique, Platform, Trend)
     * Rating (e.g., *Adopt*, *Trial*, *Assess*, *Avoid*)
     * Description (short summary)
     * Link(s) to further information
     * Optional tags/keywords

2. **Ratings & Guidance**

   * Rating reflects whether Norwegian companies should:

     * Focus on it
     * Trial it
     * Just assess it
     * Stop using it

3. **Discussion**

   * Each entry can include a discussion thread for comments/opinions.
   * Logged-in users can contribute to discussions.

4. **Filtering & Search**

   * Filter by category or rating.
   * Free text search for entries.

5. **Visualization (optional MVP feature)**

   * Radar-style visualization of technologies by rating.
   * Or simple list/grid view to start with.

---

## Technical Requirements

* **Frontend:** React/Next.js for web interface.
* **Backend:** Node.js (Fastify/Express) with REST API.
* **Database:** PostgreSQL (Supabase or Docker) to store entries and comments.
* **Auth:** Simple email login (magic link or admin-only for MVP).
* **Hosting:** Vercel (frontend) + Fly.io/Heroku (backend).

---

## Constraints

* Must allow quick addition and editing of entries (admin only for MVP).
* Must support public read-only access.
* Discussion optional for MVP but should be designed to be pluggable later.
* Visual radar view can be added later; start with list view.

---

## Goals

* **Thought Leadership:** Provide guidance for Norwegian companies on tech adoption.
* **Collaborative:** Enable internal discussions around entries.
* **Extensible:** Later versions can support voting, richer visualizations, or integrations.
* **Demo-Friendly:** MVP usable as a live demo within 1–2 days of coding.
