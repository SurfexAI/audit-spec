# Channel 2: Programmatic Onboarding

## What this channel measures

Whether an autonomous agent can sign up, create credentials, and reach a working API call without a human ever touching a browser. This is the path from "agent discovers product exists" to "agent has a valid API key and made its first successful request."

## Why it matters

An agent that finds your OpenAPI spec but cannot get an API key without clicking through a dashboard is blocked. The discovery work is wasted. Agents operate continuously and need to provision their own credentials, rotate them, and recover from failures without escalating to a human. Products that gate onboarding behind dashboard-only flows are invisible to the autonomous buyer even when their APIs are technically excellent.

## The four tiers

**Not Present**
No path exists for an agent to sign up or obtain credentials programmatically. Account creation requires a browser, email verification clicks, or manual approval. API key generation is dashboard-only with no CLI or API equivalent.

**Present but Deficient**
A programmatic path exists but breaks somewhere in the flow. Common patterns: signup is scriptable but key generation requires the dashboard; CLI auth works only after a one-time browser-based bootstrap; OAuth flow requires interactive consent that cannot be automated; documentation references a programmatic flow that returns errors or is undocumented in detail.

**Functional**
An agent can complete the full flow from zero state to working API call using only documented endpoints or CLI commands. This includes account creation, email verification (via test inbox or skip mechanism), credential generation, and a successful authenticated request. The flow works but may require multiple steps, has rough edges in error handling, or lacks idempotency.

**Optimized**
The full provisioning flow is a single documented sequence with idempotent operations, clear error responses, machine-readable status codes, and explicit support for service accounts or programmatic identities distinct from human user accounts. Quickstart docs assume the agent path as a first-class flow, not a footnote.

## Observable signals

The crawler checks for:

- Documented signup endpoint (POST /signup, /register, /accounts, or equivalent) accepting JSON
- API key or token generation endpoint reachable without a session cookie
- CLI `login` or `auth` command that accepts credentials via environment variables, stdin, or arguments rather than opening a browser
- Service account or machine identity documentation distinct from user account flows
- Quickstart documentation that shows the path from zero to first API call without screenshots of dashboards
- OAuth client credentials flow support (as opposed to authorization code flow requiring browser redirect)
- Response time under 10 seconds for signup and key generation endpoints

## Common failure modes

- Signup works via API but requires clicking an email verification link before keys can be issued
- CLI `login` command opens a browser window for OAuth consent
- Root API key can only be generated through the dashboard; programmatic keys require a parent key obtained manually
- Documentation describes a `/v1/users` POST endpoint that returns 404 or requires undocumented headers
- Rate limits on signup endpoints are aggressive enough to block legitimate agent provisioning
- Email verification has no test inbox or bypass for service accounts
- OAuth requires a registered redirect URI, which agents without a public web endpoint cannot provide

## What an agent does with this channel

A discovery agent that has read your OpenAPI spec needs credentials before it can call any authenticated endpoint. The agent attempts the documented onboarding flow. If it cannot reach a working API call within the flow, it logs the product as "found but blocked" and moves to the next candidate. The agent does not retry interactively. It does not email support. The window closes.
