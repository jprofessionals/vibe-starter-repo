# Using Cursor in This Repository

## Goals

- Make AI help **reliable and repeatable** across the team and stacks.
- Keep sensitive info **out of AI context**.
- Encode our standards as **Cursor Project Rules**.

## Required local setup

- Use Cursor with Project Rules enabled. Rules live in `.cursor/rules` and auto‑attach by path globs. See the rule front‑matter in those files.  
  _Ref:_ Cursor Rules docs.  
- Respect `.cursorignore` and `.cursorindexingignore` for secrets and noise (AI access vs. indexing).  
  _Ref:_ Cursor Ignore docs.

## Security & privacy

- Add/update `.cursorignore` (and the global ignore in Cursor settings) to exclude secrets. Cursor blocks access to ignored files, but **LLM unpredictability means this is best‑effort**—treat it as defense‑in‑depth and don’t put secrets in code.  
  _Refs:_ Cursor ignore overview and caveats.  
- Never place credentials in code; use env vars or secret stores.

## Commit messages

- We use **Conventional Commits** with optional scope and body. e.g.:
  - `feat(web): add lazy route loading`
  - `fix(api)!: validate JWT audience`
- Cursor’s **AI Commit Message** feature will follow your history and our rule prompts; always verify before committing.  
  _Refs:_ Conventional Commits, Commitlint & Cursor AI commit messages.  

## Asking the Agent

- Prefer **Inline Edit** for scoped refactors; use **Chat** for plans and reviews. Attach files and rules explicitly when needed.
- Use `@Docs` to cite framework docs. We have curated documentation in `.cursor/docs/` for:
  - **Spring Boot Security**: `.cursor/docs/spring-boot-security.md`
  - **Vite + TypeScript**: `.cursor/docs/vite-typescript.md`
  - **Kotlin + Ktor**: `.cursor/docs/kotlin-ktor.md`
  - **Python + uv**: `.cursor/docs/python-uv.md`
- Index these local docs in Cursor Settings → Docs → Add Source → File/Folder

## Source of truth

- Tooling versions are pinned in lockfiles (`uv.lock`, `package-lock.json/pnpm-lock.yaml`, Gradle catalogs) and committed.  
  _Ref:_ uv lockfile is meant to be committed.

## Rules

You can either add .mdc files manually to the .cursor/rules folder, or you can use the command `New Cursor Rule` in the "chat" tab. And then follow the prompts to create the rule.

## DOCS!

Cursor supports adding Docs the agent can cite inline:

The easiest way to add docs is to use the command `New Cursor Doc` in the "chat" tab. And then follow the prompts to create the doc.