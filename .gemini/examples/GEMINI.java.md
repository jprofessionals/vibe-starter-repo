# Monorepo Context: Java/Quarkus + TS/Vite App

## Core Principles

- **Monorepo Structure**: The project is split into a `backend/` directory managed by Maven and a `frontend/` directory managed by `pnpm`.
- **Developer Experience**: The primary goal is a fast inner-loop development experience. Use Quarkus Dev Mode (`./mvnw quarkus:dev`) which provides live reload for backend changes.

---

## Backend: Java/Quarkus (`backend/`)

- **Language & JDK**: Use Java 21. Code MUST adhere to the Google Java Style Guide.
- **Framework**: Quarkus. Leverage its dependency injection (CDI) and configuration mechanisms.
- **Build Tool**: Use Maven and the provided wrapper (`./mvnw`).
- **Database**: Use Panache ORM for Hibernate. All entities should extend `PanacheEntity`.
- **API**: Build REST APIs using JAX-RS.
- **Testing**: Write tests with JUnit 5. Use Quarkus's `@QuarkusTest` runner for integration tests, which provides a fast-booting test environment. Use Mockito for mocking.
- **Native Compilation**: Remember that a key feature is GraalVM native compilation. Ensure new dependencies are compatible.

---

## Frontend: Vite + TypeScript (`frontend/`)

- **Framework**: This project uses SvelteKit with TypeScript and Vite.
- **Styling**: Use Tailwind CSS for all styling.
- **API Interaction**: Use the native `fetch` API for all calls to the backend.
- **Testing**: Use Vitest and Svelte Testing Library for component testing.
