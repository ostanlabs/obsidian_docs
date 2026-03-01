# Relationship Reconciliation

**Maintain data integrity across bidirectional entity relationships**

---

## Overview

Relationship Reconciliation ensures **bidirectional consistency** across all entity relationships in your vault. When you create or update relationships, the system automatically syncs both sides of the relationship to maintain data integrity.

**Why is this important?**
- Prevents orphaned references
- Ensures navigation works in both directions
- Maintains data consistency after manual edits
- Fixes broken relationships from bulk operations

---

## Bidirectional Relationships

The system maintains six types of bidirectional relationships:

| Relationship | Forward Field | Reverse Field | Example |
|--------------|---------------|---------------|---------|
| **Hierarchy** | `parent` | `children` | Story → Milestone |
| **Dependency** | `depends_on` | `blocks` | Task → Story |
| **Implementation** | `implements` | `implemented_by` | Story → Feature |
| **Documentation** | `documents` | `documented_by` | Document → Feature |
| **Decision Impact** | `affects` | `decided_by` | Decision → Story |
| **Supersession** | `supersedes` | `superseded_by` | Decision → Decision |

**All relationships are automatically synced!**

---

## How Auto-Sync Works

### Example: Creating a Story

When you create a story with a parent milestone:

```yaml
# S-015.md
id: S-015
type: story
title: "User Authentication"
parent: M-001
```

**The system automatically:**
1. Adds `S-015` to `M-001.children`
2. Updates `M-001.updated_at` timestamp
3. Writes changes to `M-001.md`

**Result:**
```yaml
# M-001.md
id: M-001
type: milestone
title: "Q1 Release"
children:
  - S-015  # ← Automatically added
```

---

### Example: Adding Dependencies

When you add a dependency:

```yaml
# T-042.md
id: T-042
type: task
title: "Write tests"
depends_on:
  - T-038  # Depends on "Implement feature"
```

**The system automatically:**
1. Adds `T-042` to `T-038.blocks`
2. Updates `T-038.updated_at` timestamp

**Result:**
```yaml
# T-038.md
id: T-038
type: task
title: "Implement feature"
blocks:
  - T-042  # ← Automatically added
```

---

### Example: Implementing Features

When a story implements a feature:

```yaml
# S-015.md
id: S-015
type: story
title: "User Authentication"
implements:
  - F-001  # Implements "Auth System" feature
```

**The system automatically:**
1. Adds `S-015` to `F-001.implemented_by`
2. Updates `F-001.updated_at` timestamp

**Result:**
```yaml
# F-001.md
id: F-001
type: feature
title: "Auth System"
implemented_by:
  - S-015  # ← Automatically added
```

---

## Manual Reconciliation

### When to Reconcile

Run manual reconciliation when:
- **After bulk edits** - Manual frontmatter changes
- **After imports** - Importing entities from external sources
- **After restores** - Restoring from backups
- **Fixing inconsistencies** - Broken relationships detected

---

### Via Plugin Command

**Steps:**
1. Open Command Palette (`Cmd/Ctrl+P`)
2. Type "Reconcile All Relationships"
3. Press Enter

**What it does:**
- Scans all entity files in vault
- Detects missing reverse relationships
- Adds missing relationships
- Shows summary of changes

**Example Output:**
```
🔗 Reconciled 15 bidirectional relationships
- Added 8 children references
- Added 5 blocks references
- Added 2 implemented_by references
```

---

### Via MCP Tool (AI-Driven)

AI assistants can reconcile relationships using the `reconcile_implements_relationships` tool.

**Example:**
```json
{
  "tool": "reconcile_implements_relationships",
  "parameters": {
    "dry_run": true
  }
}
```

**Response:**
```json
{
  "scanned": 156,
  "updated": 0,
  "dry_run": true,
  "changes": [
    {
      "entity_id": "F-001",
      "field": "implemented_by",
      "action": "added",
      "values": ["S-015", "S-016"],
      "reason": "Synced from S-015.implements, S-016.implements"
    },
    {
      "entity_id": "S-015",
      "field": "implements",
      "action": "added",
      "values": ["F-001"],
      "reason": "Synced from F-001.implemented_by"
    }
  ],
  "warnings": [
    {
      "entity_id": "S-042",
      "field": "implements",
      "referenced_id": "F-999",
      "issue": "Referenced entity does not exist"
    }
  ]
}
```

