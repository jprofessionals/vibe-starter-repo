# AI agent starter-kit

This is a basic start-kid for use with assorted AI agents.

The only thing in place are guidelines and basic instructions that will help getting
the most out of the AI editor of choice. Like some suggested MCP server and a variation of rules
and project guidelines.

Feel free to do any kind of adjustments you like to get a setup more to your liking.

## Quick start (pick a path)

Choose **one** primary editor/agent to begin. You can mix later.

### Cursor (AI-first editor)

- **Get it:** https://www.cursor.com/download  
- **Why it shines:** Deep codebase awareness in-editor, in‑place edits & diffs, background agents, MCP support.  
- **1‑minute start:** Install → open this repo → press `⌘K / Ctrl+K` to “Ask AI” → run the prompt:  
  > “Create a minimal service in `/apps/api` with one endpoint `/health`, add tests, Dockerfile, and a GH Action to run tests on PR.”
- **good to know:** For a limited time cursor have the model `grok-code-fast-1` to use totally free. The base monthly subscription can be emptied fast if using the thinking models.

**Enable MCP in Cursor:** Settings → *MCP* → “Add new server” (see [MCP servers](#recommended-mcp-servers)).  
Docs: https://docs.cursor.com/welcome

### JetBrains **Junie** (agentic coding for IntelliJ/PyCharm/etc.)

- **Get it:** Install the *Junie* plugin from JetBrains Marketplace (IDE → Plugins → search “Junie”).  
- **Why it shines:** Autonomously plans multi‑step tasks using IDE features (navigate project, run code/tests, modify files), with review checkpoints.  
- **1‑minute start:** Open project → `Tools → Junie → New Task` → paste a goal (see starter prompts).  
Docs: https://www.jetbrains.com/help/idea/junie.html  
Getting started: https://www.jetbrains.com/help/junie/get-started-with-junie.html
- **good to know:** Junie integrates nicely with JetBrains IDEs, and is great when in that environment.

### **Gemini CLI** (terminal‑native agent, great with MCP)

- **Get it:** `npm i -g @google/gemini-cli` (or follow docs).  
- **Why it shines:** Local terminal workflow, ReAct loop, built‑in tools, first‑class MCP (works great with VS Code/Code Assist too).  
- **1‑minute start:** In project root, run `gemini` then:  
  > “Plan and implement a `scripts/release.ts` that bumps version, updates CHANGELOG from commits, tags, and opens a PR.”
Docs: https://cloud.google.com/gemini/docs/codeassist/gemini-cli
- **good to know:** You get a nice amount of tokens for free every day, when you run out you can fallback to the faster/cheaper flash-models, or you can supply an API key to "pay as you go".

### **Claude Code** (terminal/IDE companion)

- **Get it:** `npm i -g @anthropic-ai/claude-code` then `claude` in your project.  
- **Why it shines:** Fast “idea → plan → edits” loop in terminal; great diffs; strong at large refactors and test writing; MCP support.  
- **1‑minute start:**  
  > “Add an integration test suite for `/apps/api`. Generate realistic fixtures, run tests, and fix failures. Show diffs before applying.”
Docs: https://docs.anthropic.com/en/docs/claude-code/overview
- **good to know**: You burn tokens like crazy if selecting thinking models. Choose models suited for the task at hand, and use the thinking models sparingly. Non-thinking models are both much cheaper and much faster, increasing the roundtrip speed and your productivity.

### **Firebase Studio** (cloud workspace for AI apps)

- **Get access:** https://studio.firebase.google.com/  
- **Why it shines:** Cloud dev environment for building & shipping AI‑backed apps quickly with Gemini + Firebase services (hosting, auth, data), “agentic” experiences out of the box.  
- **1‑minute start:** New project → choose a template → let the assistant scaffold frontend + Firebase backend → deploy.
- **good to know:** Excellent starting point for building javascript based apps with Firebase. You can download the cloud based repo for local development at any time.

### Recommendation on what to use

Use whatever you want to try - but if you have no idea, here are some recommendations:

Your favourite editor is JetBrains IntelliJ?

- Use Junie

No favorite editor, but prefer IDE's to terminals and want the agent to be in the IDE?

- Use Cursor

Want the AI agent to be outside of the IDE?

- Use Gemini CLI - works well for terminal based workflows
- Use Claude Code - if money is *not* an issue (i.e. I do not pay for the use myself...)

I want to make web-first apps?

- Start with Firebase Studio

### Perform basic setup

Set up the editor of choice, add MCP servers and other tools as needed.

#### Cursor

See the [USING_CURSOR.md](.cursor/USING_CURSOR.md) file for more information.

#### Junie

See the [USING_JUNIE.md](.junie/USING_JUNIE.md) file for more information.

#### Gemini CLI

See the [GEMINI.md](.gemini/README.md) file for more information.


## Recommended MCP servers

> **MCP (Model Context Protocol)** lets tools/IDEs expose **capabilities** (files, DBs, GitHub, APIs, docs) to AI agents in a safe, standard way.

- **Context7 (Upstash)** — up‑to‑date framework & API docs in prompts.  
  Repo: https://github.com/upstash/context7
- **GitHub MCP** — issues/PRs/actions/releases/security advisories via natural language.  
  Repo: https://github.com/github/github-mcp-server
- **Community lists**  
  • Official servers & examples: https://github.com/modelcontextprotocol/servers  
  • Curated directory: https://mcpservers.org/

> **Cursor MCP sample (`~/.cursor/mcp.json`)**
>
> ```jsonc
> {
>   "mcpServers": {
>     "context7": {
>       "command": "npx",
>       "args": ["-y", "@upstash/context7"],
>       "env": { "CONTEXT7_API_KEY": "… (if needed)" }
>     },
>     "github": {
>       "command": "npx",
>       "args": ["-y", "github-mcp-server"],
>       "env": { "GITHUB_TOKEN": "ghp_..." }
>     }
>   }
> }
> ```

## Create your first prototype

1. **Clone this repo**, rename it, open in your editor of choice.  
2. **Pick one agent** (Cursor/Junie/Gemini CLI/Claude Code).  
3. **Run one of the starter prompts** below to ship a tiny, end‑to‑end slice (API + test + CI).  
4. **Commit early, commit often.** Let the agent author the first PR; you review and merge.

> **Fast demo ideas**
>
> - “Create a Python function to fetch GitHub issues and display them as JSON.”
> - “Create a Python script to archive all items (recursively) in a given folder (input to the script) by the use of AI models using https://openrouter.ai/. The script must as default run in dry mode (just printing archive suggestions) and only execute actual file move and rename operations if the user confirms.”

## Working agreements & docs (PRD? PFD? PDD?)

Heavy docs are overkill for vibe coding. Use a **one‑pager** you can paste into any agent. (Skip parts you don't need, for example constraints if you don't care, etc)

**Project Brief (copy/paste template)**

```markdown

# Problem & Outcome

- Problem: …
- Target user: …
- Outcome (single sentence): …

# Scope (MVP)

- Must have: …
- Nice to have (later): …

# Constraints

- Tech stack: …
- Perf/latency: …
- Security/compliance: …

# Inputs & Interfaces

- Inputs/data sources: …
- External APIs/tools (MCP): …

# Definition of Done

- Tests: …
- Lint/format: …
- CI passes, PR approved, deploy preview green.
```

> **When to escalate to a PRD/PDD?**  
>
> - **PRD (Product Requirements Doc)**: multiple teams, external stakeholders, or legal/compliance needs.  
> - **PDD/Design Spec**: non‑trivial architecture or risks (auth, data models, migrations, multi‑service concerns).
>
> Start with the one‑pager; promote to PRD/PDD only when complexity truly requires it.

## Agent interaction patterns

- **Limit the scope of each task.** Ask for one thin slice at a time (scaffold → tests → hook up CI → add e2e). This minimizes thrash and makes diffs reviewable.
- **Always verify the agent's work.** Run the code, inspect the results, and ask the agent to fix any issues.
- **If an agent gets stuck: *undo, restate, constrain*.** Revert the last diff, restate goals, add constraints or examples.  
- **Make task lists explicit.** Ask the agent to propose a 4–7 step plan, then approve/edit before execution.  
- **Always ask for diffs and tests.** Require unit + integration tests with fixtures.  
- **Ground the agent.** Attach the Project Brief, paste error logs, and point it at MCP tools (docs, GitHub, DB).  
- **Pin outputs.** For anything important: request structured outputs (JSON) and copy them into the repo.

---

## Example starter prompts (copy/paste)

> Use these verbatim in Cursor, Junie, Gemini CLI (`gemini`), or Claude Code (`claude`).

1) **Bootstrap API + tests + CI**

    ```text
    Goal: Create `/apps/api` (TypeScript, Fastify/Express) with `/health` returning `{status:"ok",version}`.
    Add Vitest (or Jest) unit tests and an integration test hitting `/health` via supertest.
    Add Dockerfile and a GitHub Action that runs tests on PR.
    Propose a 5–7 step plan; wait for my approval; then execute with diffs. Use Context7 MCP for any library APIs.
    Definition of Done: all tests pass locally; CI green; PR opened with clear description.
    ```

2) **Add web app + e2e**

    ```text
    Goal: Add `/apps/web` (Vite or Next.js) with one form posting to `/apps/api`.
    Add Playwright e2e test covering the form workflow.
    Update CI to run web tests headless; include test artifacts on failure.
    Plan first; then implement with small diffs. Include accessibility checks (aria labels).
    ```

