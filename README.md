# memos-skills

A shareable [Hermes Agent](https://github.com/chawza/hermes-agent) skill for managing [Memos](https://www.usememos.com/) notes via the [memos-cli](https://github.com/chawza/memos-cli) binary.

> One command install: `npx skills add github:chawza/hermes-skill-memos`

## What it does

This skill teaches any Hermes-compatible AI agent to:
- **Create** tagged memos with the right visibility (default: PRIVATE)
- **List** and **search** through memos
- **Update** and **delete** memos by their `memos/xxx` ID
- Use **#tags** for dashboard-filterable notes

## Install

```bash
npx skills add github:chawza/hermes-skill-memos
```

## Prerequisites

- [`memos-cli`](https://github.com/chawza/memos-cli) binary installed
- A running [Memos](https://www.usememos.com/) instance
- An access token from **Memos → Settings → Access Tokens**

## Quick Start

```bash
# 1. Install the CLI
curl -sL https://github.com/chawza/memos-cli/releases/latest/download/memos-cli_linux_amd64.tar.gz | tar xz -C ~/.local/bin memos

# 2. Configure
export MEMOS_BASE_URL="https://your-memos.example.com"
export MEMOS_TOKEN="memos_pat_xxx"

# 3. Test
memos list
```

Then just tell the agent things like:
- *"Log this note: review the API docs #work"*
- *"Create a task: call the vet #personal #urgent"*
- *"Search my memos for anything about the quarterly report"*

## Included Skills

| Skill | Description |
|-------|-------------|
| `memos` | Full memos CLI management — create, list, search, update, delete |

## License

MIT
