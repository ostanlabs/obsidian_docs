# Plugin Commands Reference

**Complete reference for all Canvas Project Manager plugin commands**

The plugin registers **7 commands**, available via the Command Palette (`Ctrl/Cmd+P`). Additional functionality (entity navigation, feature panels, Notion per-note sync) lives in context menus and workspace views — see [Beyond Commands](#beyond-commands-views-and-context-menus) below.

---

## Canvas Management

### Populate from Vault
**Command ID:** `populate-canvas-from-vault`
**Palette name:** Project canvas: populate from vault

Scans your vault for all entity files (Milestones, Stories, Tasks, Decisions, Documents, Features) and adds them to the current canvas.

**Use Cases:**

- Initial canvas setup
- Refreshing canvas after creating entities via MCP
- Rebuilding canvas after clearing

---

### Reposition Nodes
**Command ID:** `reposition-canvas-nodes`
**Palette name:** Project canvas: reposition nodes (v4 algorithm)

Auto-arranges canvas nodes using the V4 hierarchical layout algorithm. Layout rules (containment, sequencing, container ends, priorities) are derived from the active `schema.json` — see [Entity Schemas](entity-schemas.md#relationships-default-schema).

**Notes:**

- Writes a verbose positioning log to `positioning-verbose-<timestamp>.log` in the vault for debugging

**Use Cases:**

- After adding new entities
- After changing relationships
- Cleaning up manual repositioning

---

### Search Entity on Canvas
**Command ID:** `search-entity-on-canvas`
**Palette name:** Project canvas: search entity on canvas

Opens a search popup to find and focus on specific entities on the canvas.

**Features:**

- Search by ID or title
- Centers canvas on the selected entity

---

### Focus on In Progress
**Command ID:** `focus-in-progress`
**Palette name:** Project canvas: focus on in progress

Focuses the canvas on entities with status "In Progress".

**Use Cases:**

- Daily standup review
- Identifying active work

---

## Relationship Management

### Reconcile All Relationships
**Command ID:** `reconcile-relationships`
**Palette name:** Project canvas: reconcile all relationships

Synchronizes bidirectional relationships across the vault (forward/reverse pairs such as `parent` ↔ `children`, `depends_on` ↔ `blocks`, `implements` ↔ `implemented_by`).

**Use Cases:**

- After bulk entity creation
- After manual frontmatter edits
- Fixing relationship inconsistencies

**Notes:**

- Scans the entire vault and updates frontmatter in place
- The MCP server exposes the same operation as the [`reconcile_relationships`](mcp-tools-complete.md#reconcile_relationships) tool (with `dry_run` support)

---

## Notion Integration

### Sync Canvas Notes to Notion
**Command ID:** `sync-canvas-notes-to-notion`
**Palette name:** Project canvas: sync all canvas notes to Notion

Syncs all entities on the current canvas to the configured Notion database.

**Requirements:**

- Notion integration enabled in settings
- Notion integration token and database configured

**Notes:**

- Bidirectional sync polling starts automatically when the plugin loads (if enabled in settings) — there is no separate command for it
- Individual notes can be synced from their context menu

---

## Utility

### Regenerate Templates
**Command ID:** `regenerate-templates`
**Palette name:** Project canvas: regenerate templates

Recreates the default entity template files (existing templates are overwritten). Also available as a button in the plugin settings.

**Use Cases:**

- After template corruption
- Updating to a new template format
- Resetting customized templates

---

## Command Summary

| Command ID | Palette Name |
|------------|--------------|
| `populate-canvas-from-vault` | Project canvas: populate from vault |
| `reposition-canvas-nodes` | Project canvas: reposition nodes (v4 algorithm) |
| `search-entity-on-canvas` | Project canvas: search entity on canvas |
| `focus-in-progress` | Project canvas: focus on in progress |
| `reconcile-relationships` | Project canvas: reconcile all relationships |
| `sync-canvas-notes-to-notion` | Project canvas: sync all canvas notes to Notion |
| `regenerate-templates` | Project canvas: regenerate templates |

!!! note "In the Command Palette"
    Obsidian prefixes commands with the plugin name, so they appear as e.g. *"Canvas Project Manager: Project canvas: populate from vault"*. Hotkeys can be assigned in **Settings → Hotkeys**.

---

## Beyond Commands: Views and Context Menus

Much of the plugin's functionality is not a command:

### Workspace Views

- **Feature Details** — a right-sidebar panel showing details for the selected feature. Opened from an entity's context menu (*"Open feature details panel"*).
- **Feature Coverage** — a tab view for feature implementation/documentation coverage (see the [Feature Coverage guide](../user-guide/feature-coverage.md)).

### Context Menus

- **Canvas files** (right-click in the file explorer) — canvas-level actions
- **Entity notes** (right-click in the editor) — entity navigation (*Go to Features*, related entities) and Notion sync for the individual note
- **Canvas nodes** (right-click on a node) — node-level actions

### Automatic Behaviors

- Canvas node colors update when entity status changes (e.g., "In Progress" highlight)
- Deleting an entity's markdown file archives the corresponding Notion page (when sync is enabled)
- Notion bidirectional sync polling runs on the configured interval when enabled

---

## Best Practices

### ✅ DO

- **Use Populate** after creating entities via MCP
- **Use Reposition** after changing relationships
- **Use Reconcile** after bulk edits or manual frontmatter changes
- **Use Search** to quickly find entities on large canvases

### ❌ DON'T

- **Don't manually edit** relationship fields without running Reconcile afterwards
- **Don't create duplicate** entities (use Search to check first)

---

## Next Steps

- [Visual Canvas Guide](../user-guide/visual-canvas.md) - Learn canvas workflows
- [Entity Management](../user-guide/entity-management.md) - Entity creation and updates
- [Relationships](../user-guide/relationships.md) - Managing entity relationships
- [MCP Tools Reference](mcp-tools-complete.md) - AI-driven entity management
