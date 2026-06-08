# MCP Introduction

Everything in this repo comes from this Youtube playlist - [CampusX MCP](https://www.youtube.com/watch?v=3_TN1i3MTEU&list=PLKnIA16_Rmva_oZ9F4ayUu9qcWgF7Fyc0&index=1)

## The Problem MCP Solves

After LLMs became widely used, teams started connecting them to external tools (Jira, GitHub, Slack, Drive, etc.) via **function/tool calling** — you write a function per tool, describe it to the LLM, and it calls the right one automatically.

This worked, but created an **N × M integration problem**: if you have 3 AI chatbots and 20 tools, you need 60 separate integrations. Each is custom — different auth, data formats, error handling. One API change breaks multiple chatbots. API keys are scattered everywhere. It doesn't scale.

The real frustration: if 3 chatbots all need GitHub access, you write 3 different GitHub integrations. You'd want to write just one that works with all of them.

## What MCP Is

**MCP (Model Context Protocol)** — created by Anthropic — is a standard protocol that solves the integration problem by shifting all the heavy lifting to the **server side**.

- **MCP Client** = your AI chatbot (Claude, Cursor, your own app)
- **MCP Server** = the tool/service being connected (GitHub, Drive, Slack)

The server handles everything: auth, business logic, rate limiting, data formatting, error handling. The client just connects and speaks MCP. You don't write any integration code on the client side — just a JSON config entry per server.

Instead of N×M integrations, you get M servers (written once by the service providers themselves) that any MCP-compatible client can connect to on day one.

## Benefits

- **No client-side integration code** — just config
- **No maintenance** — if an API changes, only the server updates; your client is untouched
- **Faster setup** — thousands of MCP servers already exist; connect on day one
- **Centralized security** — one config file, all connections in one place

## Why It's Taking Over

Major AI tools (Claude Desktop, Cursor, Windsurf, Perplexity) declared MCP support → pressure on services to build MCP servers → more servers → more incentive for new clients to be MCP-compatible. Classic network effect. Expected to be an **industry standard in 3–5 years**.
