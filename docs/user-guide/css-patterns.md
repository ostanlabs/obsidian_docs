# CSS Patterns

**Visual styling system for canvas entities**

---

## Overview

The Canvas Project Manager uses **CSS classes** to provide visual differentiation for entities on the canvas. This CSS-driven approach allows for:

- **Flexible styling** - Customize colors and appearance
- **Workstream identification** - Color-coded by team/domain
- **Status visualization** - Visual indicators for progress
- **Priority highlighting** - Emphasize critical work
- **Type differentiation** - Border styles by entity type

---

## CSS Class Structure

Each entity on the canvas receives multiple CSS classes:

```css
/* Example: Story in Engineering workstream, In Progress, High Priority */
.canvas-story
.canvas-workstream-engineering
.canvas-status-in-progress
.canvas-priority-high
```

### **Class Categories**

| Category | Pattern | Purpose |
|----------|---------|---------|
| **Type** | `canvas-{type}` | Entity type (milestone, story, task) |
| **Workstream** | `canvas-workstream-{slug}` | Team/domain color coding |
| **Status** | `canvas-status-{slug}` | Progress indicators |
| **Priority** | `canvas-priority-{slug}` | Importance highlighting |

---

## Entity Type Classes

Control border thickness and style based on entity type.

### **Type Patterns**

```css
/* Milestone - Thick solid border (3px) */
.canvas-milestone {
    border-width: 3px;
    border-style: solid;
}

/* Story - Medium solid border (2px) */
.canvas-story {
    border-width: 2px;
    border-style: solid;
}

/* Task - Thin solid border (1px) */
.canvas-task {
    border-width: 1px;
    border-style: solid;
}

/* Decision - Dashed border (2px) */
.canvas-decision {
    border-width: 2px;
    border-style: dashed;
}

/* Document - Dotted border (1px) */
.canvas-document {
    border-width: 1px;
    border-style: dotted;
}

/* Feature - Medium solid border (2px) */
.canvas-feature {
    border-width: 2px;
    border-style: solid;
}
```

### **Visual Hierarchy**

```
Milestone (3px) ━━━━━━━━━━━━━━━━━━━━
Story (2px)     ━━━━━━━━━━━━━━━━━━━━
Task (1px)      ────────────────────
Decision (2px)  ┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄
Document (1px)  ····················
```

---

## Workstream Classes

Color-code entities by team or domain.

### **Default Workstreams**

```css
/* Engineering - Blue */
.canvas-workstream-engineering {
    --canvas-pm-workstream-color: var(--color-blue);
}

/* Business - Purple */
.canvas-workstream-business {
    --canvas-pm-workstream-color: var(--color-purple);
}

/* Design - Pink */
.canvas-workstream-design {
    --canvas-pm-workstream-color: var(--color-pink);
}

/* Marketing - Orange */
.canvas-workstream-marketing {
    --canvas-pm-workstream-color: var(--color-orange);
}

/* Infrastructure - Yellow */
.canvas-workstream-infra {
    --canvas-pm-workstream-color: var(--color-yellow);
}

/* Research - Green */
.canvas-workstream-research {
    --canvas-pm-workstream-color: var(--color-green);
}
```

### **Workstream Normalization**

Workstream names are normalized to CSS-safe slugs:

| Entity Field | CSS Class |
|--------------|-----------|
| `engineering` | `canvas-workstream-engineering` |
| `business` | `canvas-workstream-business` |
| `design` | `canvas-workstream-design` |
| `marketing` | `canvas-workstream-marketing` |
| `infrastructure` → `infra` | `canvas-workstream-infra` |
| `research` | `canvas-workstream-research` |

**Normalization Rules:**
1. Convert to lowercase
2. Replace spaces with hyphens
3. Remove non-alphanumeric characters
4. Apply workstream aliases (infrastructure → infra)

---

## Status Classes

Visual indicators for entity progress.

### **Status Patterns**

```css
/* Not Started - Default appearance */
.canvas-status-not-started {
    opacity: 1;
    border-radius: 12px;
}

/* In Progress - Yellow thick border */
.canvas-status-in-progress {
    border-color: var(--color-yellow) !important;
    border-width: 8px !important;
    border-style: solid !important;
    border-radius: 12px !important;
}

/* Completed - Green thick border */
.canvas-status-completed,
.canvas-status-complete,
.canvas-status-decided,
.canvas-status-approved {
    border-color: var(--color-green) !important;
    border-width: 8px !important;
    border-style: solid !important;
    border-radius: 12px !important;
}

/* Blocked - Red thick border with glow */
.canvas-status-blocked {
    border-color: var(--color-red) !important;
    border-width: 8px !important;
    border-style: solid !important;
    border-radius: 12px !important;
    box-shadow: 0 0 16px var(--color-red);
}
```

### **Status Visualization**

