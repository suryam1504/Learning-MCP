# Connecting Claude Code to Existing MCP Servers 

Using Claude Desktop (a ready-made MCP client) to connect to existing MCP servers — no building anything yet.

- Local servers is anything on your own system. Such servers, launched via `npx` or `uvx` — even if you didn't manually clone anything — are still local (the package gets installed on your machine)
- A remote server is one whose code runs on a different machine (e.g. fetched from a GitHub path via `uvx`)

## Two Ways to Connect an MCP Server to Claude Desktop

**1. JSON config file** — open Claude Desktop's `claude_desktop_config.json` and add a JSON entry for the server. Required for custom/self-built servers. More technical but works for any server.

**2. Connectors** — a built-in inside Claude Desktop. One click to connect popular SaaS tools (Google Drive, Gmail, Notion, etc.). Anthropic handles auth, API keys, and maintenance behind the scenes. Easier and more consistent, but only available for tools Anthropic has explicitly wrapped.

> Connectors exist because non-technical users shouldn't need to edit JSON files. But not every server gets a connector — Anthropic can't maintain wrappers for thousands of community servers. So both options coexist.

## Servers Connected 

Different combinations of Local-Remote, Config-Connector connections. 

| Server | Type | Method | Notes |
|---|---|---|---|
| **File System** | Local | Connector | Access/create files in specified directories only (safety feature); useful for organizing folders, summarizing projects |
| **Manim** | Local | JSON config | Generate mathematical visualizations (like 3Blue1Brown) from plain English prompts; Claude writes + executes the Manim code |
| **Google Drive** | Remote | Connector | Read-only access to Drive documents |
| **Twitter/X** | Local (via npx) | JSON config | Search tweets; posting requires read+write permissions in Twitter developer settings |

Twitter JSON was supposed to be a remote connection but it seems it is a local server only as it installs packages using npm (npx) and a path to MCP server is given:

```
{
  "mcpServers": {
    "twitter-mcp": {
      "command": "npx",
      "args": ["-y", "@enescinar/twitter-mcp"],
      "env": {
        "API_KEY": "your_api_key_here",
        "API_SECRET_KEY": "your_api_secret_key_here",
        "ACCESS_TOKEN": "your_access_token_here",
        "ACCESS_TOKEN_SECRET": "your_access_token_secret_here"
      }
    }
  }
}
```

But eh ok.

## How to Find MCP Servers

Search **"Awesome MCP Servers"** on GitHub — a community-maintained, categorized list of available servers. Also check Claude Desktop's connector marketplace for officially supported ones.

