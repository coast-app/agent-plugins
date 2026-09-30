# Coast agent plugins

Plugins for connecting AI agents to [Coast](https://coastapp.com).

## Install

```
/plugin marketplace add coast-app/agent-plugins
/plugin install coast-mcp@coast
/reload-plugins
```

Then authenticate when Claude first calls a Coast tool.

## Plugins

| Plugin | Description |
| --- | --- |
| [`coast-mcp`](./plugins/coast-mcp) | Connects your agent to your Coast workspace. In Claude Code, pulls in `coast-context` and `coast-workspace-patterns`. |
| [`coast-context`](./plugins/coast-context) | Coast product domain and terminology. |

## Codex

The same plugins work with OpenAI Codex. Install all three explicitly:

```
codex plugin marketplace add https://github.com/coast-app/agent-plugins.git
codex plugin add coast-mcp@coast
codex plugin add coast-context@coast
codex plugin add coast-workspace-patterns@coast
```

Start a new Codex session after installing.

## Repository layout

```
.claude-plugin/marketplace.json   Claude Code marketplace manifest ("coast")
.agents/plugins/marketplace.json  Codex marketplace manifest
plugins/<plugin-name>/
  .claude-plugin/plugin.json      Claude Code manifest
  .codex-plugin/plugin.json       Codex manifest
  skills/<skill>/SKILL.md         skill body
  skills/<skill>/agents/openai.yaml   Codex skill interface
```

Each plugin is versioned independently. Releases are tagged `<plugin-name>--v<version>`, created with
`claude plugin tag --push` from the plugin directory.

## Contributing

Validate before opening a pull request:

```
claude plugin validate .
```
