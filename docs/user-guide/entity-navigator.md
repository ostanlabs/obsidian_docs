# Entity Navigator

**Navigate entity relationships through an in-memory index**

---

## Overview

The Entity Navigator provides **fast relationship traversal** across your project entities using an in-memory index. It enables quick navigation between related entities through context menus, keyboard shortcuts, and programmatic access.

**Key Features:**
- **In-Memory Index** - Fast lookups without file system access
- **Bidirectional Navigation** - Parent/children, dependencies, implementations
- **Context Menu Integration** - Right-click to navigate
- **Keyboard Shortcuts** - Quick access to common relationships
- **Canvas Integration** - Navigate from canvas nodes

---

## Entity Index

The Entity Navigator maintains an **in-memory index** of all entities in your vault.

### Index Structure

Each entity is indexed with:

```typescript
{
  id: "S-015",
  type: "story",
  file: TFile,
  title: "User Authentication",
  parent: "M-001",
  depends_on: ["S-012", "S-013"],
  implements: ["F-001"],
  documents: [],
  affects: [],
  blocks: ["S-016", "S-017"],
  implemented_by: [],
  documented_by: [],
  decided_by: [],
  tier: "core",
  phase: "mvp"
}
```

### Index Building

The index is built automatically when the plugin loads:

1. **Scan vault** - Find all markdown files
2. **Parse frontmatter** - Extract entity metadata
3. **Build index** - Create in-memory map
4. **Watch changes** - Update index on file changes

**Performance:**
- **Initial build:** ~100ms for 500 entities
- **Updates:** ~5ms per file change
- **Lookups:** <1ms per query

---

## Navigation Methods

### 1. **Hierarchy Navigation**

Navigate parent-child relationships.

#### **Navigate to Parent**

**Keyboard:** `Cmd/Ctrl+Shift+P`  
**Context Menu:** Right-click → "Go to Parent"

**Example:**
```
Current: S-015 (Story)
Parent: M-001 (Milestone)
```

Opens the parent milestone file.

---

#### **Navigate to Children**

**Keyboard:** `Cmd/Ctrl+Shift+C`  
**Context Menu:** Right-click → "Go to Children"

**Example:**
```
Current: M-001 (Milestone)
Children:
  - S-015: User Authentication
  - S-016: API Integration
  - S-017: Testing
```

Opens a modal with clickable list of children.

---

### 2. **Dependency Navigation**

Navigate dependency relationships.

#### **Navigate to Dependencies**

**Context Menu:** Right-click → "Go to Dependencies"

**Example:**
```
Current: S-015 (Story)
Dependencies (depends_on):
  - S-012: Database Setup
  - S-013: User Model
```

Opens a modal with entities this one depends on.

---

#### **Navigate to Dependents**

**Context Menu:** Right-click → "Go to Dependents"

**Example:**
```
Current: S-015 (Story)
Dependents (blocks):
  - S-016: API Integration
  - S-017: Testing
```

Opens a modal with entities that depend on this one.

---

### 3. **Implementation Navigation**

Navigate feature implementation relationships.

#### **Navigate to Implemented Features**

**Context Menu:** Right-click → "Go to Features"

**Example:**
```
Current: S-015 (Story)
Implements:
  - F-001: Authentication System
  - F-002: User Management
```

Opens a modal with features this entity implements.

---

#### **Navigate to Implementors**

**Context Menu:** Right-click → "Go to Implemented By"

**Example:**
```
Current: F-001 (Feature)
Implemented By:
  - S-015: User Authentication
  - S-016: API Integration
  - M-001: Q1 Release
```

Opens a modal with entities implementing this feature.

---

### 4. **Documentation Navigation**

Navigate documentation relationships.

#### **Navigate to Documents**

**Keyboard:** `Cmd/Ctrl+Shift+D`  
**Context Menu:** Right-click → "Go to Documents"

**Example:**
```
Current: F-001 (Feature)
Documented By:
  - DOC-001: Auth Spec
  - DOC-002: API Design
```

Opens a modal with documents describing this entity.

---

### 5. **Decision Navigation**

Navigate decision relationships.

#### **Navigate to Decisions**

**Keyboard:** `Cmd/Ctrl+Shift+E`  
**Context Menu:** Right-click → "Go to Decisions"

**Example:**
```
Current: S-015 (Story)
Related Decisions:
  - DEC-001: Use JWT for Auth
  - DEC-002: OAuth Provider Choice
```

