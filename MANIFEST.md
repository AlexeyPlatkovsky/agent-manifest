---
version: 2.0.0
project: agent-manifest
url: https://github.com/AlexeyPlatkovsky/agent-manifest/blob/main/MANIFEST.md
---

# MANIFEST.md

## Purpose

This document defines what the agent-manifest framework values and what must be true of a well-designed AI instruction system.

It does not define file layouts, derivation rules, or runtime mechanics. For how these principles are applied in practice, see [IMPLEMENTATION.md](IMPLEMENTATION.md).

---

# Values

The framework values:

- useful context over complete context
- clear authority over convenient duplication
- direct execution over premature orchestration
- explicit decisions over inferred behavior
- current need over speculative design
- user consent over autonomous risky change

The items on the right still matter, but the items on the left matter more.

---

# Principles

## Context And Simplicity

### 1. Load Only What You Need

Default context should contain only what the current work needs to begin correctly.

**Prefer:**
- Small always-available instructions
- Capabilities loaded when they are relevant
- Clear entry points into deeper guidance

**Avoid:**
- Carrying task-specific guidance everywhere
- Adding context as a substitute for design
- Treating more instruction as automatically safer

---

### 2. Build For Now, Not For Later

The system should solve the real current problem before designing for imagined futures.

**Prefer:**
- The smallest structure that satisfies current need
- Generalization after the pattern is proven
- Changes traceable to an actual requirement

**Avoid:**
- Speculative configurability
- Single-use abstractions
- Future-proofing without evidence

---

### 3. Escalate Only When Justified

Complexity should be earned by real risk, coordination need, repetition, or scale.

**Prefer:**
- Direct handling for simple work
- Reusable capabilities for repeated work
- Coordination only when multiple steps or responsibilities must be managed

**Avoid:**
- Starting with the heaviest structure available
- Adding layers as a precaution
- Treating complexity as evidence of quality

---

## Authority And Structure

### 4. Give Every Artifact One Job

Every artifact should have one clear responsibility it can be judged against.

**Prefer:**
- Artifacts that can be described in one sentence
- Names that reveal responsibility
- Splitting an artifact when a second job appears

**Avoid:**
- Mixed policy, procedure, and reference content
- Broad catch-all files
- Responsibilities that depend on reader interpretation

---

### 5. Separate Policy From Execution

The system should distinguish decisions about what should happen from the work that makes it happen.

**Prefer:**
- Decision rules that classify, authorize, and constrain
- Work units that perform one kind of task
- Explicit handoffs between decision and execution

**Avoid:**
- Work units that decide their own routing
- Decision rules hidden inside procedures
- Artifacts that both govern and execute the same concern

---

### 6. Keep One Source Of Truth

Every rule, constraint, and definition should have one authoritative home.

**Prefer:**
- A single owner for each rule
- References instead of restatement
- Shared standards when multiple places need the same guidance

**Avoid:**
- Local copies of shared rules
- Competing definitions of the same behavior
- Repeating constraints across unrelated artifacts

---

### 7. Respect Existing Authority

Before creating something new, the system should identify whether an existing authority already owns the concern.

**Prefer:**
- Auditing existing sources before adding another
- Extending the rightful owner when it still fits
- Preserving coherent ownership and naming

**Avoid:**
- Parallel authorities for the same concern
- Replacing something only because it is unfamiliar
- Creating a cleaner-looking duplicate

---

## Control And Safety

### 8. Make Behavior Explicit

Assumptions, success criteria, uncertainty, and stopping conditions should be stated rather than inferred.

**Prefer:**
- Declared assumptions
- Visible success criteria
- Ambiguity surfaced before action

**Avoid:**
- Relying on the reader to infer intent
- Completion behavior that is only implied
- Soft language for hard requirements

---

### 9. Gates Must Actually Gate

A critical decision point should be able to stop work until the required decision, condition, or approval exists.

**Prefer:**
- Blocking conditions before execution
- Clear next steps when a gate blocks
- Mandatory validation for critical work

**Avoid:**
- Rules that only advise
- Gates placed after the action they control
- Critical checks that can be skipped silently

---

### 10. Ask Before You Cut

Changes that can lose work, disrupt users, or reshape authority should require explicit consent.

**Prefer:**
- Naming the risk before acting
- Explaining the intended safe outcome
- Treating consent as a requirement

**Avoid:**
- Proceeding because a change seems obvious
- Hiding risk inside a larger change
- Treating approval for one change as approval for related changes
