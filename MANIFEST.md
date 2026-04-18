---
version: 1.1.1
project: agent-manifest
url: https://github.com/AlexeyPlatkovsky/agent-manifest/blob/main/MANIFEST.md
---

# MANIFEST.md

## Purpose

This document defines the canonical philosophy and rules for managing:
- AI instructions
- context distribution
- orchestration
- modular execution

It is **technology-agnostic** and **project-agnostic**.

It answers one question:

> How should any AI system be structured to work efficiently, predictably, and without context waste?

This document is the **single source of truth** for:
- designing instruction systems
- evolving orchestration
- validating architectural decisions

---

# Framework Versioning

This framework is versioned as a single unit.

The following files must always share the same frontmatter version value:
- `MANIFEST.md`
- `01_initial.md`
- `02_review.md`
- `03_evolution.md`
- `brainstorm_protocol.md`

Rules:
- all five files must have identical `version` values at all times
- every framework refactor must increment the version in all five files simultaneously
- semantic versioning is mandatory: `MAJOR.MINOR.PATCH`

Version meaning:
- PATCH: wording fixes and clarifications
- MINOR: new skills, new rules, or structural additions
- MAJOR: breaking changes to the framework contract

This repository includes a maintenance skill at `.claude/skills/maintain-version-frontmatter/SKILL.md` to enforce this rule during future refactors.

---

# Core Principles

## 1. Minimize Always-Loaded Context

Only the **minimum required rules** must be loaded by default.

Everything else must be:
- modular
- on-demand
- explicitly invoked

Violation of this principle leads to:
- token waste
- degraded reasoning quality
- unpredictable behavior

---

## 2. Separate Policy from Execution

Two fundamentally different things must never be mixed:

### Policy (WHAT / WHEN)
- global rules
- classification
- constraints
- permissions
- expected outputs

### Execution (HOW)
- step-by-step procedures
- workflows
- implementation logic

Policy lives in:
- `AGENTS.md`

Execution lives in:
- skills
- workflows

---

## 3. Progressive Disclosure of Complexity

The system must reveal complexity **only when needed**.

Order of escalation:
1. Direct execution (trivial tasks)
2. Skill-based execution
3. Workflow-based execution
4. Multi-role / subagent execution

Never start from level 4.

---

## 4. No Duplication Rule (Critical)

Any rule must exist **only once**.

Allowed:
- referencing a rule

Forbidden:
- redefining the same rule in multiple places

Typical violations:
- task classification repeated in multiple files
- validation rules copied into workflows
- git rules duplicated in skills and root

Duplication = inconsistency risk.

---

## 5. Composability over Complexity

Build systems from:
- small
- well-defined
- reusable units

Avoid:
- large "do everything" instructions
- monolithic workflows
- tightly coupled components

---

## 6. Explicit Responsibility Boundaries

Each layer must have a **single responsibility**.

If a file does more than one job → it is wrong.

---

## 7. Deterministic > Implicit Behavior

Critical behaviors must be:
- explicitly defined
- predictable

Avoid relying on:
- "AI will figure it out"
- implicit conventions

---

## 8. Optimize for Real Tasks, Not Theory

The system must:
- minimize overhead for simple tasks
- scale for complex tasks

Avoid:
- forcing heavy orchestration on simple work
- designing for edge cases first

---

## 9. Routing Rules Must Be Mandatory Gates

Routing logic in `AGENTS.md` must be written as **imperative, blocking instructions** — not as classification guidance or reference tables.

### Why this matters

Descriptive routing ("non-trivial tasks → route via manager") reads as a suggestion.
The AI's default behavior is to begin solving immediately.
Without a hard stop, routing is bypassed — silently and without error.

### What compliant gate language looks like

```
Before taking any action on a non-trivial task:
1. STOP.
2. Load the designated routing skill or agent.
3. Do not proceed until routing is resolved.
```

The named routing capability must be concrete.
For large projects this is typically a manager skill.
For smaller systems it may be a task-specific routing capability.

### What non-compliant language looks like

