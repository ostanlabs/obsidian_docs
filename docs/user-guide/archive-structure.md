# Archive Structure

**Flat archive organization for completed entities**

---

## Overview

The Archive System provides a **flat, type-based structure** for storing completed or obsolete entities. This keeps your active workspace clean while preserving historical data for reference.

**Key Features:**
- **Flat structure** - No hierarchy in archive
- **Type-based folders** - Separate folders per entity type
- **Automatic archival** - Triggered by status changes
- **Canvas cleanup** - Archived nodes removed automatically
- **Easy restoration** - Unarchive with one command

---

## Archive Structure

### **Folder Layout**

```
vault/
├── archive/
│   ├── milestones/
│   │   ├── M-001_q1-release.md
│   │   ├── M-002_mvp-launch.md
│   │   └── M-003_beta-testing.md
│   ├── stories/
│   │   ├── S-015_user-authentication.md
│   │   ├── S-016_api-integration.md
│   │   └── S-042_payment-processing.md
│   ├── tasks/
│   │   ├── T-001_implement-jwt.md
│   │   ├── T-002_write-tests.md
│   │   └── T-038_deploy-staging.md
│   ├── decisions/
│   │   ├── DEC-001_use-postgresql.md
│   │   └── DEC-002_oauth-provider.md
│   ├── documents/
│   │   ├── DOC-001_auth-spec.md
│   │   └── DOC-002_api-design.md
│   └── features/
│       ├── F-001_authentication.md
│       └── F-002_user-management.md
├── milestones/
│   └── M-004_current-sprint.md  (active)
├── stories/
│   └── S-050_new-feature.md  (active)
└── tasks/
    └── T-100_current-task.md  (active)
```

### **Key Characteristics**

1. **Flat by Type** - All archived milestones in one folder, regardless of original hierarchy
2. **No Nesting** - Tasks don't go under stories, stories don't go under milestones
3. **Filename Preserved** - Original filename maintained (e.g., `M-001_q1-release.md`)
4. **Metadata Intact** - All frontmatter and relationships preserved

---

## Archival Triggers

### **1. Status-Based Archival**

Set entity status to trigger automatic archival:

```yaml
# In entity frontmatter
status: archived
```

**Supported statuses:**
- `archived` - Generic archival
- `completed` - Finished work (can be archived)
- `cancelled` - Abandoned work (can be archived)
- `obsolete` - No longer relevant (auto-archived)

---

### **2. Manual Archival via Plugin**

**Steps:**
1. Open Command Palette (`Cmd/Ctrl+P`)
2. Type "Populate Canvas from Vault"
3. Press Enter

**What happens:**
- Scans vault for `status: archived` entities
- Moves files to appropriate archive folders
- Removes archived nodes from canvas
- Shows summary notice

---

### **3. Programmatic Archival via MCP**

AI assistants can archive entities using the `update_entity` tool:

```json
{
  "tool": "update_entity",
  "parameters": {
    "id": "S-015",
    "archived": true,
    "archive_options": {
      "remove_from_canvas": true,
      "canvas_source": "main.canvas"
    }
  }
}
```

**Response:**
```json
{
  "id": "S-015",
  "archived": true,
  "archive_path": "archive/stories/S-015_user-authentication.md",
  "removed_from_canvas": true
}
```

---

## Archival Workflows

### **Workflow 1: Archive Completed Story**

**Scenario:** Story is complete, archive it.

**Steps:**
1. Update story status to `completed`
2. Run "Populate Canvas from Vault"
3. Story moved to `archive/stories/`
4. Story removed from canvas

**Before:**
```
stories/S-015_user-authentication.md
```

**After:**
```
archive/stories/S-015_user-authentication.md
```

---

### **Workflow 2: Archive Milestone with Children**

**Scenario:** Milestone and all its stories/tasks are complete.

**Via MCP:**
```json
{
  "tool": "update_entity",
  "parameters": {
    "id": "M-001",
    "archived": true,
    "archive_options": {
      "cascade": true,
      "remove_from_canvas": true,
      "canvas_source": "main.canvas"
    }
  }
}
```

**Result:**
```
archive/milestones/M-001_q1-release.md
archive/stories/S-015_user-authentication.md
archive/stories/S-016_api-integration.md
archive/tasks/T-001_implement-jwt.md
archive/tasks/T-002_write-tests.md
```

**Note:** All children archived recursively.

---

### **Workflow 3: Archive Decision**

**Scenario:** Decision is superseded by a newer decision.

