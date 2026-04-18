---
name: memos
description: Manage memos notes via the memos-cli binary (chawza/memos-cli). Use when you need to create, list, search, update, or delete notes in a self-hosted Memos instance. Supports tagging, visibility controls, state filtering, and task-style memos. Trigger whenever the user wants to log a note, create a task, set a reminder, or search their Memos notes.
metadata:
  author: chawza
  version: "0.3.0"
  tags: [notes, tasks, memos, cli, self-hosted]
---

# Memos

A skill for interacting with [Memos](https://www.usememos.com/) — a self-hosted, minimal notes app — via the [`memos-cli`](https://github.com/chawza/memos-cli) binary.

## Setup

### 1. Install memos-cli

```bash
go install github.com/chawza/memos-cli@latest

# Verify
memos --help
```

Or build from source:

```bash
git clone https://github.com/chawza/memos-cli.git
cd memos-cli
go build -o memos .
```

### 2. Configure

**Recommended:** Use the `auth` command to save credentials:

```bash
memos auth set --base-url https://your-memos.example.com --token memos_pat_xxx
memos auth check  # verify connectivity
```

This saves credentials to `~/.config/memos-cli/config.toml`.

**Alternative:** Set environment variables:

```bash
export MEMOS_BASE_URL="https://your-memos.example.com"
export MEMOS_TOKEN="memos_pat_xxx"
```

**Alternative:** Use CLI flags on every command:

```bash
memos --base-url https://your-memos.example.com --token xxx list
```

### 3. Get your Memos access token

1. Open your Memos instance in a browser
2. Go to **Settings → Access Tokens**
3. Create a new token and copy it

## Usage

### Key rules

- **Always use `#tags`** in memo content so users can filter by tag in the Memos dashboard. Multiple tags are supported.
- **Default visibility is PRIVATE**. Only use PROTECTED or PUBLIC if the user explicitly requests it.
- **Memo IDs are numeric** (e.g. `123`) — use them directly without the `memos/` prefix for `get`, `update`, and `delete` operations.

### Create a memo

```bash
memos create -c "Buy groceries #shopping #urgent"
memos create -c "Finish quarterly report #work #mcm" --visibility PRIVATE
memos create -c "MCM quarterly report #work #mcm #quarterly" --pinned
```

### List memos

```bash
memos list
memos list --limit 50
memos list --state ARCHIVED
memos list --output json
memos list --output table
memos list --filter 'visibility == "PRIVATE"'  # CEL filter syntax
```

### Get a memo

```bash
memos get 123
memos get 123 --output json
```

### Update a memo

```bash
memos update 123 -c "Updated content"
memos update 123 --pinned
memos update 123 --unpin
memos update 123 --visibility PUBLIC
memos update 123 --state ARCHIVED
```

Note: At least one flag must be specified for update.

### Delete a memo

```bash
memos delete 123
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

## Output Formats

The `list` and `get` commands support three output formats:

| Format | Flag | Description |
|--------|------|-------------|
| Text | `--output text` (default) | Human-readable, compact |
| JSON | `--output json` | Machine-readable, full memo struct |
| Table | `--output table` | Aligned columns (list only) |

### Text output examples

**`memos get 123`:**

```
Name:       memos/123
State:      NORMAL
Visibility: PRIVATE
Pinned:     false
Creator:    users/1
Created:    2025-04-18T10:30:00Z

Buy groceries
```

**`memos list`:**

```
  [PRIVATE] Buy groceries
* [PUBLIC]  Important announcement
  [PRIVATE] Meeting notes
```

Pinned memos show `* ` prefix, non-pinned show `  `.

## Command Reference

| Command | Description |
|---------|-------------|
| `memos auth set` | Save credentials to config file |
| `memos auth check` | Verify saved configuration |
| `memos create` | Create a new memo |
| `memos list` | List memos with filters |
| `memos get <id>` | Get a single memo |
| `memos update <id>` | Update a memo |
| `memos delete <id>` | Delete a memo |

## Configuration

Three methods, checked in priority order:

1. **Flags** — `--base-url` and `--token` on any command
2. **Environment variables** — `MEMOS_BASE_URL` and `MEMOS_TOKEN`
3. **Config file** — `~/.config/memos-cli/config.toml`

Config file format (TOML):

```toml
base_url = "https://your-memos.example.com"
token = "memos_pat_xxx"
timeout = 30
tls_skip_verify = false
```
