---
name: stress-test
description: Stress-test a PRD, tech spec, or prototype implementation by exploring the codebase and systematically probing for gaps, risks, and unresolved decisions. Use when user wants to stress test, poke holes in, challenge, or pressure-test a PRD, tech spec, design doc, or prototype.
---

# Stress Test

Probe a PRD, tech spec, or prototype for gaps and risks. Ground every question in the actual codebase.

**Voice: terse. Sacrifice grammar for concision. No filler, no hedging, no preamble. Every sentence earns its place.**

## Process

### 1. Identify the target

Ask: **"What am I stress testing?"**

Accepts: PRD, tech spec, prototype branch/files/diff, or a module in the codebase. One clarifying question max, then go.

### 2. Explore the codebase

Use the Agent tool with subagent_type=Explore. Before asking a single question, understand:

- Patterns/conventions the target must follow
- Adjacent modules affected by or depending on the target
- Existing tests revealing implicit contracts
- Error handling and failure boundaries
- Data flows crossing the target's boundary

Do NOT skip this. Codebase-grounded questions >> generic ones.

### 3. Build question set

Organize by category. Skip what doesn't apply.

| Category | Probe for |
|---|---|
| **Failure modes** | Unhandled errors, partial failures, retries, corruption, cascades |
| **Edge cases** | Nulls, concurrency, races, clock skew, large payloads |
| **Missing requirements** | Implicit contracts, defaults, ordering guarantees |
| **Backward compat** | Migrations, rollback, feature flags, API versioning |
| **Observability** | Logging, metrics, alerting, production debugging |
| **Security** | AuthN/AuthZ, input validation, secrets, injection, data exposure |
| **Scale** | 10x/100x load, hot paths, N+1, cache invalidation |
| **Operability** | Deploy strategy, config, toggles, runbook impact |
| **Test gaps** | Untested paths, tests that break on refactor |
| **Coupling** | Leaky abstractions, modules doing too much |

### 4. Present in rounds

**Round 1:** 3-5 highest-impact questions. The ones that could sink the project. Brief rationale for each.

**Next rounds:** Drill gaps the user's answers reveal. Solid answers — move on. "Haven't thought about it" — flag as open, move on. Continue until covered or user stops.

**Rules:**
- Reference real modules/types/flows. No hypotheticals
- One question per topic. No shotgunning
- If the codebase already answers it, say so instead of asking
- Label severity: **critical** vs **nice-to-have**

### 5. Verdict

Terse summary. No prose paragraphs.

```
## Stress Test Results

### Blockers
- [gap] — [why] — [next step]

### Needs decision
- [question] — [trade-offs]

### Would improve
- [rec] — [effort vs impact]

### Solid
- [what held up]
```

Ask if user wants this written to a file.