**Parameters:**
- `dry_run` (optional) - Preview changes without applying (default: false)

**Notes:**
- Always use `dry_run: true` first to preview changes
- Review warnings for broken references
- Run without `dry_run` to apply changes

---

## Common Reconciliation Scenarios

### 1. **After Bulk Import**

**Scenario:** You imported 50 stories from a CSV file with `implements` fields.

**Problem:** Features don't have `implemented_by` fields yet.

**Solution:**
```
1. Via Plugin: Run "Reconcile All Relationships"
2. Via AI: Call reconcile_implements_relationships tool
3. Verify: Check Feature Coverage view
```

**Before:**
```yaml
# S-015.md
implements: [F-001]

# F-001.md
implemented_by: []  # ← Missing!
```

**After:**
```yaml
# F-001.md
implemented_by: [S-015]  # ← Fixed!
```

---

### 2. **After Manual Frontmatter Edit**

**Scenario:** You manually edited a story's parent field.

**Problem:** Old parent still has story in `children`, new parent doesn't.

**Solution:**
```
1. Save the file
2. Run "Reconcile All Relationships"
3. Both parents are now correct
```

**Before:**
```yaml
# S-015.md
parent: M-002  # Changed from M-001

# M-001.md
children: [S-015]  # ← Should be removed

# M-002.md
children: []  # ← Should include S-015
```

**After:**
```yaml
# M-001.md
children: []  # ← Fixed!

# M-002.md
children: [S-015]  # ← Fixed!
```

---

### 3. **After Deleting an Entity**

**Scenario:** You deleted a story but other entities still reference it.

**Problem:** Broken references in `depends_on`, `implements`, etc.

**Solution:**
```
1. Run reconciliation with dry_run: true
2. Review warnings for broken references
3. Manually fix or remove broken references
4. Run reconciliation again
```

**Example Warning:**
```json
{
  "entity_id": "T-042",
  "field": "depends_on",
  "referenced_id": "S-999",
  "issue": "Referenced entity does not exist"
}
```

**Fix:**
```yaml
# T-042.md
depends_on:
  - S-999  # ← Remove this broken reference
```

---

### 4. **After Restoring from Backup**

**Scenario:** You restored files from a backup but relationships are out of sync.

**Problem:** Timestamps don't match, relationships are inconsistent.

**Solution:**
```
1. Run "Reconcile All Relationships"
2. Review changes summary
3. Verify critical relationships manually
```

---

## Relationship Integrity Rules

### **Hierarchy Rules**

1. **Stories must have a parent Milestone**
   - Orphaned stories are flagged as errors
   - Use reconciliation to find orphans

2. **Tasks must have a parent Story**
   - Orphaned tasks are flagged as errors
   - Use reconciliation to find orphans

3. **Milestones can be top-level or nested**
   - Top-level milestones have no parent
   - Nested milestones have a parent Milestone

---

### **Dependency Rules**

1. **No circular dependencies**
   - System detects cycles automatically
   - Reconciliation flags circular dependencies

2. **Dependencies must exist**
   - Broken references are flagged as warnings
   - Must be manually fixed

3. **Transitive dependencies are allowed**
   - A → B → C is valid
   - System doesn't enforce direct dependencies

---

### **Implementation Rules**

1. **Only Stories and Milestones can implement Features**
   - Tasks cannot implement features
   - Documents cannot implement features

2. **Features can be implemented by multiple entities**
   - One feature can have many implementing stories
   - Progress is calculated from all implementers

3. **Entities can implement multiple Features**
   - One story can implement multiple features
   - Common for cross-cutting features

---

### **Documentation Rules**

1. **Documents can document any entity type**
   - Features, Stories, Milestones, Decisions
   - One document can document multiple entities

2. **Entities can be documented by multiple Documents**
   - Specs, guides, ADRs, etc.
   - Coverage is calculated from all documents

---

## Reconciliation Process Details

