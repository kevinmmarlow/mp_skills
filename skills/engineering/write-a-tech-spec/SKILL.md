---
name: write-a-tech-spec
description: Create a staff-engineer-quality tech spec through user interview and codebase exploration. Bridges business need and implementation with explicit tradeoffs, alternatives, and risks. Use when user wants to write a tech spec, technical design doc, RFC, or engineering design document.
---

This skill is invoked when the user wants to write a tech spec. A good tech spec is concise, structured, and focuses heavily on tradeoffs, context, and decision-making — not just the "how" but the "why". It serves as a durable reference long after the feature ships and is readable by engineers and PMs alike.

You may skip steps if not necessary, but do not skip the interview or alternatives sections — those are where the value lives.

## Process

1. **Anchor the spec.** Ask the user for:
   - A long, detailed description of what they want to build and why.
   - Any upstream artifact: PRD, Linear ticket, incident, customer ask, prior design doc. Read it before continuing.
   - Who the audience is (team-internal, cross-team, exec-readable) — this calibrates depth.

2. **Explore the codebase** to verify their assertions and ground the proposed solution. Look for:
   - Existing modules that already solve part of the problem.
   - Current data models, API contracts, and service boundaries that will be touched.
   - Prior art for similar features (patterns, conventions, naming).
   - Hidden constraints (concurrency models, README.md files in affected dirs, deprecation notes).

3. **Interview the user relentlessly** about every aspect of the design. Walk down each branch of the decision tree and resolve dependencies one-by-one. Focus the interview on:
   - **Tradeoffs**: for every meaningful choice, surface the alternative you would have picked and why you didn't.
   - **Boundaries**: what's explicitly out of scope, and why.
   - **Failure modes**: what happens when each external dependency is down, slow, or wrong.
   - **Migration / rollback**: how do we get there from here, and how do we back out.
   - **Operability**: what does on-call see when this breaks.

4. **Sketch the major modules** to be built or modified. Prefer deep modules (lots of functionality behind a simple, stable interface) over shallow ones. Confirm the module breakdown matches the user's mental model before writing.

5. **Choose a save location.** Ask the user:
   - Local markdown file at `./specs/<slug>.md`, OR
   - Linear issue via `save_issue` (ask for team)

6. **Write the spec** using the template below. Render in Markdown. Keep prose tight — bullets over paragraphs. Diagrams via ASCII or Mermaid when they clarify; skip them when they don't.

## Template

```markdown
# <Title>

| | |
|---|---|
| **Author** | <author> |
| **Reviewers** | <names> |
| **Status** | Draft / In Review / Approved / Implemented |
| **Last updated** | <YYYY-MM-DD> |
| **Related** | <Linear / PRD / Figma / prior specs> |

## Overview

One paragraph: what is this, why now, who is asking. Written so a PM or a new engineer can orient themselves in under a minute.

## Goals

- Bulleted, concrete, measurable where possible.
- Each goal should be something a reviewer could disagree with.

## Non-goals

- Explicit boundaries. What this spec is NOT solving, even if adjacent.
- Prevents scope creep during review and implementation.

## Proposed solution

The recommended approach. Cover:

- **Architecture**: how new/changed components fit into the existing system. Diagram if it helps.
- **Data model**: schema changes, new tables/columns, migrations. Note backfill strategy.
- **API contracts**: new endpoints, RPC methods, GraphQL fields, event payloads. Include request/response shape.
- **Key flows**: step-by-step for the 1-3 most important user or system flows.
- **Module breakdown**: which modules are new, which are modified, what each owns.

## Alternatives considered

For each meaningful alternative:

- **Alternative**: one-line summary.
- **Why rejected**: the specific tradeoff that ruled it out.

Include at least the strongest competing approach. "We considered nothing else" is almost never true and almost never acceptable in a staff-level spec.

## Implementation plan

Phased rollout. For each phase:

- Scope of work.
- Milestone / exit criteria.
- Feature flag, dark-launch, or canary strategy if applicable.
- Testing strategy: what's covered by unit, integration, end-to-end, manual QA.

## Risks & dependencies

- **Risks**: security, scalability, data integrity, latency, cost. Pair each risk with a mitigation.
- **Dependencies**: other teams, services, vendors, or in-flight work this blocks on.
- **Operability**: monitoring, alerts, runbook expectations.

## Open questions

- Numbered list of unresolved decisions.
- Each question should name who can answer it.
```

## Quality bar

Before handing back the spec, verify:

- [ ] Every goal is concrete enough that a reviewer could disagree with it.
- [ ] Non-goals exist and are sharp.
- [ ] At least one alternative is discussed with an explicit rejection reason.
- [ ] Failure modes for each external dependency are addressed in Risks.
- [ ] Open questions section is not empty unless the spec is genuinely closed (rare).
- [ ] No file paths or code snippets that will rot — keep it interface-level.
- [ ] Prose is tight. Cut anything that doesn't earn its place.
