# Learning MCP

Notes and hands-on projects from a YouTube playlist on the Model Context Protocol (MCP).

Ref - [CampusX MCP](https://www.youtube.com/watch?v=3_TN1i3MTEU&list=PLKnIA16_Rmva_oZ9F4ayUu9qcWgF7Fyc0&index=1)

## Notes

| File | Topic |
|------|-------|
| `1_MCP_Intro.md` | Why MCP exists — the N×M integration problem it solves |
| `2_MCP_Architecture.md` | Host / Client / Server model, primitives, transports |
| `3_MCP_Lifecycle.md` | Initialization, capability negotiation, operation, shutdown |
| `4_Connecting_MCP_Server.md` | Connecting to existing MCP servers via Claude Desktop |
| `5_Building_Local_MCP_Servers.md` | Building local servers with FastMCP, decorator pattern |
| `6_Building_Deploying_Remote_MCP_Server.md` | Remote servers, FastMCP Cloud deployment, async + aiosqlite |
| `7_Building_MCP_Clients.md` | Building a custom MCP client with langchain-mcp-adapters |

## Projects Built

- **`fastmcp-demo-local-server/`** — Introductory server with `roll_dice` and `add_numbers` tools
- **`fastmcp-math-local-server/`** — Local math server with add, subtract, multiply, divide, power, abs tools
- **`fastmcp-expense-tracker-local/`** — Expense tracker running locally over stdio (SQLite, 3 tools + categories resource)
- **`fastmcp-expense-tracker-remote/`** — Same expense tracker converted to a remote server (streamable HTTP, async, aiosqlite), deployed on FastMCP Cloud
- **`manim-mcp-server/`** — Third-party Manim animation MCP server (cloned from GitHub)
- **`mcp-client/`** — Custom chatbot client (`client1.py` = terminal, `client2.py` = Streamlit UI) connected to the math server, remote expense tracker, and Manim server simultaneously via `MultiServerMCPClient`

## Stack

- **FastMCP v3.4.2** — server framework
- **langchain-mcp-adapters** — MCP client library
- **uv** — package manager
- **Python 3.12**
- **Claude Desktop** — MCP host used for testing
