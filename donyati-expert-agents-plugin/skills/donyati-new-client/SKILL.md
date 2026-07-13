---
name: donyati-new-client
description: Create a new client organization in the Expert Agents system so you can attach projects, knowledge, and documents to it.
---

# /donyati-new-client — Create a Client

Stand up a new client organization (a prospect or new account) without waiting for admin access.

## Usage

```
/donyati-new-client <company name>
```

## How it works

1. Checks `list_organizations` first so you don't create a duplicate.
2. Calls the `create_organization` tool with the name (and domain/industry if you provide them).
3. Reports the new `organizationId` and offers to create a project.

## Examples

```
/donyati-new-client Acme Manufacturing
/donyati-new-client Meridian Insurance — domain: meridian.com — industry: Insurance
```

## Next step

Follow with `/donyati-new-project <client> <project name>` to create an engagement, or `/donyati-upload` to add their documents.
