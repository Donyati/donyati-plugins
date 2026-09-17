---
name: donyati-interactive
description: Generate an interactive HTML deliverable for a client — process swimlane, technology roadmap, or architecture diagram — authored as a JSON spec in conversation and rendered by Expert Agents, returned as viewer/download/share links.
---

# /donyati-interactive — Interactive HTML Deliverables

Turn engagement content into an interactive, self-contained HTML deliverable
(an IEEE-style swimlane, a technology roadmap, or an architecture diagram) —
something a client can click through, not a static slide.

## Usage

```
/donyati-interactive [client] [swimlane|roadmap|architecture-diagram]
```

## How it works — the authoring flow

This is authoring, not form-filling. Gather real content before you draft
anything, and confirm the draft before you generate.

1. **Resolve the client.** `list_organizations` (and `list_projects` if this
   is engagement-scoped).
2. **Get the spec guide.** Call `list_deliverables` with the slug
   (`swimlane`, `roadmap`, or `architecture-diagram`) — it returns the
   field-by-field authoring guide and a worked example spec. Read it before
   drafting; the field names and nesting differ by slug.
3. **Gather real content.** Pull from the engagement, not from memory:
   - `search_client_knowledge` for process steps, systems, pains, decisions
     already captured for this client.
   - `get_client_briefing` for the engagement-level summary and context.
   - Documents the user pastes or uploads in this conversation.
   Every step, system, pain, gap, or milestone in the spec must trace back to
   one of these sources. **Never invent process steps, systems, or pains** —
   if the content isn't there, ask the user or say what's missing instead of
   filling the gap yourself.
4. **Draft the spec.** Build the JSON spec per the guide from Step 2, citing
   the real source IDs (workshop, workbook, interview) you pulled the content
   from wherever the schema has a place for a citation (e.g. pain/gap
   tooltips).
5. **Show a compact summary and confirm.** Before generating, show the user
   what the deliverable will contain — lane and step names for a swimlane,
   phases and milestones for a roadmap, nodes and connections for an
   architecture diagram — and get a go-ahead. This is a client-facing
   artifact; don't generate on a half-built spec.
6. **Generate.** Call `generate_deliverable` with the slug, organizationId,
   and `spec`. Return the links: the in-app viewer, the self-contained HTML
   download (attach-and-email to the client), and — only if the user asks to
   share externally — pass `create_share_link: true` for a revocable,
   30-day-expiring public link.
7. **To revise:** edit the spec and call `generate_deliverable` again with
   the `artifactId` the first call returned — it re-renders the same
   deliverable in place rather than creating a duplicate.

## Sharing and revoking

- **One deliverable, one link.** `create_share_link: true` on `generate_deliverable` (step 6) mints
  a public link for that single artifact. To pull it — the client's access ended, the content is
  stale, whatever the reason — call `revoke_share_links` with `organizationId` and `artifactId`
  (from the `generate_deliverable` reply). Pass `linkId` to revoke one specific link; omit it to
  revoke every active link for that artifact at once. Revoked links 404 immediately.
- **A client's whole set, one link.** `create_collection_link` mints a single URL listing every
  **published** interactive deliverable for a client, or for one project when `projectId` is
  given — swimlane, roadmap and architecture-diagram together, no per-artifact link management.
  Unpublished drafts never appear on it, and newly published deliverables show up on the same
  link automatically. Default lifetime is 90 days; pass `ttl_days` or `"never"`. Only create one
  when the user explicitly asks to share — it is not a byproduct of generating a deliverable.
  `revoke_collection_link` takes the same `organizationId`/`projectId` scoping, plus an optional
  `linkId` to revoke one instead of every active collection link in scope.

## Rules

- **Spec fields are plain text — no HTML or markup in any field.** Markup you
  put in a spec string is escaped by the renderer and shows up as literal
  visible tags in the output, not formatting. Write plain sentences.
- **Never invent process steps, systems, gaps, or pains.** Cite real source
  IDs. If the engagement knowledge doesn't cover something the deliverable
  needs, say so — don't fabricate to fill the shape.
- Client data stays in its organization — never mix content across clients,
  and never carry a spec drafted for one org into another's `generate_deliverable`
  call.

## Examples

```
/donyati-interactive Acme swimlane
/donyati-interactive Meridian roadmap
```
