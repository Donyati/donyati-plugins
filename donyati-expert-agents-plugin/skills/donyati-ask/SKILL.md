---
name: donyati-ask
description: Ask a Donyati platform expert agent a question — auto-routes to Oracle, SAP, OneStream, Microsoft, Workday, Salesforce, Snowflake, Databricks, Informatica, Stibo, AWS, GCP, Tagetik, Anaplan, BlackLine, Planful, Prophix, or any other covered platform
---

# /donyati-ask — Ask an Expert

Use the `consult` MCP tool from the `expert-agents` server to ask a platform expert agent a question.

## Usage

```
/donyati-ask <question>
```

## Examples

```
/donyati-ask What are the best practices for EPBCS data integration with Snowflake?
/donyati-ask How does OneStream handle intercompany eliminations?
/donyati-ask Best Anaplan vs Workday Adaptive comparison criteria for FP&A?
/donyati-ask Compare SAP BPC vs Oracle FCCS for financial consolidation
```

## How It Works

1. Your question is sent to the Expert Agents API at `expert-agents.donyati.com`
2. The platform is auto-detected from keywords (or you can specify: `platform: oracle-epm`)
3. The relevant expert agent loads its knowledge base
4. An LLM generates a response grounded in Donyati's proprietary knowledge

## Supported Platforms

`oracle-epm`, `oracle-erp`, `oracle-integration`, `oracleai`, `sap`, `onestream`, `microsoft`, `microsoftai`, `workday`, `workdayai`, `salesforce`, `salesforceai`, `snowflake`, `snowflakeai`, `databricks`, `databricksai`, `informatica`, `stibo`, `aws`, `gcp`, `tagetik`, `anaplan`, `blackline`, `blacklineai`, `planful`, `prophix`

Run `/donyati-platforms` to see the live list with article counts.

## Industry lens

Add an industry overlay to frame the answer in that vertical's vocabulary and standards:

```
/donyati-ask How do carriers handle reinsurance eliminations in FCC? — industry: insurance
/donyati-ask Demand planning options for CPG — platform: anaplan, industry: retail
```

Run `/donyati-agents` to see the available industry slugs.

## Working on a client

`/donyati-ask` answers generically unless you tell it which client you are on. When the
question is about a specific engagement:

1. Call `list_organizations` (or run `/donyati-clients`) and take the `organizationId`.
2. Optionally call `list_projects` for that org and take the `projectId`.
3. Pass `organizationId` (and `projectId`) to `consult`.

The answer is then grounded in that client's confirmed knowledge and their engagement wiki —
the same context a web user gets in Chat. Never guess an id: the tool refuses an organization
you are not granted, and you get a refusal instead of an answer.

```
/donyati-ask What did we agree on for the FCC close calendar? — client: Apex Manufacturing
```
