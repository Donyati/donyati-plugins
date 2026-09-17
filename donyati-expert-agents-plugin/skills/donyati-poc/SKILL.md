---
name: donyati-poc
description: Track proofs-of-concept and demos centrally — create one, list what exists for a client, and move a POC through its stages (idea through graduated, parked, or killed) instead of leaving it in a project folder.
---

# /donyati-poc — Track a Proof-of-Concept or Demo

Consulting work often starts as a POC or demo before it is a project. This gives it a central
record from the first conversation, so it is visible outside whoever is running it.

## Usage

```
/donyati-poc <client> [name]
```

## How it works

1. To see what already exists: call `list_pocs`, optionally with `organizationId` (from
   `list_organizations`), `stage`, `type` (`internal` | `external`) or `commercialStatus`
   (`free` | `paid`). Omit `organizationId` to see every POC you can see.
2. To start tracking a new one: call `create_poc` with a `name` and `type`. `organizationId` is
   required for `external` (client-facing) POCs, omitted for `internal` ones. Returns a `slug` —
   every later `update_poc` call needs it.
3. To move it forward: call `update_poc` with the `slug` and whatever changed — usually `stage`,
   but also sponsor, dates, or budget as they become known.

## Stages

```
idea → approved → building → piloting → converting → graduated
                                       ↘ parked
                                       ↘ killed
```

`graduated` means the POC became a real product. Azure spend tracking only turns on from
`approved` onward — an `idea` has nothing to meter yet.

### Two rules `update_poc` will refuse you on

Both are the POC page's rules, enforced from the same function, so the connector cannot drift
from the browser. Say them before you call, because the model otherwise reads a refusal as a
bug and retries.

1. **A graduated or killed POC is final.** Every edit is refused — not just its stage, but its
   budgets, its sponsor and its linked Project. There is no un-graduate and no un-kill today.
   **`parked` is the reversible pause**; if a POC might come back, park it rather than kill it.
2. **Moving to `killed` *or* `parked` needs a `killedReason` in the same call.** The field is
   named for the kill and it is required for both, because the question is the same one: why
   is this move happening. A stage change with no reason is refused before anything is written.

A POC you cannot see refuses on scope first, so a refusal never tells you that some other
client's POC exists or what stage it reached.

## What `list_pocs` may withhold from you

The listing itself is scoped to the clients you are granted; internal POCs (no client
organization) appear only to someone who can see every client. Two **fields** are gated
separately on top of that:

| Field | Needs |
|---|---|
| Per-POC **Azure spend** | **Products** together with **Product Costs** or **Financial Actuals** — the same grants the POC Budget tab asks for. **Never** returned to a `dea_*` service key |
| The linked **Project** | **Products**, or the `projects` scope on a key |

When spend is withheld the reply says so and names the permission. Read that line: an answer
with no spend figure means *withheld*, not *nothing spent*. A `dea_*` key is excluded from the
Azure number on purpose — a key carries scope strings rather than roles, and no scope in the
vocabulary can either grant or withhold cost, so there is no way to authorize it per key.

## Linking to a Project

A POC can be linked to a Project (a `client_engagements` row — the thing an SOW is generated
against) via `engagementId` on either `create_poc` or `update_poc`, in the **same** organization.
Naming a Project from another client is refused, checked against the resolved Project rather
than the id you passed.
This is separate from `productId`, which names the IP product the POC is *of* when it is a POC of
an existing product — never the product it becomes on graduation (that's a separate graduate
flow, not part of `create_poc`/`update_poc`). Neither `productId` nor graduation is the delivery
Project.

## Where this POC came from, or led to

Call `get_lineage` with the `slug` (as `pocSlug`) to walk the funnel behind and ahead of it — the
opportunity or demo that converted into it, and the Project it delivered through if it graduated
with one linked. See `/donyati-projects` for the same walk starting from a Project.

## Examples

```
/donyati-poc Meridian "Capital Request Intake"
/donyati-poc — list every POC I can see that's still piloting
```

Update one already tracked:

```json
{ "slug": "capital-request-intake", "stage": "piloting", "clientSponsorEmail": "d.chen@meridian.example" }
```

Park one, with the reason the field requires:

```json
{ "slug": "capital-request-intake", "stage": "parked", "killedReason": "Sponsor moved to the ERP programme; revisit at FY close" }
```

## What stays in the admin console

There is no `delete_poc`, no un-graduate and no graduate flow on the connector, and the Budget
tab's cost mapping is web-only. Deleting or graduating a POC rewrites what it links to, so both
stay signed-in actions at https://expert-agents.donyati.com/admin/pocs.
