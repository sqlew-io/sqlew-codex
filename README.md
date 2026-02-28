# sqlew-codex

Codex Skills and append system prompt for [sqlew](https://github.com/sqlew-io/sqlew) –Context sharing MCP server with Plan-to-ADR integration.

## Features

* **Skills**: Auto-triggered guidance for Plan Mode decision/constraint formatting
* **System Prompt**: Revoke sqlew command before/after planning

## Installation

### Prerequisites

Install the sqlew MCP server globally:

```sh
npm i -g sqlew
```

### Clone this repository

```sh
git clone https://github.com/sqlew-io/sqlew-codex-skills
```

### Install Skills and System Prompt

```sh
cp sqlew-codex-skills/copy_to_codex_dir/* ~/.codex
```

### Edit your config file to activate System Prompt and MCP

in `~/.codex/config.toml`

```toml
project_doc_fallback_filenames = ["sqlew_agents.md"]
.
.
.
[mcp_servers.sqlew]
command = "sqlew"
```

---

## Uninstallation

Simply remove skills and config lines.

```sh
rm -rf ~/.codex/skills/sqlew-decision-format
rm ~/.codex/sqlew_agents.md
```

## Related

- **[sqlew MCP Server](https://github.com/sqlew-io/sqlew)** - The MCP server that provides decision/constraint management
- **[sqlew-plugin](https://github.com/sqlew-io/sqlew-plugin)** - for Claude Code

## License

Apache-2.0