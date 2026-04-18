---
name: memos
description: Manage memos notes via the memos-cli binary (chawza/memos-cli). Use when you need to create, list, search, update, or delete notes in a self-hosted Memos instance. Supports tagging, visibility controls, and task-style memos. Trigger whenever the user wants to log a note, create a task, set a reminder, or search their Memos notes.
metadata:
  author: chawza
  version: "1.0.0"
  tags: [notes, tasks, memos, cli, self-hosted]
---

# Memos

A skill for interacting with [Memos](https://www.usememos.com/) — a self-hosted, minimal notes app — via the [`memos-cli`](https://github.com/chawza/memos-cli) binary.

## Setup

### 1. Install memos-cli

```bash
# Linux (amd64)
curl -sL https://github.com/chawza/memos-cli/releases/latest/download/memos-cli_linux_amd64.tar.gz | tar xz -C ~/.local/bin memos

# macOS (Apple Silicon)
curl -sL https://github.com/chawza/memos-cli/releases/latest/download/memos-cli_darwin_arm64.tar.gz | tar xz -C ~/.local/bin memos

# Verify
memos --help
```

### 2. Get your Memos access token

1. Open your Memos instance in a browser
2. Go to **Settings → Access Tokens**
3. Create a new token and copy it

### 3. Configure

Set your instance URL and token as environment variables:

```bash
export MEMOS_BASE_URL="https://your-memos.example.com"
export MEMOS_TOKEN="memos_pat_xxx"
```

Or use CLI flags on every command:
```bash
memos --base-url https://your-memos.example.com --token xxx list
```

## Usage

### Key rules

- **Always use `#tags`** in memo content so users can filter by tag in the Memos dashboard. Multiple tags are supported.
- **Default visibility is PRIVATE**. Only use PROTECTED or PUBLIC if the user explicitly requests it.
- **Memo IDs are the `name` field** returned by the API (e.g. `memos/PbfqhaH6nc7oJdoqAW6pMr`) — not numeric IDs.

### Create a memo

```bash
memos create --content "Buy groceries #shopping #urgent"
memos create -c "Finish quarterly report #work #mcm" --visibility PRIVATE
memos create -c "MCM quarterly report #work #mcm #quarterly" --pinned
```

### List memos

```bash
memos list --limit 20
memos list --output json
memos list --filter 'visibility == "PRIVATE"'  # AIP-160 filter syntax
```

### Search memos

```bash
memos search "quarterly report"
memos search "mcm" --limit 10
```
Note: search is client-side — fetches memos then filters by keyword.

### Get a memo

```bash
memos get "memos/PbfqhaH6nc7oJdoqAW6pMr"
memos get "memos/PbfqhaH6nc7oJdoqAW6pMr" --output json
```

### Update a memo

```bash
memos update "memos/PbfqhaH6nc7oJdoqAW6pMr" --content "Updated content"
memos update "memos/PbfqhaH6nc7oJdoqAW6pMr" --pinned
memos update "memos/PbfqhaH6nc7oJdoqAW6pMr" --unpin
memos update "memos/PbfqhaH6nc7oJdoqAW6pMr" --visibility PUBLIC
```

### Delete a memo

```bash
memos delete "memos/PbfqhaH6nc7oJdoqAW6pMr"
```

## Visibility Options

| Value | Description |
|-------|-------------|
| `PRIVATE` | Only visible to you (default) |
| `PROTECTED` | Visible to anyone with the link |
| `PUBLIC` | Visible to everyone |

## Tagging

Tags are written directly in the content using `#tagname`. Memos auto-parses them for dashboard filtering.

```bash
memos create -c "Weekly review #work #weekly #review"
memos create -c "Grocery list #shopping #home"
```

## Memo ID Format

When you create a memo, the output shows the memo ID:
```
Created memo memos/n7yGJoHpmJDkvZJjzihdRX
```

Use the full `memos/xxx` string — not a numeric ID — for `get`, `update`, and `delete` operations.

## API Reference

The skill wraps the [Memos API v1](https://www.usememos.com/docs/api/latest):
- Base: `https://your-memos.com/api/v1`
- Auth: `Bearer <token>`
- Filter: [AIP-160](https://google.aip.dev/160) syntax
- Pagination: `pageSize` + `pageToken`

## Future Extensions

- MCP server mode for AI agent-native tool access
- Bulk import/export
- Webhook-driven watch mode
