# Building MCP Clients

## Why Build Your Own Client

Claude Desktop has its own built-in MCP client. But if you're building your own chatbot, you need to wire up MCP yourself. The library used here is **`langchain-mcp-adapters`** — a lightweight wrapper that makes MCP tools compatible with LangChain/LangGraph. Chosen because it's the simplest approach and fits naturally into the LangChain ecosystem.

## The Core Pattern

```python
from langchain_mcp_adapters.client import MultiServerMCPClient

async with MultiServerMCPClient(servers) as client:
    tools = await client.get_tools()
    llm_with_tools = llm.bind_tools(tools)
```

`MultiServerMCPClient` takes a `servers` dict (same format as Claude Desktop's config JSON), connects on startup, and returns LangChain-compatible tool objects.

## Server Config Format

```python
servers = {
    "math": {                          # local server — stdio transport
        "transport": "stdio",
        "command": "uv",
        "args": ["run", "fastmcp", "run", "/path/to/math/main.py"]
    },
    "expense": {                       # remote server — streamable HTTP
        "transport": "streamable_http",
        "url": "https://your-fastmcp-cloud-url"
    }
}
```

Adding a third server (e.g. Manim) is just another key in this dict — no other code changes needed.

## Full Tool Call Flow

1. User sends a prompt
2. `llm_with_tools.invoke(prompt)` — LLM decides which tool(s) to call and with what args
3. Check `response.tool_calls` — if empty, return `response.content` directly (normal chat)
4. For each tool call: look up tool by name, `await tool.ainvoke(args)`
5. Wrap result in a `ToolMessage` (needs `tool_call_id`)
6. Re-invoke LLM with full history: `[prompt, response, tool_message]`
7. Return final `content`

Loop step 4–6 to handle multiple tool calls in one response.

## What Was Built

A Streamlit chatbot connected to three MCP servers simultaneously:
- **Math server** (local, stdio) — arithmetic operations
- **Expense tracker** (remote, streamable HTTP) — add/list/summarize expenses
- **Manim server** (local, stdio, third-party) — generates animation videos

The Streamlit UI wraps the same async client logic. Launched via `streamlit run client2.py`.