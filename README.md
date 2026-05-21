# Surfex Agent Readiness Audit

The public specification for how Surfex audits whether autonomous AI agents can find, read, and integrate a company's product without human help.

## What Surfex audits

Surfex evaluates six channels that determine whether an AI agent can operate against your product on its own:

1. **OpenAPI / Swagger Specification** - Can an agent read your API contract?
2. **Programmatic Onboarding** - Can an agent get a working API key without a human clicking through a dashboard?
3. **MCP Registry Presence** - Can an agent find your Model Context Protocol server in the registries where it looks?
4. **CLI Scriptability** - Can an agent install and run your command-line tools headlessly?
5. **Repository Agent-Readiness** - Can an agent read your codebase and understand how to integrate?
6. **A2A Agent Card** - Can an agent discover your capabilities through the Agent-to-Agent protocol?

Each channel is graded on a four-tier scale: Not Present, Present but Deficient, Functional, or Optimized. The combined channel results determine an overall Agent Readiness Grade from A to D.

## Why this matters

The end user is changing. For a growing share of online products, the entity selecting, integrating, and using software is no longer a human. It is an autonomous AI agent acting on behalf of someone who never opens your website.

Products built for human buyers fail in this new environment. A signup flow that requires a CAPTCHA blocks the agent. A pricing page that hides API access behind a sales call blocks the agent. A repository with no AGENTS.md leaves the agent unable to integrate. The companies that build agent-compatible infrastructure early will be the ones agents can work with. The companies that wait will find out too late.

## How the grading works

The Agent Readiness Grade is derived from a verification swarm that simulates how an autonomous agent would attempt to discover, evaluate, and integrate your product.

- **Grade A** - An agent completes the full discovery-to-integration flow autonomously
- **Grade B** - An agent finds and evaluates the product but is blocked at onboarding
- **Grade C** - An agent finds the product but cannot evaluate it
- **Grade D** - The product is invisible to agents

The full grading rubric, including per-channel thresholds and verification logic, lives in [docs/grading.md](docs/grading.md).

## The six channels

Each channel has its own specification covering what counts as Not Present, Present but Deficient, Functional, and Optimized. The specifications are deterministic. They describe observable signals on a company's public surfaces.

- [Channel 1: OpenAPI / Swagger Specification](channels/01-openapi.md)
- [Channel 2: Programmatic Onboarding](channels/02-onboarding.md)
- [Channel 3: MCP Registry Presence](channels/03-mcp.md)
- [Channel 4: CLI Scriptability](channels/04-cli.md)
- [Channel 5: Repository Agent-Readiness](channels/05-repo.md)
- [Channel 6: A2A Agent Card](channels/06-a2a.md)

## Run the audit

Surfex provides a free scan at [surfex.ai](https://surfex.ai) that returns your Agent Readiness Grade and a coverage map across the six channels. The full audit, including the verification report and a prioritized improvement plan, is available at [surfex.ai/pricing](https://surfex.ai).

## Who this is for

The specifications in this repository are for any team building a product where AI agents are part of how it gets found and used. The current scope is most directly applicable to developer tool companies and SaaS platforms. The channel set will expand as agent infrastructure matures across other verticals.

If you build software, you can read this repository and self-assess. If you want a verified third-party assessment with delta tracking, competitor benchmarking, and prioritized improvement artifacts, run the audit at [surfex.ai](https://surfex.ai).

## License

Apache License 2.0. The specifications, methodology, and rubric in this repository are free to read, reference, cite, and build on. See [LICENSE](LICENSE) for full terms.

## About Surfex

Surfex is the Agent Readiness audit for companies whose end users are now AI agents. Learn more at [surfex.ai](https://surfex.ai) or follow [@SurfexAI](https://x.com/SurfexAI) on X.