```
Non-trivial tasks should be routed via the manager skill.
```
```
| Non-trivial + Low Risk | Workflow + validation |
```

These are descriptions of intent, not enforcement mechanisms.

### Rules

- Routing rules MUST use imperative language: STOP, MUST, DO NOT proceed
- Routing rules MUST specify the exact next action (which skill/agent to load)
- Routing rules MUST appear at the top of AGENTS.md, before capability registry
- Trivial task classification MUST be explicit — the AI must justify opting out of routing
- Skills listed as "required" for task completion MUST use mandatory language, not "use for" suggestions

### Applies to

- Task routing gates (trivial vs non-trivial)
- Completion gates (validation, review loops)
- Skill invocation requirements (when a skill is not optional)

---

# Canonical Structure

## Root Layer

### `MANIFEST.md`
- philosophy
- principles
- system design rules

### `AGENTS.md`
- operational contract
- rules AI must follow
- task classification
- available capabilities

---

## Execution Layer (Stored under `.claude/` by default)

### `.claude/skills/`
Atomic, reusable procedures.

Claude skills use the current directory format:
- `.claude/skills/<skill-name>/SKILL.md`

Examples:
- implement feature
- validate
- work with git

Rules:
- must be self-contained
- must not include orchestration logic

---

### `.claude/workflows/`
Multi-step execution patterns.

Used only for:
- non-trivial tasks

Rules:
- define order of steps
- reference skills
- do not redefine skills

---

### `.claude/agents/`
Specialized roles (subagents).

Used only when:
- context isolation is beneficial
- independent reasoning improves results

Examples:
- code reviewer
- architect
- researcher

---

### `.claude/skills/manager/` (or equivalent)
Routing and orchestration logic.

Responsibilities:
- interpret task classification
- select workflow
- choose skills/subagents

Must NOT:
- duplicate AGENTS.md
- implement execution steps

---

# Mandatory Skills

## Brainstorm Skill (Required in Every Project)

Every project built with this framework **must** include a brainstorm skill.

This is non-negotiable regardless of project size.

### Why it is mandatory

Discussion and design decisions are a core part of any project lifecycle.
Without a canonical brainstorm skill:
- AI behavior during discussions is unpredictable
- question/answer protocols are inconsistent
- design decisions are not properly structured

### What it must implement

The brainstorm skill must follow the protocol defined in `brainstorm_protocol.md`.
It must NOT redefine the protocol inline.

### Where it lives

`.claude/skills/brainstorm/SKILL.md`

It must reference `brainstorm_protocol.md` as its canonical behavior source.
It must NOT redefine the protocol inline.

### Registration

It must be registered in `AGENTS.md` like any other skill:
- name: brainstorm
- purpose: structured discussion and design decision support
- when to use: any time a design, architecture, or workflow decision must be made
- when not to use: during execution phases; after a decision is already made

## Task-Complete Skill (Required for Non-Trivial Tasks)

Every project built with this framework **must** include a task-complete skill.

This skill is mandatory for every non-trivial task and exempt for trivial tasks.

### Why it is mandatory

Non-trivial execution needs an explicit exit gate.
Without a canonical task-complete skill:
- pipelines can terminate without a verifiable closure step
- skipped or deviated steps can go undocumented
- users cannot reliably confirm that the planned work actually finished

### Purpose

Mandatory exit gate for every non-trivial task.
Produces a structured closure report so the user can verify that work was completed correctly and no steps were silently skipped.

### Scope

This skill is mandatory for every non-trivial task and exempt for trivial tasks.

### Enforcement Model

The manager skill is responsible for appending task-complete as the final step of any non-trivial pipeline at routing time.
Individual pipelines do not need to declare it.
Enforcement is centralized in the manager, not distributed across pipelines.

### Report Format

The skill must produce a markdown table with at least 3 columns:

| Step | Skill / Agent | Comment |
|------|---------------|---------|
| ... | ... | ... |

Rules:
- every executed step must appear as a row
- the `Comment` column must be left blank unless a step deviated from plan, was skipped, or warrants user attention
- skipped steps must always include a comment explaining why

