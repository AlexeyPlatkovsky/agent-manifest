---
version: 2.0.0
project: agent-manifest
url: https://github.com/AlexeyPlatkovsky/agent-manifest/blob/main/IMPLEMENTATION.md
---

# IMPLEMENTATION.md

## Purpose

This document defines how the agent-manifest framework realizes the values and principles in [MANIFEST.md](MANIFEST.md).

`MANIFEST.md` states what must be true. `IMPLEMENTATION.md` defines the practices, layers, gates, and file conventions that make those truths operational.

---

# Framework Sources

The framework has four source types:

- `MANIFEST.md` defines values and principles.
- `IMPLEMENTATION.md` defines framework mechanics.
- `NN_name.md` files define framework stages.
- `protocols/*.md` define framework protocols used by stages.

Framework protocols are not project runtime files. They are inputs for generating or reviewing project-local capabilities.

---

# Framework Stages

A stage is a framework prompt file named with the `NN_name.md` pattern.

Current stages:
- `00_project_profile.md`: create or update `.ai/docs/project_specification.md`
- `01_initial_composition.md`: build the initial instruction system
- `02_review.md`: audit an instruction system for compliance
- `03_capability_expansion.md`: add justified skills, pipelines, agents, conventions, or docs
- `04_tool_adoption.md`: adopt an external tool into an existing instruction system

Stage rules:
- `00_project_profile.md` must run before any other stage
- stages 01-04 must stop if `.ai/docs/project_specification.md` is missing
- stages apply `MANIFEST.md`, `IMPLEMENTATION.md`, framework protocols, and the project specification together
- stage files are not project runtime artifacts

## Stage Standards

These standards apply to every stage. Stages declare what is stage-specific without restating these rules.

### Context Required

Every stage must declare a "Context Required" list naming the files it needs.

If any listed file is missing, the stage must stop and ask the user to provide it.

If `.ai/docs/project_specification.md` is missing, stages 01-04 must stop and require `00_project_profile.md` first.

### Brainstorming

When a stage uses brainstorming, it must cite `protocols/brainstorm.md` and follow it exactly. Stages must not restate brainstorm protocol rules. A stage may name only the stage-specific scope of the conversation and the topics in scope.

### Composition Anchor

When a stage enters a composition, implementation, or fix-application phase, it must apply this document's §Project Landscape (including §Layer Purity), §Principle Implementation, and §Framework Protocol Contract. Stage-specific composition rules supplement, not replace, these anchors.

### Phase Discipline

Stages must not modify files during inventory, audit, discovery, discussion, brainstorm, clarification, reconciliation, or proposal phases. File modifications belong only in the stage's composition or implementation phase, and only after explicit user approval when the stage requires it.

---

# Project Landscape

Generated or reviewed instruction systems use these layers when justified by the project.

## Project Profile

The project profile is the reusable specification stored at `.ai/docs/project_specification.md`.

Rules:
- it is created or updated by `00_project_profile.md`
- it records the user's role, recurring duties, AI tools, capability triggers, quality expectations, authoritative sources, domain vocabulary, and accepted assumptions
- stages 01-04 must read it before acting
- if it is missing, stages 01-04 must stop and require `00_project_profile.md`
- it is project knowledge, not a behavioral rule

## Root Contract

The root contract is the always-loaded policy layer. It classifies tasks, enforces gates, defines constraints, and routes work. It does not execute task procedures.

For single-tool projects:
- the selected tool's official native entrypoint may hold the full operational contract
- the native entrypoint must be verified against current official documentation during composition
- supporting artifacts should use the selected tool's native structure by default
- `AGENTS.md` is not required

For multi-tool or AI-agnostic projects:
- `AGENTS.md` is the canonical root operational contract
- every tool-specific entry file must be a thin adapter
- each adapter must explicitly instruct the tool to load and follow `AGENTS.md` before doing project work
- each adapter must use imperative language, name the exact canonical file path, and state that `AGENTS.md` wins over the adapter if there is any conflict
- each adapter must stop the tool if `AGENTS.md` is unavailable instead of letting the tool proceed from memory or inference
- no tool-specific file may become a second source of truth

A thin adapter is not a vague pointer. It should be short, but it must still be operationally complete for that tool. Avoid adapter language such as "see", "refer to", or "follow if needed" when the adapter is meant to impose a mandatory root contract.

Minimum adapter content:
- state that the file is only an adapter for the named tool
- instruct the tool to read `AGENTS.md` before starting any project task
- instruct the tool to follow every applicable rule in `AGENTS.md`
- state that `AGENTS.md` overrides the adapter on conflict
- require the tool to stop and ask for `AGENTS.md` if it cannot read it

Shared multi-tool storage uses:
- `.ai/agents`
- `.ai/conventions`
- `.ai/docs`
- `.ai/pipelines`
- `.ai/skills`

