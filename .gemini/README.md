# README: Supercharging Your Workflow with Gemini CLI

This is the projects `.gemini` directory! This folder is the control center for customizing how `gemini-cli` interacts with your code. By configuring the files here, you can transform the AI from a general-purpose assistant into a highly specialized expert on your specific tech stack and project conventions.

This guide will walk you through the best practices for leveraging `gemini-cli`'s powerful features.

---

## 1. The `GEMINI.md` Context File: Your Project's Brain

The `GEMINI.md` file is the most crucial piece of configuration. It provides persistent instructions and context to the AI for this specific project.

### Best Practices for `GEMINI.md`

* **Be Specific**: Clearly define the tech stack, libraries, coding standards, and architectural patterns. The more specific you are, the better the AI's output will be.

* **Use Modular Imports**: For complex projects, don't put everything in one file. Break down your context into smaller, focused markdown files (e.g., `frontend-rules.md`, `backend-db-schema.md`) and import them into your main `GEMINI.md` using the `@./path/to/file.md` syntax.

* **Implement Gated Protocols**: To make the AI's behavior more predictable and safe, structure your `GEMINI.md` with distinct "modes" or "protocols." This prevents the AI from taking unintended actions.

#### Example Protocol Structure:

```
# --- TOP LEVEL INSTRUCTIONS ---
# Always follow these rules.

<PROTOCOL:PLAN>
# When I ask for a plan, you are in "Plan Mode".
# - Your ONLY job is to analyze the request and list the steps to solve it.
# - You MUST NOT write any code or modify any files in this mode.
# - List the files you will need to create or edit.
</PROTOCOL:PLAN>

<PROTOCOL:IMPLEMENT>
# When I approve a plan, you are in "Implement Mode".
# - Your job is to execute the approved plan.
# - You are now allowed to use file system tools to write and edit code.
# - Stick strictly to the steps outlined in the plan.
</PROTOCOL:IMPLEMENT>
```

---

## 2. MCP Servers & `settings.json`: Extending the AI's Abilities

Model Context Protocol (MCP) servers allow you to give `gemini-cli` new tools. You can run a local server that exposes custom functions (e.g., interacting with a proprietary API, running a complex build script), and the AI can learn to use them.

* **Setup**: You configure MCP servers in your `~/.gemini/settings.json` file.

* **Use Case**: Create a tool that fetches live data from your staging environment, or add web search capabilities.

### Example: `settings.json` with Context7

Here is an example of what your `~/.gemini/settings.json` file could look like. This example sets up an MCP server for `context7`, a powerful tool for adding web search capabilities to `gemini-cli`.

```json
{
  "mcpServers": {
    "context7": {
      "command": "npx",
      "args": [
        "-y",
        "ctx-agent@latest",
        "--port",
        "7777"
      ],
      "timeout": 15000,
      "trust": true
    }
  }
}
```

### Using Context7 in `gemini-cli`

Once your `settings.json` is configured, `gemini-cli` will automatically start the server when needed. You can then use the `@context7` tool to fetch information from the web and include it in your prompts.

**Example Usage:**
```
> Can you explain the main features of the Ktor framework? @context7:"Ktor framework features"
```
This command will use the `context7` tool to search the web for "Ktor framework features" and provide the results as context to the Gemini model, allowing it to give you a well-informed answer.

---

## 3. Direct Prompts: For Quick, One-Off Tasks

While `GEMINI.md` is for persistent context, direct prompts are for immediate needs.

* **Interactive Mode**: Run `gemini` to start a chat session.

* **Non-Interactive (`-p`)**: Pass a single instruction and get a direct response.

  * `gemini -p "Summarize the changes in @./src/main.ts"`

* **Piping**: Pipe content directly into the CLI.

  * `cat README.md | gemini -p "Rewrite this to be more concise."`

---

## 4. Custom Commands: Your Personal Shortcuts

If you find yourself typing the same complex instructions repeatedly, save them as custom commands.

* **How it works**: Create `.toml` files in your `~/.gemini/commands/` directory. Each file defines a command, a description, and the prompt template to execute.

* **Example (`~/.gemini/commands/test/gen.toml`):**

  ```toml
  description = "Generates a pytest unit test for a given function."
  prompt = """
  You are a testing expert specializing in Python.
  Based on the code in the file I provide, write a comprehensive
  unit test using pytest for the following requirement: {{args}}
  """
  ```

* **Usage**: `/test:gen "Test the user login endpoint" @./api/auth.py`

---

## 5. Environment Variables: The Ultimate Override

For the highest level of control, you can use environment variables.

* **`GEMINI_SYSTEM_MD`**: This is the most powerful option. By setting this variable to the path of a markdown file, you can **completely replace** the default system prompt of `gemini-cli`. This allows you to fundamentally change the AI's core behavior, but should be used with caution as you might override essential instructions needed for the CLI to function correctly.
