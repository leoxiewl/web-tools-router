# Tool Guide

Tool names vary between hosts. Use the host-native search and fetch capabilities when available; the semantic roles matter more than exact names.

## Host-native web search

Use for keyword discovery when no URL is known. Prefer primary and authoritative sources. Open promising result URLs with the host's fetch/read capability.

Do not silently replace a broken configured search provider with browser scraping. Explain the change of route.

## Host-native web fetch

Use for a known static URL. Escalate when the response is empty, blocked, unrelated boilerplate, a CAPTCHA, or an unrendered JavaScript shell.

## OpenCLI

Use OpenCLI for deterministic, site-specific commands that match the requested operation.

Check availability and discover commands instead of guessing:

```bash
command -v opencli
opencli --version
opencli list
opencli <site> --help -f yaml
opencli <site> <command> --help
```

Prefer machine-readable output:

```bash
opencli <site> <command> [arguments] -f json
```

Use `opencli doctor` when a command requires its browser bridge. A listed site is not proof that the exact requested operation exists.

OpenCLI is preferred over browser automation only when it exposes a matching deterministic command. For arbitrary page interaction, go directly to agent-browser.

## agent-browser

Use agent-browser for rendered pages, authentication, interactive controls, visual capture, and unfamiliar sites.

Load version-matched instructions before relying on remembered syntax:

```bash
command -v agent-browser
agent-browser --version
agent-browser skills get core --full
```

Typical interaction:

```bash
agent-browser open https://example.com
agent-browser snapshot -i --json
agent-browser click @e2
agent-browser fill @e3 "value"
agent-browser snapshot -i --json
```

For a known URL that may be readable without launching a browser:

```bash
agent-browser read https://example.com/article --json
```

Use stable refs from the latest snapshot. Take a new snapshot after navigation or a material page change. Prefer semantic locators when refs are unavailable.

### Browser identity

Do not hard-code Chrome, Edge, or another engine into this Skill.

- Use an isolated session by default.
- Use a named profile, saved state, auto-connect, or CDP only when the task requires an authenticated browser.
- Never close, restart, or kill a user's browser without explicit permission.
- Do not copy or export credentials, cookies, or browser profile data.

Inspect the installed agent-browser help for the supported profile and connection flags because these can change between versions.

## Adding or replacing a tool

Update this file with:

1. The semantic role the tool implements.
2. Availability and health checks.
3. Discovery commands.
4. Structured-output mode.
5. Authentication and browser-state behavior.
6. Known failure signals.

Keep `SKILL.md` unchanged unless the routing semantics themselves change.
