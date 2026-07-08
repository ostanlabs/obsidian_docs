# Schema & Customization

> **`schema.json` is the runtime single source of truth for entity types, relationships, positioning, and workstreams**

Every project vault carries a `schema.json` at its root. Both surfaces read it: the MCP server derives its validation rules from it, and the plugin derives its positioning rules from it. Change the schema and **both** change behavior — no rebuild, no plugin update.

---

## What Lives in schema.json

| Section | Contents |
|---------|----------|
| **`settings`** | ID padding, archive layout, filename pattern (`{title}`, preserve-case) and overlap priority order for the canvas |
| **`entityTypes`** | The six built-in types (milestone, story, task, decision, document, feature) with their ID prefixes, folders, statuses, and custom fields |
| **`relationships`** | The [seven relationship types](relationships.md) — field pairs (`forward`/`reverse`), cardinality, canvas edge styling, graph rules (cycle prevention, transitive reduction), and **positioning metadata** |
| **`workstreams`** | Allowed workstream values, the default (`engineering`), normalization aliases (`infra` → `infra`, `dev` → `engineering`, …), and lane colors |

### Positioning metadata

Each relationship carries a `positioning` block that assigns it one of two roles:

- **`containment`** — the relationship nests one entity inside another. `containerEnd` says which side is the container (`from` or `to`); `priority` breaks ties when an entity has several possible containers.
- **`sequencing`** — the relationship orders siblings. `forwardDirection` (`before`/`after`) says which way the forward field points.

This is exactly how the [containment chain](relationships.md#containment-vs-sequencing) is defined — it's schema data, not engine code. Add a new relationship with a `positioning` block and the layout engine places it with **no code changes**:

```jsonc
{
  "name": "review", "label": "Review",
  "pairs": [{ "from": "task", "to": "decision",
              "forward": "reviewed_by", "reverse": "reviews" }],
  "cardinality": { "forward": "one", "reverse": "many" },
  "positioning": { "role": "containment", "containerEnd": "to", "priority": 2 }
}
```

---

## Bootstrap Behavior

- **First MCP run:** if no `schema.json` exists in the project folder, the server writes the built-in default schema there. An existing `schema.json` is **never overwritten**.
- **The plugin is read-only:** it looks for `schema.json` in the nearest ancestor folder above your canvas and reads it. It never writes or bootstraps one — that's the MCP server's job.
- **Invalid schema on disk:** both surfaces fall back to the built-in defaults and log a warning rather than failing.

---

## Editing the Schema

### Option 1: The Interactive Designer

Ask your AI assistant to call **`get_schema_designer`**. You get a self-contained HTML page, pre-populated with your vault's *active* schema:

- **Hexagon layout** of the six core types — the execution hierarchy (milestone, story, task) on the left, the knowledge side (feature, document, decision) on the right — with relationship edges drawn between them
- Toggle whole relationships or individual type-pairs on and off
- A **per-relationship positioning editor** (role, container end, direction, priority)
- A **copy** button that exports the result as JSON ready for `set_schema`

Save the HTML to a file, open it in a browser, design, copy, apply.

### Option 2: `set_schema`

Apply a full schema object (or just a replacement `relationships` array):

```
"Apply this schema: { ...designer output... }"
```

The MCP server validates the payload — relationship pair shapes, positioning metadata (`role` ∈ {containment, sequencing}, `containerEnd` ∈ {from, to}, `forwardDirection` ∈ {before, after}, `priority` ≥ 0) — persists it to `schema.json`, and **hot-reloads** its own validation rules immediately.

### Option 3: Edit schema.json Directly

Fine for small tweaks (adding a workstream value, changing a lane color). The MCP server picks the file up on restart; prefer `set_schema` so you get validation.

!!! warning "Invalid schemas are rejected"
    `set_schema` refuses payloads that fail validation and reports the errors — your existing schema stays in place. If you hand-edit `schema.json` into an invalid state, both the MCP server and the plugin fall back to the built-in default schema and log the problems, so a broken file degrades gracefully but your customizations stop applying until fixed.

---

## Hot Reload

| Surface | When schema changes take effect |
|---------|--------------------------------|
| **MCP validation** (`validate_project`, write-time relationship checks) | Immediately after `set_schema` |
| **Plugin positioning** | Next plugin load / canvas reposition — no rebuild required |

---

## Inspecting the Active Schema

- **`get_schema`** — returns the full active schema as JSON
- **`get_schema_designer`** — the same data, visualized and editable

---

## See Also

- [Relationships](relationships.md) — the default relationship set in detail
- [Visual Canvas](visual-canvas.md) — how positioning metadata drives layout
- [Workstream Normalization](workstream-normalization.md) — alias rules defined in the schema