```
Not Started:  ┌──────────────┐
              │              │  (Default)
              └──────────────┘

In Progress:  ┏━━━━━━━━━━━━━━┓
              ┃              ┃  (Yellow, 8px)
              ┗━━━━━━━━━━━━━━┛

Completed:    ┏━━━━━━━━━━━━━━┓
              ┃              ┃  (Green, 8px)
              ┗━━━━━━━━━━━━━━┛

Blocked:      ┏━━━━━━━━━━━━━━┓
              ┃   (glow)     ┃  (Red, 8px + shadow)
              ┗━━━━━━━━━━━━━━┛
```

**Design Note:** Thick borders (8px) ensure status is visible when zoomed out on large canvases.

---

## Priority Classes

Highlight high-priority work.

### **Priority Patterns**

```css
/* Critical - Pulsing red glow */
.canvas-priority-critical {
    animation: canvas-pm-pulse 2s infinite;
}

@keyframes canvas-pm-pulse {
    0%, 100% { box-shadow: 0 0 4px var(--color-red); }
    50% { box-shadow: 0 0 12px var(--color-red); }
}

/* High - Red left border accent */
.canvas-priority-high {
    border-left: 4px solid var(--color-red) !important;
}

/* Medium - Yellow left border accent */
.canvas-priority-medium {
    border-left: 4px solid var(--color-yellow) !important;
}

/* Low - Green left border accent */
.canvas-priority-low {
    border-left: 4px solid var(--color-green) !important;
}
```

### **Priority Visualization**

```
Critical:  ┌──────────────┐
           │ (pulsing)    │  (Animated red glow)
           └──────────────┘

High:      ┃┌─────────────┐
           ┃│             │  (Red left accent)
           ┃└─────────────┘

Medium:    ┃┌─────────────┐
           ┃│             │  (Yellow left accent)
           ┃└─────────────┘

Low:       ┃┌─────────────┐
           ┃│             │  (Green left accent)
           ┃└─────────────┘
```

**Note:** Only Milestones and Stories have priority fields.

---

## CSS Class Generation

CSS classes are automatically generated from entity frontmatter.

### **Generation Logic**

```typescript
// From entity frontmatter:
{
  type: "story",
  workstream: "engineering",
  status: "in-progress",
  priority: "high"
}

// Generated CSS classes:
[
  "canvas-story",
  "canvas-workstream-engineering",
  "canvas-status-in-progress",
  "canvas-priority-high"
]
```

### **Normalization Function**

```typescript
function normalizeForCss(value: string): string {
  return value
    .toLowerCase()
    .replace(/\s+/g, '-')
    .replace(/[^a-z0-9-]/g, '');
}

// Examples:
normalizeForCss("In Progress") → "in-progress"
normalizeForCss("Not Started") → "not-started"
normalizeForCss("Engineering") → "engineering"
```

---

## Customizing Styles

### **Method 1: CSS Snippets**

Create a custom CSS snippet in `.obsidian/snippets/`:

**File:** `.obsidian/snippets/my-custom-styles.css`

```css
/* Override workstream colors */
.canvas-workstream-engineering {
    --canvas-pm-workstream-color: #0066cc;
}

.canvas-workstream-design {
    --canvas-pm-workstream-color: #ff1493;
}

/* Custom status styles */
.canvas-status-in-progress {
    border-color: #ffa500 !important;
    border-width: 6px !important;
}

/* Custom priority styles */
.canvas-priority-critical {
    border: 4px solid red !important;
    background-color: rgba(255, 0, 0, 0.1) !important;
}
```

**Enable:**
1. Open Obsidian Settings
2. Go to Appearance → CSS Snippets
3. Enable `my-custom-styles`

---

### **Method 2: Theme Variables**

Override Obsidian theme variables:

```css
/* In your theme or snippet */
:root {
    --color-blue: #0066cc;
    --color-purple: #9b59b6;
    --color-pink: #ff1493;
    --color-orange: #ff8c00;
    --color-yellow: #ffd700;
    --color-green: #32cd32;
    --color-red: #dc143c;
}
```

---

### **Method 3: Custom Workstreams**

Add custom workstream styles:

```css
/* Custom workstream: Data Science */
.canvas-workstream-data-science {
    --canvas-pm-workstream-color: #8b008b;
}

/* Custom workstream: DevOps */
.canvas-workstream-devops {
    --canvas-pm-workstream-color: #ff4500;
}

/* Custom workstream: QA */
.canvas-workstream-qa {
    --canvas-pm-workstream-color: #20b2aa;
}
```

**Usage:**
```yaml
# In entity frontmatter
workstream: data-science
```

---

## Common Customization Examples

### **Example 1: Softer Colors**

```css
/* Pastel color scheme */
.canvas-workstream-engineering {
    --canvas-pm-workstream-color: #b3d9ff;
}

.canvas-workstream-design {
    --canvas-pm-workstream-color: #ffb3d9;
}

.canvas-status-in-progress {
    border-color: #ffffb3 !important;
}

.canvas-status-completed {
    border-color: #b3ffb3 !important;
}
```

