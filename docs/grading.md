# Grading Rubric

This document explains how the Agent Readiness Grade is derived from the six channel audits. The grading model is the Coverage Map plus Verification approach, not weighted percentage scoring.

## The four-tier scale

Each of the six channels is graded against the same four-tier scale:

**Not Present**
No implementation of the channel exists. Agents using this discovery path cannot find or use the product through this surface.

**Present but Deficient**
The channel exists but breaks somewhere in the agent flow. Common patterns include incomplete implementations, broken authentication paths, missing required fields, or response times that exceed the 10-second threshold used in the audit. Agents may discover the surface but cannot complete the workflow it implies.

**Functional**
The channel works for an autonomous agent. The agent can complete the primary workflow the channel is designed to support. Implementation has rough edges but does not block agent use.

**Optimized**
The channel meets the standard a sophisticated implementer would aim for. Agent-first design choices are evident throughout. Documentation, error handling, and discoverability are tuned for autonomous consumption.

The specific signals each tier is measured against are documented in the corresponding channel spec.

## The Coverage Map

The audit produces a Coverage Map showing the status of all six channels for the audited product. The Coverage Map is the primary output of the discovery phase, before any agent simulation runs. It answers the question "what surfaces does this product expose to agents?"

The Coverage Map is informational. It does not produce a grade on its own.

## The Agent Readiness Grade

The Agent Readiness Grade (A, B, C, or D) is derived from running a verification swarm against the Coverage Map. The swarm simulates autonomous agent behavior across four stages: Discover (the agent finds the product), Evaluate (the agent reads enough to judge fit), Provision (the agent initiates an account and obtains a working credential without a human), and Execute (the agent performs a real operation). The audit walks three independent paths through those stages, one each for the API, CLI, and MCP surfaces, and records the deepest stage each path reaches. The grade reflects how far an agent gets when attempting to use the product, not how many channels happen to exist.

Each letter is defined by what an autonomous agent can and cannot do. Definitions are evaluated in precedence order: A first, then B, then C, then D. The first match wins.

Quality signals beyond the grading floor — endpoint richness, response times, documentation depth — inform channel status and findings, never the letter. The letter measures autonomy milestones; the Coverage Map is where quality lives.

**Grade A: Full discovery-to-integration flow completed autonomously**
At least one path completed all four stages without human intervention, from discovery through a real authenticated operation. Provision requires verified initiation: the agent can obtain credentials on its own, through a programmatic signup or a documented headless CLI auth path, and can use them headlessly. A tool whose signup requires a browser cannot reach Grade A, no matter how well its keys work once a human has supplied one. At least one combination of channels supports an end-to-end autonomous integration. No company receives a public Grade A without a witnessed Phase 2 execution run; Phase 1 evidence alone cannot produce a public A.

**Grade B: One real step from full autonomy**
Grade B is reachable two ways. Under live execution probes, a path completes Provision but the real operation fails at runtime, holding the path at Stage 3. Under structured probes, the deepest path is blocked at Provision only by one thing, the first API key requiring a dashboard visit, while every other provisioning condition passes, including the initiation requirement, and at least two of the three paths reach Evaluate. Both mean the same thing: the agent reached a working position and one real step stands between it and full autonomy. For the structured-probe case, that step is a human-in-the-loop first key, which is a defensible security boundary rather than a gap in machine readability.

A B is never awarded on a defaulted or semantically-empty signal. "Every other provisioning condition passes" must be evidenced, not assumed: the promotion requires a positively classified key-issuance method (a documented key-creation API endpoint or CLI command, or an explicit dashboard-issuance flow — "auto-generated at signup" does not count, because how a key is generated says nothing about how it is retrieved, and an unclassified method is no evidence at all) and the dashboard-first-key fact itself must be observed in the product's documentation rather than standing as the audit's conservative default. When the shape of a B is present but the evidence is not, the letter is C and the report names exactly which evidence is missing and what would supply it. Every report prints the raw signals the letter reads in a Grade-inputs block, so the letter can be re-derived from the report itself.

**Grade C: Found, but no autonomous path completes**
At least one path reaches Discover and the audit does not qualify for A or B. The report distinguishes two cases under the same letter. In the first, a path evaluated the surface but provisioning is blocked by more than the first-key step, for example no programmatic signup or no headless auth, or only one path reached Evaluate. In the second, the product was found but its surfaces were not machine-parseable enough to evaluate. The second case is always reported as "found but not parseable", naming the specific gap, never as absence.

**Grade D: Invisible**
No path reaches Discover. The product is not findable through a public MCP registry, an ownership-verified repository, or a package-manager CLI. D means invisible to agents, and only that. A product that was found but could not be evaluated is C, not D.

### Grade precedence