### **Scan Phase**

1. **Find all entity files**
   - Scans vault for files matching entity patterns
   - Filters by `type` field in frontmatter
   - Excludes archived entities

2. **Parse frontmatter**
   - Extracts all relationship fields
   - Validates field types
   - Detects malformed data

---

### **Analysis Phase**

1. **Build relationship graph**
   - Maps all forward relationships
   - Maps all reverse relationships
   - Identifies missing links

2. **Detect inconsistencies**
   - Forward exists but reverse missing
   - Reverse exists but forward missing
   - Broken references (entity doesn't exist)

---

### **Reconciliation Phase**

1. **Add missing relationships**
   - Adds missing reverse relationships
   - Adds missing forward relationships
   - Updates `updated_at` timestamps

2. **Remove broken references** (optional)
   - Flags broken references as warnings
   - Requires manual intervention
   - Prevents data loss

---

### **Report Phase**

1. **Summary statistics**
   - Total entities scanned
   - Total relationships updated
   - Total warnings

2. **Detailed changes**
   - Entity ID
   - Field modified
   - Action taken (added/removed)
   - Reason for change

3. **Warnings**
   - Broken references
   - Orphaned entities
   - Circular dependencies

---

## Best Practices

### ✅ DO

**Regular Reconciliation:**
- Run reconciliation after bulk operations
- Run reconciliation after imports
- Run reconciliation weekly for large vaults
- Use `dry_run: true` first

**Manual Edits:**
- Edit one relationship side only
- Let reconciliation sync the other side
- Verify changes with reconciliation
- Check warnings for broken references

**Data Integrity:**
- Fix broken references immediately
- Don't delete entities with dependencies
- Archive instead of delete
- Use Entity Navigator to verify relationships

### ❌ DON'T

**Avoid:**
- Manually editing both sides of a relationship
- Ignoring reconciliation warnings
- Deleting entities without checking dependencies
- Running reconciliation without `dry_run` first

**Performance:**
- Don't run reconciliation on every save
- Don't reconcile during bulk operations
- Don't ignore circular dependency warnings

---

## Troubleshooting

### **Reconciliation takes too long**

**Causes:**
- Large vault (>1000 entities)
- Many broken references
- Slow file system

**Solution:**
1. Use `dry_run: true` to preview
2. Fix broken references first
3. Run reconciliation in batches
4. Consider archiving old entities

---

### **Relationships keep breaking**

**Causes:**
- Manual edits on both sides
- Concurrent edits by multiple users
- Plugin conflicts

**Solution:**
1. Edit only one side of relationship
2. Let auto-sync handle the other side
3. Disable conflicting plugins
4. Use version control (git)

---

### **Circular dependency detected**

**Causes:**
- A depends on B, B depends on A
- Transitive cycle: A → B → C → A

**Solution:**
1. Run `analyze_project_state` to find cycles
2. Review dependency graph
3. Break the cycle manually
4. Run reconciliation to verify

**Example:**
```
Cycle detected: S-015 → S-016 → S-015
Fix: Remove S-016 from S-015.depends_on
```

---

### **Orphaned entities found**

**Causes:**
- Parent was deleted
- Parent reference is broken
- Import error

**Solution:**
1. Find orphans with reconciliation warnings
2. Assign new parent
3. Or archive orphaned entities
4. Run reconciliation to verify

---

## Integration with Other Features

### **With Entity Navigator**

1. Navigate to entity
2. Check relationships in sidebar
3. Click to navigate to related entities
4. Verify bidirectional links work

---

### **With Feature Coverage**

1. Run reconciliation
2. Open Feature Coverage view
3. Verify implementation counts are correct
4. Check for missing links

---

### **With Visual Canvas**

1. Run reconciliation
2. Populate canvas from vault
3. Verify edges match relationships
4. Check for missing connections

---

## Next Steps

- [Relationships Guide](relationships.md) - Understanding relationship types
- [Entity Management](entity-management.md) - Creating and updating entities
- [Feature Coverage](feature-coverage.md) - Tracking implementation status
- [MCP Tools Reference](../reference/mcp-tools-complete.md) - `reconcile_implements_relationships` tool

