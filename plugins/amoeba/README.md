# Amoeba AI plugin for Claude

This plugin connects Claude to [Amoeba AI](https://app.amoebai.com) and ships two skills: one explains how an organization is modelled there, so Claude can answer questions about departments, teams, spaces and explorations without guessing, and one hands over a department's Executive Brief.

It contains:

- `.mcp.json`: the Amoeba connector, a remote MCP server that signs in with your Amoeba account through OAuth.
- `skills/amoeba`: how the hierarchy works, what the trend values mean, and which tool to call for which question.
- `skills/department-brief`: how to fetch a department's latest Executive Brief, present it, and turn it into text for Slack or a checklist. Amoeba writes the brief; the skill only reads it.

All tools are read-only. The plugin never writes to your Amoeba organization.

## Install

In Claude Code:

```bash
claude plugin marketplace add AmoebAI/amoeba-claude-plugin
claude plugin install amoeba@amoebai
```

In claude.ai, add this repository as a marketplace under Plugins, install **Amoeba AI**, then connect the Amoeba connector from the plugin's Connectors tab.
