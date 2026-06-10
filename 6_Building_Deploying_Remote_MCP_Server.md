# Building & Deploying a Remote MCP Server

The entire server code is identical to a local server. The only difference is the transport line at the bottom:

```python
# Local
mcp.run()  # defaults to stdio

# Remote
mcp.run(transport="streamable-http", host="0.0.0.0", port=8000)
```

Everything else — tools, resources, decorators — stays the same.

## Deployment: FastMCP Cloud

FastMCP offers a free hosting service at `fastmcp.cloud`. Steps:
1. Push your server code to a GitHub repo
2. Go to fastmcp.cloud → "Deploy from your own code"
3. Connect GitHub, select repo, set entrypoint (`main.py`), click Deploy
4. FastMCP Cloud auto-detects new commits and rebuilds on every push

Once deployed, you get a public URL for your remote server and share it with anyone.

## Connecting to Claude Desktop (Pro Plan)

Go to Claude Desktop → Settings → Connectors → Add Custom Connector → paste the remote server URL. Restart Claude Desktop.

> Only available on the Pro plan currently.

## Connecting Without Pro Plan: Local Proxy

FastMCP lets you run a local proxy server that forwards to the remote — so you connect Claude Desktop to a local server (which is free) and the local server talks to the remote.

```python
from fastmcp import FastMCP
mcp = FastMCP.as_proxy("https://your-remote-url", name="My Proxy")
mcp.run()  # stdio — registers as local server
```

Register via: `uv run fastmcp install claude-desktop main.py`

The proxy adds latency but works on free plans.

## What I Upgraded: Async + aiosqlite

The original expense tracker used synchronous SQLite — one user at a time, all others block. Fixed by:
- Switching `sqlite3` → `aiosqlite`
- Making all tool functions `async def` with `await` on DB calls

This allows the server to handle concurrent users without blocking.

## The Unresolved Problem: No Authentication

The deployed expense tracker has a critical flaw: **no user isolation**. All users share one database with no user ID column, so every user sees every other user's expenses. Fixing this requires:
1. Adding a `user_id` column to every table
2. Implementing authentication so the server knows *who* is calling

Just a logic issue, main task was deploying remote MCP server.