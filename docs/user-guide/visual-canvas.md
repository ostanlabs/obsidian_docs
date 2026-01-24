# Visual Canvas Guide

Learn how to use the Obsidian Canvas Project Manager plugin for visual project management.

---

## Canvas Layout

The plugin organizes entities on a canvas using a hierarchical, workstream-based layout.

```mermaid
graph TD
    subgraph "Engineering Lane"
        M1[M-001: MVP]
        S1[S-001: Auth]
        S2[S-002: API]
        T1[T-001: JWT]
        T2[T-002: OAuth]
    end
    
    subgraph "Design Lane"
        M2[M-002: UI/UX]
        S3[S-003: Components]
        T3[T-003: Button]
    end
    
    M1 --> S1
    M1 --> S2
    S1 --> T1
    S1 --> T2
    M2 --> S3
    S3 --> T3
    
    style M1 fill:#e3f2fd
    style M2 fill:#e3f2fd
    style S1 fill:#fff9c4
    style S2 fill:#fff9c4
    style S3 fill:#fff9c4
```

---

## Plugin Commands

### Populate from Vault
Reads all entities from vault and adds them to canvas.

**Usage:** `Ctrl/Cmd+P` → "Project Canvas: Populate from vault"

### Reposition Nodes
Automatically arranges nodes in hierarchical layout.

**Usage:** `Ctrl/Cmd+P` → "Project Canvas: Reposition nodes"

### Refresh Canvas
Reloads entities and updates display.

**Usage:** `Ctrl/Cmd+P` → "Project Canvas: Refresh"

---

## Visual Features

- **Color Coding** - Different colors for entity types
- **Status Indicators** - Visual status badges
- **Relationship Lines** - Arrows show dependencies
- **Workstream Lanes** - Horizontal lanes by workstream
- **Hierarchical Layout** - Parent-child positioning

---

## Best Practices

!!! tip "Regular Refresh"
    Refresh canvas after making changes via AI

!!! tip "Use Reposition"
    Run reposition after adding many entities

!!! tip "Manual Adjustments"
    Feel free to manually adjust node positions

---

See [User Guide Overview](overview.md) for complete documentation.
