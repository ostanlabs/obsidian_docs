# Workstream Normalization

**Automatic workstream name standardization for consistent organization**

---

## Overview

Workstream Normalization automatically converts various workstream names to **standard slugs**, preventing fragmentation and ensuring consistent organization across your project.

**Key Benefits:**
- **Prevents fragmentation** - No more `eng`, `engineering`, `dev` as separate workstreams
- **Consistent CSS styling** - Normalized names map to CSS classes
- **AI-friendly** - AI agents can use natural language, system normalizes
- **Transparent feedback** - System tells you when normalization occurs

---

## Standard Workstreams

The system recognizes six standard workstreams:

| Standard Slug | Description | Common Use Cases |
|---------------|-------------|------------------|
| `engineering` | Technical implementation | Code, APIs, infrastructure, technical tasks |
| `business` | Business operations | Strategy, planning, business decisions |
| `design` | Design & UX | UI/UX design, user research, prototypes |
| `marketing` | Marketing & growth | Content, SEO, campaigns, analytics |
| `infra` | Infrastructure & DevOps | Deployment, monitoring, CI/CD |
| `research` | Research & exploration | Spikes, POCs, investigation, R&D |

---

## Normalization Rules

### **Rule 1: Alias Mapping**

Common variations are automatically mapped to standard slugs:

| Input | Normalized To | Reason |
|-------|---------------|--------|
| `eng` | `engineering` | Common abbreviation |
| `dev` | `engineering` | Development = Engineering |
| `development` | `engineering` | Full form |
| `biz` | `business` | Common abbreviation |
| `infrastructure` | `infra` | Shortened form preferred |
| `ops` | `infra` | Operations = Infrastructure |
| `operations` | `infra` | Full form |
| `ux` | `design` | UX is part of design |
| `ui` | `design` | UI is part of design |
| `mktg` | `marketing` | Common abbreviation |
| `r&d` | `research` | Research & Development |
| `rnd` | `research` | R&D abbreviation |

---

### **Rule 2: Case Normalization**

All workstream names are converted to lowercase:

| Input | Normalized To |
|-------|---------------|
| `Engineering` | `engineering` |
| `BUSINESS` | `business` |
| `Design` | `design` |

---

### **Rule 3: Whitespace Handling**

Spaces are replaced with hyphens for CSS compatibility:

| Input | Normalized To |
|-------|---------------|
| `data science` | `data-science` |
| `product management` | `product-management` |

**Note:** Multi-word workstreams are allowed but must be defined in CSS.

---

### **Rule 4: Special Character Removal**

Non-alphanumeric characters (except hyphens) are removed:

| Input | Normalized To |
|-------|---------------|
| `r&d` | `research` (via alias) |
| `eng/dev` | `engineering` (via alias) |
| `ui/ux` | `design` (via alias) |

---

## How Normalization Works

### **During Entity Creation**

When creating an entity via MCP:

```json
{
  "tool": "create_entity",
  "parameters": {
    "type": "story",
    "data": {
      "title": "User Authentication",
      "workstream": "dev"  // ← Input
    }
  }
}
```

**System Response:**
```json
{
  "id": "S-015",
  "title": "User Authentication",
  "workstream": "engineering",  // ← Normalized
  "message": "Normalized workstream: 'dev' → 'engineering'"  // ← Feedback
}
```

---

### **During Entity Update**

When updating an entity:

```json
{
  "tool": "update_entity",
  "parameters": {
    "id": "S-015",
    "data": {
      "workstream": "infrastructure"  // ← Input
    }
  }
}
```

**System Response:**
```json
{
  "id": "S-015",
  "workstream": "infra",  // ← Normalized
  "message": "Normalized workstream: 'infrastructure' → 'infra'"  // ← Feedback
}
```

---

## AI Agent Feedback

The system provides **transparent feedback** to AI agents when normalization occurs.

### **Feedback Format**

```
Normalized workstream: '{input}' → '{normalized}'
```

### **Examples**

| Input | Feedback |
|-------|----------|
| `dev` | `Normalized workstream: 'dev' → 'engineering'` |
| `infrastructure` | `Normalized workstream: 'infrastructure' → 'infra'` |
| `biz` | `Normalized workstream: 'biz' → 'business'` |
| `ux` | `Normalized workstream: 'ux' → 'design'` |

### **Why Feedback Matters**

1. **AI Learning** - Agents learn to use standard names
2. **Transparency** - Users see what happened
3. **Debugging** - Easy to spot normalization issues
4. **Consistency** - Reinforces standard vocabulary

---

## CSS Integration

Normalized workstreams map directly to CSS classes.

### **CSS Class Pattern**

```css
.canvas-workstream-{normalized-slug}
```

### **Examples**

