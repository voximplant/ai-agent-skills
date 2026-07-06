# Voximplant AI Agent Skills

Voximplant AI Agent Skills help coding agents build, deploy, test, and debug Voximplant voice applications. They work with Cursor, Claude Code, Codex, and other clients that support the Agent Skills format.

Use them when you want an agent to write VoxEngine scenarios, automate Voximplant setup, deploy and test call flows, or debug real calls from platform logs.

## Quick Install


| Client                     | Recommended path                                               | Status                   |
| -------------------------- | -------------------------------------------------------------- | ------------------------ |
| Cursor                     | [Install from cursor.directory or local plugin](#install-in-cursor)   | Listed on cursor.directory; local plugin available |
| Claude Code                | [Add this repository as a plugin marketplace](#install-in-claude-code) | Available from this repo |
| Codex                      | [Add this repository as a plugin marketplace](#install-in-codex)       | Available from this repo |
| Manual setup (only skills) | [Copy standalone skill folders](#manual-install)                      | Available from this repo |

## Contents

- [Included Skills](#included-skills)
- [Install In Cursor](#install-in-cursor)
- [Install In Claude Code](#install-in-claude-code)
- [Install In Claude Cowork](#install-in-claude-cowork)
- [Install In Codex](#install-in-codex)
- [Manual Install](#manual-install)
- [Useful Prompts](#useful-prompts)
- [What You Need To Provide](#what-you-need-to-provide)
- [Documentation Sources](#documentation-sources)
- [Credentials And Security](#credentials-and-security)
- [Testing And Debugging](#testing-and-debugging)
- [Feedback And Contributions](#feedback-and-contributions)
- [Beta Notice](#beta-notice)

## Included Skills


| Skill                       | Use when                                                                                                                                              |
| --------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| `voximplant-voxengine-dev`  | Writing, reviewing, validating, or debugging VoxEngine scenario code.                                                                                 |
| `voximplant-management-api` | Automating Voximplant account setup, resources, deployment, users, secrets, sessions, logs, and recordings through the Management API or API clients. |


Many workflows need both skills. `voximplant-voxengine-dev` handles scenario code and code-level debugging. `voximplant-management-api` handles platform resources, deployment, call history, logs, recordings, and uploading Voximplant secrets.

## Install In Cursor

The official Cursor Marketplace is currently closed to new submissions. Cursor recommends [cursor.directory](https://cursor.directory), a community-run directory that auto-detects plugins from a public GitHub repository.

### Cursor Directory (Community)

Find **Voximplant AI Agent Skills** on [cursor.directory](https://cursor.directory) and follow the install instructions shown there.

If it is not listed yet, anyone can add it: go to [cursor.directory/plugins/new](https://cursor.directory/plugins/new), sign in with GitHub or Google, paste `https://github.com/voximplant/ai-agent-skills`, and submit. The repository already ships the `.cursor-plugin/marketplace.json`, `plugin.json`, and `skills/*/SKILL.md` layout that cursor.directory auto-detects.

For other install paths, see below.

### Local Plugin Testing

Use this when you want the full Cursor plugin package installed directly, without going through cursor.directory.

macOS/Linux:

```bash
git clone https://github.com/voximplant/ai-agent-skills.git
mkdir -p ~/.cursor/plugins/local
ln -s "$(pwd)/ai-agent-skills/plugins/voximplant-ai-agent-skills" ~/.cursor/plugins/local/voximplant-ai-agent-skills
```

Windows PowerShell:

```powershell
git clone https://github.com/voximplant/ai-agent-skills.git
New-Item -ItemType Directory -Force "$env:USERPROFILE\.cursor\plugins\local"
Copy-Item -Recurse -Force "ai-agent-skills\plugins\voximplant-ai-agent-skills" "$env:USERPROFILE\.cursor\plugins\local\voximplant-ai-agent-skills"
```

After copying or linking the plugin, run **Developer: Reload Window** in Cursor.

### Standalone Skills

If you only need the two skills and do not need plugin metadata, use [Manual Install](#manual-install) to copy the skill folders into Cursor.

### Team Marketplace

For Cursor Teams or Enterprise, an admin can import this GitHub repository as a team marketplace from the Cursor dashboard when the workspace supports team marketplaces.

## Install In Claude Code

There are two command surfaces:

- Use slash commands inside an interactive Claude Code session.
- Use `claude plugin ...` commands from a normal terminal such as PowerShell, macOS Terminal, or a Linux shell.

Inside Claude Code:

```text
/plugin marketplace add voximplant/ai-agent-skills
/plugin install voximplant-ai-agent-skills@voximplant
```

From PowerShell, macOS Terminal, or a Linux shell:

```bash
claude plugin marketplace add voximplant/ai-agent-skills
claude plugin install voximplant-ai-agent-skills@voximplant
```

Optional checks:

```bash
claude plugin marketplace list
claude plugin list
claude plugin details voximplant-ai-agent-skills@voximplant
```

After installation, start a new Claude Code session or reload plugins if Claude Code asks you to do so.

## Install In Claude Cowork

Claude Code and Claude Cowork use different plugin contexts. Installing the plugin in Claude Code does not automatically make it available in Cowork.

To install in Cowork:

1. Open Claude and switch to **Cowork**.
2. Open **Customize** in the left sidebar.
3. Open **Plugins**.
4. Choose **Add marketplace**.
5. Enter:

   ```text
   voximplant/ai-agent-skills
   ```

   If GitHub shorthand is not accepted, use the full repository URL:

   ```text
   https://github.com/voximplant/ai-agent-skills
   ```

6. Open the added marketplace.
7. Find **Voximplant AI Agent Skills**.
8. Click **Install**.
9. Open a new Cowork chat and check that the Voximplant plugin appears in the loaded tools or plugin context.

Cowork does not provide the same local terminal and git workflow as Claude Code. Use Claude Code for local development loops and Cowork for collaborative agent workflows where a GUI-based plugin install is preferred.

## Install In Codex

Add this repository as a plugin marketplace:

```text
codex plugin marketplace add voximplant/ai-agent-skills
```

If this command fails, see [Codex Troubleshooting](#codex-troubleshooting).

Then install the plugin from the Codex UI:

1. Open the plugin browser in Codex (`/plugins`).
2. To the right of the search field, open the marketplace dropdown.
3. Change **Built by OpenAI** to **Voximplant Plugins**.
4. In **Developer Tools**, find **Voximplant AI Agent Skills**.
5. Click install for **Voximplant AI Agent Skills**.

Public Codex Plugin Directory publication is a follow-up. The repository marketplace path works before public directory approval.

### Codex Troubleshooting

The `codex plugin marketplace ...` commands are available in recent Codex CLI versions. If Codex returns `unexpected argument 'marketplace'`, your shell is probably running an older Codex binary or a stale executable from `PATH`.

On Windows, update Codex with Winget:

```powershell
winget upgrade --id OpenAI.Codex
codex --version
codex plugin marketplace --help
```

If `winget` says Codex is up to date but `codex --version` still shows an older version, open a new terminal and check which executable is being used:

```powershell
Get-Command codex -All
where.exe codex
```

Make sure the active `codex` command points to the updated OpenAI Codex install, then run the marketplace command again.

## Manual Install

Manual install copies only the standalone skill folders. Use this when your client does not support plugins yet, or when you prefer to manage skills directly without marketplace/plugin metadata.

Copy these folders:

```text
plugins/voximplant-ai-agent-skills/skills/voximplant-voxengine-dev
plugins/voximplant-ai-agent-skills/skills/voximplant-management-api
```

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

Common destinations:


| Client      | User-level destination | Project-level destination |
| ----------- | ---------------------- | ------------------------- |
| Cursor      | `~/.cursor/skills/`    | `.cursor/skills/`         |
| Claude Code | `~/.claude/skills/`    | `.claude/skills/`         |
| Codex       | `~/.agents/skills/`    | `.agents/skills/`         |


### Cursor Standalone Skills

macOS/Linux:

```bash
git clone https://github.com/voximplant/ai-agent-skills.git
mkdir -p ~/.cursor/skills
cp -R ai-agent-skills/plugins/voximplant-ai-agent-skills/skills/voximplant-voxengine-dev ~/.cursor/skills/
cp -R ai-agent-skills/plugins/voximplant-ai-agent-skills/skills/voximplant-management-api ~/.cursor/skills/
```

Windows PowerShell:

```powershell
git clone https://github.com/voximplant/ai-agent-skills.git
New-Item -ItemType Directory -Force "$env:USERPROFILE\.cursor\skills"
Copy-Item -Recurse -Force "ai-agent-skills\plugins\voximplant-ai-agent-skills\skills\voximplant-voxengine-dev" "$env:USERPROFILE\.cursor\skills\"
Copy-Item -Recurse -Force "ai-agent-skills\plugins\voximplant-ai-agent-skills\skills\voximplant-management-api" "$env:USERPROFILE\.cursor\skills\"
```

### Claude Code Standalone Skills

macOS/Linux:

```bash
git clone https://github.com/voximplant/ai-agent-skills.git
mkdir -p ~/.claude/skills
cp -R ai-agent-skills/plugins/voximplant-ai-agent-skills/skills/voximplant-voxengine-dev ~/.claude/skills/
cp -R ai-agent-skills/plugins/voximplant-ai-agent-skills/skills/voximplant-management-api ~/.claude/skills/
```

Windows PowerShell:

```powershell
git clone https://github.com/voximplant/ai-agent-skills.git
New-Item -ItemType Directory -Force "$env:USERPROFILE\.claude\skills"
Copy-Item -Recurse -Force "ai-agent-skills\plugins\voximplant-ai-agent-skills\skills\voximplant-voxengine-dev" "$env:USERPROFILE\.claude\skills\"
Copy-Item -Recurse -Force "ai-agent-skills\plugins\voximplant-ai-agent-skills\skills\voximplant-management-api" "$env:USERPROFILE\.claude\skills\"
```

### Codex Standalone Skills

macOS/Linux:

```bash
git clone https://github.com/voximplant/ai-agent-skills.git
mkdir -p ~/.agents/skills
cp -R ai-agent-skills/plugins/voximplant-ai-agent-skills/skills/voximplant-voxengine-dev ~/.agents/skills/
cp -R ai-agent-skills/plugins/voximplant-ai-agent-skills/skills/voximplant-management-api ~/.agents/skills/
```

Windows PowerShell:

```powershell
git clone https://github.com/voximplant/ai-agent-skills.git
New-Item -ItemType Directory -Force "$env:USERPROFILE\.agents\skills"
Copy-Item -Recurse -Force "ai-agent-skills\plugins\voximplant-ai-agent-skills\skills\voximplant-voxengine-dev" "$env:USERPROFILE\.agents\skills\"
Copy-Item -Recurse -Force "ai-agent-skills\plugins\voximplant-ai-agent-skills\skills\voximplant-management-api" "$env:USERPROFILE\.agents\skills\"
```

Reload or restart the client if the skills do not appear immediately.

## Useful Prompts

```text
Use Voximplant AI Agent Skills to build an inbound Voice AI scenario.
```

```text
Use Voximplant AI Agent Skills to deploy and test this VoxEngine scenario.
```

```text
I am new to Voximplant. Please choose safe defaults and tell me exactly which credentials you need.
```

If you are not a developer, start with your use case. A good agent should explain the Voximplant pieces, propose a simple first test, and ask only for the credentials or approvals that are actually needed.

## What You Need To Provide

For most first projects, you only need:

1. A Voximplant account.
2. A service account JSON file. For the easiest workflow, use a broad role such as Developer or Owner; for stricter security, use task-specific roles.
3. Provider API keys only if the use case needs an external AI, LLM, speech, or voice provider.
4. Phone-number verification and account balance only if you need to buy or rent phone numbers.

The skills can help with the rest: writing the VoxEngine scenario, creating applications, rules, users, secrets, uploading code, testing through a softphone, retrieving logs, and debugging failures.

## Documentation Sources

Agents should use these sources to verify API names, method signatures, and current examples:

- `https://docs.voximplant.ai/llms.txt` for the documentation index
- `https://docs.voximplant.ai/api-reference/llms.txt` for API Reference discovery
- section-level indexes such as `https://docs.voximplant.ai/platform/voxengine/llms.txt`
- page Markdown by appending `.md` to a docs page URL

Do not paste large documentation dumps into prompts by default. Prefer narrow page-level Markdown fetches. For VoxEngine TypeScript declarations, follow `voximplant-voxengine-dev`; the declaration file is best downloaded locally and used for validation rather than loaded as a general README source.

## Credentials And Security

For real account actions, the user usually needs only:

- a Voximplant account
- a Voximplant service account JSON
- provider API keys only when the chosen use case needs an external AI/LLM/voice provider
- phone-number verification and account balance only when the workflow requires buying or renting phone numbers
- approval before the agent creates, changes, deletes, uploads, buys, binds, or starts platform resources

For the easiest agent workflow, create a service account with a broad role such as Developer or Owner. If security is more important and you know the exact task, use the minimum roles for that task instead.

The simplest credential handoff is to put the service account JSON in the project root and tell the agent the filename, for example `voximplant-service-account.json`. If you keep the default filename ending in `_private.json`, the agent should find it automatically. Do not commit the file. The agent should check or update `.gitignore` before working with local credentials.

Treat service account JSON files as path-only secrets by default. Give the agent the filename or path, not the JSON contents. Reading the private key into chat may send it through the LLM provider and store it in conversation logs.

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