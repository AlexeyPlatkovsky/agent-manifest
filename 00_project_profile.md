---
version: 2.1.0
project: agent-manifest
url: https://github.com/AlexeyPlatkovsky/agent-manifest/blob/main/00_project_profile.md
---

# 00_project_profile.md — Project Profile

## Context Required

Before starting, ensure the following files are available in this session:
- `MANIFEST.md`
- `IMPLEMENTATION.md`
- `protocols/brainstorm.md`

If any are missing, stop and ask the user to provide them.

---

## Purpose

Create or update `.ai/docs/project_specification.md`.

The project specification is the reusable profile that later stages use to avoid working in a vacuum. It captures the user's role, recurring duties, project purpose, AI tool surface, domain vocabulary, authoritative sources, quality expectations, and accepted assumptions.

Later stages must read this file before work begins. If it is missing, they must stop and require this stage first.

---

## Working Mode

Work in exactly 4 phases:
1. Inventory
2. Profile Capture
3. Research, only when needed and approved
4. Composition

Before Profile Capture begins, present a short inferred inventory summary as a Markdown table. Do not begin with raw prose or a batch of questions.

During Profile Capture, follow `protocols/brainstorm.md` for every missing or ambiguous profile point that requires user input.

During Research, use external sources only when local context is insufficient and the user approves current best-practice research.

---

## Phase 1 — Inventory

Investigate the repository before asking questions.

Identify:
- project purpose and domain from existing files
- existing documentation that appears authoritative
- existing tests, examples, or workflows that reveal business logic
- configured AI tools and tool-specific entrypoints
- likely capability triggers based on repository evidence
- likely user work areas based on repository evidence
- existing `.ai/docs/project_specification.md`, if present

Do not create or edit files in Phase 1.

At the end of Phase 1, summarize what was inferred before asking for clarification.

The summary must be:
- short
- table-based
- based only on repository evidence
- separated from the first clarification question

Use this table shape:

| Area | Inferred from local context | Confidence / gap |
| --- | --- | --- |
| Project purpose | ... | ... |
| Authoritative sources | ... | ... |
| AI/tool surface | ... | ... |
| Quality workflow | ... | ... |
| Open profile gaps | ... | ... |

---

## Phase 2 — Profile Capture

Ask only for missing or ambiguous profile information.

Required profile fields:
- primary user role or position in this project
- recurring duties the instruction system should support
- AI tool mode: single-tool or multi-tool / AI-agnostic
- exact AI tools in use now or required immediately
- authoritative local sources for domain and business logic
- preferred quality expectations and workflows
- whether external best-practice research is allowed

For role and duties, provide selectable options and allow multiple choices or free-form answers.

Example duty options:
- coding and refactoring
- test writing and automation
- code review
- documentation creation and editing
- architecture and design work
- bug investigation
- release or deployment support
- project or domain research

Follow `protocols/brainstorm.md` for every clarification question.

Stage-specific addition: allow free-form correction when the listed options do not fit a factual profile field such as a specific role, tool name, or path.

Do not ask the first clarification question in the same message as the inventory summary. Let the user confirm or correct the summary first, then proceed one question at a time.

---

## Phase 3 — Research

Run this phase only when:
- local sources are insufficient, and
- the user approved external best-practice research.

Research should identify candidate practices for the user's role and recurring duties. Treat external findings as candidate guidance, not project authority.

Before writing them into the profile:
- summarize the candidate practices
- ask the user which ones to accept, reject, or defer
- do not silently convert external advice into project conventions

---

## Phase 4 — Composition

Create or update `.ai/docs/project_specification.md`.

The file must include:
- project purpose
- user role or position
- recurring duties
- AI tool mode and exact tools
- capability triggers that are already known
- domain vocabulary, if known
- authoritative local sources
- quality expectations
- preferred workflows
- accepted external best practices, if any
- rejected or irrelevant assumptions, if any
- open questions or profile gaps

Store the profile at `.ai/docs/project_specification.md`.

This path is the framework-standard profile location for every project mode. Later stages depend on this exact file.

---

## Final Output Format

When the profile is complete, provide:
1. where the project specification was written
2. what role, duties, tools, and known capability triggers were captured
3. what authoritative sources were identified
4. what assumptions remain open
5. whether later stages may proceed
