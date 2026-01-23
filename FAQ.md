# Frequently Asked Questions

> **Common questions and troubleshooting for Obsidian AI Project Management**

---

## Table of Contents

1. [Installation & Setup](#installation--setup)
2. [MCP Server Issues](#mcp-server-issues)
3. [Plugin Issues](#plugin-issues)
4. [Entity Management](#entity-management)
5. [Canvas & Visualization](#canvas--visualization)
6. [Relationships & Dependencies](#relationships--dependencies)
7. [Performance & Limits](#performance--limits)
8. [Workflow Questions](#workflow-questions)

---

## Installation & Setup

### Q: Do I need both the MCP server and the plugin?

**A:** No, but they work best together.

- **MCP Server only:** You can manage entities via AI, but won't have visual canvas organization
- **Plugin only:** You can visualize entities on canvas, but won't have AI-assisted management
- **Both together:** Full AI-native project management with visual review (recommended)

### Q: Which AI assistants are supported?

**A:** Currently supported:
- **Claude Desktop** - Full support
- **Cursor IDE** - Full support

Coming soon:
- Cline
- Other MCP-compatible clients

### Q: Can I use this with an existing Obsidian vault?

**A:** Yes! The system creates its own folders (`milestones/`, `stories/`, etc.) and won't interfere with your existing notes.

### Q: What if I already have a `milestones/` folder?

**A:** The system will use the existing folder. Make sure existing files don't conflict with the entity ID format (M-xxx, S-xxx, etc.).

---

## MCP Server Issues

### Q: "MCP tools not available" in Claude/Cursor

**Solutions:**

1. **Restart the AI assistant** - Quit completely and reopen
2. **Check config file syntax** - Ensure valid JSON (no trailing commas)
3. **Verify Node.js** - Run `node --version` (should be 18+)
4. **Check VAULT_PATH** - Must be absolute path with forward slashes
5. **Check logs** - Look for errors in AI assistant's developer console

**Example correct config:**
```json
{
  "mcpServers": {
    "obsidian": {
      "command": "npx",
      "args": ["-y", "obsidian-accomplishments-mcp"],
      "env": {
        "VAULT_PATH": "/Users/john/Documents/MyVault",
        "DEFAULT_CANVAS": "main.canvas"
      }
    }
  }
}
```

### Q: "No entities found" when asking AI about project

**Solutions:**

1. **Check VAULT_PATH** - Verify it points to correct vault
2. **Check entity files exist** - Look in `milestones/`, `stories/`, `tasks/` folders
3. **Check file format** - Entities must have valid YAML frontmatter with `type` field
4. **Restart MCP server** - Restart AI assistant to reload index

### Q: AI creates entities but they don't appear in vault

**Solutions:**

1. **Check VAULT_PATH** - Ensure it's the correct absolute path
2. **Check permissions** - Ensure MCP server can write to vault directory
3. **Check for errors** - Ask AI "What was the last error?"
4. **Refresh Obsidian** - Close and reopen vault

---

## Plugin Issues

### Q: Plugin doesn't appear in Community Plugins

**A:** The plugin is currently under review. Install from source:

```bash
git clone https://github.com/ostanlabs/obsidian_plugin.git
cd obsidian_plugin
npm install && npm run build
cp main.js manifest.json styles.css /path/to/vault/.obsidian/plugins/canvas-project-manager/
```

Then enable in Obsidian Settings → Community Plugins.

### Q: "No active canvas found" error

**Solutions:**

1. **Create a canvas file** - Create `main.canvas` in your vault
2. **Open the canvas** - Make sure canvas file is open and focused
3. **Check file extension** - Must be `.canvas` not `.md`

### Q: Canvas is empty after "Populate from vault"

**Solutions:**

1. **Check entity files exist** - Look in `milestones/`, `stories/`, `tasks/` folders
2. **Check frontmatter** - Entities must have `type` field
3. **Check for archived entities** - Archived entities won't appear
4. **Check console** - Open Developer Console (Ctrl/Cmd+Shift+I) for errors
5. **Try "Reposition nodes"** - Run after populating

### Q: Nodes overlap or are positioned incorrectly

**Solutions:**

1. **Run "Reposition nodes"** - This applies the hierarchical layout algorithm
2. **Check parent relationships** - Ensure `parent` fields are set correctly
3. **Check workstreams** - Entities are organized by workstream lanes
4. **Manual adjustment** - You can drag nodes manually if needed

---

## Entity Management

### Q: What's the difference between a Story and a Task?

**A:**
- **Story (S-xxx):** User story or feature description. Contains multiple tasks. Parent is a Milestone.
- **Task (T-xxx):** Actionable work item. Parent is a Story.

Think: Milestone → Story → Task (3-tier hierarchy)

### Q: Can I create entities manually in Obsidian?

**A:** Yes, but it's easier to use AI:

**Manual creation:**
1. Create file in appropriate folder (e.g., `milestones/M-001.md`)
2. Add YAML frontmatter with required fields
3. Run "Populate from vault" to add to canvas

**AI creation (recommended):**
```
"Create a milestone called 'Q1 Launch' with target date March 31"
```

### Q: How do I delete an entity?

**A:** 
- **Via AI:** `"Delete M-001"` or `"Archive M-001"` (recommended)
- **Manually:** Delete the file from vault, then refresh canvas

**Note:** Archiving is preferred over deletion (moves to `archive/` folder).

### Q: Can I rename an entity ID?

**A:** Not recommended. Entity IDs are used in relationships. If you must:

1. Update the ID in the file's frontmatter
2. Rename the file to match
3. Update all references in other entities
4. Run "Reconcile relationships" command

Better: Create a new entity and archive the old one.

---

## Canvas & Visualization

### Q: How do I change the canvas layout?

**A:** The plugin uses an automatic hierarchical layout algorithm. To customize:

1. **Run "Reposition nodes"** - Applies automatic layout
2. **Manual adjustment** - Drag nodes to desired positions
3. **Workstream organization** - Entities are grouped by workstream

### Q: Can I have multiple canvases?

**A:** Yes! You can create multiple canvas files:

- `main.canvas` - Main project view
- `engineering.canvas` - Engineering workstream only
- `features.canvas` - Feature planning

Use "Populate from vault" on each canvas.

### Q: How do I hide certain entity types?

**A:** Use the visibility toggle buttons in the plugin toolbar:

- **M** - Toggle milestones
- **S** - Toggle stories
- **T** - Toggle tasks
- **De** - Toggle decisions
- **Do** - Toggle documents

### Q: Can I export the canvas as an image?

**A:** Use Obsidian's built-in canvas export:

1. Open canvas
2. Right-click → Export as image
3. Choose PNG or SVG format

---

## Relationships & Dependencies

### Q: What's the difference between `parent` and `depends_on`?

**A:**
- **parent:** Hierarchical relationship (Story belongs to Milestone)
- **depends_on:** Dependency relationship (Task A must complete before Task B)

Example:
```yaml
# T-002 is a child of S-001 and depends on T-001
id: T-002
parent: S-001        # Hierarchy: belongs to S-001
depends_on: [T-001]  # Dependency: needs T-001 to complete first
```

### Q: How do bidirectional relationships work?

**A:** The system automatically maintains both sides:

```
You set:  S-001.parent = M-001
System:   Automatically adds S-001 to M-001.children

You set:  T-001.depends_on = [T-002]
System:   Automatically adds T-001 to T-002.blocks
```

### Q: "Circular dependency detected" error

**A:** You're trying to create a dependency cycle:

```
Example cycle:
  A depends on B
  B depends on C
  C depends on A  ← This creates a cycle!
```

**Solution:** Review your dependencies and break the cycle.

### Q: How do I fix broken relationships?

**A:** Use the reconciliation command:

**Via AI:**
```
"Reconcile all relationships"
```

**Via Plugin:**
1. Open command palette (Ctrl/Cmd+P)
2. Run "Reconcile all relationships"

This fixes any inconsistent bidirectional relationships.

---

## Performance & Limits

### Q: How many entities can the system handle?

**A:** Tested with:
- ✅ 100 milestones
- ✅ 500 stories
- ✅ 2000 tasks
- ✅ 200 decisions/documents/features

Performance may degrade with very large canvases (>500 nodes).

### Q: The canvas is slow with many nodes

**Solutions:**

1. **Use multiple canvases** - Split by workstream or milestone
2. **Archive completed work** - Moves entities off canvas
3. **Filter by entity type** - Hide types you don't need to see
4. **Use AI for navigation** - Instead of visual browsing

### Q: File sync conflicts with Obsidian Sync

**A:** If using Obsidian Sync:

1. **Don't run MCP and edit manually simultaneously** - Choose one at a time
2. **Wait for sync** - Let Obsidian Sync complete before AI operations
3. **Use version history** - Obsidian Sync keeps versions if conflicts occur

---

## Workflow Questions

### Q: Should I use AI or the plugin to create entities?

**A:** **Use AI for creation, plugin for review:**

✅ **AI:** Fast bulk creation, maintains relationships automatically  
✅ **Plugin:** Visual review, navigation, manual adjustments

### Q: How do I collaborate with a team?

**A:** 
1. **Share vault** - Use Obsidian Sync, Git, or shared folder
2. **Each person configures MCP** - Point to same vault
3. **Coordinate changes** - Avoid simultaneous edits to same entities
4. **Use decisions** - Document team decisions in DEC-xxx entities

### Q: Can I integrate with other tools (Jira, GitHub, etc.)?

**A:** Not directly, but you can:

1. **Export data** - Entities are markdown files, easy to parse
2. **Use AI** - Ask AI to generate reports or summaries
3. **Custom scripts** - Write scripts to sync with other tools

### Q: How do I backup my project data?

**A:** Your data is just markdown files in your vault:

1. **Obsidian Sync** - Automatic cloud backup
2. **Git** - Version control for vault
3. **File backup** - Regular backups of vault folder
4. **Export** - Copy entity folders to backup location

---

## Still Need Help?

- **GitHub Issues:** [Report bugs](https://github.com/ostanlabs/obsidian_plugin/issues)
- **GitHub Discussions:** [Ask questions](https://github.com/ostanlabs/obsidian_plugin/discussions)
- **Documentation:** [Read the guides](./guides/)

---

**Last Updated:** January 2026

