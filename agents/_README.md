---
version: 2.7.2
project: agent-manifest
url: https://github.com/AlexeyPlatkovsky/agent-manifest/blob/main/agents/_README.md
---

# Agents

This directory contains canonical framework agent templates.

Agent templates are framework inputs. They are copied into generated project instruction systems only when their structured frontmatter applies.
This file is an index and is not an agent template or derivation input.

## How To Use Agent Templates

- Treat each agent file as the canonical source for that specialized role at framework design time.
- Use `conventions/framework-metadata.md` for metadata rules.
- Use `conventions/capability-derivation.md` for derivation and copied-agent rules.

## Current Agents

### `artifact-acceptance-tester.md`

Scenario-based acceptance test agent for new or materially changed instruction artifacts before they are accepted.

### `instruction-evaluator.md`

Read-only review agent for instruction artifacts before they are accepted into a project instruction system.
