# Quick Start Guide

!!! success "Goal"
    Get up and running with AI project management in 15 minutes

This guide walks you through setting up the system and creating your first project using AI commands while observing the results visually in Obsidian.

---

## Prerequisites

- [x] **Obsidian** installed
- [x] **Node.js** 18+ installed
- [x] **Claude Desktop** or **Cursor** IDE
- [x] An empty Obsidian vault (or create a new one)

---

## Step 1: Install MCP Server (5 minutes)

=== "Claude Desktop"

    1. Open Claude Desktop configuration file:
       - **macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`
       - **Windows:** `%APPDATA%\Claude\claude_desktop_config.json`

    2. Add the MCP server configuration:

    ```json
    {
      "mcpServers": {
        "obsidian": {
          "command": "npx",
          "args": ["-y", "obsidian-accomplishments-mcp"],
          "env": {
            "VAULT_PATH": "/absolute/path/to/your/vault",
            "DEFAULT_CANVAS": "main.canvas"
          }
        }
      }
    }
    ```

    3. **Replace** `/absolute/path/to/your/vault` with your actual vault path

    4. **Restart Claude Desktop**

=== "Cursor"

    1. Open Cursor Settings → MCP Servers
    2. Add new server with the same configuration as above
    3. Restart Cursor

!!! check "Verify"
    Ask Claude/Cursor: "List available MCP tools" - you should see obsidian tools listed

---

## Step 2: Install Plugin (5 minutes)

!!! warning "Note"
    Plugin is under review. Install from source for now.

1. Clone and build the plugin:

```bash
git clone https://github.com/ostanlabs/obsidian_plugin.git
cd obsidian_plugin
npm install
npm run build
```

2. Copy to your vault:

```bash
mkdir -p /path/to/vault/.obsidian/plugins/canvas-project-manager
cp main.js manifest.json styles.css /path/to/vault/.obsidian/plugins/canvas-project-manager/
```

3. In Obsidian:
   - **Settings → Community Plugins → Enable Canvas Project Manager**

4. Create a canvas file:
   - Create new file: `main.canvas`

!!! check "Verify"
    Open command palette (`Ctrl/Cmd+P`) and search for "Project Canvas" - you should see plugin commands

---

## Step 3: Create Your First Project with AI (5 minutes)

Now let's create a simple learning project using AI and watch it appear on the canvas!

### 3.1 Create the Learning Milestone

Ask your AI assistant:

```
Create a milestone called "Learn Obsidian Project Management" with target date 
one week from today, workstream "learning", and priority "High"
```

**Expected Response:**
```
✓ Created M-001: Learn Obsidian Project Management
  - Target: [date]
  - Workstream: learning
  - Priority: High
```

### 3.2 Add Stories

Ask your AI:

```
Create two stories under M-001:
1. "Set up the system" - status "Completed"
2. "Create first real project" - status "Not Started"
Both in workstream "learning"
```

**Expected Response:**
```
✓ Created S-001: Set up the system (parent: M-001)
✓ Created S-002: Create first real project (parent: M-001)
```

### 3.3 Add Tasks

Ask your AI:

```
Create three tasks under S-002:
1. "Define project goals" - status "Open"
2. "Break down into milestones" - status "Open"  
3. "Create initial stories" - status "Open"
All in workstream "learning"
```

**Expected Response:**
```
✓ Created T-001: Define project goals (parent: S-002)
✓ Created T-002: Break down into milestones (parent: S-002)
✓ Created T-003: Create initial stories (parent: S-002)
```

---

## Step 4: Visualize on Canvas (2 minutes)

Now let's see what we created!

1. **Open** `main.canvas` in Obsidian

2. **Run command** (Ctrl/Cmd+P):
   - "Project Canvas: Populate from vault"

3. **Run command** again:
   - "Project Canvas: Reposition nodes"

**You should see:**

```mermaid
graph TD
    M[M-001: Learn Obsidian PM<br/>Target: date | Priority: High]
    S1[S-001: Set up<br/>✓ Complete]
    S2[S-002: Create<br/>Not Started]
    T1[T-001: Define goals]
    T2[T-002: Break down]
    T3[T-003: Create stories]
    
    M --> S1
    M --> S2
    S2 --> T1
    S2 --> T2
    S2 --> T3
    
    style M fill:#e3f2fd
    style S1 fill:#c8e6c9
    style S2 fill:#fff9c4
    style T1 fill:#ffe0b2
    style T2 fill:#ffe0b2
    style T3 fill:#ffe0b2
```

---

## Step 5: Iterate and Observe (3 minutes)

Now let's practice the AI → Visual workflow:

### Check Status

Ask AI:
```
What's the status of M-001?
```

### Take Action

Ask AI:
```
Mark T-001 as Complete
```

### Observe Change

1. In Obsidian, **refresh canvas** (close and reopen, or run "Populate from vault" again)
2. **See** T-001 now shows as complete with different styling

### Report Progress

Ask AI:
```
Show me what's left to complete M-001
```

### Move Forward

Ask AI:
```
Mark T-002 as InProgress and assign it to me
```

**Refresh canvas** → See T-002 status updated

---

## 🎉 Success!

You've now:

- [x] Set up MCP Server and Plugin
- [x] Created a 3-tier project structure via AI
- [x] Visualized it on Obsidian canvas
- [x] Iterated: AI action → Visual review

---

## Next Steps

<div class="grid cards" markdown>

-   :material-book-open-variant:{ .lg .middle } __User Guide__

    ---

    Learn all features and workflows

    [:octicons-arrow-right-24: User Guide](../user-guide/overview.md)

-   :material-lightbulb-on:{ .lg .middle } __First Project__

    ---

    Create your first real project

    [:octicons-arrow-right-24: First Project Guide](first-project.md)

-   :material-folder-open:{ .lg .middle } __Sample Project__

    ---

    Explore a complete example project

    [:octicons-arrow-right-24: Sample Vault](../examples/sample-vault.md)

</div>

---

## Troubleshooting

!!! failure "No entities found"
    - Check that `VAULT_PATH` in MCP config points to the correct vault
    - Verify files were created in vault (check `milestones/`, `stories/`, `tasks/` folders)

!!! failure "Canvas is empty"
    - Make sure you ran "Populate from vault" command
    - Check that `main.canvas` file exists
    - Look for errors in Developer Console (Ctrl/Cmd+Shift+I)

!!! failure "MCP tools not available"
    - Restart Claude Desktop / Cursor
    - Check MCP config file syntax (valid JSON)
    - Verify Node.js is installed: `node --version`

For more help, see [FAQ](../faq.md)

