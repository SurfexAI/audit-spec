# Channel 4: CLI Scriptability

## What this channel measures

Whether your command-line interface can be driven by an autonomous agent. This means headless authentication, machine-readable output, deterministic exit codes, idempotent operations, and behavior that does not depend on a human reading terminal output or responding to prompts.

## Why it matters

CLIs are the most underrated agent surface. Agents that already operate in shell environments (Claude Code, Cursor agent mode, autonomous coding agents) prefer CLI invocation over HTTP when both are available, because CLIs encapsulate authentication, retries, and product-specific logic into single commands. A scriptable CLI is often the fastest path to integration for an agent already working in a developer's terminal. Most CLIs fail at agent use because they were designed for humans: interactive prompts, ANSI color codes mixed into output, exit code 0 on partial failures, no JSON mode.

## The four tiers

**Not Present**
No CLI exists. The product is accessible only through dashboard or HTTP API. Agents working in shell environments must build their own wrappers.

**Present but Deficient**
A CLI exists but cannot be driven non-interactively. Common patterns: authentication requires a browser-based OAuth flow with no token-based alternative; commands prompt for confirmation without a `--yes` or `--force` flag; output mixes structured data with ANSI codes and progress bars; exit codes are inconsistent or always 0; no `--json` or `--output json` flag exists; commands are not idempotent and fail on retry.

**Functional**
The CLI supports headless authentication via API key, environment variable, or non-interactive token flow. Commands accept all required inputs as arguments or flags without prompting. Output is parseable, either through a `--json` flag or default structured format. Exit codes follow conventions (0 success, non-zero failure with distinct codes for different error classes). The primary workflow is documented as a sequence of CLI commands.

**Optimized**
All commands support `--json` output. Authentication supports multiple machine-friendly methods (env vars, config files, stdin). Operations are idempotent where semantically possible (create-or-update patterns). Exit codes are documented and stable across versions. The CLI ships with shell completion, but more importantly, ships with documentation explicitly aimed at agent use. Long-running operations support polling or watching with structured event output. Errors include machine-readable error codes alongside human-readable messages.

## Observable signals

The crawler checks for:

- Installation instructions reachable from product documentation
- Package available on a major registry (npm, PyPI, Homebrew, Cargo) matching the product name
- Authentication documented via API key, token, or environment variable
- `--json`, `--output json`, or equivalent flag documented for primary commands
- `--help` output that includes flags for non-interactive operation (`--yes`, `--no-input`, `--non-interactive`)
- Exit code documentation in the CLI reference
- Examples in documentation that use the CLI in shell scripts or CI workflows
- Absence of required prompts in primary workflows

## Common failure modes

- CLI requires running `tool login` once interactively before any other command works, with no alternative
- `--json` flag exists but only on a subset of commands; the most important commands still output unstructured text
- Authentication works headlessly but rate limits are tuned for human use and block scripted invocation
- Errors print to stdout instead of stderr, polluting the parseable output channel
- Exit code is 0 even when the operation partially failed
- Commands depend on terminal width or color support and produce garbled output in non-TTY environments
- Updates to the CLI break flag names or output format without major version bumps
- The CLI exists but is undocumented or hidden in a subdirectory of the GitHub repo

## What an agent does with this channel

An agent in a shell environment with a task that touches your product first checks whether your CLI is installed. If not installed, it runs the documented install command. It authenticates using credentials from environment variables. It executes the operation with `--json` and parses the response. It checks exit code to determine success. If any step requires interactive input that cannot be supplied via flags, the agent gives up on the CLI path and falls back to direct HTTP calls, or skips the integration entirely if HTTP is also blocked.