**Steps:**
1. Create new decision (DEC-002)
2. Set `DEC-001.superseded_by = DEC-002`
3. Set `DEC-001.status = obsolete`
4. Run "Populate Canvas from Vault"
5. DEC-001 moved to `archive/decisions/`

---

### **Workflow 4: Bulk Archive**

**Scenario:** Archive all completed entities from Q1.

**Via AI:**
```
"Archive all completed milestones, stories, and tasks from Q1 2024"
```

**AI Workflow:**
```
1. list_entities (filters: {status: "completed", created_before: "2024-04-01"})
2. For each entity:
   - update_entity (archived: true, archive_options: {remove_from_canvas: true})
3. Summary: "Archived 45 entities (3 milestones, 18 stories, 24 tasks)"
```

---

## Restoration Workflows

### **Workflow 1: Unarchive Single Entity**

**Via Plugin:**
1. Open Command Palette (`Cmd/Ctrl+P`)
2. Type "Unarchive Stories and Tasks"
3. Select entity types to restore
4. Confirm

**Via MCP:**
```json
{
  "tool": "update_entity",
  "parameters": {
    "id": "S-015",
    "archived": false,
    "restore_options": {
      "restore_children": false,
      "add_to_canvas": true,
      "canvas_source": "main.canvas"
    }
  }
}
```

**Result:**
```
Before: archive/stories/S-015_user-authentication.md
After:  stories/S-015_user-authentication.md
Status: Not Started
```

---

### **Workflow 2: Unarchive by Type**

**Via Plugin:**
1. Run "Unarchive Stories and Tasks"
2. Check entity types to restore:
   - ☑ Milestones (3)
   - ☑ Stories (18)
   - ☐ Tasks (24)
   - ☐ Decisions (2)
   - ☐ Documents (1)
3. Click "Unarchive Selected"

**Result:**
- 3 milestones moved to `milestones/`
- 18 stories moved to `stories/`
- All set to `status: Not Started`
- All set to `archived: false`

---

## Archive Metadata

Archived entities maintain all their metadata and relationships.

### **Frontmatter Changes**

**Before Archival:**
```yaml
---
id: S-015
type: story
title: User Authentication
status: completed
workstream: engineering
parent: M-001
depends_on: [S-012, S-013]
implements: [F-001]
archived: false
created_at: 2024-01-15T10:00:00Z
updated_at: 2024-02-20T15:30:00Z
---
```

**After Archival:**
```yaml
---
id: S-015
type: story
title: User Authentication
status: archived  # ← Changed
workstream: engineering
parent: M-001
depends_on: [S-012, S-013]
implements: [F-001]
archived: true  # ← Changed
created_at: 2024-01-15T10:00:00Z
updated_at: 2024-02-20T16:45:00Z  # ← Updated
---
```

**Key Points:**
- `status` set to `archived`
- `archived` flag set to `true`
- `updated_at` timestamp updated
- **All relationships preserved**

---

### **Relationship Integrity**

Archived entities maintain their relationships:

**Example:**
```yaml
# archive/stories/S-015_user-authentication.md
parent: M-001  # Still references active milestone
implements: F-001  # Still implements feature
depends_on: [S-012, S-013]  # Dependencies preserved
```

**Important:** Active entities can still reference archived entities.

---

## Canvas Integration

### **Automatic Canvas Cleanup**

When entities are archived, they're automatically removed from the canvas.

**Process:**
1. Entity status set to `archived`
2. Run "Populate Canvas from Vault"
3. File moved to archive folder
4. Canvas node removed
5. Edges to/from node removed

**Before:**
```
Canvas: main.canvas
Nodes: M-001, S-015, S-016, T-001
Edges: M-001 → S-015, S-015 → T-001
```

**After archiving S-015:**
```
Canvas: main.canvas
Nodes: M-001, S-016, T-001
Edges: (S-015 edges removed)
```

---

### **Manual Canvas Cleanup**

Remove archived nodes without moving files:

**Steps:**
1. Open Command Palette (`Cmd/Ctrl+P`)
2. Type "Remove Archived Nodes from Canvas"
3. Press Enter

**What it does:**
- Scans canvas for nodes with `archived: true`
- Removes nodes from canvas
- Removes edges to/from archived nodes
- Leaves files in place

---

## Archive Exclusion

Archived entities are excluded from most operations.

### **Excluded From:**

1. **Canvas Population** - Not added to canvas
2. **Vault Scans** - Not included in entity lists
3. **Relationship Traversal** - Not shown in navigation
4. **Feature Coverage** - Not counted in implementation
5. **Project Analysis** - Not included in metrics

### **Included In:**

