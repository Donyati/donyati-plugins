---
name: donyati-assessment
description: Draft a client assessment from what has already been ingested for a project — questions aimed at what the discovery material does NOT answer — and check what became of the proposals already drafted: pushed, edited since, withdrawn, or waiting on a fix. Publishing stays with a consultant in the admin console.
---

# /donyati-assessment — Draft a client assessment, and track it

Turns a project's discovery material into a proposed assessment, and reports the state of the
ones already drafted. Calls the `propose_assessment` tool.

## Usage

```
/donyati-assessment <client> <project>              # what state are the proposals in?
/donyati-assessment <client> <project> draft        # draft a new one
```

## Why draft it from the corpus rather than sending a standard questionnaire

1. **It asks about the gaps.** Everything confirmed for the project, plus the extracted text of
   its documents, goes in; the draft is aimed at what that material does *not* establish. Facts
   the corpus already answers are restated as dimension context instead of being asked again,
   so the client is not asked to re-tell you what they sent you.
2. **Org-level documents count.** Material uploaded against the client rather than against one
   workstream — the RFP, the systems inventory, the org chart — is read too.
3. **It cannot publish.** What comes back is a **draft**. Nothing reaches a client until a
   consultant opens the project's *Assessment proposals* tab in the admin console and pushes
   it to DoEvolve. This tool has no push, and no withdraw.
4. **Scoring is checked before anything goes out.** DoEvolve scores multiple-choice answers by
   reading each option's score, so an option without one reports every respondent at the floor.
   A draft missing them comes back flagged rather than pushable.

## How it works

1. Resolve the client with `list_organizations` and the project with `list_projects`. Never
   guess an organization or project id.
2. **Status first.** Call `propose_assessment` with `list: true`:

   ```json
   { "organizationId": 36, "projectId": 71, "list": true }
   ```

   You get, per proposal: its id and status, whether a pushed one has been **edited since it
   was pushed** (the client is still answering the older version until someone pushes again),
   when it was pushed, its participant link, and any validation error still to fix.
3. **Draft** by calling it without `list`, optionally naming the topic areas — one dimension
   each:

   ```json
   {
     "organizationId": 36,
     "projectId": 71,
     "topicAreas": ["Close & Consolidation", "Reporting", "Data quality"]
   }
   ```

   Omit `topicAreas` and the draft picks 3–5 from the material.
4. Report the dimensions, the question counts and the sources back to the user, then tell them
   where to review it: the project's **Assessment proposals** tab.

## What to tell the user

- A draft is not live. Say so plainly; the tab is where it becomes client-facing.
- If a proposal comes back **differing from live**, that is not cosmetic: respondents are
  answering the older version, and only a re-push from the console changes that.
- If one comes back **withdrawn**, its participant link no longer accepts answers. Answers
  already given are kept in DoEvolve.
- A validation error names the field. Repairing it is done in the tab's editor, which can add
  and remove dimensions, questions and options; option values are frozen after a push, because
  answers are stored against them.

## Permissions

Listing needs **Clients (View)**. Drafting needs **Clients (Manage)** — Consultant, Sales,
Delivery Lead and Leadership hold Clients at View only, which is enough to check state and not
to draft. `dea_*` service keys are exempt from the role check; issuing the key was the
authorization.
