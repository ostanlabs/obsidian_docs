# Configuration

Configuration options for the MCP server and the Canvas Project Manager plugin.

---

## MCP Server Configuration

The MCP server is the bundled `bin/mcp-server.mjs` from the plugin repository. It uses **stdio** transport and requires a single environment variable: `VAULT_PATH`.

### Claude Code

```bash
claude mcp add obsidian-mcp \
  --env VAULT_PATH=/path/to/your/vault \
  -- node /path/to/obsidian_plugin/bin/mcp-server.mjs
```

### Claude Desktop / generic MCP client

```json
{
  "mcpServers": {
    "obsidian-mcp": {
      "command": "node",
      "args": ["/path/to/obsidian_plugin/bin/mcp-server.mjs"],
      "env": {
        "VAULT_PATH": "/path/to/your/vault"
      }
    }
  }
}
```

### Environment Variables

| Variable | Required | Description |
|----------|----------|-------------|
| `VAULT_PATH` | ✅ | Absolute path to the Obsidian vault. The server refuses to start without it. |
| `DEBUG` | ❌ | Set to enable verbose diagnostic logging. |

---

## `schema.json` — the Vault's Runtime Schema

The active schema lives at **`<vault>/schema.json`** and is the single source of truth for both MCP validation and plugin canvas positioning.

- **Bootstrapped automatically:** on the MCP server's first run against a vault, `schema.json` is written from the built-in default schema.
- **Edited via tools:** use [`set_schema`](mcp-tools-complete.md#set_schema) (validated; invalid schemas are rejected and not saved) or the [`get_schema_designer`](mcp-tools-complete.md#get_schema_designer) HTML designer. Changes hot-reload without a restart.
- **Fallback:** if `schema.json` is invalid, the server falls back to the default schema and surfaces the validation errors via [`get_schema`](mcp-tools-complete.md#get_schema).

See [Entity Schemas](entity-schemas.md) for the default schema's contents.

---

## Plugin Settings

Configure in **Obsidian Settings → Canvas Project Manager**.

### Basics

| Setting | Description |
|---------|-------------|
| Notes base folder | Default folder where generated notes are saved |
| Infer base folder from canvas location | Create notes in the same folder as the canvas file, ignoring the base folder |

### Templates

| Setting | Description |
|---------|-------------|
| Use template folder | Scan a folder for templates and choose from multiple options |
| Template folder | Folder containing template files (shown when the toggle above is enabled) |
| Regenerate default templates | Button — recreates the default template files (overwrites existing) |

### ID Generation

| Setting | Description |
|---------|-------------|
| ID zero-padding length | Number of digits for IDs (e.g., 3 for `001`) |

### Effort Avenues

| Setting | Description |
|---------|-------------|
| Effort options | One effort avenue per line |
| Default effort | Effort avenue to pre-select |

### Canvas Display

| Setting | Description |
|---------|-------------|
| Show ID on cards | Include the item ID in card display |
| Effort color map | One mapping per line: `effort:colorid` (Obsidian color index or hex) |
| In progress color | Obsidian color index for "in progress" nodes (1=red, 2=orange, 3=yellow, 4=green, 5=cyan, 6=purple) |

### Notion Integration

| Setting | Description |
|---------|-------------|
| Enable Notion sync | Master toggle for Notion syncing (reveals the settings below) |
| Integration token | Notion internal integration token |
| Parent page ID | Notion page ID where the database will be created |
| Database name | Name for the Notion database |
| Database ID | Notion database ID (filled after initialization) |
| Initialize Notion database | Button — creates a new database in Notion with the required schema |
| Sync on note creation | Automatically sync when a new note is created |
| Sync on demand only | Disable automatic sync; only sync via manual command |
| Auto-sync on md file change | Sync when a markdown file is modified (2s debounce) |

### Entity Navigator

| Setting | Description |
|---------|-------------|
| Show dataview warning | Warn if the Dataview plugin is not installed |
| Open behavior | How to open multiple related files: new tabs, horizontal split, or vertical split |
| Milestones / Stories / Tasks / Decisions / Documents / Features folder | Folder path per entity type |

### Developer

| Setting | Description |
|---------|-------------|
| Debug mode | Print verbose `console.debug`/`console.log` output to the developer console |

---

See the [Installation Guide](../getting-started/installation.md) for setup details.
