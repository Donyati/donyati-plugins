---
name: donyati-accelerators
description: Search Donyati's delivery accelerator library — reusable scripts, templates, workbooks, and toolkits consultants use across engagements, primarily EPM
---

# /donyati-accelerators — Search the Accelerator Library

Use the `search_accelerators` MCP tool to find delivery accelerators consultants have
already built — before writing something from scratch.

## Usage

```
/donyati-accelerators <query>
```

## Filtering

Scope by platform and/or category:

```
/donyati-accelerators platform: oracle-epm query: FCC metadata migration
/donyati-accelerators category: script query: substitution variable
/donyati-accelerators platform: onestream category: template
```

Categories: `script`, `template`, `workbook`, `toolkit`, `document`.

## Examples

```
/donyati-accelerators substitution variable updater
/donyati-accelerators platform: essbase query: metadata export
/donyati-accelerators category: workbook query: reconciliation
```

## Output

Up to 20 matching accelerators (name, platform, category, languages, tags, slug, short
description), published versions only. Returns a permission message rather than results
if your role does not hold the Accelerator Library capability — ask an administrator via
Roles & Permissions.

## What this doesn't do (yet)

This is search only — no version detail, changelog, or download. Get the accelerator's
`slug` from a result and open it in the web app at
`https://expert-agents.donyati.com/accelerators/<slug>` for the full detail and download.
