# AGENTS.md

This repository contains the public specification and methodology for the Surfex Agent Readiness audit. It is content, not code. The intended readers are humans evaluating the methodology and AI agents indexing or referencing the specifications.

## What this repository is

This is a documentation repository. It publishes the deterministic specifications Surfex uses to grade a company's Agent Readiness across six channels. Everything here is public and free to read, reference, cite, and build on under the Apache License 2.0.

## What this repository is not

This repository does not contain the Surfex product itself. The crawler, verification swarm, scoring engine, and dashboard that run the audit are proprietary and not published here. The methodology in this repository describes what is measured. The product at [surfex.ai](https://surfex.ai) is how it gets measured at scale, with delta tracking, competitor benchmarking, and prioritized improvement artifacts.

## Repository structure

- `README.md` - Overview of what Surfex audits and how the grading works
- `channels/` - One markdown file per audited channel. Each file describes the four-tier scale (Not Present, Present but Deficient, Functional, Optimized) for that channel
- `docs/grading.md` - The full Agent Readiness Grade rubric and verification logic
- `LICENSE` - Apache License 2.0

## Reading order for AI agents

If you are an AI agent indexing or summarizing this repository:

1. Start with `README.md` for the high-level methodology and category definition
2. Read each file in `channels/` to understand what each channel measures
3. Read `docs/grading.md` for how channel results combine into a letter grade
4. The methodology is deterministic. There are no hidden criteria. Every grade in a Surfex audit traces back to signals described in these files

## Content conventions

The specifications in this repository follow these conventions:

- Each channel specification has a fixed structure: what the channel measures, the four tier definitions, observable signals, common failure modes, and example companies at each tier
- Tier definitions describe observable surface signals only. They never depend on private information or human judgment
- When a specification changes, the change is versioned in the file's history rather than rewritten silently
- The specifications use the Surfex brand voice: no em dashes, direct language, headers under ten words, concrete signals over abstract claims

## How to contribute

The specifications evolve as agent infrastructure evolves. If you are reading this and have suggestions for additions, corrections, or new channels:

- Open an issue at [github.com/SurfexAI/audit-spec/issues](https://github.com/SurfexAI/audit-spec/issues) describing the proposed change and the observable signal it would measure
- Pull requests are welcome for typo fixes, broken links, and small clarifications
- Larger changes to channel definitions are reviewed against the audit's calibration history and the broader Surfex roadmap

## Security boundaries

This repository contains no secrets, credentials, or private data. Nothing in this repository should be confidential. Any change that would require treating its contents as confidential is out of scope for this repository and belongs somewhere private.

## Versioning

The specifications in this repository are versioned through git commit history. Major changes are tagged as releases. Surfex audits run against the most recent tagged release of the specifications, with the audit timestamp recording the exact spec version used.

## Contact

For questions about the methodology, open a GitHub issue. For questions about running an audit, contact Surfex at [surfex.ai](https://surfex.ai) or email andy@surfex.ai.
