# Workspace Management

**Organize and access document collections beyond project entities**

---

## Overview

Workspaces allow you to organize and access **document collections** that exist outside the standard project entity structure (Milestones, Stories, Tasks, etc.). This is useful for:

- **Architecture Decision Records (ADRs)** - Design decisions and rationale
- **Technical Documentation** - API docs, system architecture, guides
- **Meeting Notes** - Team meetings, planning sessions
- **Research** - Investigation notes, competitive analysis
- **Knowledge Base** - How-to guides, troubleshooting docs

Workspaces enable AI assistants to read, search, and update these documents using MCP tools.

---

## Workspace Configuration

Workspaces are defined in `workspaces.json` at the vault root:

```json
{
  "docs": {
    "path": "/vault/docs",
    "description": "Technical documentation and guides"
  },
  "adrs": {
    "path": "/vault/architecture/decisions",
    "description": "Architecture Decision Records"
  },
  "meetings": {
    "path": "/vault/meetings",
    "description": "Meeting notes and agendas"
  }
}
```

**Fields:**
- `path` - Absolute path to the workspace folder
- `description` - Human-readable description of workspace contents

---

## Managing Workspaces

### Via MCP Tool (AI-Driven)

AI assistants can manage workspaces using the `manage_workspaces` tool.

#### **Add Workspace**

```json
{
  "tool": "manage_workspaces",
  "parameters": {
    "action": "add",
    "name": "research",
    "path": "/vault/research",
    "description": "Research notes and competitive analysis"
  }
}
```

**Response:**
```json
{
  "success": true,
  "action": "add",
  "workspace": "research",
  "message": "Workspace 'research' added successfully. Restart the MCP server for changes to take full effect.",
  "workspaces": {
    "docs": {...},
    "adrs": {...},
    "research": {
      "path": "/vault/research",
      "description": "Research notes and competitive analysis"
    }
  }
}
```

---

#### **Update Workspace**

```json
{
  "tool": "manage_workspaces",
  "parameters": {
    "action": "update",
    "name": "research",
    "description": "Research notes, competitive analysis, and market research"
  }
}
```

**Notes:**
- Can update `path`, `description`, or both
- Omitted fields remain unchanged

---

#### **Remove Workspace**

```json
{
  "tool": "manage_workspaces",
  "parameters": {
    "action": "remove",
    "name": "research"
  }
}
```

**Warning:** This only removes the workspace configuration, not the actual files.

---

### Via Manual Edit

You can also edit `workspaces.json` directly:

1. Open `workspaces.json` in your vault root
2. Add/modify/remove workspace entries
3. Save the file
4. Restart the MCP server

---

## Listing Workspaces

### Via MCP Tool

```json
{
  "tool": "list_workspaces"
}
```

**Response:**
```json
{
  "workspaces": [
    {
      "name": "docs",
      "path": "/vault/docs",
      "description": "Technical documentation and guides",
      "file_count": 23
    },
    {
      "name": "adrs",
      "path": "/vault/architecture/decisions",
      "description": "Architecture Decision Records",
      "file_count": 8
    }
  ],
  "count": 2,
  "config_last_changed": "2026-01-24T14:30:00Z"
}
```

---

## Listing Files in Workspace

### Via MCP Tool

```json
{
  "tool": "list_files",
  "parameters": {
    "workspace": "docs",
    "max_items": 50
  }
}
```

**Response:**
```json
{
  "workspace": "docs",
  "workspace_description": "Technical documentation and guides",
  "files": [
    {
      "name": "architecture/system-overview.md",
      "path": "/vault/docs/architecture/system-overview.md",
      "size": 2048,
      "last_changed": "2026-01-24T14:30:00Z"
    },
    {
      "name": "api/authentication.md",
      "path": "/vault/docs/api/authentication.md",
      "size": 1536,
      "last_changed": "2026-01-23T10:15:00Z"
    }
  ],
  "count": 2
}
```

