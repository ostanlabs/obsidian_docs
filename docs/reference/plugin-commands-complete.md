# Plugin Commands Reference

**Complete reference for all Canvas Project Manager plugin commands**

Access commands via Command Palette (`Ctrl/Cmd+P`) or assigned hotkeys.

---

## Canvas Management

### Populate from Vault
**Command ID:** `populate-canvas-from-vault`  
**Name:** Project Canvas: Populate from vault

Scans your vault for all entity files (Milestones, Stories, Tasks, Decisions, Documents, Features) and adds them to the current canvas.

**Use Cases:**
- Initial canvas setup
- Refreshing canvas after creating entities via MCP
- Rebuilding canvas after clearing

**Notes:**
- Skips duplicate nodes automatically
- Respects entity type visibility toggles
- Excludes archived entities

---

### Reposition Nodes
**Command ID:** `reposition-canvas-nodes`  
**Name:** Project Canvas: Reposition nodes (V4 algorithm)

Auto-arranges canvas nodes using hierarchical layout algorithm.

**Layout Rules:**
- Milestones centered horizontally
- Stories positioned below their parent milestone
- Tasks fan out below their parent story
- Entities grouped by workstream (vertical lanes)
- Dependencies shown as edges

**Use Cases:**
- After adding new entities
- After changing relationships
- Cleaning up manual repositioning

---

### Search Entity on Canvas
**Command ID:** `search-entity-on-canvas`  
**Name:** Project Canvas: Search Entity on Canvas  
**Hotkey:** `Cmd/Ctrl+Shift+F`

Opens search popup to find and focus on specific entities.

**Features:**
- Fuzzy search by ID or title
- Highlights matching entity
- Centers canvas on selected entity
- Shows entity type and status

---

### Focus on In Progress
**Command ID:** `focus-in-progress`  
**Name:** Project Canvas: Focus on In Progress

Filters canvas to show only entities with status "In Progress".

**Use Cases:**
- Daily standup review
- Sprint planning
- Identifying active work

**Notes:**
- Hides all other entities temporarily
- Use "Populate from vault" to restore full view

---

## Relationship Management

### Reconcile All Relationships
**Command ID:** `reconcile-relationships`  
**Name:** Project Canvas: Reconcile All Relationships

Synchronizes all bidirectional relationships across the vault.

**Synced Relationships:**
- `implements` ↔ `implemented_by`
- `documents` ↔ `documented_by`
- `affects` ↔ `decided_by`
- `depends_on` ↔ `blocks`

**Use Cases:**
- After bulk entity creation
- After manual frontmatter edits
- Fixing relationship inconsistencies

**Notes:**
- Scans entire vault
- Updates frontmatter in-place
- Shows summary of changes

---

## Feature Management

### Create Feature
**Command ID:** `create-feature`  
**Name:** Project Canvas: Create Feature

Opens modal to create a new Feature entity.

**Fields:**
- Title (required)
- User Story (required)
- Tier: OSS or Premium
- Phase: MVP, 0, 1, 2, 3, 4, 5
- Status: Planned, In Progress, Complete, Deferred
- Workstream

---

### Create Features Canvas
**Command ID:** `create-features-canvas`  
**Name:** Project Canvas: Create Features Canvas

Creates a new canvas with tier/phase grid layout for features.

**Grid Layout:**
```
         MVP    Phase 0   Phase 1   Phase 2   Phase 3
OSS      [ ]      [ ]       [ ]       [ ]       [ ]
Premium  [ ]      [ ]       [ ]       [ ]       [ ]
```

---

### Auto-Layout Features
**Command ID:** `auto-layout-features`  
**Name:** Project Canvas: Auto-Layout Features

Positions feature entities on canvas by tier and phase.

**Use Cases:**
- After creating features canvas
- After adding new features
- Reorganizing feature roadmap

---

### Populate Features Canvas
**Command ID:** `populate-features-canvas`  
**Name:** Project Canvas: Populate Features Canvas

Adds all feature entities from vault to the current canvas.

---

### Set Feature Phase
**Command ID:** `set-feature-phase`  
**Name:** Project Canvas: Set Feature Phase

Updates the phase of a feature entity.

**Phases:** MVP, 0, 1, 2, 3, 4, 5

---

### Set Feature Tier
**Command ID:** `set-feature-tier`  
**Name:** Project Canvas: Set Feature Tier

Updates the tier of a feature entity.

**Tiers:** OSS, Premium

---

### Import Future Features
**Command ID:** `import-future-features`  
**Name:** Project Canvas: Import Future Features

Parses `FUTURE_FEATURES.md` and creates feature entities.

**Expected Format:**
```markdown
## Feature Name
- Tier: OSS
- Phase: MVP
- User Story: As a... I want... so that...
```

---

### Suggest Feature Links
**Command ID:** `suggest-feature-links`  
**Name:** Project Canvas: Suggest Feature Links

Finds matching milestones/stories for features based on title similarity.

---

### Bulk Link Features
**Command ID:** `bulk-link-features`
**Name:** Project Canvas: Bulk Link Features

Batch relationship management for features.

---

## Entity Navigation

### Navigate to Parent
**Command ID:** `navigate-to-parent`
**Name:** Project Canvas: Navigate to Parent

Opens the parent entity of the currently selected entity.

**Applies to:** Stories (→ Milestone), Tasks (→ Story)

---

### Navigate to Children
**Command ID:** `navigate-to-children`
**Name:** Project Canvas: Navigate to Children

Shows all child entities of the currently selected entity.

**Applies to:** Milestones (→ Stories), Stories (→ Tasks)

---

