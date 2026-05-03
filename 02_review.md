---
version: 2.0.0
project: agent-manifest
url: https://github.com/AlexeyPlatkovsky/agent-manifest/blob/main/02_review.md
---

# 02_review.md — Instruction System Review

## Context Required

Before starting, ensure the following files are available in this session:
- `MANIFEST.md`
- `IMPLEMENTATION.md`
- `protocols/_README.md`
- all canonical protocol files under `protocols/` required by this framework version
- `.ai/docs/project_specification.md`
- the project's full instruction system: root contract, skills, pipelines, agents, conventions, and docs

If `.ai/docs/project_specification.md` is missing, stop and require `00_project_profile.md` first.

If anything else required for review is missing, stop and ask for it.

---

## Purpose

Audit the current instruction system against `MANIFEST.md` and `IMPLEMENTATION.md`.

Use `.ai/docs/project_specification.md` as the authoritative profile source for user role, recurring duties, capability triggers, AI tools, quality expectations, and local authority sources.

This is a skeptical review.
Assume the system is wrong until proven correct.

Do not implement fixes before the audit and clarification phases are complete.

---

## Working Mode

Work in exactly 4 phases:
1. Audit
2. Clarification
3. Final Validation
4. Implementation and Verification, only when the user requests it

During Clarification, follow `protocols/brainstorm.md`.
Do not modify files during Phases 1-3.

---

## Phase 1 — Audit

### 0. Root Contract Presence

Determine which root contract model applies.

Compare the detected model against `.ai/docs/project_specification.md`.

For single-tool projects:
- verify that the tool's official native entrypoint exists and acts as the full operational contract
- verify the native entrypoint against the tool's current official docs when needed
- verify that supporting artifacts follow the selected tool's native structure

For multi-tool or AI-agnostic projects:
- verify that `AGENTS.md` exists and acts as the root operational contract
- verify that supported tool-specific entry files are thin adapters to `AGENTS.md`
- verify that shared skills use the framework-standard format `.ai/skills/<skill_name>/SKILL.md`
- verify that each shared skill uses Claude-style YAML frontmatter with at least `name` and `description`
- verify that shared project conventions, when present, live in the project conventions layer

In both cases:
- verify that routing and capability declarations are visible from the root contract
- verify that execution details are delegated to skills, pipelines, and agents

### Principle Compliance

Audit against each `MANIFEST.md` principle. For each, flag concrete violations rather than restating the principle.

- §1 Load Only What You Need — minimal root contract; skills, pipelines, agents, conventions, and docs are on-demand; adapters are thin
- §2 Build For Now, Not For Later — every artifact ties to a current trigger; no speculative abstractions
- §3 Escalate Only When Justified — direct execution by default; pipelines and agents anchored to concrete triggers
- §4 Give Every Artifact One Job — single responsibility per file; no orchestration in execution skills; conventions hold standards, not procedures
- §5 Separate Policy From Execution — policy and routing in the root contract; procedures in skills and pipelines; adapters do not duplicate policy
- §6 Keep One Source Of Truth — no duplicated behavioral requirements across root/skills/pipelines/conventions/docs
- §7 Respect Existing Authority — existing capabilities audited first; project-native names preserved when they satisfy the framework; near-duplicates flagged for user decision
- §8 Make Behavior Explicit — assumptions, success criteria, stopping conditions, and validation expectations are visible
- §9 Gates Must Actually Gate — main routing gate uses imperative language, names the next capability, appears before the registry, classifies trivial work, and enforces validation/completion. Descriptive routing is a critical violation.
- §10 Ask Before You Cut — risky changes (rename, delete, merge, authority shift) require explicit consent; the review names the risk and the safe target state

### Protocol Inventory and Applicability

- read every canonical protocol under `protocols/`, excluding `protocols/_README.md`
- treat protocol frontmatter as authoritative
- determine which protocol triggers are present
- derive required capabilities from `implementation` and `requires_when`

### Protocol Coverage

For each required protocol, verify:
- a corresponding project capability exists
- the capability is standalone and project-local
- it implements the protocol's mandatory rules
- it includes any needed project-specific adaptation without contradiction

Flag as a major violation if a project skill depends on framework protocol files or framework paths at runtime.

### Project Conventions

For each shared project convention, verify:
- at least two skills or agents reference it or clearly need it
- it defines shared standards rather than one task procedure
- it does not classify, route, sequence, or execute work
- referencing skills or agents do not copy the same standards locally
- it does not duplicate facts that belong in reference docs
- in multi-tool or AI-agnostic projects, it lives under `.ai/conventions`

Flag as a violation if a convention exists for one skill only.

### Imported Capability Adoption

If the project adopted an external tool, framework, or starter kit, verify the cleanup conditions in `04_tool_adoption.md` §Phase 4 still hold (no demo residue, no foreign skill bundle, imports resolve, capability registry up to date).

### Structure and Refactor Risks

Check for:
- near-duplicate capabilities
- monolithic pipeline registries
- mixed AI roots
- stale unsupported tool entrypoints
- oversized files that likely violate single responsibility
- imported orchestration layers that bypass or weaken the project's canonical routing path
- project conventions that create competing authority

### Validation and Completion

- does every non-trivial pipeline include explicit validation
- is `task-complete` enforced for non-trivial work
- are stronger review loops reserved for higher-risk work

---

## Phase 1 Output

Provide:

### Root Contract Report

| Location | Mode | Status | Notes |
|----------|------|--------|-------|
| ... | Single-tool / Multi-tool | Valid / Ambiguous / Missing | ... |

### Protocol Coverage Report

| Protocol | Required for This Project | Expected Capability | Status | Notes |
|----------|---------------------------|---------------------|--------|-------|
| ... | Yes / No | ... | Valid / Missing / Ambiguous / Overbuilt | ... |

### Violations

List findings by severity with file references.
Prioritize:
- critical routing failures
- incorrect root contract model
- duplicated or blurred responsibilities
- protocol coverage failures
- imported-framework adoption failures such as demo residue, broken compilation, or competing orchestration

Then list:
- open ambiguities that require clarification
- residual risks or testing gaps

---

## Phase 2 — Clarification

If genuine ambiguity remains, ask only the minimum questions needed to complete the audit.

Follow `protocols/brainstorm.md` exactly:
- one question at a time
- 2-3 options
- explicit trade-offs
- stop and wait

Do not mix clarification with fixes.

---

## Phase 3 — Final Validation

Produce:
- final verdict: compliant, partially compliant, or non-compliant
- the minimum fix plan required for compliance
- which issues are risky changes that require user approval

Do not implement until the user explicitly asks.

---

## Phase 4 — Implementation and Verification

Only after user request:
- apply the approved fixes
- verify the resulting system
- report what changed and what remains