Opens a modal with decisions affecting this entity.

---

## Context Menu Integration

Right-click on any entity (file or canvas node) to access navigation options.

### **On Markdown Files**

1. Right-click on entity file in file explorer
2. See "Entity Navigator" submenu
3. Click navigation option

**Menu Structure:**
```
Entity Navigator
├── Go to Parent: M-001 (Milestone)
├── Go to Children (3)
├── Go to Dependencies (2)
├── Go to Dependents (5)
├── Go to Features (1)
├── Go to Documents (2)
└── Go to Decisions (1)
```

**Disabled items** appear grayed out when no relationships exist.

---

### **On Canvas Nodes**

1. Right-click on entity node on canvas
2. See "Navigate to..." options
3. Click to open related entities

**Integration:**
- Opens files in new pane
- Maintains canvas focus
- Updates index automatically

---

## Keyboard Shortcuts

### **Default Shortcuts**

| Shortcut | Command | Description |
|----------|---------|-------------|
| `Cmd/Ctrl+Shift+P` | Navigate to Parent | Open parent entity |
| `Cmd/Ctrl+Shift+C` | Navigate to Children | Show children modal |
| `Cmd/Ctrl+Shift+D` | Navigate to Documents | Show documents modal |
| `Cmd/Ctrl+Shift+E` | Navigate to Decisions | Show decisions modal |

**Note:** Shortcuts work when an entity file is active.

---

### **Customizing Shortcuts**

1. Open Obsidian Settings
2. Go to Hotkeys
3. Search for "Canvas Project Manager"
4. Find "Navigate to..." commands
5. Set custom shortcuts

---

## Navigation Modals

When navigating to multiple entities, a modal appears with a clickable list.

### **Modal Features**

**List Display:**
```
Navigate to Children (3)

✓ S-015: User Authentication
  Status: in-progress
  Workstream: engineering

✓ S-016: API Integration
  Status: not-started
  Workstream: engineering

✓ S-017: Testing
  Status: not-started
  Workstream: qa
```

**Interactions:**
- **Click** - Open entity file
- **Cmd/Ctrl+Click** - Open in new pane
- **Hover** - Show full title
- **ESC** - Close modal

---

## Canvas Integration

The Entity Navigator integrates seamlessly with canvas visualization.

### **Navigate from Canvas**

1. Right-click on canvas node
2. Select "Navigate to..."
3. Related entities open in sidebar

**Example Workflow:**
```
1. Open project canvas
2. Right-click on Story node
3. Select "Go to Parent"
4. Milestone opens in sidebar
5. Canvas remains focused
```

---

### **Visual Feedback**

When navigating from canvas:
- **Highlighted nodes** - Related entities glow
- **Edge highlighting** - Relationship arrows emphasized
- **Focus preservation** - Canvas stays in view

---

## Common Workflows

### 1. **Explore Project Hierarchy**

**Scenario:** Understand project structure.

**Steps:**
1. Open top-level milestone
2. Navigate to children (stories)
3. Navigate to story children (tasks)
4. Navigate back to parent

**Keyboard Flow:**
```
M-001 (open)
  ↓ Cmd+Shift+C
S-015 (select from modal)
  ↓ Cmd+Shift+C
T-042 (select from modal)
  ↓ Cmd+Shift+P
S-015 (back to parent)
```

---

### 2. **Trace Dependencies**

**Scenario:** Find what blocks a story.

**Steps:**
1. Open blocked story
2. Navigate to dependencies
3. Check dependency status
4. Navigate to dependents to see impact

**Example:**
```
S-015: User Authentication (blocked)
  ↓ Navigate to Dependencies
S-012: Database Setup (in-progress)
  ↓ Navigate to Dependents
S-015, S-016, S-017 (all blocked)
```

---

### 3. **Review Feature Implementation**

**Scenario:** Check feature progress.

**Steps:**
1. Open feature file
2. Navigate to implementors
3. Review implementation status
4. Navigate to documents for specs

**Example:**
```
F-001: Authentication System
  ↓ Navigate to Implemented By
S-015: User Authentication (in-progress)
S-016: API Integration (not-started)
  ↓ Navigate to Documents
DOC-001: Auth Spec (complete)
```

---

### 4. **Find Related Decisions**

**Scenario:** Understand technical decisions.

**Steps:**
1. Open story or feature
2. Navigate to decisions
3. Review decision rationale
4. Check what else is affected

