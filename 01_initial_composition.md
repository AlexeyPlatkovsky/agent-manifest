---
version: 2.0.0
project: agent-manifest
url: https://github.com/AlexeyPlatkovsky/agent-manifest/blob/main/01_initial_composition.md
---

# 01_initial_composition.md — Initial Composition

## Context Required

Before starting, ensure the following files are available in this session:
- `MANIFEST.md`
- `IMPLEMENTATION.md`
- `protocols/_README.md`
- all canonical protocol files under `protocols/` required by this framework version
- `.ai/docs/project_specification.md`

If `.ai/docs/project_specification.md` is missing, stop and require `00_project_profile.md` first.

If any framework source is missing, stop and ask the user to provide it.

---

## Purpose

Create or adjust the smallest coherent AI instruction system that fully aligns with `MANIFEST.md` and `IMPLEMENTATION.md`.

The result must be correct enough to stand on its own.
`02_review.md` may catch bugs, but it is not the planned repair phase for an intentionally rough draft.

You must also:
- use `.ai/docs/project_specification.md` as the authoritative profile source
- ask only for profile clarification when the profile has gaps that block composition
- derive required capabilities from protocol frontmatter, not from memorized protocol names
- keep generated project skills standalone and project-local

---

## Working Mode

Work in exactly 3 phases:
1. Inventory
2. Discussion
3. Composition

During Discussion, follow `protocols/brainstorm.md` exactly.
During Composition, do not return to discussion.

---

## Phase 1 — Inventory

Investigate the repository before proposing any changes.

Read `.ai/docs/project_specification.md` first. Treat it as the authoritative source for user role, recurring duties, capability triggers, AI tools, quality expectations, and local authority sources unless the repository clearly contradicts it.

### A. Project Nature

- what kind of project is this
- likely capability triggers
- maturity: prototype, active development, stable, or legacy
- single-purpose or multi-domain

### B. Tech Stack

- languages and frameworks
- test stack
- build and CI tooling
- runtime or deployment environment

### C. Existing Documentation

- root docs
- architecture or design docs
- conventions docs
- pipeline docs
- existing AI instruction files
- duplicated or conflicting guidance

### D. Existing Instruction System

- what acts as the current root operational contract
- whether `AGENTS.md` exists
- whether native tool entrypoints exist
- whether skills, pipelines, agents, or reference docs already exist
- whether shared project rules already exist
- whether routing logic exists
- whether execution skills are improperly coupled to orchestration

### E. AI Tool Surface

- which AI tools are already configured or clearly intended
- which tool-specific entry files exist
- whether the repo is single-tool or multi-tool
- whether existing tool-specific files contain real policy or are already adapters

### F. Reusable Project Knowledge

Identify reusable knowledge that may justify reference docs:
- architecture
- commands
- domain context
- repository structure

Identify repeated best-practice statements that may justify shared project rules:
- coding standards
- review standards
- testing standards
- domain rules

Do not decide what to create yet.

### G. Protocol Inventory

- read every canonical protocol under `protocols/`, excluding `protocols/_README.md`
- treat protocol frontmatter as authoritative
- determine which protocol triggers are present
- determine which optional protocols may be justified by project needs and present triggers
- identify any cross-capability enforcement declared by the protocols

### H. Structural Risk Inventory

Explicitly identify:
- duplicated skills or near-duplicate names
- monolithic pipeline registries that would need splitting
- existing capabilities that may already satisfy a required protocol
- any risky change that would require user approval before implementation

---

## Phase 1 Output

Stop after inventory and provide:
1. project summary
2. current instruction-system summary
3. initial compliance check
4. main risks, gaps, and ambiguities
5. proposed discussion agenda

End with an explicit checkpoint:
- say Phase 1 is complete
- ask the user to confirm or correct the inventory
- state that Phase 2 will begin only after confirmation
- do not ask the first discussion question in the same message

Do not edit or create files in Phase 1.

---

## Phase 2 — Discussion

Resolve only the high-impact decisions identified in Phase 1.

Rules:
- ask one question at a time
- provide 2-3 concrete options
- highlight trade-offs and risks
- stop and wait after each question
- never mix discussion with execution

Ask only what is actually needed to complete a correct design. Do not repeat profile questions that `.ai/docs/project_specification.md` already answers.

High-impact discussion topics may include:
- contradictions between the profile and repository evidence
- primary AI landscape when the repo mixes tools
- single-tool versus multi-tool or AI-agnostic structure
- where instruction artifacts should live
- whether existing capabilities exactly satisfy required protocol-derived capabilities
- whether pipelines are justified and which real pipeline should be formalized first
- whether reference docs should be created
- whether shared project rules are justified by repeated behavior across skills or agents
- whether existing files should be migrated, split, merged, or preserved
- whether unselected AI entry files should be removed or neutralized

