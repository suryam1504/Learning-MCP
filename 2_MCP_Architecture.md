# MCP Architecture

## Core Components

Three entities in every MCP setup:

- **Host** — the AI chatbot (Claude Desktop, Cursor, your own app). Coordinates everything, talks to an LLM under the hood.
- **Client** — created by the host, one per server. Acts as the intermediary that speaks the MCP protocol. Host never talks to a server directly.
- **Server** — the external tool (GitHub, Slack, Drive). Does all the actual work.

Client-server is **one-on-one** — 3 servers means 3 clients. This keeps communication channels independent (decoupling) and lets you scale by just adding more clients.

## What Servers Expose (Primitives)

- **Tools** — actions the AI can invoke (create issue, query DB, list repos)
- **Resources** — static documents the AI can read (README, DB schema)
- **Prompts** — predefined prompt templates the server offers to guide the AI's output format

Standard functions exist for each: `tools/list`, `tools/call`, `resources/read`, `prompts/get`, etc.

## How Messages Are Sent

**Data layer:** All client-server communication is in **JSON-RPC 2.0** — a lightweight format for calling remote functions via JSON. Much simpler than REST (no headers, no metadata). Also supports batching and fire-and-forget notifications (no response required).

**Transport layer:** How those JSON-RPC messages physically travel depends on the server type:
- **Local server** (same machine) → **STDIO** — host launches server as a subprocess and pipes messages through stdin/stdout. Fast and secure.
- **Remote server** (over internet) → **HTTP + SSE** — messages sent as POST requests; SSE allows the server to stream responses back incrementally.

The data and transport layers are intentionally separate — JSON-RPC is transport-agnostic, so the same message format works over STDIO, HTTP, or anything else. Swapping transports requires zero changes to the data layer.