Stages are numbered by depth: Discover is 1, Evaluate is 2, Provision is 3, Execute is 4. The letter comes from the deepest stage any path reaches:

- A when a path reaches depth 4
- B when a path reaches depth 3, or when the deepest path reaches depth 2 with provisioning blocked only by the dashboard-issued first key — the key-issuance method positively classified and the dashboard-bootstrap fact observed, per the evidence bar above — and at least two of the three paths at depth 2 or deeper
- C when a path reaches depth 1 or 2 and the audit does not qualify for A or B
- D when no path reaches depth 1

### Methodology version

This grading model is methodology version v1.6, effective 2026-07-14. It adds the Grade B evidence bar: a B is never awarded on a defaulted or semantically-empty signal. The audit now records whether the dashboard-bootstrap fact was observed in the product's documentation or is standing as the audit's conservative default, and the B promotion requires the observation. It also requires a positively classified key-issuance method — "auto-generated at signup" no longer counts, because generation style says nothing about retrieval. Every report now carries a Grade-inputs block printing the raw signals the letter reads.

We publish what the rule found, in the same spirit as the v1.4 note below: applied to our own long-standing acceptance baseline, the evidence bar demoted a company whose B we had recorded as "positively evidenced" — the evidence turned out to be a conservative default derived from a page the analyzer could not truly read. The rule found one of its subjects in our own baseline, and the baseline was corrected rather than the rule. A grade that can audit its own history is worth more than one that cannot. (Methodology v1.5, effective 2026-07-14 earlier the same day, made detection-layer changes only — live MCP protocol handshakes, canonical-domain spec discovery, A2A card strictness — with no grading-rule changes.)

The v1.4 changes (effective 2026-07-08) remain in effect: Grade A requires verified initiation. Provision now asks two questions instead of one. Can the agent obtain credentials on its own, through a programmatic signup or a documented headless CLI auth path? And can it use them headlessly? Before v1.4, one path answered only the second question, so a tool with browser-only signup could reach Grade A on the strength of credential consumption alone.

We publish the correction plainly because honest grading is the point of the methodology. In July 2026 an internal review found our engine could award an A to a tool whose own audit report stated that no agent could bootstrap an account. The initiation requirement closes that gap. A grade that cannot contradict its own findings is worth more than a grade that flatters.

v1.4 also fixes an evidence rule: how a key is generated says nothing about how it is retrieved. A key that is auto-generated at signup but displayed only in a dashboard still requires a human. Generation style and retrieval path are independent facts, and the audit now treats them that way.

The v1.3 changes (effective 2026-07-02) remain in effect: Grade B is reachable without live execution probes, and Grade D is reserved for genuine absence rather than also covering products that were found but could not be evaluated. Audits run before an effective date stay frozen under the methodology version stamped on them. A newer grade requires a new scan.

## Why v2 does not use weighted scoring

Earlier versions of the methodology assigned percentage weights to each channel and produced a numerical score (for example, 67/100). The v2 model replaced weighted scoring for three reasons.

First, weights imply universal importance ranking. In practice, the channels that matter most vary by product category. An e-commerce product weights A2A and structured data heavily. A developer tools product weights OpenAPI and CLI heavily. A single weight scheme cannot serve both correctly.

Second, percentage scores invite gaming. A product can boost its score by adding minor improvements to many channels rather than achieving real agent-readiness on the channels that matter for its category. The grade-based model rewards completed flows rather than checklist coverage.

Third, the grade is what buyers actually need. The question agents ask is not "what is this product's score?" but "can I use this product?" A, B, C, and D answer that question directly.

## The verification swarm

The verification swarm is a set of agents that simulate the actions an autonomous agent would take when attempting to use the product. The crawlers perform all live HTTP requests against the product's public surfaces during the discovery phase. The verification swarm then evaluates the collected evidence against the agent flow, channel by channel, without touching the target again. The output of each evaluation feeds the best-path logic that determines the final grade.

A future version of the swarm will incorporate language-model-driven agents for tasks that require interpretation rather than structured probing. The grade output remains the same A/B/C/D scale regardless of which probe layer produced it.

## Forward-looking: NotApplicable status

The current four-tier scale assumes all six channels apply to every audited product. This understates the actual agent-readiness of products in categories where specific channels do not apply. A repository that exists only to publish methodology does not need a build system, so the standard Channel 5 signals understate its quality. An e-commerce product does not need a CLI in the same way a developer tool does.

A NotApplicable status is on the roadmap for v2.1. When NotApplicable applies to a channel, that channel is excluded from grade derivation rather than counted as Not Present. NotApplicable is determined by deterministic rules per channel based on product category, not by self-declaration. This will allow the audit to grade SaaS broadly and e-commerce products against the channels that apply to their category while preserving the same A/B/C/D output.
