# MCP Tools Summary

Quick reference for the **22 MCP server tools**. For parameters, examples, and return shapes see the [complete MCP Tools Reference](mcp-tools-complete.md).

---

## All Tools at a Glance

### Entity CRUD

| Tool | One-liner |
|------|-----------|
| `create_entity` | Create a milestone, story, task, decision, document, or feature |
| `get_entity` | Get a single entity by ID as JSON |
| `update_entity` | Update entity fields, status, or relationships |
| `list_entities` | List all entities, optionally filtered by type |

### Bulk Operations

| Tool | One-liner |
|------|-----------|
| `entities` | Batch get, or batch create/update/archive with `client_id` idempotency, `dry_run` and `atomic` options |

### Search & Navigation

| Tool | One-liner |
|------|-----------|
| `search_entities` | Full-text search, filtered listing, or hierarchy navigation (`from_id` + `direction`) |

### Schema

| Tool | One-liner |
|------|-----------|
| `get_schema` | Read the active schema (`schema.json` or the built-in default), its source, and validation errors |
| `set_schema` | Write `<vault>/schema.json` and hot-reload; invalid schemas are rejected and not saved |
| `get_schema_designer` | Self-contained HTML relationship designer pre-populated with the vault's schema |

### Analysis & Validation

| Tool | One-liner |
|------|-----------|
| `get_project_overview` | High-level status summary across workstreams |
| `analyze_project_state` | Deep analysis: blockers and suggested actions |
| `get_feature_coverage` | Feature implementation and documentation coverage |
| `validate_project` | Hard violations **plus** soft fan-out advisories (non-blocking guidelines) |

### Maintenance

| Tool | One-liner |
|------|-----------|
| `reconcile_relationships` | Fix bidirectional relationships (parent↔children, depends_on↔blocks, implements↔implemented_by) |
| `rebuild_index` | Rebuild the in-memory entity index from vault files |
| `cleanup_completed` | Archive completed stories/tasks under completed milestones |

### Documents & Files

| Tool | One-liner |
|------|-----------|
| `read_docs` | Read a workspace document (not entity files) |
| `update_doc` | Create/update a workspace document |
| `list_files` | List vault files by directory/pattern |
| `manage_documents` | Decision history and document freshness checks |

### Semantic Search

| Tool | One-liner |
|------|-----------|
| `search_docs` | Hybrid vector + BM25 search with relevance-weighted excerpt budgets and `min_score` filtering |
| `msrl_status` | Semantic search index status (state, snapshot, stats, watcher) |

---

!!! warning "Relationship fields must be nested"
    On `create_entity`, `update_entity`, and `entities` batch ops, relationship fields (`parent`, `depends_on`, `implements`, ...) must be nested under the `relationships` key. Flat relationship keys are **silently ignored**. See the [complete reference](mcp-tools-complete.md) for examples.

---

## Next Steps

- [Complete MCP Tools Reference](mcp-tools-complete.md) - Parameters, examples, return shapes
- [Entity Schemas](entity-schemas.md) - Entity structure and the runtime schema
- [Configuration](configuration.md) - Configure the MCP server
