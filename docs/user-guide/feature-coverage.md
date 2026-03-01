# Feature Coverage Workflow

**Track feature implementation, documentation, and testing status**

---

## Overview

The Feature Coverage system helps you track the **completeness** of your features across three dimensions:

1. **Implementation** - Which Milestones and Stories implement this feature?
2. **Documentation** - Which Documents document this feature?
3. **Testing** - Are there test references for this feature?

This workflow is essential for:
- **Release planning** - Identify which features are ready to ship
- **Gap analysis** - Find features missing implementation, docs, or tests
- **Progress tracking** - Monitor feature completion across tiers and phases
- **Quality assurance** - Ensure all features are properly documented and tested

---

## Feature Entity Structure

Features are tracked as entities with the following classification:

### **Tier** (OSS vs Premium)
- **OSS** - Open source features available to all users
- **Premium** - Premium features for paid users

### **Phase** (Roadmap Timeline)
- **MVP** - Minimum viable product features
- **0-5** - Numbered phases for roadmap planning

### **Status** (Implementation State)
- **Planned** - Not yet started
- **In Progress** - Currently being implemented
- **Complete** - Fully implemented
- **Deferred** - Postponed to future phase

---

## Using the Feature Coverage View

### Opening the View

**Via Command Palette:**
```
Cmd/Ctrl+P → "Project Canvas: Open Feature Coverage"
```

The view opens in the right sidebar and shows a table of all features with their coverage status.

---

### View Components

#### 1. **Filters**

Filter features by:
- **Tier** - OSS, Premium, or All
- **Phase** - MVP, 0-5, or All
- **Status** - Planned, In Progress, Complete, Deferred, or All
- **Show only with gaps** - Only show features missing impl/docs/tests

#### 2. **Summary Statistics**

Shows aggregate metrics:
```
Total: 24
With Implementation: 18 (75%)
With Docs: 15 (63%)
With Tests: 12 (50%)
Complete: 10 (42%)
```

#### 3. **Coverage Table**

| ID | Title | Tier | Phase | Status | Impl | Docs | Tests |
|----|-------|------|-------|--------|------|------|-------|
| F-001 | User Auth | OSS | MVP | Complete | 2 | 1 | ✓ |
| F-002 | API Keys | OSS | 0 | In Progress | 1 | 0 | ✗ |
| F-003 | Premium UI | Premium | 1 | Planned | 0 | 0 | ✗ |

**Column Meanings:**
- **Impl** - Number of implementing entities (Milestones/Stories)
- **Docs** - Number of documenting entities (Documents)
- **Tests** - ✓ if `test_refs` field is populated, ✗ otherwise

**Gap Highlighting:**
- Cells with `0` or `✗` are highlighted in red (gaps)
- Click on Feature ID to open the feature file

---

## AI-Driven Feature Coverage

### Using the MCP Tool

AI assistants can query feature coverage using the `get_feature_coverage` tool:

**Example Query:**
```
"Show me all OSS features in MVP phase that are missing tests"
```

**MCP Tool Call:**
```json
{
  "tool": "get_feature_coverage",
  "parameters": {
    "tier": "OSS",
    "phase": "MVP",
    "include_tests": true
  }
}
```

**Response:**
```json
{
  "summary": {
    "total": 8,
    "implemented": 6,
    "documented": 5,
    "tested": 3,
    "gaps": {
      "missing_implementation": ["F-005"],
      "missing_docs": ["F-002", "F-007"],
      "missing_tests": ["F-001", "F-003", "F-006", "F-008"]
    }
  },
  "features": [
    {
      "id": "F-001",
      "title": "User Authentication",
      "tier": "OSS",
      "phase": "MVP",
      "status": "Complete",
      "implementation": {
        "milestones": ["M-001"],
        "stories": ["S-015", "S-016"],
        "progress_percent": 100
      },
      "documentation": {
        "specs": ["DOC-003"],
        "guides": [],
        "coverage": "partial"
      },
      "testing": {
        "test_refs": [],
        "has_tests": false
      }
    }
  ]
}
```

---

## Common Workflows

### 1. **Find Features Missing Implementation**

**Human (via Plugin):**
1. Open Feature Coverage view
2. Check "Show only with gaps"
3. Look for features with `Impl = 0`

