# Building a Local MCP Server 

## Library Situation (FastMCP vs MCP SDK)

Anthropic's official **MCP SDK** was verbose and boilerplate-heavy. A developer built **FastMCP** on top of it — much simpler syntax. FastMCP got absorbed into the SDK, then later spun off as its own independent library (v2). Both exist today, code looks nearly identical. FastMCP v2 is the cleaner choice going forward.

## How a Server Is Structured

Any Python function decorated with `@mcp.tool()` becomes an MCP tool. Add a docstring and it becomes the tool's description that the LLM reads to decide when to call it. Resources use `@mcp.resource()`. 

## What I Built: Expense Tracker MCP Server

A local MCP server connected to Claude Desktop. You manage expenses by talking naturally — no forms, no apps.

**Tools built:**
- `add_expense` — inserts a transaction into SQLite (date, amount, category, subcategory, note). Claude infers dates from natural language like "last Sunday".
- `list_expenses(start_date, end_date)` — fetches transactions in a date range
- `summarize_expenses(start_date, end_date, category?)` — total spend, optionally by category

**Resource added:**
- `categories` — a JSON file of all valid categories/subcategories exposed as an MCP resource. Forces Claude to always pick from a consistent set, preventing messy DB entries like "Education" vs "education" vs "Upskilling".

## Interesting Extra: FastAPI → MCP 

If you already have a FastAPI backend, FastMCP can wrap it as an MCP server automatically — all existing endpoints become tools. No rewriting needed. Useful for companies that want their product accessible via AI clients without starting from scratch.
