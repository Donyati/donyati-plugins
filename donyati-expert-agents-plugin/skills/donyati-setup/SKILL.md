---
name: donyati-setup
description: First-time setup for the Donyati Expert Agents plugin — verifies your API key works, prints your identity, and offers to persist DONYATI_API_KEY in your shell rc
---

# /donyati-setup — First-Time Setup

Use this once when you receive your API key from Matt. It confirms your key works and shows you the scopes you have.

## Steps

1. **Get a key from Matt.** He generates them at `https://expert-agents.donyati.com/admin/api-keys` and sends them via Slack/Teams. Keys look like `dea_xxxxxxxxxxxx...`.

2. **Set the env var.** In your terminal (or `~/.zshrc` / `~/.bashrc`):
   ```
   export DONYATI_API_KEY="dea_paste_your_key_here"
   ```

3. **Verify the connection.** This skill calls the `whoami` MCP tool, which echoes back the key's identity.

## Usage

```
/donyati-setup
```

The skill will:
1. Check that `DONYATI_API_KEY` is set in the environment
2. Call the `whoami` tool from the `expert-agents` MCP server
3. Print your key name, owner email, scopes, and rate limit
4. Tell you what to do if it fails

## Troubleshooting

- **"DONYATI_API_KEY not set"** → run the `export` line above and restart Claude Code
- **"Unauthorized"** → the key is invalid or has been revoked; ask Matt for a new one
- **"Tool not found: whoami"** → the MCP server isn't connected; check that `expert-agents` is in your `claude mcp list`

## Manually adding the MCP server

If `expert-agents` isn't in your MCP list:

```
claude mcp add --transport http expert-agents \
  https://expert-agents.donyati.com/api/mcp \
  --header "Authorization: Bearer $DONYATI_API_KEY"
```