**Parameters:**
- `workspace` (required) - Workspace name
- `max_items` (optional) - Max files to return (default: 20, max: 200)

---

## Reading Documents

### Via MCP Tool

```json
{
  "tool": "read_docs",
  "parameters": {
    "workspace": "docs",
    "doc_name": "architecture/system-overview.md"
  }
}
```

**Response:**
```json
{
  "workspace": "docs",
  "workspace_description": "Technical documentation and guides",
  "doc_name": "architecture/system-overview.md",
  "content": "# System Overview\n\n## Architecture\n...",
  "line_count": 156,
  "last_changed": "2026-01-24T14:30:00Z"
}
```

**Parameters:**
- `workspace` (required) - Workspace name
- `doc_name` (required) - Document name (auto-adds .md extension)
- `line_range` (optional) - `[start, end]` to read specific lines

**Example (Read Lines 10-50):**
```json
{
  "tool": "read_docs",
  "parameters": {
    "workspace": "docs",
    "doc_name": "architecture/system-overview",
    "line_range": [10, 50]
  }
}
```

---

## Updating Documents

### Via MCP Tool

The `update_doc` tool supports multiple operations:

#### **Create Document**

```json
{
  "tool": "update_doc",
  "parameters": {
    "workspace": "docs",
    "name": "new-guide.md",
    "operation": "create",
    "content": "# New Guide\n\nContent here."
  }
}
```

---

#### **Replace Entire Document**

```json
{
  "tool": "update_doc",
  "parameters": {
    "workspace": "docs",
    "name": "existing-doc.md",
    "operation": "replace",
    "content": "# Updated Content\n\nNew content."
  }
}
```

---

#### **Append to Document**

```json
{
  "tool": "update_doc",
  "parameters": {
    "workspace": "docs",
    "name": "existing-doc.md",
    "operation": "append",
    "content": "\n## New Section\n\nAppended content."
  }
}
```

---

#### **Prepend to Document**

```json
{
  "tool": "update_doc",
  "parameters": {
    "workspace": "docs",
    "name": "existing-doc.md",
    "operation": "prepend",
    "content": "# Header\n\n"
  }
}
```

---

#### **Insert at Line**

```json
{
  "tool": "update_doc",
  "parameters": {
    "workspace": "docs",
    "name": "existing-doc.md",
    "operation": "insert_at",
    "content": "Inserted line",
    "start_line": 10
  }
}
```

---

#### **Replace Line Range**

```json
{
  "tool": "update_doc",
  "parameters": {
    "workspace": "docs",
    "name": "existing-doc.md",
    "operation": "replace_at",
    "content": "New content for lines 10-15",
    "start_line": 10,
    "end_line": 15
  }
}
```

---

#### **Delete Document**

```json
{
  "tool": "update_doc",
  "parameters": {
    "workspace": "docs",
    "name": "obsolete-doc.md",
    "operation": "delete"
  }
}
```

---

## Common Workflows

### 1. **Create Architecture Decision Record**

**Scenario:** Document a technical decision.

**Steps:**
1. Create ADR workspace (if not exists)
2. Create ADR document
3. Link to affected entities

**AI Workflow:**
```
1. manage_workspaces (action: add, name: "adrs", path: "/vault/adrs", description: "Architecture Decision Records")
2. update_doc (workspace: "adrs", name: "ADR-001-database-choice.md", operation: "create", content: "# ADR-001: Use PostgreSQL\n\n...")
3. create_entity (type: decision, data: {title: "Use PostgreSQL", affects: ["S-015"]})
```

---

### 2. **Search Documentation**

**Scenario:** Find relevant documentation for a feature.

**AI Workflow:**
```
1. list_workspaces - find available workspaces
2. search_docs (query: "authentication implementation", filters: {doc_uri_prefix: "docs/"})
3. read_docs (workspace: "docs", doc_name: "api/authentication.md")
```

**Note:** Uses semantic search (MSRL) for meaning-based search across all workspaces.

---

### 3. **Update Meeting Notes**