The framework-standard shared skill format is `.ai/skills/<skill_name>/SKILL.md` using markdown with Claude-style YAML frontmatter. At minimum, the frontmatter must include `name` and `description`.

If a project already stores capabilities elsewhere, migration is a structural refactor and requires user approval.

## Skills

Skills are atomic execution capabilities.

Rules:
- one responsibility
- no orchestration logic
- no cross-skill routing
- no duplicated root policy
- standalone project-local behavior

## Pipelines

Pipelines are ordered orchestration for non-trivial work.

Rules:
- pure sequencing only
- reference skills or agents
- do not redefine skill behavior
- include validation for non-trivial work
- use one file per pipeline when more than one pipeline exists or when a pipeline is substantial
- create a pipeline when a repeated task type is non-trivial and has a distinct ordered execution path
- do not collapse distinct workflows into one generic pipeline when their steps, validation, or review gates differ

For software projects, common pipeline candidates include feature implementation, code review, and code refactoring. Create these only when the project profile or repository evidence shows they are real recurring work, but explicitly test them during initial composition for software repositories.

## Agents

Agents are specialized roles used only when context isolation or specialized reasoning is clearly justified.

Rules:
- do not create a default agent layer without evidence of need
- keep agent responsibilities distinct from skills and pipelines
- use agents for isolation, specialization, or parallel responsibility, not decoration

## Conventions

Conventions are mandatory shared standards used by multiple skills or agents.

Rules:
- create a convention only when at least two skills or agents need the same behavior
- never create a convention for one skill only
- define how to approach a category of work, not how to perform one task
- do not classify, route, sequence, or execute work
- skills and agents reference conventions instead of copying them
- in multi-tool or AI-agnostic projects, store shared conventions under `.ai/conventions`
- use one file per convention area, such as `.ai/conventions/code.md` or `.ai/conventions/testing.md`

Consistency between skills is an emergent property of sharing the same convention source.
Common behavior must live in one place to prevent drift.
When a skill or agent references a convention, that convention is mandatory for that skill or agent.

## Reference Docs

Reference docs hold reusable project knowledge.

Rules:
- use docs for facts such as architecture, commands, domain context, and repository structure
- keep docs on demand, not always loaded
- do not use docs to enforce behavior that belongs in the root contract, a skill, a pipeline, an agent, or a convention

## Layer Purity

Every artifact must stay inside the responsibility boundary of its layer.

Tests:
- pipeline body = ordered references to skills, agents, or single trivial commands; no embedded execution procedure, no "how to" prose, no standards, no DSL or coding rules, no checklists that belong in a skill or convention
- skill body = one atomic execution capability; may cite conventions and reference docs, but does not sequence sibling skills and does not restate standards already owned by a convention
- convention body = shared standards only; no classification, routing, sequencing, or execution; no task procedure
- root contract / manager body = routing and gates only; no execution bodies

If a layer is about to absorb content that belongs elsewhere, place the content in the correct layer first. A pipeline whose body could be deleted without losing execution detail because the detail lives only there is a skill mislabeled as a pipeline.

---

# Principle Implementation

## Context And Simplicity

### 1. Load Only What You Need

Implementation:
- keep the root contract small and policy-focused
- load skills, pipelines, agents, conventions, and docs only when relevant
- avoid placing task-specific instructions in always-loaded files
- split large instruction files when they are large because they have multiple jobs

The 150-line target remains a strong guideline for generated project instruction files, not a hard cap. Do not sacrifice clarity, correctness, or single responsibility just to satisfy the target.

### 2. Build For Now, Not For Later

Implementation:
- create only artifacts the current project actually needs
- reuse good existing project capabilities before adding new ones
- preserve existing naming when it is already coherent
- add configurability only when the current project requires it
- generalize only after a repeated pattern exists

### 3. Escalate Only When Justified

Classify work by complexity and risk.

Complexity:
- trivial
- non-trivial

Risk:
- low
- medium
- high
- system-level

Execution expectation:
- trivial + low risk: direct execution
- non-trivial + low or medium risk: pipeline plus validation
- non-trivial + high risk: pipeline plus stronger review
- system-level: strongest available routing path

Translate this matrix into mandatory gate language in the root contract. Do not copy the matrix itself into generated root contracts.

## Authority And Structure

### 4. Give Every Artifact One Job

Implementation:
- root contract: policy and routing gates
- skill: atomic task execution
- pipeline: ordered orchestration
- agent: specialized role or isolated context
- convention: shared standard
- reference doc: reusable project knowledge

If a file grows because it is doing too many jobs, split it rather than expanding it indefinitely.

### 5. Separate Policy From Execution

