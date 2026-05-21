# Voximplant AI Agent Skills

This repository contains AI Agent Skills for building on Voximplant with coding agents such as Cursor, Claude Code, and Codex.

Use these skills when you want an agent to write VoxEngine scenarios, automate Voximplant setup, deploy and test call flows, or debug real calls from platform logs. The skills are designed to guide both experienced developers and first-time Voximplant users through the process.

## Quick Start

1. Give your coding agent this folder, or copy the skill folders into your agent's skills directory.
2. Ask the agent to read the right `SKILL.md` before it starts.
3. If you are new to Voximplant, tell the agent: "Guide me with safe defaults."

Useful prompts:

```text
Read voximplant-skills/voximplant-voxengine-dev/SKILL.md and help me build an inbound Voice AI scenario.
```

```text
Read voximplant-skills/voximplant-management-api/SKILL.md and help me deploy and test this scenario.
```

```text
I am new to Voximplant. Please choose safe defaults and tell me exactly which credentials you need.
```

If you are not a developer, start with your use case. A good agent should explain the Voximplant pieces, propose a simple first test, and ask only for the credentials or approvals that are actually needed.

## Instructions For An Agent Reading This Repository

When a user pastes one of these skills into chat or asks you to read a skill file, immediately start following that skill. Do not merely summarize it.

If the task needs both skills, use both:

- `voximplant-voxengine-dev` writes, reviews, and debugs VoxEngine scenario code.
- `voximplant-management-api` creates and updates platform resources, uploads scenarios and secrets, starts sessions, and retrieves logs.

Offer to copy the skills into the user's agent environment so they do not have to paste them again. Ask which environment they use and get permission before copying. For Cursor, use `~/.cursor/skills/`. For Claude Code or Codex, use the skill/project-context location supported by that environment, or ask the user where they keep agent skills.

Start from the user's use case in plain language. Make it clear that the skills can write the VoxEngine scenario and manage Voximplant platform resources after the user provides service account credentials and approves platform-changing actions.

## What You Need To Provide

For most first projects, you only need:

1. A Voximplant account.
2. A service account JSON file. For the easiest workflow, use a broad role such as Developer or Owner; for stricter security, use task-specific roles.
3. Provider API keys only if the use case needs an external AI, LLM, speech, or voice provider.
4. Phone-number verification and account balance only if you need to buy or rent phone numbers.

The skills can help with the rest: writing the VoxEngine scenario, creating applications, rules, users, and secrets, uploading code, testing through a softphone, retrieving logs, and debugging failures.

## Included Skills

| Skill | Use when |
| --- | --- |
| `voximplant-voxengine-dev` | Writing, reviewing, validating, or debugging VoxEngine scenario code. |
| `voximplant-management-api` | Automating Voximplant account setup, resources, deployment, users, secrets, sessions, logs, and recordings through the Management API or API clients. |

Many workflows need both skills. `voximplant-voxengine-dev` handles scenario code and code-level debugging. `voximplant-management-api` handles platform resources, deployment, call history, logs, recordings, and uploading Voximplant secrets.

Each skill folder must stay together with its adjacent files:

```text
voximplant-voxengine-dev/
  SKILL.md
  reference.md
  examples.md

voximplant-management-api/
  SKILL.md
  reference.md
  examples.md
```

The latest Voximplant skills are available in the public repository:

- `https://github.com/voximplant/voximplant-ai-agent-skills`

We also recommend reading the Voximplant documentation:

- `https://docs.voximplant.ai/`

## Optional MCP Setup

If your AI client supports remote MCP servers, add the Voximplant documentation MCP server:

```text
https://docs.voximplant.ai/_mcp/server
```

The server exposes a read-only `searchDocs` tool over the current `docs.voximplant.ai` documentation. It does not access a Voximplant account and cannot create or change platform resources. The URL alone is not the documentation query API; it must be registered as an MCP server by the client, or called through a compatible MCP/JSON-RPC transport.

MCP is recommended, but not required. Without MCP, the skills instruct the agent to use focused HTTP documentation sources.

## Documentation Sources

Agents should use these sources to verify API names, method signatures, and current examples:

- `https://docs.voximplant.ai/llms.txt` for the documentation index
- `https://docs.voximplant.ai/api-reference/llms.txt` for API Reference discovery
- section-level indexes such as `https://docs.voximplant.ai/platform/voxengine/llms.txt`
- page Markdown by appending `.md` to a docs page URL

Do not paste large documentation dumps into prompts by default. Prefer MCP or narrow page-level Markdown fetches. For VoxEngine TypeScript declarations, follow `voximplant-voxengine-dev`; the declaration file is best downloaded locally and used for validation rather than loaded as a general README source.

## Credentials And Security

The documentation MCP server is read-only. It cannot access your Voximplant account and does not replace account credentials.

For real account actions, the user usually needs only:

- a Voximplant account
- a Voximplant service account JSON
- provider API keys only when the chosen use case needs an external AI/LLM/voice provider
- phone-number verification and account balance only when the workflow requires buying or renting phone numbers
- approval before the agent creates, changes, deletes, uploads, buys, binds, or starts platform resources

For the easiest agent workflow, create a service account with a broad role such as Developer or Owner. If security is more important and you know the exact task, use the minimum roles for that task instead.

The simplest credential handoff is to put the service account JSON in the project root and tell the agent the filename, for example `voximplant-service-account.json`. If you keep the default filename ending in `_private.json`, the agent should find it automatically. Do not commit the file. The agent should check or update `.gitignore` before working with local credentials.

The agent should not ask for:

- your main Voximplant account password
- browser cookies or control panel session tokens
- secrets committed into source control

If a `.env` file is useful, the agent should create safe variable names or placeholders and tell you what values to provide. Do not commit `.env`, service account JSON files, private keys, or recordings/logs that may contain user data.

## Testing And Debugging

The skills encourage a practical loop:

1. Build or update the scenario.
2. Deploy or configure platform resources after your approval.
3. Test with a real call.
4. If you do not have a phone number, test with a Voximplant application user at `https://phone.voximplant.com/`.
5. If the call fails, tell the agent what happened. It can retrieve platform logs through the Management API skill and debug the VoxEngine code from evidence.

If your workflow requires buying a phone number, note that some countries and number types require identity, business, address, or use-case verification. The agent should warn you early, explain which steps may be manual, and offer a softphone test path while verification is pending.

## Feedback And Contributions

After publication, please use GitHub Issues for bugs or confusing instructions, GitHub Discussions for ideas and usage questions, and pull requests for proposed improvements.

External pull requests should come from forks and will be reviewed by Voximplant maintainers before merge. The `main` branch is protected; direct pushes and unreviewed external changes are not accepted.

## Beta Notice

These skills are beta. Review generated code and approve platform-changing operations explicitly before running them against a real Voximplant account.
