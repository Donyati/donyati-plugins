---
name: donyati-client-agent
description: Ask a client's own published assistant (client agent) a question the way one of their users would — grounded on the client's documents only, framed for a persona such as end-user or administrator.
---

# /donyati-client-agent — Ask as a Client User

A **client agent** is a client's own assistant: one client, one expert platform, personas that
decide which of the client's uploaded documents may ground an answer. Admins set them up under
**Admin → Expert Agents → Client Agents**; published ones appear on the **Client** tab of the
agent gallery and at `/c/<slug>`. This command asks one exactly as the client's user would.

## Usage

```
/donyati-client-agent <client> — <question>
/donyati-client-agent MetroNet — How do I submit my Q3 forecast?
/donyati-client-agent MetroNet — How do I add a new Market? — persona: administrator
/donyati-client-agent MetroNet — What does Aggregate Markets do? — agent: metronet-planning-assistant
```

## How it works

1. Resolve the client by name with `list_organizations` and take its `organizationId`.
2. Call `consult` with `organizationId` and either
   - `clientAgent: <slug>` — the client's published agent (sets the platform from the agent and
     defaults `persona` to its first persona), or
   - `persona: <slug>` alone — the org's active agent for that platform is used (`end-user` and
     `administrator` are the built-in personas).
3. The answer is **documents-only**: nothing from Donyati's internal client facts or wiki, only
   documents whose audience includes the persona. Every source is a client document and is
   verified against what was retrieved.

`clientAgent` needs `organizationId` — a slug is unique per client, not globally. An unknown
slug or persona is refused rather than silently answered generically.

## What to tell the user

- Present the answer as the client user would see it and list the client documents it cited.
- If no client document backed the answer, say so plainly. That gap is logged for the client's
  admin (Client Agents → *Usage and unanswered questions*) — it is the list of what to document
  next.
- A Business Partner (end-user) persona is deflected from administrator tasks by design; switch
  `persona: administrator` to see the full procedure.

Also available as the `/donyati-client-agent` prompt in Claude Desktop and claude.ai.
