# Claude Desktop plugin marketplace

A vendored copy of three plugins from `anthropics/claude-plugins-official`, published as a
marketplace Claude Desktop can install from.

## Why this exists

Claude Desktop's third-party (3P) mode refuses any marketplace plugin whose `source` points at a
different repository. In the official manifest, `superpowers`, `browser-use`, and
`chrome-devtools-mcp` all point at external repos, so Desktop logs them as

```
262 external-source plugin(s) excluded from host install
```

and never lists them. Copying the plugin contents into this repo and pointing each entry at a
relative path satisfies that rule.

## Layout

```
.claude-plugin/marketplace.json
plugins/
  superpowers/          # skills, hooks, agents
  browser-use/          # .mcp.json (stdio, uvx)
  chrome-devtools-mcp/  # mcpServers in plugin.json (stdio, npx)
```

## Setup

Add to the Desktop 3P configuration under `plugins.marketplaces`:

```json
{ "source": "github", "repo": "mkhanplative/claude-desktop-plugins", "credentialKind": "userGit", "installationPreference": "available" }
```

`credentialKind: "userGit"` is required while this repo is private; it uses the git credential
helper already configured for the macOS user. Then install the plugins from
**Customize → Plugins → Discover → Organization**.

## Updating

Plugins are copied verbatim from `~/.claude/plugins/cache/claude-plugins-official/<name>/<version>/`.
To update, copy the new version over the old one, bump `version` in the plugin's
`.claude-plugin/plugin.json` if it isn't already current, and push. Devices re-sync on their next
launch or at the periodic re-fetch, about every ten minutes.

`browser-use/.mcp.json` is rewritten to the `{"mcpServers": {...}}` wrapper. The upstream copy
keys the server name at the top level.
