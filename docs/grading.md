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

The Agent Readiness Grade (A, B, C, or D) is derived from running a verification swarm against the Coverage Map. The swarm simulates autonomous agent behavior across the discovery-to-integration flow. The grade reflects how far an agent gets when attempting to use the product, not how many channels happen to exist.

The grade is determined by best-path logic. The audit identifies the path that gets an agent furthest through the discovery-to-integration flow and grades against that path's completion point.

**Grade A: Full discovery-to-integration flow completed autonomously**
The verification swarm completed the full path from discovery through authenticated API call without human intervention. At least one combination of channels supports an end-to-end autonomous integration.

**Grade B: Found and evaluated, blocked at onboarding**
The swarm discovered the product and could evaluate its capabilities, but could not complete the provisioning flow to reach a working API call. The product is visible to agents but unreachable without human help.

**Grade C: Found but cannot evaluate**
The swarm discovered the product exists but could not retrieve enough information to evaluate whether it matches the agent's task. Discovery surfaces exist but their content is too sparse or broken for an agent to make a selection decision.

**Grade D: Invisible**
The swarm could not discover the product through any of the six channels. The product is not findable through autonomous agent discovery paths.

## Why v2 does not use weighted scoring

Earlier versions of the methodology assigned percentage weights to each channel and produced a numerical score (for example, 67/100). The v2 model replaced weighted scoring for three reasons.

First, weights imply universal importance ranking. In practice, the channels that matter most vary by product category. An e-commerce product weights A2A and structured data heavily. A developer tools product weights OpenAPI and CLI heavily. A single weight scheme cannot serve both correctly.

Second, percentage scores invite gaming. A product can boost its score by adding minor improvements to many channels rather than achieving real agent-readiness on the channels that matter for its category. The grade-based model rewards completed flows rather than checklist coverage.

Third, the grade is what buyers actually need. The question agents ask is not "what is this product's score?" but "can I use this product?" A, B, C, and D answer that question directly.

## The verification swarm

The verification swarm is a set of agents that simulate the actions an autonomous agent would take when attempting to use the product. The current implementation uses structured probes that exercise each channel against the agent flow. The output of each probe feeds the best-path logic that determines the final grade.

A future version of the swarm will incorporate language-model-driven agents for tasks that require interpretation rather than structured probing. The grade output remains the same A/B/C/D scale regardless of which probe layer produced it.

## Forward-looking: NotApplicable status

The current four-tier scale assumes all six channels apply to every audited product. This understates the actual agent-readiness of products in categories where specific channels do not apply. A repository that exists only to publish methodology does not need a build system, so the standard Channel 5 signals understate its quality. An e-commerce product does not need a CLI in the same way a developer tool does.

A NotApplicable status is on the roadmap for v2.1. When NotApplicable applies to a channel, that channel is excluded from grade derivation rather than counted as Not Present. NotApplicable is determined by deterministic rules per channel based on product category, not by self-declaration. This will allow the audit to grade SaaS broadly and e-commerce products against the channels that apply to their category while preserving the same A/B/C/D output.