1. **Search** - Can be found via search
2. **Direct Access** - Can be opened directly
3. **Relationship References** - Can be referenced by active entities
4. **Reconciliation** - Relationships are synced

---

## Best Practices

### ✅ DO

**Archival:**
- Archive completed work regularly
- Use cascade for milestone archival
- Remove from canvas when archiving
- Preserve relationships

**Organization:**
- Keep archive folders clean
- Use consistent naming
- Archive by quarter or sprint
- Document archival decisions

**Restoration:**
- Use dry-run first
- Restore by type
- Update status after restore
- Re-add to canvas if needed

### ❌ DON'T

**Avoid:**
- Manually moving files to archive
- Deleting archived entities
- Breaking relationships before archival
- Archiving entities with active dependencies

**Performance:**
- Don't archive thousands of entities at once
- Don't restore entire archive
- Don't keep archive in sync with canvas
- Don't reference archived entities from active work

---

## Common Scenarios

### **Scenario 1: End of Sprint**

**Goal:** Archive all completed work from sprint.

**Steps:**
1. Mark all completed entities as `status: completed`
2. Run "Populate Canvas from Vault"
3. Verify archive folders
4. Clean up canvas

**Result:**
```
archive/milestones/M-001_sprint-15.md
archive/stories/S-015_feature-a.md
archive/stories/S-016_feature-b.md
archive/tasks/T-001_task-a.md
archive/tasks/T-002_task-b.md
```

---

### **Scenario 2: Quarterly Cleanup**

**Goal:** Archive all Q1 work.

**Via AI:**
```
"Archive all completed milestones, stories, and tasks from Q1 2024.
Remove them from the canvas."
```

**AI Workflow:**
```
1. list_entities (filters: {status: "completed", created_before: "2024-04-01"})
2. For each entity:
   - update_entity (archived: true, archive_options: {remove_from_canvas: true})
3. Summary report
```

---

### **Scenario 3: Restore Cancelled Work**

**Goal:** Restore a cancelled story to resume work.

**Steps:**
1. Find story in `archive/stories/`
2. Run "Unarchive Stories and Tasks"
3. Select "Stories"
4. Confirm
5. Update status to `in-progress`
6. Add to canvas

**Result:**
```
Before: archive/stories/S-042_payment-processing.md (status: cancelled)
After:  stories/S-042_payment-processing.md (status: in-progress)
```

---

### **Scenario 4: Reference Archived Decision**

**Goal:** Link new story to archived decision.

**Frontmatter:**
```yaml
# stories/S-050_new-feature.md
id: S-050
type: story
title: New Feature
depends_on:
  - DEC-001  # Archived decision
```

**Note:** Relationship works even though DEC-001 is archived.

---

## Troubleshooting

### **Files not moving to archive**

**Causes:**
- Status not set to `archived`
- Archive folder doesn't exist
- File permissions issue

**Solution:**
1. Check `status` field in frontmatter
2. Run "Populate Canvas from Vault"
3. Check console for errors
4. Verify archive folder exists

---

### **Archived nodes still on canvas**

**Causes:**
- Canvas not refreshed
- Nodes added manually
- Archive cleanup not run

**Solution:**
1. Run "Populate Canvas from Vault"
2. Or run "Remove Archived Nodes from Canvas"
3. Refresh canvas view

---

### **Can't find archived entity**

**Causes:**
- Wrong archive folder
- File renamed
- File deleted

**Solution:**
1. Search vault for entity ID
2. Check all archive subfolders
3. Check file naming pattern
4. Restore from backup if deleted

---

### **Relationships broken after archival**

**Causes:**
- Manual file move
- Relationship not synced
- Entity deleted instead of archived

**Solution:**
1. Run "Reconcile All Relationships"
2. Check entity still exists in archive
3. Verify relationship fields
4. Restore entity if needed

---

## Integration with Other Features

### **With Relationship Reconciliation**

1. Archive entities
2. Run reconciliation
3. Verify relationships intact
4. Check for orphaned references

---

### **With Feature Coverage**

1. Archive implementing stories
2. Feature coverage recalculated
3. Archived implementations excluded
4. Progress updated

---

### **With Visual Canvas**

1. Archive entities
2. Run "Populate Canvas from Vault"
3. Archived nodes removed
4. Canvas layout updated

---

## Next Steps

- [Entity Management](entity-management.md) - Creating and updating entities
- [Relationship Reconciliation](relationship-reconciliation.md) - Maintaining data integrity
- [Visual Canvas](visual-canvas.md) - Canvas organization
- [MCP Tools Reference](../reference/mcp-tools-complete.md) - `update_entity` archival options

