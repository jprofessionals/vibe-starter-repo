# Monorepo Context: Kotlin/Ktor + TS/Vite App

## Core Principles

- **Monorepo Structure**: The project is split into a `backend/` directory managed by Gradle and a `frontend/` directory managed by `pnpm`.
- **API Contract**: The frontend and backend communicate via a REST API defined with an OpenAPI 3.0 specification. The schema is the source of truth.

---

## Backend: Kotlin/Ktor (`backend/`)

- **Build Tool**: This is a Gradle multi-project build. Use the Gradle wrapper (`./gradlew`) for all tasks.
- **Language**: Use Kotlin 1.9+. Adhere to the official Kotlin coding conventions.
- **Framework**: The web framework is Ktor. Use features like plugins for routing, serialization, and authentication.
- **Static Analysis**: All Kotlin code MUST pass `detekt` checks before merging.
- **Testing**: Use Kotest for all tests (unit, integration). Use MockK for mocking dependencies.
- **Database**: Use the Exposed DAO library for type-safe SQL. Database migrations are handled by Flyway.
- **Concurrency**: Use Kotlin Coroutines for all asynchronous operations.

---

## Frontend: Vite + TypeScript (`frontend/`)

- **Framework**: This project uses Vue 3 with TypeScript (using `<script setup>`) and Vite.
- **State Management**: Use Pinia for global state management.
- **Styling**: Use Tailwind CSS for all styling.
- **API Client**: Generate a type-safe API client from the OpenAPI schema using a tool like `openapi-typescript-codegen`.
- **Testing**: Use Vitest and Vue Testing Library for component testing.
