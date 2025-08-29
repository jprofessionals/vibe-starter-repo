# Monorepo Context: Full-Stack TypeScript App

## Core Principles

- **Language**: All code MUST be written in TypeScript with strict mode enabled.
- **Monorepo Tool**: This project uses `pnpm` workspaces. All dependency management MUST use `pnpm` commands (`pnpm add`, `pnpm install`, etc.).
- **Code Formatting**: All files MUST be formatted with Prettier on save.
- **Linting**: All code MUST pass ESLint checks. Do not disable rules inline without a comment explaining why.
- **Structure**: The monorepo is organized into `apps/` (for deployable applications like the web frontend and the API) and `packages/` (for shared libraries like UI components or validation schemas).

---

## Backend: Node.js (`apps/api`)

- **Framework**: Use Fastify for its performance and low overhead.
- **Database ORM**: Use Prisma for type-safe database access. All database schema changes MUST be done through Prisma migrations (`prisma migrate dev`).
- **Testing**: Use Vitest for unit and integration tests. Tests should be co-located with the source files in `*.test.ts` files.
- **API Specification**: APIs should be documented using OpenAPI 3.0 standards.

---

## Frontend: Vite + TypeScript (`apps/web`)

- **Framework**: This project uses React with TypeScript and Vite.
- **Component Structure**: Follow a feature-based folder structure.
- **State Management**: Use Zustand for global state management due to its simplicity.
- **Styling**: Use Tailwind CSS for all styling. Avoid writing custom CSS files.
- **Testing**: Use Vitest and React Testing Library for component testing.

---

## Shared Packages (`packages/*`)

- **`@repo/ui`**: Shared React UI components.
- **`@repo/eslint-config`**: The central ESLint configuration.
- **`@repo/typescript-config`**: The central `tsconfig.json` settings.
