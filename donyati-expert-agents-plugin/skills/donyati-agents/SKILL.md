---
name: donyati-agents
description: List the full Donyati agent roster — platform experts, industry experts (insurance, manufacturing, retail, …), and Donyati specialty agents like Havagi — with how to invoke each.
---

# /donyati-agents — Full Agent Roster

See every expert agent Donyati offers, grouped by type. `/donyati-platforms` covers only the software platforms; this shows industries and Donyati specialty agents too.

## Usage

```
/donyati-agents
```

No arguments. Calls the `list_agents` tool and presents the roster.

## What you'll see

- **Products** — platform experts (Oracle EPM, SAP, Snowflake, …) → ask via `/donyati-ask`
- **Industries** — vertical experts (insurance, manufacturing, retail, …) → ask via `/donyati-ask <question> — industry: <slug>` (stacks an industry lens on the platform answer)
- **Donyati** — firm specialty agents (Havagi SOW reviewer → `/donyati-sow-review`)

## When to use

- Before `/donyati-ask` when you want an industry-framed answer and need the slug
- To answer "what experts do we have?" in a sales conversation