### Navigate to Dependencies
**Command ID:** `navigate-to-dependencies`
**Name:** Project Canvas: Navigate to Dependencies

Shows all entities this entity depends on (`depends_on` field).

---

### Navigate to Dependents
**Command ID:** `navigate-to-dependents`
**Name:** Project Canvas: Navigate to Dependents

Shows all entities that depend on this entity (`blocks` field).

---

### Navigate to Documents
**Command ID:** `navigate-to-documents`
**Name:** Project Canvas: Navigate to Documents

Shows all documents related to this entity.

**Relationships:**
- `documented_by` field (entity → documents)
- `documents` field (document → entities)

---

### Navigate to Decisions
**Command ID:** `navigate-to-decisions`
**Name:** Project Canvas: Navigate to Decisions

Shows all decisions related to this entity.

**Relationships:**
- `decided_by` field (entity → decisions)
- `affects` field (decision → entities)

---

### Navigate to Features
**Command ID:** `navigate-to-features`
**Name:** Project Canvas: Navigate to Features

Shows all features related to this entity.

**Relationships:**
- `implements` field (entity → features)
- `implemented_by` field (feature → entities)

---

### Navigate to Implemented By
**Command ID:** `navigate-to-implemented-by`
**Name:** Project Canvas: Navigate to Implemented By

Shows all entities that implement this feature.

**Applies to:** Features only

---

## Notion Integration

### Sync Canvas Notes to Notion
**Command ID:** `sync-canvas-notes-to-notion`
**Name:** Project Canvas: Sync all canvas notes to Notion

Syncs all entities on the current canvas to Notion database.

**Requirements:**
- Notion integration enabled in settings
- Notion integration token configured
- Notion database ID configured

---

### Sync Note to Notion
**Command ID:** `sync-note-to-notion`
**Name:** Project Canvas: Sync note to Notion

Syncs the currently open entity to Notion.

---

### Start Notion Sync Polling
**Command ID:** `start-notion-sync-polling`
**Name:** Project Canvas: Start Notion Sync Polling

Enables automatic bidirectional sync with Notion.

**Sync Interval:** Configurable in settings (default: 5 minutes)

---

## Utility Commands

### Regenerate Templates
**Command ID:** `regenerate-templates`
**Name:** Project Canvas: Regenerate templates

Recreates all entity templates in the `.templates/` folder.

**Use Cases:**
- After template corruption
- Updating to new template format
- Resetting customized templates

---

### Start HTTP Server
**Command ID:** `start-http-server`
**Name:** Project Canvas: Start HTTP Server

Starts HTTP server for external tool integration.

**Default Port:** 12312 (configurable in settings)

**Endpoints:**
- `GET /entities` - List all entities
- `GET /entities/:id` - Get specific entity
- `POST /entities` - Create entity
- `PUT /entities/:id` - Update entity

---

### Stop HTTP Server
**Command ID:** `stop-http-server`
**Name:** Project Canvas: Stop HTTP Server

Stops the HTTP server.

---

### Open Feature Coverage View
**Command ID:** `open-feature-coverage`
**Name:** Project Canvas: Open Feature Coverage

Opens the Feature Coverage sidebar view.

**Shows:**
- All features with implementation status
- Test coverage status
- Documentation coverage status
- Gap analysis (missing impl/docs/tests)

**Filters:**
- By tier (OSS/Premium)
- By phase (MVP/0-5)
- By status (Planned/In Progress/Complete)
- By gaps (missing impl/docs/tests)

---

## Command Summary

| Category | Count | Commands |
|----------|-------|----------|
| **Canvas Management** | 4 | Populate, Reposition, Search, Focus |
| **Relationship Management** | 1 | Reconcile |
| **Feature Management** | 9 | Create, Canvas, Layout, Populate, Phase, Tier, Import, Suggest, Bulk Link |
| **Entity Navigation** | 8 | Parent, Children, Dependencies, Dependents, Documents, Decisions, Features, Implemented By |
| **Notion Integration** | 3 | Sync Canvas, Sync Note, Start Polling |
| **Utility** | 4 | Regenerate Templates, Start HTTP, Stop HTTP, Feature Coverage |
| **TOTAL** | **29** | |

---

## Keyboard Shortcuts

| Shortcut | Command | Description |
|----------|---------|-------------|
| `Cmd/Ctrl+Shift+F` | Search Entity on Canvas | Find and focus entity |

**Note:** Additional shortcuts can be configured in Obsidian Settings → Hotkeys → Canvas Project Manager

---

## Context Menu Commands

Right-click on entities (canvas nodes or markdown files) to access:

- **Navigate to...** - Parent, Children, Dependencies, etc.
- **Sync to Notion** - Sync individual entity
- **Set Feature Phase/Tier** - Update feature classification
- **Link to Feature** - Create feature relationship

---

## Best Practices

### ✅ DO

- **Use Populate** after creating entities via MCP
- **Use Reposition** after changing relationships
- **Use Reconcile** after bulk edits
- **Use Feature Coverage** to track implementation gaps
- **Use Search** to quickly find entities on large canvases

### ❌ DON'T

- **Don't manually edit** relationship fields without running Reconcile
- **Don't delete** entities from canvas (archive them instead)
- **Don't create duplicate** entities (use Search to check first)

---

## Next Steps

- [Visual Canvas Guide](../user-guide/visual-canvas.md) - Learn canvas workflows
- [Entity Management](../user-guide/entity-management.md) - Entity creation and updates
- [Relationships](../user-guide/relationships.md) - Managing entity relationships
- [MCP Tools Reference](mcp-tools.md) - AI-driven entity management

