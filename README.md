# Web Tools Router

A portable Agent Skill that routes web tasks to the lightest suitable capability:

```text
native web search/fetch → deterministic OpenCLI adapter → agent-browser
```

This is not a rigid fallback chain. Search and fetch are separate branches, OpenCLI is preferred only when an exact structured command exists, and interactive or authenticated tasks can go directly to agent-browser.

The Skill follows the [Agent Skills specification](https://agentskills.io/specification) and is designed for Codex, Claude Code, WorkBuddy, and other Skills-compatible agents.

## Install

### skills CLI

```bash
npx skills add https://github.com/leoxiewl/web-tools-router
```

### Manual

Copy `SKILL.md`, `references/`, and optionally `agents/` into a `web-tools-router` directory under the host's Skill path:

| Host | Typical user-level path |
|---|---|
| Codex | `~/.codex/skills/web-tools-router/` |
| Claude Code | `~/.claude/skills/web-tools-router/` |
| WorkBuddy | `~/.codebuddy/skills/web-tools-router/` |

WorkBuddy requires `user-invocable: true` explicitly for a manually installed
Skill to appear in its `/` menu. Its separate `~/.workbuddy/skills/` directory
is used by the marketplace loader and is not the user Skill menu source.
Restart the host or open a new task if it does not refresh Skills dynamically.

## Optional tools

The Skill uses the host's native Web Search and Web Fetch when available. OpenCLI and agent-browser are optional escalation paths:

```bash
command -v opencli && opencli --version
command -v agent-browser && agent-browser --version
```

Install and configure optional tools from their upstream projects:

- [OpenCLI](https://github.com/jackwener/OpenCLI)
- [agent-browser](https://github.com/vercel-labs/agent-browser)

No browser engine is forced. Use isolated browser state by default and configure a named profile or CDP connection only when an authenticated task requires it.

## Routing

```text
Web task
├─ No URL → native search → OpenCLI search → agent-browser
└─ Known URL
   ├─ Static → native fetch → matching OpenCLI adapter or agent-browser
   └─ Interactive/authenticated → exact OpenCLI command or agent-browser
```

See [references/routing.md](references/routing.md) for the complete decision procedure, [references/tools.md](references/tools.md) for tool usage, and [references/safety.md](references/safety.md) for confirmation and trust boundaries.

## Development

Routing behavior is evaluated against [evals/routing-cases.md](evals/routing-cases.md). When replacing a concrete tool without changing routing semantics, update only `references/tools.md`.

## License

[MIT](LICENSE)
