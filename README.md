# WebStoating Skills

Plugin marketplace for Claude Code and Codex: WebStoating's agent skills.

This repository is a catalog only. Each plugin lives in its own repository; the catalog points at it, and the agent installs just the plugin's folder.

## Install

Add the marketplace once, then install the plugins you want.

**Claude Code**

```bash
claude plugin marketplace add WebStoating/skills
claude plugin install <plugin>@webstoating-skills
```

In a session, `/plugin marketplace add WebStoating/skills` works too, and `/plugin` lists what's available.

**Codex**

```bash
codex plugin marketplace add WebStoating/skills
codex plugin add <plugin>@webstoating-skills
```

In the Codex CLI, `/plugins` lists what's available.

Start a new session after installing so the agent picks up the plugin.

## Update

```bash
claude plugin marketplace update webstoating-skills
codex plugin marketplace upgrade webstoating-skills
```

## Plugins

| Plugin | Repository | What it does |
|---|---|---|
| `software-project-audit` | [WebStoating/software-project-audit](https://github.com/WebStoating/software-project-audit) | Risk-based, evidence-driven audit of Python and JS/TS projects (OWASP Top 10, CWE Top 25) with pinned Docker scanners and a validated report. Claude Code: `/software-project-audit:audit`. |

## Adding a plugin

The plugin repository keeps the plugin in a subfolder, so its tests and fixtures are never installed:

```
<plugin>/
├── .claude-plugin/plugin.json   # Claude Code manifest
├── plugin.json                  # Codex manifest (same name and version)
└── skills/<skill>/SKILL.md
```

Then add one entry to each catalog file, with the same `name`:

`.claude-plugin/marketplace.json` (Claude Code):

```json
{
  "name": "<plugin>",
  "description": "<one line>",
  "source": { "source": "git-subdir", "url": "https://github.com/WebStoating/<repo>.git", "path": "<plugin>", "ref": "main" }
}
```

`.agents/plugins/marketplace.json` (Codex):

```json
{
  "name": "<plugin>",
  "source": { "source": "git-subdir", "url": "https://github.com/WebStoating/<repo>.git", "path": "./<plugin>", "ref": "main" },
  "policy": { "installation": "AVAILABLE", "authentication": "ON_INSTALL" },
  "category": "Coding"
}
```

Check the result before you commit:

```bash
claude plugin validate --strict .
claude plugin validate --strict <path to the plugin folder>
```

## License

[Apache-2.0](LICENSE)