Implementation:
- put classification, routing gates, constraints, and required outputs in the root contract
- put task procedures in skills
- put sequencing in pipelines
- put specialized role behavior in agents
- keep project knowledge in docs and shared standards in conventions

Execution skills must not contain manager handoff text, stage metadata, or routing to other skills.

### 6. Keep One Source Of Truth

Implementation:
- centralize each behavioral requirement in the layer responsible for it
- reference conventions and docs instead of copying their text into skills or agents
- derive project capabilities from framework protocol metadata, not from memorized protocol names
- do not let project skills depend on framework protocol files or framework-only paths at runtime

### 7. Respect Existing Authority

If an existing project capability exactly satisfies a required capability, reuse it.

An exact match means:
- the responsibility matches
- required framework protocol coverage matches
- no contradiction with the framework exists

If an existing capability is only close:
- treat it as non-equivalent
- stop and ask the user whether to split, preserve, replace, or create another artifact

Use the project's existing capability names when they already satisfy the framework. Do not create a near-duplicate capability just to match a canonical protocol filename.

If a project-native name fulfills a protocol:
- keep the project-native name
- note the protocol mapping once in the root contract or review report

If the naming convention is unclear or conflicts with framework defaults, ask the user.

## Control And Safety

### 8. Make Behavior Explicit

Before non-trivial implementation:
- state assumptions explicitly
- surface ambiguity instead of guessing
- define success criteria
- name intended verification

Avoid descriptive routing without a stop gate, implied validation, and implied completion behavior.

### 9. Gates Must Actually Gate

For non-trivial tasks, the root contract must use imperative blocking language.

Compliant pattern:
- classify the task first
- if trivial: proceed directly and state the classification
- if non-trivial: stop, load the concrete routing capability, and do not implement until routing resolves
- if unsure: treat as non-trivial

Routing gates must appear before the capability registry.

Validation is mandatory for non-trivial work:
- every non-trivial pipeline must include at least one explicit validation step
- stronger review loops apply only for higher-risk work
- validation should be automated and repeatable where possible
- `task-complete` is the required closure skill for non-trivial work
- `task-complete` enforcement should be centralized in the routing layer, not repeated across execution skills

### 10. Ask Before You Cut

If compliance or implementation requires a risky change, stop and ask the user before changing it.

Risky changes include:
- moving capabilities to a new directory
- splitting a monolithic file
- merging or deleting duplicated artifacts
- renaming or replacing existing capabilities
- choosing between multiple valid implementation contracts

When asking, explain:
- what should change
- why the change is needed
- what the safe target state would be

---

# Framework Protocol Contract

Protocols in `protocols/` are canonical framework inputs. They are not project runtime files.

Every protocol must declare structured frontmatter:
- `implementation: mandatory | optional`
- `requires_when: [...]`

Protocol frontmatter is authoritative for derivation and review. Prompt logic must not infer applicability from prose when metadata is present.

Each protocol body must define:
- purpose
- mandatory rules that implementations must preserve
- allowed project-specific adaptations
- output contracts, when applicable

## Capability Derivation

Capability derivation must come from canonical protocol metadata.

Rules:
- only protocols whose `requires_when` trigger is present may require implementation
- `protocols/_README.md` is an index of available protocols and must not participate in capability derivation
- protocols marked `implementation: mandatory` define required project capabilities when their trigger is present
- protocols marked `implementation: optional` may be implemented only when their trigger is present and the project genuinely needs them

Generated project skills must:
- be standalone project artifacts
- include the protocol's mandatory behavior
- include minimal project-specific adaptation
- avoid references to framework files, protocol files, or framework-only paths

For multi-tool or AI-agnostic projects, generated project skills must use the framework-standard skill format defined above.

---

# Capability Triggers

Prefer the smallest coherent system that satisfies the triggers actually present in the project profile and repository evidence.

Use these triggers:
- multiple AI tools or AI-agnostic portability need: root contract plus thin adapters
- open design decisions or setup/profile clarification choices with trade-offs: brainstorming capability
- non-trivial routed work: validation and task-complete capability
- routing must choose between multiple skills, pipelines, or agents: manager-equivalent capability
- repeated multi-step workflow or repeated non-trivial task type with distinct steps: pipeline
- repeated task type: skill
- repeated behavior across skills or agents: project convention
- reusable project facts such as architecture, commands, domain vocabulary, or source locations: reference doc
- context isolation or specialized reasoning need: agent
- high-risk or system-level work: stronger review and validation gates

Do not create managers, pipelines, agents, conventions, or docs by default. Create them only when a concrete trigger exists.

---

# Final Rule

Reject, defer, or redesign any proposed change that:
- adds unnecessary always-loaded context
- duplicates an existing source of truth
- creates a competing authority
- introduces orchestration where direct execution would suffice
- forces a risky change without user approval