At the end of discussion:
- produce a decision summary
- ask the user to confirm it before Phase 3

---

## Phase 3 — Composition

Begin only after the user confirms the decision summary.

### Composition Rules

- `MANIFEST.md` and `IMPLEMENTATION.md` are the canonical framework sources
- for multi-tool or AI-agnostic projects, `AGENTS.md` is the project root contract
- for single-tool projects, use the tool's official native entrypoint as the full root contract
- verify the chosen tool's native entrypoint convention from current official docs during composition
- tool-specific adapters must stay thin and must not duplicate policy
- generated project skills must be standalone project artifacts
- generated project skills must include protocol-mandated behavior plus minimal project-specific adaptation
- generated project skills must not reference framework protocol files or framework-only paths
- execution skills must stay isolated and must not contain manager or cross-skill routing language
- use `pipeline` terminology consistently
- keep file size near the 150-line target when possible without harming quality

### Protocol-Derived Capabilities

Derive required capabilities from protocol frontmatter:
- if `implementation: mandatory` and a `requires_when` trigger is present, the corresponding project capability must exist
- if `implementation: optional` and a `requires_when` trigger is present, implement it only if the project clearly needs it

Use the protocol filename basename as the default capability name only when the project does not already have an exact equivalent.

An existing capability counts as an exact equivalent only if:
- responsibility matches
- mandatory protocol coverage matches
- no contradiction exists

If the existing capability is only close:
- treat it as non-equivalent
- stop and ask the user before splitting, merging, replacing, or duplicating anything

### Root Contract Rules

For multi-tool or AI-agnostic projects:
- create or update `AGENTS.md`
- create or update thin tool-specific adapters that say to follow `AGENTS.md` strictly
- store shared capabilities under `.ai/skills`, `.ai/pipelines`, `.ai/agents`, `.ai/rules`, and `.ai/docs` unless the user explicitly chose another location
- create each shared skill as `.ai/skills/<skill_name>/SKILL.md`
- use Claude-style YAML frontmatter in each shared skill with at least `name` and `description`

For single-tool projects:
- place the full contract directly in the official native entrypoint
- use the tool's native structure for supporting artifacts by default
- do not create `AGENTS.md` unless the user explicitly asked for AI-agnostic or multi-tool structure

### Risky Change Safety

Before any risky change, stop and ask the user.

This includes:
- splitting monolithic pipeline files
- moving artifacts into `.ai/`
- renaming capabilities
- merging or deleting duplicated artifacts
- replacing existing tool entrypoints
- choosing between multiple valid implementation contracts

When asking, explain:
- what should change
- why it is needed
- what the compliant target state would be

### Pipelines

Rules:
- create a pipeline only when a repeated or non-trivial multi-step workflow is present
- anchor each pipeline to a real workflow discovered in the profile or repository
- if multiple valid workflows could become the first pipeline, ask the user which one to formalize first
- use one pipeline per file when more than one pipeline exists or when the pipeline is substantial

### Reference Docs

Rules:
- create reference docs only when reusable project knowledge clearly justifies them
- use reference docs for facts such as architecture, commands, project context, domain vocabulary, and authoritative source locations
- do not create reference docs by default

### Project Rules

Rules:
- create shared project rules only when at least two skills or agents need the same best-practice statements
- store shared rules under `.ai/rules` in multi-tool or AI-agnostic projects unless the user explicitly chose another location
- do not create a rule for a single skill only
- reference shared rules from skills or agents instead of copying them

### Validation and Completion

Rules:
- every non-trivial pipeline must include at least one explicit validation step
- stronger review loops apply only for higher-risk work
- `task-complete` must be the closure step for non-trivial work

### Scope Boundaries

If multiple AI tools are detected but the user selected only some of them:
- update only the selected tools
- do not silently rewrite unselected tool entrypoints
- if stale unselected tool files remain, ask whether to remove or neutralize them

---

## Final Output Format

When composition is complete, provide:

### 1. Final Assessment
- what was found
- what was wrong or missing
- what decisions were made

### 2. Target Architecture
- what files now exist
- what each file is responsible for

### 3. Change Summary
- what was created
- what was updated
- what was removed or merged

### 4. Remaining Trade-Offs
- what is intentionally kept simple
- what can be added later if the project grows

---

## Quality Bar

The final system must:
- align with `MANIFEST.md`
- align with `IMPLEMENTATION.md`
- be project-specific rather than generic
- stand on its own without requiring `02_review.md` to rescue obvious flaws
- avoid duplicated rules
- avoid competing authorities
- avoid unnecessary abstraction
- preserve good existing project capabilities where possible
- keep routing centralized in the correct layer
- derive capabilities from protocol metadata rather than a hardcoded list
