---
name: apiiro-mcp-secure-prompt-examples
description: |
  Generates 10 example `apiiro_secure_prompt` MCP tool invocations tailored to the user's current repository, runs them, and produces a before/after CSV showing the original prompt vs the security-enriched version. Analyzes the repo's tech stack, frameworks, API patterns, data models, and integrations, then produces realistic developer prompts that would trigger security classification — demonstrating how `apiiro_secure_prompt` adds value for that specific codebase. Requires Apiiro MCP to be configured. Use when the user wants to see what secure-prompt looks like in practice, wants to demo the feature, or needs onboarding examples for their team. Trigger on phrases like "show me secure-prompt examples", "generate example prompts for my repo", "demo secure-prompt", or "what prompts would trigger security guidance".
---

# Apiiro Secure Prompt Examples (MCP)

Generates 10 realistic `apiiro_secure_prompt` MCP tool invocations tailored to the current repository, then runs them and produces a CSV showing the original prompt alongside the security-enriched output.

## Prerequisites

| Requirement | Notes |
|-------------|-------|
| Apiiro MCP configured | MCP client is connected to an Apiiro MCP server; tools are available after a successful session. |
| `apiiro_secure_prompt` tool available | If it does not appear, the server may not expose that tool — check with your Apiiro administrator. |
| `apiiro_resolve_repository_key` tool available | Needed to obtain the `repository_key` for the current repo. |

## What to do

1. **Resolve the repository key** — call `apiiro_resolve_repository_key` once with:
   - `repository_name` from the current repo
   - `remote_url` from `git remote get-url origin`
   - `branch` from `git branch --show-current`

   Store the returned `repository_key` for all subsequent calls.

2. **Analyze the repository** — explore the codebase to identify:
   - Language, frameworks, and runtime (e.g., TypeScript/Node, Python/Django, Go/Gin)
   - API patterns (REST, GraphQL, gRPC, WebSocket)
   - Authentication and authorization mechanisms
   - Database and data models
   - External service integrations (cloud providers, third-party APIs, SCM, messaging)
   - CI/CD and deployment configuration
   - File handling, crypto, or payment logic
   - Any domain-specific security surface (e.g., multi-tenant isolation, webhook processing)

3. **Generate 10 prompts** that meet ALL of these criteria:
   - **Realistic**: something a developer on this repo would actually type
   - **Specific**: references actual files, modules, patterns, or domain concepts from the repo
   - **Classifies as YES**: falls into one of these classifier categories — backend/API code, database operations, auth/secrets, user data, external integrations, CI/CD, file operations, crypto, network, input validation, or trust boundary changes
   - **Diverse**: cover at least 5 different classifier YES categories across the 10 prompts

4. **Present the examples** — for each, show:
   - The `apiiro_secure_prompt` tool call with its parameters
   - A one-line note on which security areas would be addressed
   - Only include `file_path` when scoping to a specific file genuinely adds value — most prompts should work without it

   Example format:
   ```
   apiiro_secure_prompt(
     developer_prompt: "add a webhook endpoint that receives GitHub push events and triggers a pipeline rebuild",
     repository_key: "<resolved_key>"
   )
   ```
   > Security areas: input validation, external service integration, webhook signature verification

5. **Ask the user if they'd like to run them.** When they confirm:

6. **Run all 10 tool calls** — invoke `apiiro_secure_prompt` for each prompt in parallel, passing `developer_prompt`, `repository_key`, and optionally `file_path`.

7. **Generate a CSV file** at `docs/secure-prompt-examples.csv` (or a path the user specifies) with these columns:
   - `prompt` — the original developer prompt text
   - `file_path` — the `file_path` parameter value, or `None` if not used
   - `before` — the original prompt (unchanged, as a baseline)
   - `after` — the enriched prompt returned by the MCP tool

   Use proper CSV quoting — double-quote fields that contain commas or quotes, and escape inner quotes by doubling them.

8. **Show a summary** of the results — highlight which examples got the most interesting enrichment and the security categories covered.

## Constraints

- Do NOT invoke `apiiro_secure_prompt` until the user confirms they want to run the examples
- Do NOT fabricate file paths — only reference files/dirs that actually exist in the repo
- Keep prompts concise (1-2 sentences) like a developer would naturally type
- Each prompt should be a task to implement, not a question or review request (those would classify as NO)
