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
| [`coast-mcp`](./plugins/coast-mcp) | Connects Claude to your Coast workspace and bundles guidance on the Coast data model. |

## Repository layout

```
.claude-plugin/marketplace.json   marketplace manifest ("coast")
plugins/<plugin-name>/            one directory per plugin
```

Each plugin is versioned independently. Releases are tagged `<plugin-name>--v<version>`, created with
`claude plugin tag --push` from the plugin directory.

## Contributing

Validate before opening a pull request:

```
claude plugin validate .
```
