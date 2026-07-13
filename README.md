# Donyati Plugins — Claude Code marketplace

Public distribution for the **Donyati Expert Agents** Claude Code plugin. This repo holds only the plugin manifest + slash-command definitions; all AI runs on the hosted service at https://expert-agents.donyati.com.

## Install (Claude Code)

```
/plugin marketplace add Donyati/donyati-plugins
/plugin install donyati-expert-agents@donyati-plugins
```

You need a Donyati API key (ask Matt) exported as `DONYATI_API_KEY`. Full setup, commands, and examples are in [`donyati-expert-agents-plugin/README.md`](donyati-expert-agents-plugin/README.md) and [`USAGE_GUIDE.md`](donyati-expert-agents-plugin/USAGE_GUIDE.md).

## On Claude Desktop / claude.ai web (no key, no install)

Add a custom connector pointing at `https://expert-agents.donyati.com/api/mcp` and sign in with your Donyati Microsoft 365 account — same `/donyati-*` commands, no API key. See the plugin README.

## Updates

```
/plugin marketplace update donyati-plugins
/plugin install donyati-expert-agents@donyati-plugins
```

*Source of truth is the internal `consulting-agents` repo; this mirror is refreshed on each plugin release.*