**AI (via MCP):**
```json
{
  "tool": "get_feature_coverage",
  "parameters": {
    "summary_only": false
  }
}
```
Then filter `gaps.missing_implementation` array.

---

### 2. **Track MVP Readiness**

**Human:**
1. Filter by Phase: MVP
2. Filter by Status: Complete
3. Check that all have Impl > 0, Docs > 0, Tests = ✓

**AI:**
```json
{
  "tool": "get_feature_coverage",
  "parameters": {
    "phase": "MVP",
    "include_tests": true
  }
}
```
Check `summary.implemented`, `summary.documented`, `summary.tested` vs `summary.total`.

---

### 3. **Link Features to Implementation**

**Scenario:** You've implemented a feature but haven't linked it yet.

**Steps:**
1. Create Feature entity (F-XXX)
2. Create/update implementing entities (Milestones/Stories)
3. Add `implements: ["F-XXX"]` to entity frontmatter
4. Feature Coverage view automatically shows the link

**AI Workflow:**
```
1. create_entity (type: feature, data: {...})
2. update_entity (id: "S-015", add_to: {implements: ["F-001"]})
3. get_feature_coverage (feature_ids: ["F-001"]) - verify link
```

---

### 4. **Document Features**

**Scenario:** Feature is implemented but not documented.

**Steps:**
1. Create Document entity (DOC-XXX)
2. Add `documents: ["F-XXX"]` to document frontmatter
3. Feature Coverage view shows Docs count increase

**AI Workflow:**
```
1. get_feature_coverage (show_gaps_only: true) - find undocumented features
2. create_entity (type: document, data: {documents: ["F-001"], ...})
3. get_feature_coverage (feature_ids: ["F-001"]) - verify documentation
```

---

### 5. **Add Test References**

**Scenario:** Feature is implemented and documented but not tested.

**Steps:**
1. Open Feature file (F-XXX.md)
2. Add `test_refs` field to frontmatter:
   ```yaml
   test_refs:
     - tests/auth/test_login.py
     - tests/auth/test_logout.py
   ```
3. Feature Coverage view shows Tests = ✓

**AI Workflow:**
```
1. get_feature_coverage (include_tests: true, show_gaps_only: true)
2. update_entity (id: "F-001", data: {test_refs: ["tests/auth/test_login.py"]})
3. get_feature_coverage (feature_ids: ["F-001"]) - verify tests
```

---

## Coverage Metrics Explained

### **Implementation Coverage**

Tracks which Milestones and Stories implement the feature.

**Relationship:**
- Feature has `implemented_by: ["M-001", "S-015"]`
- Milestone/Story has `implements: ["F-001"]`

**Progress Calculation:**
```
progress_percent = (completed_entities / total_entities) * 100
```

Where:
- `completed_entities` = Stories/Milestones with status "Complete"
- `total_entities` = All Stories/Milestones in `implemented_by`

**Example:**
```yaml
# F-001.md
implemented_by:
  - M-001  # Complete
  - S-015  # Complete
  - S-016  # In Progress
```
Progress: 66% (2 of 3 complete)

---

### **Documentation Coverage**

Tracks which Documents document the feature.

**Coverage Levels:**
- **Full** - Has both specs and guides
- **Partial** - Has either specs or guides
- **None** - No documentation

**Document Types:**
- **Specs** - Technical specifications (DOC-XXX)
- **Guides** - User guides (DOC-XXX)

**Relationship:**
- Feature has `documented_by: ["DOC-003"]`
- Document has `documents: ["F-001"]`

---

### **Testing Coverage**

Tracks whether the feature has test references.

**Test References:**
- File paths to test files
- Test suite names
- Test case identifiers

**Example:**
```yaml
# F-001.md
test_refs:
  - tests/auth/test_login.py
  - tests/auth/test_logout.py
  - tests/auth/test_session.py
```

**Coverage:**
- `has_tests: true` if `test_refs` is non-empty
- `has_tests: false` otherwise

---

## Feature Lifecycle

### 1. **Planning Phase**

**Status:** Planned
**Coverage:** 0% implementation, 0% documentation, 0% testing

**Actions:**
- Create Feature entity
- Define user story
- Set tier and phase
- Identify acceptance criteria

---

