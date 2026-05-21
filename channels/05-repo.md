# Channel 5: Repository Agent-Readiness

## What this channel measures

Whether agents working inside your codebase, or the codebases of developers integrating your product, can navigate the repository, understand conventions, and contribute correctly without human guidance. This is the only channel where the audited surface is the source repository itself rather than a deployed endpoint.

## Why it matters

Coding agents are now the primary consumers of repository content. Claude Code, Cursor agent mode, Devin, Factory.ai, and similar tools read repository files to understand context before making changes. A repository without agent-oriented documentation forces these agents to infer conventions from code, which produces inconsistent results, broken builds, and pull requests that miss project standards. Products whose integration involves SDK installation, configuration files, or example code benefit further: a developer's coding agent will read your example repo before writing integration code, and a well-structured example repo becomes a direct sales asset.

## The four tiers

**Not Present**
The repository has no agent-oriented documentation. No AGENTS.md, no CLAUDE.md, no .cursor/ rules, no copilot-instructions.md. The README is human-oriented marketing copy. Agents working in the repo must infer everything from code and commit history.

**Present but Deficient**
Some agent documentation exists but is incomplete or outdated. Common patterns: an AGENTS.md exists but covers only build commands and not architecture; .cursor/ rules duplicate README content without adding project-specific guidance; documentation references files or commands that no longer exist; multiple agent docs contradict each other; the docs cover the happy path but omit testing, linting, or deployment.

**Functional**
The repository contains agent-oriented documentation that covers build, test, lint, and primary development workflows. Architecture is documented at a level sufficient for an agent to make targeted changes without breaking conventions. At minimum, AGENTS.md exists at the root with project structure, key commands, and coding standards. Documentation is current with the codebase.

**Optimized**
Multi-tool agent documentation is in place: AGENTS.md at root, CLAUDE.md or equivalent for Claude-specific guidance, .cursor/rules/ for Cursor, .github/copilot-instructions.md for Copilot, plus a /rules/ or /docs/agents/ directory with deeper guidance on architecture decisions, testing patterns, and release processes. Documentation is verified by CI to prevent drift. Examples are runnable. The repository is structured so that an agent dropped into it with no context can ship a meaningful change on its first attempt.

## Observable signals

The crawler checks for:

- AGENTS.md at repository root
- CLAUDE.md or CLAUDE/ directory at repository root
- .cursor/ directory with rules files
- .github/copilot-instructions.md
- A /rules/, /docs/agents/, or equivalent directory
- README.md that includes development setup beyond marketing copy
- CONTRIBUTING.md with concrete commands
- Presence of structured config (package.json scripts, Makefile, justfile, Taskfile.yml) documenting key commands
- CI configuration that exercises build, test, and lint
- Recent commit activity in agent documentation files (signal of maintenance)

## Common failure modes

- AGENTS.md is a copy-paste of the README with no agent-specific content
- Documentation tells agents to "run the tests" without specifying the command
- Multiple agent doc files exist with conflicting instructions
- Repository has a sophisticated build system but no documentation of the entry points
- Documentation references private internal tools or wikis the agent cannot access
- Examples in documentation are out of sync with current API surface
- The repository is a docs-only or methodology-only repo where most repo-readiness signals do not apply, but the audit treats it as a software repo and grades it below its actual quality

## What an agent does with this channel

An agent working in the repository reads agent-oriented documentation before making changes. AGENTS.md gets read first when present, followed by tool-specific files. The agent uses these to determine which commands to run for build verification, which patterns to follow when writing new code, and which areas of the codebase require extra care. If no agent documentation exists, the agent falls back to inferring from README, file structure, and recent commits, which produces lower-quality output. For products whose integration involves the developer's own repository, an agent will read your example repos to learn integration patterns before writing code in the developer's project.

## Note on non-software repositories

This channel was designed for software repositories. Repositories whose purpose is methodology, specification, or documentation alone may not have meaningful build, test, or lint requirements. The current audit grades these repositories against the same signals, which understates their actual agent-readiness. A NotApplicable status for specific signals is on the Surfex roadmap to handle this case correctly.