**Example:**
```
S-015: User Authentication
  ↓ Navigate to Decisions
DEC-001: Use JWT for Auth
  ↓ Check affects field
S-015, S-016, F-001 (all affected)
```

---

## Index Management

### **Automatic Updates**

The index updates automatically when:
- **File created** - New entity added to index
- **File modified** - Entity metadata updated
- **File deleted** - Entity removed from index
- **File renamed** - Entity ID updated

**Update Speed:** ~5ms per file change

---

### **Manual Rebuild**

Force rebuild the index:

1. Open Command Palette (`Cmd/Ctrl+P`)
2. Type "Rebuild Entity Index"
3. Press Enter

**When to rebuild:**
- After bulk imports
- After vault restore
- If navigation seems broken
- After plugin update

---

### **Index Status**

Check index status in console:

```javascript
// Open Developer Console (Cmd/Ctrl+Shift+I)
app.plugins.plugins['canvas-project-manager'].entityIndex.getStats()

// Output:
{
  total: 156,
  milestones: 12,
  stories: 48,
  tasks: 82,
  features: 8,
  documents: 4,
  decisions: 2
}
```

---

## Performance Optimization

### **Index Size**

The index is optimized for large vaults:

| Entities | Index Size | Build Time | Lookup Time |
|----------|------------|------------|-------------|
| 100 | ~50 KB | ~50ms | <1ms |
| 500 | ~250 KB | ~100ms | <1ms |
| 1000 | ~500 KB | ~200ms | <1ms |
| 5000 | ~2.5 MB | ~1s | <1ms |

---

### **Memory Usage**

**Typical usage:**
- **Small vault (100 entities):** ~1 MB
- **Medium vault (500 entities):** ~3 MB
- **Large vault (1000 entities):** ~5 MB

**Note:** Index is rebuilt on plugin load, not persisted to disk.

---

## Best Practices

### ✅ DO

**Navigation:**
- Use keyboard shortcuts for common operations
- Use context menu for exploration
- Use modals to review multiple relationships
- Navigate from canvas for visual context

**Index Management:**
- Let auto-updates handle changes
- Rebuild after bulk operations
- Check console for index stats
- Monitor performance with large vaults

**Workflows:**
- Start from top-level entities
- Navigate down hierarchy
- Trace dependencies both ways
- Review related documents and decisions

### ❌ DON'T

**Avoid:**
- Manually editing index (it's read-only)
- Rebuilding index on every change
- Navigating to non-existent entities
- Ignoring disabled menu items

**Performance:**
- Don't keep thousands of entities in memory
- Don't navigate during bulk operations
- Don't rebuild index unnecessarily

---

## Troubleshooting

### **Navigation menu not showing**

**Causes:**
- File is not an entity
- Index not built yet
- Plugin not loaded

**Solution:**
1. Check file has entity frontmatter
2. Rebuild entity index
3. Restart Obsidian

---

### **Entity not found in index**

**Causes:**
- File created recently
- Index out of sync
- Invalid entity ID

**Solution:**
1. Wait for auto-update (~5ms)
2. Rebuild entity index
3. Check entity ID format

---

### **Keyboard shortcuts not working**

**Causes:**
- Shortcut conflict
- File not active
- Plugin disabled

**Solution:**
1. Check Hotkeys settings
2. Ensure entity file is focused
3. Restart plugin

---

### **Slow navigation**

**Causes:**
- Large vault (>5000 entities)
- Index rebuild in progress
- Disk I/O bottleneck

**Solution:**
1. Wait for index build to complete
2. Archive old entities
3. Use SSD for vault storage

---

## Integration with Other Features

### **With Relationship Reconciliation**

1. Run reconciliation
2. Index auto-updates with new relationships
3. Navigate to verify changes
4. Check bidirectional links work

---

### **With Feature Coverage**

1. Open Feature Coverage view
2. Click on feature
3. Navigate to implementors
4. Review implementation status

---

### **With Visual Canvas**

1. Populate canvas from vault
2. Right-click on nodes
3. Navigate to related entities
4. Verify edges match relationships

---

## Next Steps

- [Relationships Guide](relationships.md) - Understanding relationship types
- [Relationship Reconciliation](relationship-reconciliation.md) - Maintaining data integrity
- [Visual Canvas](visual-canvas.md) - Canvas integration
- [Plugin Commands Reference](../reference/plugin-commands-complete.md) - All navigation commands