### When NOT to use

Do not invoke for trivial tasks.
Do not invoke for single-step, low-risk, or purely cosmetic changes.

### Where it lives

`.claude/skills/task-complete/SKILL.md`

### Registration

It must be registered in `AGENTS.md` like any other skill:
- name: task-complete
- purpose: mandatory closure reporting for non-trivial tasks
- when to use: final step of any non-trivial pipeline, appended by the manager
- when not to use: trivial, single-step, low-risk, or purely cosmetic tasks

---

# Capability Registry

AGENTS.md must contain the canonical registry of all available:

- skills
- agents

Each entry must include:
- name
- purpose
- when to use
- when not to use

This registry exists to ensure:
- all actors (main agent and subagents) share the same capabilities view
- consistent routing and decision making
- no hidden or implicit functionality

Restrictions:
- AGENTS.md must NOT contain implementation details
- AGENTS.md must NOT duplicate skill or agent logic
- Detailed behavior must remain in corresponding files

If a capability is not listed in AGENTS.md:
- it is considered non-existent for orchestration purposes

---

# Task Classification Model

## Dimension 1: Complexity

- Trivial
- Non-trivial

## Dimension 2: Risk

- Low
- Medium
- High
- System-level (cross-cutting)

---

## Execution Matrix

| Type | Execution |
|------|----------|
| Trivial + Low Risk | Direct skill execution |
| Non-trivial + Low/Medium Risk | Workflow + validation |
| Non-trivial + High Risk | Workflow + review loop |
| System-level | Architect + full pipeline |

This matrix is **not a reference table**. It must be translated into mandatory gate language in `AGENTS.md`.

The AI must classify the task **before** beginning any work.
If the task is non-trivial, the AI must STOP and load the designated routing capability for that project.
Direct execution of non-trivial tasks without routing is a violation.

---

# Review Strategy

Review is **not mandatory for every task**.

Use when:
- logic is complex
- changes affect multiple areas
- risk is high

Skip or simplify when:
- change is isolated
- behavior is obvious
- risk is low

---

# Validation Rules

Validation is mandatory unless task is purely informational.

Types:
- tests
- lint
- type checks

Validation must:
- be automated
- be repeatable
- not depend on AI judgment

---

# Context Management Rules

## Always Loaded

- MANIFEST.md (conceptually)
- AGENTS.md

## On Demand

- skills
- workflows
- subagents
- reference docs

---

## Context Anti-Patterns

Avoid:

- large root files
- loading all skills at once
- mixing unrelated instructions
- embedding workflows in root files

---

# Project Size Guidelines

## Small Projects

Avoid:
- workflows
- manager skill
- subagents

Use:
- AGENTS.md
- minimal skills
- brainstorm skill (mandatory)
- task-complete skill (mandatory for non-trivial work)

---

## Medium Projects

Must have:
- skills
- basic workflows
- explicit routing gates that name the next capability
- manager skill for centralized non-trivial routing and completion enforcement

Always include:
- brainstorm skill
- task-complete skill

---

## Large Projects

Must have:
- manager skill
- workflows
- risk-based routing
- subagents (selectively)
- brainstorm skill
- task-complete skill

Must avoid:
- duplication
- uncontrolled growth of instructions

---

# Reference Documentation Strategy

Optional but recommended for reusable knowledge:

Examples:
- architecture
- conventions
- domain rules

Rules:
- only create if reused by multiple skills
- must not be always loaded
- must be referenced, not embedded

---

# Evolution Rules

The system must evolve based on:

- real usage
- observed failures
- measurable improvements

When updating:
- update MANIFEST.md first
- then propagate changes

---

# What This System Optimizes For

- minimal context usage
- predictable execution
- modular extensibility
- cross-AI compatibility
- maintainability over time

---

# What This System Avoids

- over-engineering
- instruction duplication
- unnecessary abstraction
- AI-specific lock-in (except storage convenience)
- hidden behavior

---

# Final Rule

If a new idea:
- increases complexity
- duplicates logic
- or increases always-loaded context

→ it must be rejected or redesigned.

---
