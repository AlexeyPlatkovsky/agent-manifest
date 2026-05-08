---
version: 2.1.2
project: agent-manifest
url: https://github.com/AlexeyPlatkovsky/agent-manifest/blob/main/02_review.md
---

# 02_review.md — Instruction System Review

## Context Required

Before starting, ensure the following files are available in this session:
- `MANIFEST.md`
- `IMPLEMENTATION.md`
- frontmatter for all canonical protocol files under `protocols/`, excluding `protocols/_README.md`; full bodies only for triggered, required, or directly conflicting protocols
- frontmatter for all canonical agent template files under `agents/`, excluding `agents/_README.md`; full bodies only for triggered, required, or directly conflicting templates
- `.ai/docs/project_specification.md`
- target project root or instruction-system location, unless obvious from the session
- the project's full instruction system: root contract, skills, pipelines, agents, conventions, and docs

Optional index context:
- `protocols/_README.md`

If `.ai/docs/project_specification.md` is missing, stop and require `00_project_profile.md` first.

If anything else required for review is missing, stop and ask for it.

If the target project root or instruction-system location is ambiguous, stop and ask before auditing.

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

During Clarification, follow `protocols/brainstorm.md` only when ambiguity involves open design decisions or trade-off choices. Use the minimum direct factual question for purely factual missing context.
Do not modify files during Phases 1-3.

---

## Phase 1 — Audit

### 0. Root Contract Presence

Determine which root contract model applies.

Compare the detected model against `.ai/docs/project_specification.md`.

For single-tool projects:
- verify that the tool's official native entrypoint exists and acts as the full operational contract
- verify the native entrypoint against the tool's current official docs when validating native entrypoint or adapter structure; if current official docs cannot be checked, report the uncertainty or stop when it blocks the audit
- verify that supporting artifacts follow the selected tool's native structure

For multi-tool or AI-agnostic projects:
- verify that `AGENTS.md` exists and acts as the root operational contract
- verify that supported tool-specific entry files are thin adapters to `AGENTS.md`
- verify that adapters use explicit mandatory language, name the exact root contract path, require the tool to load and follow it before project work, state that `AGENTS.md` wins on conflict, and stop if it is unavailable
- verify that shared skills use the framework-standard format `.ai/skills/<skill_name>/SKILL.md`
- verify that each shared skill uses Claude-style YAML frontmatter with at least `name` and `description`
- verify that shared project conventions, when present, live in the project conventions layer

In both cases:
- verify that routing and capability declarations are visible from the root contract
- verify that execution details are delegated to skills, pipelines, and agents

### Principle Compliance

Audit against `MANIFEST.md` and `IMPLEMENTATION.md` §Principle Implementation. For each principle, flag concrete violations rather than restating the owning rule.

Audit-specific red flags:
- always-loaded files contain task procedures
- artifacts exist without a current trigger
- one file mixes policy, procedure, sequencing, and reference facts
- the same behavioral requirement has multiple owners
- routing gates are descriptive instead of blocking
- risky changes are proposed without naming required user approval

### Protocol Inventory and Applicability

- read frontmatter for every canonical protocol under `protocols/`, excluding `protocols/_README.md`; read full bodies only for triggered, required, or directly conflicting protocols
- treat protocol frontmatter as authoritative
- verify every protocol has `version`, `project`, `url`, `implementation`, `requires_when`, and human-readable trigger phrases with spaces rather than slug-style underscores
- determine which protocol triggers are present
- derive required capabilities from `implementation` and `requires_when`

### Agent Template Inventory and Applicability

- read frontmatter for every canonical agent template under `agents/`, excluding `agents/_README.md`; read full bodies only for triggered, required, or directly conflicting templates
- treat agent template frontmatter as authoritative
- determine which agent template triggers are present
- derive required project-local agents from `implementation` and `requires_when`
- verify every agent template has `version`, `project`, `url`, `name`, `description`, `implementation`, `requires_when`, and human-readable trigger phrases with spaces rather than slug-style underscores

### Protocol Coverage

For each required protocol, verify:
- a corresponding project capability exists
- the capability is standalone and project-local
- it implements the protocol's mandatory rules
- it includes any needed project-specific adaptation without contradiction

Flag as a major violation if a project capability depends on framework protocol files or framework paths at runtime.

### Agent Template Coverage

For each required framework agent template, verify:
- a corresponding project-local agent exists
- the agent preserves the template's specialized role, constraints, and output contract
- the agent remains on demand instead of being inlined into an always-loaded root contract or adapter
- the root contract or manager-equivalent can route applicable work to it
- for `instruction-evaluator`, instruction-artifact review work routes to it

Flag as a major violation if a required framework agent is missing or if its full instructions are copied into always-loaded context.

### Project Conventions

Audit project conventions against `IMPLEMENTATION.md` §Conventions.

Flag concrete violations, especially:
- a convention exists for one skill only
- a convention classifies, routes, sequences, or executes work
- referencing skills or agents copy the same standards locally
- facts that belong in reference docs are enforced as conventions

### Layer Purity

Audit every artifact against `IMPLEMENTATION.md` §Layer Purity. Flag any cross-layer leakage as a major violation.

Audit-specific red flags:
- a pipeline whose body could be deleted without losing execution detail because the detail lives only there (the pipeline is acting as a skill)
- a pipeline that references skills which do not exist (no callable target for the sequence)
- a pipeline or skill that restates content already in a convention or reference doc
- a project where pipelines exist but the skill layer is empty or near-empty — strong signal that pipelines absorbed execution

### Imported Capability Adoption

If the project adopted an external tool, framework, or starter kit, treat `04_tool_adoption.md` §Phase 4 as the authority for cleanup criteria. Verify evidence that its cleanup checks still hold, or route unresolved adoption cleanup issues back to that stage.

### Structure and Refactor Risks

Check for:
- near-duplicate capabilities
- monolithic pipeline registries
- missing pipelines for repeated non-trivial task types with distinct steps, validation, or review gates
- mixed AI roots
- stale unsupported tool entrypoints
- passive tool-specific adapters that merely point at the root contract without enforcing it
- oversized files that likely violate single responsibility
- imported orchestration layers that bypass or weaken the project's canonical routing path
- project conventions that create competing authority

### Validation and Completion

- does every non-trivial pipeline include explicit validation
- is `task-complete` enforced for non-trivial routed work
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

### Agent Template Coverage Report

| Agent Template | Required for This Project | Expected Agent | Status | Notes |
|----------------|---------------------------|----------------|--------|-------|
| ... | Yes / No | ... | Valid / Missing / Ambiguous / Overbuilt | ... |

### Violations

List findings by severity with file references.
Prioritize:
- critical routing failures
- incorrect root contract model
- duplicated or blurred responsibilities
- layer purity failures (pipelines containing execution, skills sequencing siblings, conventions holding procedures, root contract executing)
- protocol coverage failures
- agent template coverage failures
- imported-framework adoption failures such as demo residue, broken compilation, or competing orchestration

Then list:
- open ambiguities that require clarification
- residual risks or testing gaps

---

## Phase 2 — Clarification

If genuine ambiguity remains, ask only the minimum questions needed to complete the audit.

Follow `protocols/brainstorm.md` only for ambiguity that involves meaningful options or trade-offs. Do not mix clarification with fixes.

If no clarification is needed, state that Phase 2 has no questions and proceed to Phase 3.

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