3) **Refactor + guardrails**

    ```text
    Goal: Identify the 3 highest-complexity modules and refactor them.
    Add type coverage checks, strict TS config, and precommit hooks (lint-staged + biome/eslint + prettier).
    Report risk items and how you mitigated each. Provide a changelog summary for the PR.
    ```

4) **Ship docs automation**

    ```text
    Goal: Add `scripts/release.(ts|py)` to bump version, update CHANGELOG from conventional commits, tag, and open a GitHub release + PR.
    Dry run locally first; then wire a GH Action that runs on `release/*` branches.
    ```

---

## Project layout

Suggested project layout for use with agents.

```text
.
├─ .cursor/          # Cursor settings + MCP config (if using Cursor)
├─ .junie/            # Junie task/playbooks (when using Junie)
├─ prompts/          # Saved prompts / briefs
└─ scripts/          # Local tooling the agent can call

```

## Safety, quality & review

- **Never auto‑merge.** Agents open PRs; humans review.
- **Pin secrets to envs, not prompts.** Use environment variables for MCP servers and tool credentials.
- **Prefer typed interfaces and tests over prose.** Ask agents to *prove* changes by tests and type checks.
- **Track diffs, not blobs.** Always request diffs before applying large edits.
- **Use MCP for *grounding*.** Prefer MCP‑backed docs/SDKs (Context7, GitHub MCP) over generic web search in prompts.

## Suggested resources

- **Cursor** — [https://docs.cursor.com/welcome](https://docs.cursor.com/welcome)
- **Junie (JetBrains)** — [https://www.jetbrains.com/help/idea/junie.html](https://www.jetbrains.com/help/idea/junie.html)
- **Gemini CLI** — [https://cloud.google.com/gemini/docs/codeassist/gemini-cli](https://cloud.google.com/gemini/docs/codeassist/gemini-cli)
- **Claude Code** — [https://docs.anthropic.com/en/docs/claude-code/overview](https://docs.anthropic.com/en/docs/claude-code/overview)
- **Firebase Studio** — [https://firebase.google.com/products/generative-ai](https://firebase.google.com/products/generative-ai)
- **MCP spec & servers** — [https://modelcontextprotocol.io/specification/](https://modelcontextprotocol.io/specification/) | [https://github.com/modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers)
- **Context7** — [https://github.com/upstash/context7](https://github.com/upstash/context7)
- **GitHub MCP server** — [https://github.com/github/github-mcp-server](https://github.com/github/github-mcp-server)

### Prompting guides (short, high‑signal)

- Anthropic: Prompt engineering overview — [https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)
- OpenAI: Prompt engineering best practices — [https://platform.openai.com/docs/guides/prompt-engineering/](https://platform.openai.com/docs/guides/prompt-engineering/)
- Google: Prompt design strategies (Gemini) — [https://ai.google.dev/gemini-api/docs/prompting-strategies](https://ai.google.dev/gemini-api/docs/prompting-strategies)

### Additional reading

- [Gemini CLI vs Claude Code vs Cursor](https://blog.getbind.co/2025/06/27/gemini-cli-vs-claude-code-vs-cursor-which-is-the-best-option-for-coding/)

