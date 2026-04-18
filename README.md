# memos-skills

A shareable [Hermes Agent](https://github.com/chawza/hermes-agent) skill for managing [Memos](https://www.usememos.com/) notes via the [memos-cli](https://github.com/chawza/memos-cli) binary.

## What it does

This skill teaches any Hermes-compatible AI agent to:

- **Create** tagged memos with the right visibility (default: PRIVATE)
- **List** and filter memos by state, visibility, or CEL expressions
- **Get**, **update**, and **delete** memos by numeric ID
- Use **#tags** for dashboard-filterable notes

## Install

```bash
npx skills add github:chawza/memos-skills
```

## Prerequisites

- Go 1.21+ (for `go install`)
- A running [Memos](https://www.usememos.com/) instance
- An access token from **Memos → Settings → Access Tokens**

## Quick Start

```bash
# 1. Install the CLI
go install github.com/chawza/memos-cli@latest

# 2. Configure (recommended)
memos auth set --base-url https://your-memos.example.com --token memos_pat_xxx
memos auth check

# 3. Test
memos list
```

Or configure via environment variables:

```bash
export MEMOS_BASE_URL="https://your-memos.example.com"
export MEMOS_TOKEN="memos_pat_xxx"
```

Then just tell the agent things like:

- "Log this note: review the API docs #work"
- "Create a task: call the vet #personal #urgent"
- "Show me my archived memos"
- "Update memo 123 to say 'Completed' and archive it"

## Included Skills

| Skill | Description |
|-------|-------------|
| `memos` | Full memos CLI management — create, list, get, update, delete |

## License

MIT
