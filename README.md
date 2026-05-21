# Expense Tracker MCP Server

An MCP (Model Context Protocol) server that lets AI assistants like Claude log, list, and summarize personal expenses through natural conversation.

## What it does

- **Add expenses** — record a spend with date, amount, category, optional subcategory, and a note
- **List expenses** — retrieve all entries within a date range
- **Summarize expenses** — get category-level totals and counts for any period
- **Browse categories** — exposes a structured category taxonomy as an MCP resource

## Tools

| Tool | Description |
|------|-------------|
| `add_expense(date, amount, category, subcategory?, note?)` | Insert a new expense row |
| `list_expenses(start_date, end_date)` | Return all expenses between two dates (inclusive) |
| `summarize(start_date, end_date, category?)` | Aggregate totals by category; optionally filter to one category |

Dates use `YYYY-MM-DD` format.

## Resource

`expense:///categories` — JSON resource listing all supported categories and their subcategories (food, transport, housing, health, travel, etc.).

## Setup

**Prerequisites:** Python 3.11+, [uv](https://github.com/astral-sh/uv)

```bash
git clone <repo-url>
cd mcp-repo
uv sync
```

## Running the server

### HTTP mode (direct use / testing)

```bash
uv run python main.py
```

Server starts at `http://0.0.0.0:8000`.

### STDIO mode (Claude Desktop)

Add this to your Claude Desktop `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "expense-tracker": {
      "command": "uv",
      "args": ["run", "python", "/path/to/mcp-repo/main.py"],
      "env": {}
    }
  }
}
```

## Proxy

[proxy.py](proxy.py) exposes a remote FastMCP Cloud server locally over STDIO — useful for sharing a hosted instance with Claude Desktop without re-deploying.

```bash
uv run python proxy.py
```

## Categories

The full taxonomy lives in [categories.json](categories.json) and covers: food, transport, housing, utilities, health, education, entertainment, shopping, subscriptions, personal care, travel, business, investments, taxes, and more.

## Database

Expenses are stored in a SQLite database at the system temp directory (`$TMPDIR/expenses.db`) so the server works without write access to the project folder.