| Workstream | CSS Class |
|------------|-----------|
| `engineering` | `.canvas-workstream-engineering` |
| `business` | `.canvas-workstream-business` |
| `design` | `.canvas-workstream-design` |
| `marketing` | `.canvas-workstream-marketing` |
| `infra` | `.canvas-workstream-infra` |
| `research` | `.canvas-workstream-research` |

### **Color Mapping**

```css
/* Default colors */
.canvas-workstream-engineering { --canvas-pm-workstream-color: var(--color-blue); }
.canvas-workstream-business { --canvas-pm-workstream-color: var(--color-purple); }
.canvas-workstream-design { --canvas-pm-workstream-color: var(--color-pink); }
.canvas-workstream-marketing { --canvas-pm-workstream-color: var(--color-orange); }
.canvas-workstream-infra { --canvas-pm-workstream-color: var(--color-yellow); }
.canvas-workstream-research { --canvas-pm-workstream-color: var(--color-green); }
```

See [CSS Patterns](css-patterns.md) for customization.

---

## Common Workflows

### **Workflow 1: AI Creates Entity with Natural Language**

**Scenario:** AI agent uses natural language for workstream.

**AI Request:**
```
"Create a story for user authentication in the dev workstream"
```

**MCP Call:**
```json
{
  "tool": "create_entity",
  "parameters": {
    "type": "story",
    "data": {
      "title": "User Authentication",
      "workstream": "dev"
    }
  }
}
```

**System Response:**
```json
{
  "id": "S-015",
  "workstream": "engineering",
  "message": "Normalized workstream: 'dev' → 'engineering'"
}
```

**AI Learns:** Next time, AI uses `engineering` directly.

---

### **Workflow 2: User Manually Sets Workstream**

**Scenario:** User edits frontmatter directly.

**Before:**
```yaml
# stories/S-015.md
---
id: S-015
type: story
title: User Authentication
workstream: infrastructure
---
```

**After "Populate Canvas from Vault":**
```yaml
# stories/S-015.md
---
id: S-015
type: story
title: User Authentication
workstream: infra  # ← Normalized
cssclasses:
  - canvas-story
  - canvas-workstream-infra  # ← CSS class updated
---
```

---

### **Workflow 3: Bulk Update Workstreams**

**Scenario:** Rename all `dev` workstreams to `engineering`.

**Via AI:**
```
"Update all stories with workstream 'dev' to 'engineering'"
```

**AI Workflow:**
```
1. list_entities (filters: {type: "story", workstream: "dev"})
2. For each story:
   - update_entity (id: story.id, data: {workstream: "engineering"})
3. Summary: "Updated 15 stories from 'dev' to 'engineering'"
```

**Result:** All normalized to `engineering`, CSS classes updated.

---

### **Workflow 4: Add Custom Workstream**

**Scenario:** Add a new workstream `data-science`.

**Steps:**
1. Create entity with `workstream: data-science`
2. Define CSS class in snippet
3. Use consistently

**CSS Snippet:**
```css
/* .obsidian/snippets/custom-workstreams.css */
.canvas-workstream-data-science {
    --canvas-pm-workstream-color: #8b008b;
}
```

**Usage:**
```json
{
  "tool": "create_entity",
  "parameters": {
    "type": "story",
    "data": {
      "title": "ML Model Training",
      "workstream": "data-science"
    }
  }
}
```

**Note:** No normalization occurs for custom workstreams.

---

## Normalization Examples

### **Example 1: Engineering Variations**

| Input | Normalized | Feedback |
|-------|-----------|----------|
| `eng` | `engineering` | `Normalized workstream: 'eng' → 'engineering'` |
| `dev` | `engineering` | `Normalized workstream: 'dev' → 'engineering'` |
| `development` | `engineering` | `Normalized workstream: 'development' → 'engineering'` |
| `Engineering` | `engineering` | (Case normalized, no feedback) |

---

### **Example 2: Infrastructure Variations**

| Input | Normalized | Feedback |
|-------|-----------|----------|
| `infrastructure` | `infra` | `Normalized workstream: 'infrastructure' → 'infra'` |
| `ops` | `infra` | `Normalized workstream: 'ops' → 'infra'` |
| `operations` | `infra` | `Normalized workstream: 'operations' → 'infra'` |
| `Infra` | `infra` | (Case normalized, no feedback) |

---

### **Example 3: Design Variations**

| Input | Normalized | Feedback |
|-------|-----------|----------|
| `ux` | `design` | `Normalized workstream: 'ux' → 'design'` |
| `ui` | `design` | `Normalized workstream: 'ui' → 'design'` |
| `ui/ux` | `design` | `Normalized workstream: 'ui/ux' → 'design'` |
| `Design` | `design` | (Case normalized, no feedback) |

---

### **Example 4: Business Variations**

| Input | Normalized | Feedback |
|-------|-----------|----------|
| `biz` | `business` | `Normalized workstream: 'biz' → 'business'` |
| `Business` | `business` | (Case normalized, no feedback) |

