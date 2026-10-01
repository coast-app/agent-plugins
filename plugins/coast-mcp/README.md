# coast-mcp

Connects Claude to your [Coast](https://coastapp.com) workspace over the Coast MCP server, with guidance
for finding data and supplying valid field values.

## Install

```
/plugin marketplace add coast-app/agent-plugins
/plugin install coast-mcp@coast
/reload-plugins
```

## What you get

**MCP server** — `https://mcp.coastapp.com/mcp`, registered as `coast`. Authentication happens through
your browser the first time Claude calls a Coast tool; no API key or configuration is needed.

**Skill** — `coast-mcp-basics` covers tool selection, identifiers, and field value shapes for entity
reads and writes. The companion [Coast Context](../coast-context/) plugin explains product concepts and modeling choices; [Coast Workspace Patterns](../coast-workspace-patterns/) supplies configuration recipes. Claude installs both as dependencies. See the [marketplace setup](../../README.md#install) for Codex installation.

## What Claude can do

Read and write across your workspace: query workspaces and entities, read workflow templates, create
and update entities, manage recurring entities, work with automations, view templates, dashboards,
and messaging.

Claude acts as you and sees only what your Coast account can see.