---

### **Example 2: High Contrast**

```css
/* High contrast for accessibility */
.canvas-status-in-progress {
    border-color: #000000 !important;
    border-width: 10px !important;
}

.canvas-status-blocked {
    border-color: #ff0000 !important;
    border-width: 10px !important;
    box-shadow: 0 0 20px #ff0000;
}

.canvas-priority-critical {
    background-color: rgba(255, 0, 0, 0.3) !important;
}
```

---

### **Example 3: Minimalist**

```css
/* Minimal styling */
.canvas-status-in-progress,
.canvas-status-completed,
.canvas-status-blocked {
    border-width: 2px !important;
    border-radius: 4px !important;
    box-shadow: none !important;
}

.canvas-priority-critical {
    animation: none;
    border-left: 2px solid var(--color-red) !important;
}
```

---

### **Example 4: Dark Mode Optimized**

```css
/* Dark mode adjustments */
.theme-dark .canvas-workstream-engineering {
    --canvas-pm-workstream-color: #4da6ff;
}

.theme-dark .canvas-status-completed {
    border-color: #66ff66 !important;
}

.theme-dark .canvas-status-blocked {
    border-color: #ff6666 !important;
    box-shadow: 0 0 20px rgba(255, 102, 102, 0.5);
}
```

---

## CSS Snippet Management

The plugin automatically creates and manages a CSS snippet.

### **Auto-Generated Snippet**

**Location:** `.obsidian/snippets/canvas-project-manager.css`

**Contents:**
- Entity type styles
- Workstream colors
- Status indicators
- Priority styles

**Management:**
- Created on plugin load
- Updated on plugin update
- Enabled automatically

---

### **Manual Management**

**Disable auto-generated snippet:**
1. Open Obsidian Settings
2. Go to Appearance → CSS Snippets
3. Disable `canvas-project-manager`

**Use custom snippet instead:**
1. Create `.obsidian/snippets/my-styles.css`
2. Copy styles from `canvas-project-manager.css`
3. Customize as needed
4. Enable `my-styles`

---

## Best Practices

### ✅ DO

**Styling:**
- Use CSS snippets for customization
- Test styles on different zoom levels
- Use high contrast for status indicators
- Keep workstream colors distinct

**Organization:**
- Group related styles together
- Comment your customizations
- Use theme variables when possible
- Test in both light and dark modes

**Performance:**
- Avoid complex animations
- Use CSS variables for colors
- Minimize use of `!important`
- Keep selectors simple

### ❌ DON'T

**Avoid:**
- Editing auto-generated snippet (it gets overwritten)
- Using inline styles on canvas nodes
- Overriding core Obsidian styles
- Using too many custom workstreams

**Performance:**
- Don't use heavy animations on many nodes
- Don't use complex gradients
- Don't use large box-shadows
- Don't override every default style

---

## Troubleshooting

### **Styles not applying**

**Causes:**
- CSS snippet not enabled
- Syntax error in custom CSS
- Specificity conflict

**Solution:**
1. Check Appearance → CSS Snippets
2. Validate CSS syntax
3. Use `!important` if needed
4. Reload Obsidian

---

### **Colors look wrong**

**Causes:**
- Theme overriding colors
- Dark/light mode mismatch
- Custom theme variables

**Solution:**
1. Check theme CSS variables
2. Add theme-specific overrides
3. Test in both modes
4. Use absolute colors instead of variables

---

### **Status borders not visible**

**Causes:**
- Zoomed out too far
- Border width too small
- Color contrast too low

**Solution:**
1. Increase border width (8px → 12px)
2. Use higher contrast colors
3. Add box-shadow for emphasis
4. Test at different zoom levels

---

### **Custom workstream not working**

**Causes:**
- Workstream name not normalized
- CSS class not defined
- Typo in frontmatter

**Solution:**
1. Check normalization: `infrastructure` → `infra`
2. Define CSS class: `.canvas-workstream-{slug}`
3. Verify frontmatter field
4. Rebuild entity index

---

## Integration with Other Features

### **With Visual Canvas**

1. Populate canvas from vault
2. CSS classes applied automatically
3. Zoom out to see status colors
4. Use workstream colors for lanes

---

### **With Entity Navigator**

1. Navigate to entity
2. Check frontmatter fields
3. Verify CSS classes in DevTools
4. Update styles as needed

---

### **With Workstream Normalization**

1. Entity has `workstream: infrastructure`
2. Normalized to `infra`
3. CSS class: `canvas-workstream-infra`
4. Styled with yellow color

---

## Next Steps

- [Visual Canvas](visual-canvas.md) - Canvas layout and organization
- [Entity Management](entity-management.md) - Creating and updating entities
- [Workstream Normalization](workstream-normalization.md) - Workstream mapping rules
- [Plugin Commands Reference](../reference/plugin-commands-complete.md) - Canvas commands

