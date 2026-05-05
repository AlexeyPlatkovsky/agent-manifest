---
version: 2.1.0
project: agent-manifest
url: https://github.com/AlexeyPlatkovsky/agent-manifest/blob/main/03_capability_expansion.md
---

# 03_capability_expansion.md — Capability Expansion

## Context Required

Before starting, ensure the following files are available in this session:
- `MANIFEST.md`
- `IMPLEMENTATION.md`
- `protocols/_README.md`
- all canonical protocol files under `protocols/` relevant to the current system
- `.ai/docs/project_specification.md`
- the current instruction system: root contract, skills, pipelines, agents, conventions, and docs

If `.ai/docs/project_specification.md` is missing, stop and require `00_project_profile.md` first.

If other required context is missing, stop and ask for it.

---

## Purpose

Expand a correct baseline instruction system into a more complete one that reflects real team habits and recurring work.

Use `.ai/docs/project_specification.md` as the authoritative profile source for role, duties, quality expectations, and accepted assumptions.

When the user already asks for specific new capabilities, keep expansion anchored to that explicit request instead of resetting to broad discovery of team habits.

This stage is not for building from scratch.
It assumes `01_initial_composition.md` already produced a valid baseline.

It is not for adopting external tools, libraries, or frameworks.
If the user's request bundles tool adoption with capability expansion, split the work: run `03_capability_expansion.md` first, then hand off tool adoption to `04_tool_adoption.md`.

---

## Working Mode

Work in exactly 4 phases:
1. Discovery
2. Brainstorm
3. Proposal
4. Composition

During Brainstorm, follow `protocols/brainstorm.md`.
During Proposal, present the full proposal at once.
During Composition, do not return to discussion.

---

## Phase 1 — Discovery

Read the current instruction system and identify:
- what `.ai/docs/project_specification.md` says the user does most often
- what skills, pipelines, agents, conventions, and docs already exist
- what recurring work they already cover
- what important recurring work is still missing
- whether any existing skill actually represents a repeated non-trivial workflow that needs a pipeline
- whether the user already named concrete additions or target responsibilities
- which design decisions are still genuinely open versus already decided by the user request
- whether the current system still matches protocol requirements
- whether duplication or blurred responsibilities have crept in

Provide a brief current-state summary, then move to Phase 2.
Do not propose solutions yet.

---

## Phase 2 — Brainstorm

Ask only about unresolved high-impact decisions.

If the user already requested concrete additions, start with a scoped question about that requested capability set.
Do not reset to a broad "typical day or week" discovery prompt.

Use a broad recurring-work question only when the request is intentionally open-ended and the missing capability areas are still unknown.

Every Brainstorm question must follow `protocols/brainstorm.md` exactly.

Explore only areas not already covered well by the existing system, such as:
- review habits
- release or deployment routines
- testing and debugging patterns
- onboarding
- docs maintenance
- shared coding, review, testing, or domain standards
- recurring cross-team coordination
- constraints or variants of the explicitly requested capability set

Do not ask for broad workflow narration when Discovery and the user request already provide enough evidence to discuss the requested additions directly.

Stop when:
- the user has described the meaningful recurring work
- no major new pipelines or specialized roles are emerging
- you have enough evidence to justify a concrete proposal

Before leaving Phase 2, ask whether anything important is still missing.

---

## Phase 3 — Proposal

Present the complete proposed delta all at once.

Group proposals by type:
1. Skills
2. Pipelines
3. Agents
4. Conventions
5. Docs

For each proposed addition, provide:
- name
- type
- purpose
- what user-described need justifies it
- what existing artifacts it depends on
- why it belongs in that layer

Before presenting, verify that each proposal:
- does not duplicate an existing artifact
- fits the correct layer
- uses a pipeline, not only a skill, when the recurring work has distinct ordered steps, validation, or review gates
- is justified by actual repository or user evidence

Ask the user to approve, reject, or modify the proposal set before implementation.

---

## Composition

Begin only after explicit user approval.

Apply `IMPLEMENTATION.md` §Stage Standards §Composition Anchor.

Stage-specific rules:
- if Discovery found a newly present protocol trigger, materialize every triggered protocol whose `implementation` is `mandatory` as a standalone project skill before any other addition
- preserve existing good artifacts unless the user approved changes
- update the applicable root contract and capability registry with each new capability

---

## Final Summary

After implementation, report:
1. what was added
2. what was updated
3. what was intentionally excluded
4. remaining gaps
5. compliance confirmation