### 2. **Implementation Phase**

**Status:** In Progress
**Coverage:** Partial implementation, 0-50% documentation, 0% testing

**Actions:**
- Create implementing Milestones/Stories
- Link via `implements` field
- Track progress in Feature Coverage view

---

### 3. **Documentation Phase**

**Status:** In Progress
**Coverage:** 50-100% implementation, Partial documentation, 0% testing

**Actions:**
- Create specification documents
- Create user guides
- Link via `documents` field

---

### 4. **Testing Phase**

**Status:** In Progress
**Coverage:** 100% implementation, Full documentation, Partial testing

**Actions:**
- Write tests
- Add `test_refs` to feature
- Verify all acceptance criteria covered

---

### 5. **Complete**

**Status:** Complete
**Coverage:** 100% implementation, Full documentation, 100% testing

**Criteria:**
- All implementing entities are Complete
- Has both specs and guides
- Has test references
- All acceptance criteria met

---

## Best Practices

### ✅ DO

**Feature Creation:**
- Create features early in planning phase
- Define clear user stories
- Set realistic phases
- Link features to implementing entities as you create them

**Coverage Tracking:**
- Review Feature Coverage view weekly
- Filter by "Show only with gaps" to find work
- Use AI to bulk-link features to implementations
- Keep `test_refs` up to date

**Documentation:**
- Create specs before implementation
- Create guides after implementation
- Link documents to features immediately
- Use consistent document naming

### ❌ DON'T

**Avoid:**
- Creating features without user stories
- Implementing features without linking them
- Marking features Complete without full coverage
- Deferring features without updating status

**Performance:**
- Don't create hundreds of features without phases
- Don't leave features in "In Progress" indefinitely
- Don't skip documentation for "small" features

---

## Integration with Other Workflows

### **With Visual Canvas**

1. Create Features Canvas (tier/phase grid)
2. Populate with features
3. Auto-layout by tier and phase
4. Visual overview of roadmap

**Commands:**
- `Project Canvas: Create Features Canvas`
- `Project Canvas: Populate Features Canvas`
- `Project Canvas: Auto-Layout Features`

---

### **With AI Workflows**

1. AI queries feature coverage
2. AI identifies gaps
3. AI creates implementing entities
4. AI links entities to features
5. Human reviews in Feature Coverage view

**Example AI Session:**
```
Human: "What features are missing implementation?"
AI: [calls get_feature_coverage with show_gaps_only]
AI: "F-005 and F-012 have no implementation. Should I create stories for them?"
Human: "Yes, create stories for F-005"
AI: [calls batch_update to create stories with implements: ["F-005"]]
AI: "Created S-042 and S-043 implementing F-005"
```

---

### **With Notion Sync**

Features can be synced to Notion for external tracking:

1. Enable Notion integration
2. Sync features to Notion database
3. Update in Notion (status, phase, etc.)
4. Sync back to Obsidian
5. Feature Coverage view reflects changes

---

## Troubleshooting

### **Feature not showing in Coverage view**

**Causes:**
- File doesn't match `F-XXX` pattern
- Frontmatter `type` is not "feature"
- File is in wrong folder

**Solution:**
1. Check filename matches `F-001.md` format
2. Verify frontmatter has `type: feature`
3. Click "Refresh" button in Coverage view

---

### **Implementation count is 0 but entities exist**

**Causes:**
- Entities don't have `implements` field
- Feature doesn't have `implemented_by` field
- Relationship not bidirectional

**Solution:**
1. Add `implements: ["F-XXX"]` to entity frontmatter
2. Run "Reconcile All Relationships" command
3. Refresh Feature Coverage view

---

### **Documentation count is wrong**

**Causes:**
- Documents don't have `documents` field
- Feature doesn't have `documented_by` field

**Solution:**
1. Add `documents: ["F-XXX"]` to document frontmatter
2. Run "Reconcile All Relationships" command
3. Refresh Feature Coverage view

---

## Next Steps

- [Entity Management](entity-management.md) - Learn how to create and update features
- [Relationships](relationships.md) - Understand entity relationships
- [AI Workflows](ai-workflows.md) - AI-driven feature management
- [MCP Tools Reference](../reference/mcp-tools-complete.md) - `get_feature_coverage` tool details