**Scenario:** AI assistant takes meeting notes.

**AI Workflow:**
```
1. update_doc (workspace: "meetings", name: "2026-01-24-standup.md", operation: "create", content: "# Standup 2026-01-24\n\n")
2. update_doc (workspace: "meetings", name: "2026-01-24-standup.md", operation: "append", content: "## Attendees\n- Alice\n- Bob\n\n")
3. update_doc (workspace: "meetings", name: "2026-01-24-standup.md", operation: "append", content: "## Discussion\n- Feature X progress\n- Blocker on S-015\n")
```

---

### 4. **Organize Research Notes**

**Scenario:** Collect research findings.

**Steps:**
1. Create research workspace
2. Create research documents
3. Link to relevant features

**AI Workflow:**
```
1. manage_workspaces (action: add, name: "research", path: "/vault/research")
2. update_doc (workspace: "research", name: "competitor-analysis.md", operation: "create", content: "...")
3. create_entity (type: document, data: {title: "Competitor Analysis", documents: ["F-005"]})
```

---

## Integration with Semantic Search

Workspaces are automatically indexed by the semantic search system (MSRL).

**Benefits:**
- **Hybrid search** - Vector + keyword matching across all workspace documents
- **Incremental updates** - Changes reflected in ~750ms
- **Exact provenance** - Returns file path, heading, and character position

**Example:**
```json
{
  "tool": "search_docs",
  "parameters": {
    "query": "authentication flow",
    "filters": {
      "doc_uri_prefix": "docs/api/"
    },
    "top_k": 5
  }
}
```

See [Semantic Search Guide](semantic-search.md) for details.

---

## Best Practices

### ✅ DO

**Workspace Organization:**
- Create workspaces for distinct document types
- Use descriptive workspace names
- Keep workspace paths organized
- Document workspace purpose in description

**Document Management:**
- Use consistent naming conventions
- Organize documents in subfolders
- Keep documents focused and concise
- Link documents to relevant entities

**AI Workflows:**
- Use `list_workspaces` to discover available workspaces
- Use `list_files` to explore workspace contents
- Use `search_docs` for semantic search
- Use `read_docs` with `line_range` for large documents

### ❌ DON'T

**Avoid:**
- Creating workspaces for every folder
- Mixing entity files with workspace documents
- Creating duplicate workspaces
- Deleting workspace folders without removing config

**Performance:**
- Don't read entire large documents without `line_range`
- Don't create hundreds of small workspaces
- Don't use `replace` operation for small edits (use `replace_at` instead)

---

## Troubleshooting

### **Workspace not found**

**Causes:**
- Workspace not in `workspaces.json`
- Typo in workspace name
- MCP server not restarted after config change

**Solution:**
1. Run `list_workspaces` to see available workspaces
2. Check `workspaces.json` for correct name
3. Restart MCP server if config was changed

---

### **Document not found**

**Causes:**
- Document doesn't exist
- Wrong workspace
- Incorrect file path

**Solution:**
1. Run `list_files` to see available documents
2. Check file path (case-sensitive)
3. Verify document has `.md` extension

---

### **Permission denied**

**Causes:**
- Workspace path doesn't exist
- No write permissions
- Path is outside vault

**Solution:**
1. Verify workspace path exists
2. Check file system permissions
3. Ensure path is within vault

---

## Testing Status

**Workspace Management has comprehensive test coverage:**

✅ **list_workspaces** - 2 tests
✅ **list_files** - 5 tests
✅ **read_docs** - 7 tests
✅ **update_doc** - 12 tests

**Total:** 26 tests covering all workspace operations

See `obsidian_mcp/src/tools/index.test.ts` for test details.

---

## Next Steps

- [Semantic Search](semantic-search.md) - Search across workspace documents
- [Entity Management](entity-management.md) - Link documents to entities
- [AI Workflows](ai-workflows.md) - AI-driven documentation workflows
- [MCP Tools Reference](../reference/mcp-tools-complete.md) - Complete tool documentation

