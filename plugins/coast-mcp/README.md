# coast-mcp

Connects Claude to your [Coast](https://coastapp.com) workspace over the Coast MCP server, and bundles
a skill covering how Coast data is organised.

## Install

```
/plugin marketplace add coast-app/agent-plugins
/plugin install coast-mcp@coast
/reload-plugins
```

## What you get

**MCP server** — `https://mcp.coastapp.com/mcp`, registered as `coast`. Authentication happens through
your browser the first time Claude calls a Coast tool; no API key or configuration is needed.

**Skill** — `coast-mcp-basics` explains workspaces, workflow templates, workflow entities, and
components, and covers the field value shapes that most often cause failed writes.

## What Claude can do

Read and write across your workspace: query workspaces and entities, read workflow templates, create
and update entities, manage recurring entities, work with automations, view templates, dashboards,
and messaging.

Claude acts as you and sees only what your Coast account can see.
