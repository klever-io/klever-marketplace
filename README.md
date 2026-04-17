# Klever Marketplace for Claude Code

The official [Claude Code](https://code.claude.com) plugin marketplace for Klever. Install Klever's plugins to connect Claude Code to on-chain data, smart contracts, and developer tooling in the Klever ecosystem.

## Quick start

From inside Claude Code, add the marketplace:

```
/plugin marketplace add klever-io/klever-marketplace
```

Then browse and install plugins:

```
/plugin
```

Or install a specific plugin directly:

```
/plugin install klever-vm@klever-marketplace
```

After installation, restart Claude Code (or run `/plugin` to enable) and the plugin's tools will appear in your session.

## Available plugins

| Plugin | Description | Install |
| :--- | :--- | :--- |
| [`klever-vm`](./plugins/klever-vm) | HTTP MCP server at `https://mcp.klever.org/mcp` that exposes Klever VM data and tooling to Claude | `/plugin install klever-vm@klever-marketplace` |

## CLI alternatives

All marketplace operations are also available from the terminal:

```bash
# Add the marketplace
claude plugin marketplace add klever-io/klever-marketplace

# Install a plugin
claude plugin install klever-vm@klever-marketplace

# List installed plugins
claude plugin list

# Update the marketplace catalog
claude plugin marketplace update klever-marketplace

# Uninstall a plugin
claude plugin uninstall klever-vm@klever-marketplace
```

You can also install the MCP server directly (without the plugin wrapper) using:

```bash
claude mcp add -t http klever-vm https://mcp.klever.org/mcp
```

Installing through the marketplace is recommended because it groups Klever tooling under a single catalog, tracks versions, and enables one-click updates.

## Repository layout

```
klever-marketplace/
├── .claude-plugin/
│   └── marketplace.json          # Marketplace catalog (lists all plugins)
├── plugins/
│   └── klever-vm/
│       ├── .claude-plugin/
│       │   └── plugin.json       # Plugin manifest
│       └── .mcp.json             # MCP server definition
└── README.md
```

- `.claude-plugin/marketplace.json` is the catalog Claude Code reads when users run `/plugin marketplace add`.
- Each plugin lives in `plugins/<plugin-name>/` with its own `.claude-plugin/plugin.json` manifest.
- Plugins that bundle an MCP server declare it in `.mcp.json` at the plugin root.

## Adding a new plugin

1. Create a new directory under `plugins/` (kebab-case name, no spaces).
2. Add `.claude-plugin/plugin.json` with at minimum a `name` and `version`.
3. Drop plugin components into the plugin root:
   - `.mcp.json` for MCP servers
   - `skills/<name>/SKILL.md` for skills
   - `commands/*.md` for slash commands
   - `agents/*.md` for subagents
   - `hooks/hooks.json` for lifecycle hooks
4. Register the plugin in `.claude-plugin/marketplace.json` by adding an entry to the `plugins` array with a relative `source` path.
5. Validate locally:
   ```bash
   claude plugin validate .
   ```
6. Test locally before publishing:
   ```
   /plugin marketplace add ./klever-marketplace
   /plugin install <your-plugin>@klever-marketplace
   ```

## Plugin manifest example

`plugins/<plugin-name>/.claude-plugin/plugin.json`:

```json
{
  "name": "plugin-name",
  "version": "1.0.0",
  "description": "What this plugin does",
  "author": { "name": "Klever" },
  "mcpServers": "./.mcp.json"
}
```

## MCP server configuration

For an HTTP MCP server, `plugins/<plugin-name>/.mcp.json`:

```json
{
  "mcpServers": {
    "server-id": {
      "type": "http",
      "url": "https://your-mcp-endpoint/mcp"
    }
  }
}
```

For a local command-based MCP server, use `command` and `args` instead of `type`/`url`. Use `${CLAUDE_PLUGIN_ROOT}` to reference files bundled with the plugin.

## References

- [Claude Code plugins](https://code.claude.com/docs/en/plugins)
- [Plugin marketplaces](https://code.claude.com/docs/en/plugin-marketplaces)
- [Plugins reference](https://code.claude.com/docs/en/plugins-reference)
- [MCP documentation](https://code.claude.com/docs/en/mcp)

## License

MIT
