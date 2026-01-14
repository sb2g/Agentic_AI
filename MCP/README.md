# Overview
- This directory goes over Model Context Protocol (MCP).

# MCP basics
- MCP standardizes how AI agents communicate with the outside world (tools/functions/APIs).
- Architecture:
    - Follows Client Server architecture
        - `MCP host` is the application where the AI model runs (e.g., a chatbot interface, AI agents, IDEs).
        - `MCP client` resides within the host and is responsible for making requests to external tools and data sources. It acts on behalf of the AI model and maintains 1:1 connections with the server while residing inside the host.
        - `MCP server` exposes an external tool or data source (like a database, API, or local file system). It serves information and capabilities to the client in a standardized format that the AI can understand.
    - Protocol
        - MCP is composed of two distinct layers:
            - A standardized message format that defines the structure of the messages.
                - JSON-RPC 2.0: This is a simple and lightweight remote procedure call protocol that uses JSON for its data structures. Think of it as the standardized envelope for every message sent between a client and a server. Using JSON-RPC ensures that all communications are structured predictably, with clear definitions for requests, successful responses, and error conditions, making the protocol easy to implement and debug.
            - A transport layer that defines how the messages are delivered.
                - Stdio transport: This transport uses the standard input stdin and standard output stdout streams for communication. It is ideal for local, process-to-process communication, such as when a code editor (the host) runs a local tool (the server) as a child process.
                - Streamable HTTP transport: This transport uses HTTP for communication, including Server-Sent Events (SSE) for streaming responses from the server back to the client. It is designed for remote interactions over a network, enabling agents to connect to web-based services.
        - Difference from REST, gRPC:
            - Dynamic capability discovery: Instead of relying on static documentation, MCP enables an agent to programmatically ask a tool at runtime, “What can you do?” This allows for on-the-fly discovery, and the use of new capabilities without requiring manual pre-programming.
            - Standardized conversational protocol: While REST is stateless and gRPC offers general-purpose bidirectional channels, MCP provides a standardized protocol for stateful, agentic conversations. This allows tools to proactively push updates or request information from the AI using a shared, understood language, and not just an open data pipe.
            - A domain-specific language for AI: MCP uses a purpose-built vocabulary with core elements like tools, resources, and prompts. This gives agents a rich, context-aware language for interaction that is far more specific and powerful than the generic data models of traditional APIs.
- MCP introduces a powerful layer of abstraction. 
    - Instead of teaching AI agent how to speak to every individual API, we only need to teach it how to speak one language: MCP. 
    - The MCP server handles the messy, specific details of talking to the underlying service. It then exposes that capability as a standardized MCP component.
- Components of MCP:
    - `Tools`: Tools are the action-oriented functions (verbs) the agent can execute. This is similar to a POST request in a traditional API, as it causes a change or performs an action.
    - `Resources`: Resources are read-only sources of information that allow the agent to retrieve data for context. This is similar to a GET request, where the goal is simply to fetch information without altering it.
    - `Prompts`: Prompts are pre-defined recipes for interaction that reside on the server. They provide an optimized and reusable way to perform common tasks, saving users from the overhead of complex prompt engineering.
- How to define?
    - MCP provides software development kits (SDKs) in various languages that make defining these primitives incredibly simple. 
    - When creating a component on an MCP Server, we typically just write a standard function and then use a special decorator to register it with the protocol. 
    - Tool:
        - `@mcp.tool()`
            - The decorator automatically makes the function discoverable and callable by any connected MCP client. 
            - The function’s type hints (a: int) and docstring are also used by the protocol to tell the client what parameters the tool expects, and what it does.
        - When a client needs this tool's functionality, this function can be used.
    - Resource:
        - `@mcp.resource(<resource_name>, mime_type=<>)`
        - When a client requests `<resource_name>`, server will execute this function
    - Prompt:
        - `@mcp.prompt(name=<prompt_name>, description=<description>)`
        - Registers a server-side prompt named <name> that performs action in <description>

# References:
1. https://www.educative.io/courses/advanced-model-context-protocol/
2. https://learn.deeplearning.ai/courses/mcp-build-rich-context-ai-apps-with-anthropic/
