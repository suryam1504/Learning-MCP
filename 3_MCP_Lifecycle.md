# MCP Lifecycle

The MCP lifecycle is the sequence of steps that govern how a client and server establish, use, and end a **session** (one continuous connection between them).

## 3 Phases

### 1. Initialization (Handshake)
The first thing that happens when a host starts — before any real work. Three steps:

1. **Client → Server:** sends an `initialize` request with its MCP protocol version, capabilities (what it can do for the server), and its name/version
2. **Server → Client:** responds with its own protocol version, capabilities (tools, resources, prompts it supports), and its name/version
3. **Client → Server:** sends an `initialized` notification to confirm the connection is live

Neither side can send anything else until all three steps complete (except pings). If the protocol versions don't match and the client doesn't support the server's version, it disconnects.

**Capability negotiation** happens here — both sides declare what they support, setting expectations for the whole session. Key capabilities:
- Client can offer: **Roots** (filesystem access), **Sampling** (let server use client's AI), **Elicitation** (server can request missing info from client)
- Server can offer: **Tools**, **Resources**, **Prompts**, **Logging**

### 2. Operation
Two parts that happen automatically after initialization:

- **Capability discovery** — client immediately calls `tools/list`, `resources/list`, `prompts/list` to get the exact list of available tools/resources/prompts from the server (often batched into one request)
- **Normal usage** — client calls `tools/call` with tool name + arguments whenever the user needs something; server responds with results

### 3. Shutdown
No JSON-RPC messages involved — handled entirely by the transport layer:
- **Local (STDIO):** client closes the server's input stream → waits → if server doesn't exit, sends `SIGTERM` → then `SIGKILL` if needed
- **Remote (HTTP):** client simply closes the HTTP connection

## Special Cases

- **Pings** — lightweight requests either side can send to confirm the other is still alive; useful during long-running tasks to prevent the OS/firewall from silently dropping idle connections
- **Error handling** — errors follow the JSON-RPC standard error object format (code + message + optional debug data). Common codes: `-32601` method not found, `-32602` invalid params, `-32700` invalid JSON
- **Timeouts** — set per-request in the MCP SDK; if exceeded, client sends a `notifications/cancelled` notification and the server stops processing that request
- **Progress notifications** — for long-running tasks, server sends periodic `notifications/progress` updates (with a progress token) so the client can show the user real-time status
