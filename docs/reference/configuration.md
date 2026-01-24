# Configuration

Configuration options for MCP server and plugin.

## MCP Server Configuration

```json
{
  "mcpServers": {
    "obsidian": {
      "command": "npx",
      "args": ["-y", "obsidian-accomplishments-mcp"],
      "env": {
        "VAULT_PATH": "/path/to/vault",
        "DEFAULT_CANVAS": "main.canvas"
      }
    }
  }
}
```

## Environment Variables

- `VAULT_PATH` - Absolute path to Obsidian vault
- `DEFAULT_CANVAS` - Default canvas file name

## Plugin Settings

Configure in Obsidian Settings → Canvas Project Manager.

---

See [Installation Guide](../getting-started/installation.md) for setup details.
