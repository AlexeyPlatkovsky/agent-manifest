---
version: 2.1.1
project: agent-manifest
url: https://github.com/AlexeyPlatkovsky/agent-manifest/blob/main/agents/_README.md
---

# Agents

This directory contains canonical framework agent templates.

Agent templates are framework inputs. They are copied into generated project instruction systems only when their structured frontmatter applies.
This file is an index and is not an agent template or derivation input.

## How To Use Agent Templates

- Treat each agent file as the canonical source for that specialized role at framework design time.
- Use `implementation` and `requires_when` as the authoritative applicability metadata.
- Write `requires_when` entries as human-readable trigger phrases with spaces, matching protocol metadata style.
- Copy mandatory matching templates into the project-local agent layer.
- Copy optional matching templates only when their trigger is present and the project genuinely needs them.
- Keep copied agents on demand; root contracts and adapters should route to them rather than inline their full instructions.

## Current Agents

### `instruction-evaluator.md`

Read-only review agent for instruction artifacts before they are accepted into a project instruction system.
