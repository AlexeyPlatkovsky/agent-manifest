---
version: 2.0.0
project: agent-manifest
url: https://github.com/AlexeyPlatkovsky/agent-manifest/blob/main/README.md
---

# AI Instruction Framework

The Agent Manifesto is a portable, tool-agnostic framework for organizing AI instruction systems. `MANIFEST.md` defines the framework's values and principles; `IMPLEMENTATION.md` defines the practices that apply them. Together they keep instruction systems minimal, explicit, and scalable across single-tool and multi-tool environments.

---

## How To Use

The framework is delivered as a set of stages. A stage is any `NN_name.md` file. Each stage is self-contained — attach or reference it in your AI tool and ask the tool to run it. The exact syntax depends on your tool (`@file` in Claude Code, Cursor, and most modern agents), but the idea is the same across all of them:

> `run @<stage-file>.md`

Run `00_project_profile.md` before any other stage. After that, pick the stage that matches your current situation.

---

### Stage 00 — Profile The Project

**When:** before any other framework stage, or when the user's role, duties, tools, or project assumptions changed.

**Run:**
```
run @00_project_profile.md
```

**What happens:**
- The AI captures the project purpose, user role, recurring duties, and AI tool surface.
- It identifies authoritative local sources, domain vocabulary, and quality expectations.
- It optionally researches current best practices when local context is insufficient and the user approves.

**Outcome:** `.ai/docs/project_specification.md`, the reusable profile required by every later stage.

---

### Stage 01 — Compose The Initial System

**When:** starting from scratch, or refactoring an existing messy instruction system.

**Run:**
```
run @01_initial_composition.md
```

**What happens:**
- The AI inventories your repository.
- It reads `.ai/docs/project_specification.md`.
- It asks only unresolved design questions needed for composition.
- It derives required capabilities from protocol metadata.
- It preserves good existing capability names where they already satisfy the framework.
- It asks before any risky change (splits, moves, merges, deletions, contract choices).

**Outcome:** the smallest coherent instruction system that fully aligns with `MANIFEST.md` and `IMPLEMENTATION.md`.

---

### Stage 02 — Review For Compliance

**When:** after significant instruction changes, or when you want a compliance check on an existing system.

**Run:**
```
run @02_review.md
```

**What happens:**
- Validates the correct root-contract model.
- Checks routing gates, duplication, and responsibility boundaries.
- Verifies protocol coverage from structured metadata.
- Produces a minimal fix plan before any implementation.

**Outcome:** a validated instruction system and a short, targeted list of fixes if anything is off.

---

### Stage 03 — Expand Capabilities

**When:** a valid baseline already exists and the team has real recurring workflows to encode.

**Run:**
```
run @03_capability_expansion.md
```

**What happens:**
- Learns recurring work directly from you.
- Proposes new skills, pipelines, agents, conventions, and docs grounded in actual usage.
- Materializes newly-triggered mandatory protocols as standalone skills when new capability triggers appear.

**Outcome:** an instruction system that reflects how your team actually works, without speculative abstractions.

---

### Stage 04 — Adopt External Tools

**When:** adopting a specific external tool, library, or framework into an existing instruction system.

**Run:**
```
run @04_tool_adoption.md
```

**What happens:**
- Inventories the tool's runtime surface, demos, and foreign instruction artifacts.
- Reconciles foreign skills into standalone project skills, wrapped libraries, references, or discards.
- Enforces cleanup of demo content and broken imports before completion.

**Outcome:** the external tool is cleanly integrated, with no leftover demo noise or conflicting instructions.

---

## What This Repository Contains

- `MANIFEST.md` — framework values and principles
- `IMPLEMENTATION.md` — framework mechanics and operational rules
- `protocols/_README.md` — protocol index and usage notes
- `protocols/*.md` — canonical protocol definitions used by stages
- `00_project_profile.md` — creates or updates the reusable project specification
- `01_initial_composition.md` — builds or adjusts a baseline instruction system
- `02_review.md` — audits an instruction system against the framework
- `03_capability_expansion.md` — expands a correct baseline with new capabilities
- `04_tool_adoption.md` — adopts an external tool or framework into an existing instruction system

---

## Core Model

This framework supports two root-contract modes:

- **Single-tool projects:** the selected AI tool's official native entrypoint may hold the full project contract.
- **Multi-tool or AI-agnostic projects:** `AGENTS.md` is the canonical root contract, and tool-specific files are thin adapters.

For multi-tool shared storage, the default layout is:
- `.ai/agents`
- `.ai/conventions`
- `.ai/docs`
- `.ai/pipelines`
- `.ai/skills`

Project skills are standalone project artifacts. They are derived from framework protocols during composition, but they must not reference framework protocol files at runtime.

Project conventions are mandatory shared standards used by multiple skills or agents. They belong in `.ai/conventions` only when at least two skills or agents need the same behavior, so common guidance has one source of truth and does not drift.

Use one file per convention area, such as:
- `.ai/conventions/code.md`
- `.ai/conventions/testing.md`
- `.ai/conventions/test_coverage.md`
- `.ai/conventions/review.md`

The project profile is stored at `.ai/docs/project_specification.md`. Stages 01-04 must stop if it is missing.

In multi-tool or AI-agnostic projects, the framework-standard skill format is:
- `.ai/skills/<skill_name>/SKILL.md`
- Claude-style YAML frontmatter with at least `name` and `description`

Single-tool projects should still use the selected tool's native supporting-artifact structure.

---

## Protocol Metadata

Protocol frontmatter is authoritative for derivation and review.

Each protocol declares:
- `implementation: mandatory | optional`
- `requires_when: [...]`

In the bundled protocol set:
- `brainstorm` is mandatory when open design decisions or setup/profile clarifications require choosing between meaningful options
- `task-complete` is mandatory for non-trivial routed work
- `manager` is mandatory when routing must choose between multiple capabilities or centralize validation/completion

---

## Capability Triggers

The framework derives structure from concrete triggers, not broad labels.

- multiple AI tools or portability need → root contract plus thin adapters
- open design decisions or setup/profile clarification choices → brainstorming capability
- non-trivial routed work → validation and task completion
- routing across multiple capabilities → manager-equivalent routing
- repeated multi-step workflow → pipeline
- repeated task type → skill
- shared behavior across skills or agents → project convention
- reusable project facts → reference doc
- context isolation or specialized reasoning → agent

---

## Operating Rules

- Keep instruction files near 150 lines when possible without harming quality.
- Prefer minimal, surgical changes that trace directly to the user's request.
- Keep execution skills isolated from orchestration.
- Use `pipeline` terminology consistently.
- Do not duplicate behavioral requirements across root contracts, skills, pipelines, agents, conventions, or docs.
- Do not perform risky changes silently.

---

## License

© Alexey Platkovsky. Licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).
