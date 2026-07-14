# Channel 3: MCP Registry Presence

## What this channel measures

Whether your product is discoverable through the Model Context Protocol ecosystem. This covers two layers: hosting an MCP server that exposes your product's capabilities as tools an agent can call, and listing that server in registries where agents look for available tools.

## Why it matters

MCP is the dominant protocol for agent-to-tool communication as of 2026, with 97M monthly SDK downloads and broad adoption across Claude, Cursor, Windsurf, and other agent runtimes. Agents increasingly start their workflow by querying MCP registries to find tools that match their task. A product without an MCP server is unreachable through this discovery path. A product with an MCP server but no registry presence is reachable only if the agent already knows the server URL, which defeats the discovery purpose.

## The four tiers

**Not Present**
No MCP server exists for the product. No registry listings. Agents using MCP-native discovery cannot find or invoke the product.

**Present but Deficient**
An MCP server exists but is not listed in any public registry, or is listed but with broken metadata. Common patterns: server URL returns 404 or requires undocumented authentication; registry listing exists but points to a deprecated endpoint; server exposes tools but tool descriptions are too vague for an agent to select correctly; authentication flow for the MCP server is interactive-only.

**Functional**
A working MCP server exists, is listed in at least one public registry (Anthropic's registry, Smithery, mcp.so, or equivalent), and exposes the product's primary capabilities as documented tools. Agents can discover the server, authenticate, and invoke tools without human intervention. Tool descriptions are specific enough for agent selection.

**Optimized**
The MCP server is listed in multiple major registries with accurate metadata. Tool descriptions follow MCP best practices with explicit input schemas, output schemas, and example invocations. Authentication supports both OAuth and API key flows. The server handles rate limiting, error responses, and tool versioning according to MCP spec. Capabilities exposed through MCP match or exceed what is available through the REST API.

## Observable signals

The crawler checks for:

- DNS record or documented URL for an MCP server at a predictable location (mcp.{domain}, {domain}/mcp, or referenced in product docs)
- A live protocol handshake: the crawler sends a real MCP `initialize` request over Streamable HTTP to each candidate endpoint, including path-bearing URLs discovered in documentation and registry listings
- Tool list enumerated through a live `tools/list` request when the handshake succeeds, with non-empty descriptions and input schemas. Live enumeration replaces documentation-derived tool counts
- Auth-gated servers classified by their rejection response: a 401 or 403 carrying an OAuth bearer challenge or a JSON-RPC error object confirms a live MCP server whose tools are credential-gated
- Documentation analysis as the fallback: when no candidate endpoint returns an MCP-shaped response, tool enumeration falls back to the server's published documentation and the audit records that the live probe got no MCP-shaped response
- Listing in Anthropic's MCP registry (registry.modelcontextprotocol.io or equivalent)
- Listing in third-party registries (Smithery, mcp.so, Glama, PulseMCP)
- Documented authentication mechanism for the MCP server
- Tool descriptions longer than 50 characters and containing actionable verbs
- Response time under 10 seconds for `tools/list` requests

## Common failure modes

- Product blog announces an MCP server but the server URL is unreachable from outside their network
- MCP server exists but is gated behind a paid tier or beta program with manual approval
- Registry listing exists but the server URL has changed and the listing was never updated
- Tools are exposed but their descriptions are placeholder text or copied from REST endpoint summaries without adaptation for agent context
- Server requires a session token obtained through a dashboard flow rather than supporting machine credentials
- MCP server covers a subset of the product's actual capabilities, leaving agents with a partial integration

## What an agent does with this channel

An agent starting a new task queries available MCP registries for tools matching the task domain. If your product is listed and the server is reachable, the agent loads your tool definitions into its context and uses them as candidates for invocation. The agent does not visit your website to learn about your product. The agent does not read your marketing copy. The MCP listing and tool descriptions are your entire pitch. If the listing is absent, broken, or vague, the agent picks a competitor.
