# Sample Vault

Explore a complete example project structure.

## Overview

The sample vault demonstrates a SaaS product development project with:

- 1 Milestone (MVP Foundation)
- 1 Story (User Authentication)
- 1 Decision (PostgreSQL Database)

## Structure

```
sample-vault/
├── schema.json
├── milestones/
│   └── MVP_Foundation.md
├── stories/
│   └── User_Authentication.md
└── decisions/
    └── PostgreSQL_Database.md
```

!!! note "Title-only filenames"
    Entity files are named after their **title** — the entity ID (`M-001`, `S-001`, `DEC-001`) lives in the YAML frontmatter, not the filename. The `schema.json` is bootstrapped by the MCP server on first run.

## Viewing the Sample

1. Copy `examples/sample-vault/` to your Obsidian vault
2. Run MCP server pointing to your vault
3. Use AI to explore: "List all entities"
4. Visualize on canvas

---

See [Quick Start](../getting-started/quick-start.md) to create your own project.
