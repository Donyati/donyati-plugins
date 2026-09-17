---
name: donyati-compare
description: Compare enterprise platforms using independent Donyati expert agents with neutral synthesis — eliminates platform bias
---

# /donyati-compare — Multi-Agent Platform Comparison

Use the `compare` MCP tool from the `expert-agents` server to get independent expert analyses from multiple platform agents, then a neutral synthesis.

## Usage

```
/donyati-compare <topic or question>
```

## Examples

```
/donyati-compare Oracle EPM vs OneStream for financial consolidation
/donyati-compare SAP vs Workday for HR and payroll
/donyati-compare Snowflake vs Databricks for AI/ML workloads
/donyati-compare Anaplan vs Workday Adaptive vs Planful for FP&A
/donyati-compare Microsoft D365 vs Salesforce for CRM
```

## How It Works

1. Platforms are auto-detected from your question (or specify explicitly)
2. Each platform's expert agent independently analyzes the topic — the experts can't see each other's responses
3. A neutral synthesis agent creates a fair comparison table
4. Results include "When to Choose X" guidance for each platform

Output is grounded in Donyati's proprietary knowledge base, not vendor marketing.

## Comparing for a specific client

A comparison is usually a recommendation for someone. To ground it in what we already know
about them:

1. Call `list_organizations` (or run `/donyati-clients`) and take the `organizationId`.
2. Optionally call `list_projects` for that org and take the `projectId`.
3. Pass `organizationId` (and `projectId`) to `compare`.

Every expert analysis and the synthesis then see the client's **confirmed** knowledge — not
the synthesized wiki, because a comparison often goes in front of the client. Never guess an
id; the tool refuses an organization you are not granted.

```
/donyati-compare Oracle EPM vs OneStream for consolidation — client: Apex Manufacturing
```