---

### **Example 5: Research Variations**

| Input | Normalized | Feedback |
|-------|-----------|----------|
| `r&d` | `research` | `Normalized workstream: 'r&d' → 'research'` |
| `rnd` | `research` | `Normalized workstream: 'rnd' → 'research'` |
| `Research` | `research` | (Case normalized, no feedback) |

---

## Best Practices

### ✅ DO

**Consistency:**
- Use standard workstream names
- Let normalization handle variations
- Define custom workstreams in CSS
- Document custom workstreams

**AI Interaction:**
- Allow AI to use natural language
- Review normalization feedback
- Teach AI standard names over time
- Use consistent vocabulary

**Organization:**
- Limit to 6-8 workstreams
- Use descriptive names
- Map to team structure
- Keep workstreams stable

### ❌ DON'T

**Avoid:**
- Creating too many workstreams
- Using inconsistent names
- Ignoring normalization feedback
- Changing workstream names frequently

**Performance:**
- Don't create workstreams per project
- Don't use entity-specific workstreams
- Don't mix workstream and status
- Don't use workstream for priority

---

## Troubleshooting

### **Workstream not normalizing**

**Causes:**
- Custom workstream (no alias defined)
- Typo in workstream name
- Normalization disabled

**Solution:**
1. Check workstream name spelling
2. Add alias to normalization rules
3. Or define custom CSS class

---

### **Wrong normalization**

**Causes:**
- Unexpected alias mapping
- Ambiguous input

**Solution:**
1. Use standard name directly
2. Check normalization rules
3. Report issue if incorrect

**Example:**
```
Input: "ops"
Expected: "operations"
Actual: "infra"
Fix: Use "operations" directly
```

---

### **CSS class not applying**

**Causes:**
- Workstream not normalized
- CSS class not defined
- CSS snippet not enabled

**Solution:**
1. Check normalized workstream value
2. Define CSS class for workstream
3. Enable CSS snippet
4. Reload Obsidian

---

### **AI using wrong workstream**

**Causes:**
- AI not learning from feedback
- Inconsistent usage
- Ambiguous instructions

**Solution:**
1. Review normalization feedback
2. Use explicit workstream names
3. Correct AI in conversation
4. Update AI instructions

**Example:**
```
User: "Create story in dev workstream"
AI: Creates with workstream: "dev"
System: Normalizes to "engineering"
User: "Use 'engineering' workstream directly"
AI: Next time uses "engineering"
```

---

## Advanced Topics

### **Custom Normalization Rules**

For advanced users, custom normalization rules can be added to the MCP server.

**Location:** `obsidian_mcp/src/services/workstream-normalizer.ts`

**Example:**
```typescript
const WORKSTREAM_ALIASES: Record<string, string> = {
  // Existing aliases
  'eng': 'engineering',
  'dev': 'engineering',

  // Custom aliases
  'data-sci': 'data-science',
  'ml': 'data-science',
  'ai': 'data-science',
};
```

**Note:** Requires MCP server restart.

---

### **Workstream Analytics**

Track workstream usage across your project:

**Via AI:**
```
"Show me workstream distribution across all stories"
```

**AI Workflow:**
```
1. list_entities (filters: {type: "story"})
2. Group by workstream
3. Count per workstream
4. Generate report
```

**Example Output:**
```
Workstream Distribution (Stories):
- engineering: 45 (60%)
- design: 15 (20%)
- business: 10 (13%)
- marketing: 5 (7%)
```

---

### **Workstream Migration**

Migrate from old workstream names to new standard:

**Scenario:** Migrate from `effort` to `workstream`.

**Via AI:**
```
"Migrate all entities from 'effort' field to 'workstream' field"
```

**AI Workflow:**
```
1. list_entities (all types)
2. For each entity with 'effort' field:
   - update_entity (id: entity.id, data: {workstream: entity.effort})
   - Remove 'effort' field
3. Summary: "Migrated 150 entities"
```

---

## Integration with Other Features

### **With CSS Patterns**

1. Workstream normalized
2. CSS class generated: `canvas-workstream-{slug}`
3. Color applied from CSS
4. Visual differentiation on canvas

---

### **With Visual Canvas**

1. Entities positioned by workstream
2. Workstream lanes created
3. Color-coded by workstream
4. Easy visual organization

---

### **With Entity Navigator**

1. Navigate to entity
2. Check workstream field
3. Verify normalization
4. Update if needed

---

## Next Steps

- [CSS Patterns](css-patterns.md) - Workstream color customization
- [Visual Canvas](visual-canvas.md) - Workstream-based layout
- [Entity Management](entity-management.md) - Creating entities with workstreams
- [MCP Tools Reference](../reference/mcp-tools-complete.md) - `create_entity` and `update_entity` tools

