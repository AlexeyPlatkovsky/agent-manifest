---
version: 2.1.2
project: agent-manifest
url: https://github.com/AlexeyPlatkovsky/agent-manifest/blob/main/protocols/_README.md
---

# Protocols

This directory contains canonical framework protocols.

Protocols are framework inputs.
They are not copied into projects and are not referenced by project runtime files.

## How To Use Protocols

This file is an index and must not participate in capability derivation.

Use `IMPLEMENTATION.md` §Framework Protocol Contract as the authority for protocol metadata, derivation, and generated project capability rules.

## Current Protocols

### `brainstorm.md`

Structured discussion behavior for open design, setup, and profile decisions with meaningful trade-offs.

### `task_complete.md`

Closure reporting for non-trivial routed work.

### `manager.md`

Centralized routing and completion enforcement when routing must choose between multiple capabilities.
