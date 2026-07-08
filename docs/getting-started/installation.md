# Installation Guide

> **Complete setup instructions for MCP Server and Canvas Project Manager Plugin**

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [MCP Server Installation](#mcp-server-installation)
3. [Plugin Installation](#plugin-installation)
4. [Vault Setup](#vault-setup)
5. [Verification](#verification)

---

## Prerequisites

### Required

- **Obsidian** - Latest version ([download](https://obsidian.md/))
- **Node.js** - Version 18 or later ([download](https://nodejs.org/))
- **AI Assistant** - One of:
  - Claude Code ([docs](https://docs.claude.com/en/docs/claude-code))
  - Claude Desktop ([download](https://claude.ai/download))
  - Cursor IDE ([download](https://cursor.sh/))

### Recommended

- **Git** - For plugin installation from source
- **Code editor** - For editing configuration files (VS Code, etc.)

### Check Your Setup

```bash
# Verify Node.js installation
node --version
# Should show v18.x.x or higher

# Verify npm installation
npm --version
# Should show 9.x.x or higher
```

---

## MCP Server Installation

The MCP server enables AI assistants to manage your project entities. It is the **bundled stdio server** `bin/mcp-server.mjs`, built from the same repository as the plugin — both share one entity engine and on-disk format, so they can work on the same vault concurrently.

!!! warning "Outdated setups"
    Older docs referenced `npx @ostanlabs/obsidian-mcp` or `obsidian-accomplishments-mcp`. These are outdated — the MCP server is now built from the plugin repository as described below.

### Step 1: Build the Server

```bash
git clone https://github.com/ostanlabs/obsidian_plugin.git
cd obsidian_plugin
npm install
npm run build        # builds the plugin AND bin/mcp-server.mjs
```

(You can build only the server with `npm run build:mcp`.)

### Step 2: Register with Your AI Assistant

The server speaks MCP over **stdio** and targets one project folder via the `VAULT_PATH` environment variable — the folder that contains `milestones/`, `stories/`, `tasks/`, etc. On first run it bootstraps a `schema.json` there if none exists.

=== "Claude Code (CLI)"

    ```bash
    claude mcp add obsidian-mcp \
      -e VAULT_PATH="/path/to/vault/Projects/YourProject" \
      -- node /absolute/path/to/obsidian_plugin/bin/mcp-server.mjs
    ```

=== "Claude Desktop / JSON config"

    Configuration file location:

    - **macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`
    - **Windows:** `%APPDATA%\Claude\claude_desktop_config.json`
    - **Linux:** `~/.config/Claude/claude_desktop_config.json`

    ```json
    {
      "mcpServers": {
        "obsidian-mcp": {
          "command": "node",
          "args": ["/absolute/path/to/obsidian_plugin/bin/mcp-server.mjs"],
          "env": { "VAULT_PATH": "/path/to/vault/Projects/YourProject" }
        }
      }
    }
    ```

    If you already have other MCP servers, add the `obsidian-mcp` entry to the existing `mcpServers` object.

=== "Cursor"

    Open Cursor → Settings → Features → MCP Servers (or edit `~/.cursor/mcp_config.json`) and add the same JSON entry as the Claude Desktop tab.

### Step 3: Configure VAULT_PATH

| Variable | Description | Example |
|----------|-------------|---------|
| `VAULT_PATH` | **Absolute path** to your project folder (inside your vault) | `/Users/john/Vault/Projects/MyProject` |

**Important:**
- Use **absolute paths** for `VAULT_PATH`
- Use **forward slashes** (`/`) even on Windows
- No trailing slash on `VAULT_PATH`
- Point at the **project folder** (the one holding the entity folders), not necessarily the vault root

### Step 4: Restart AI Assistant

- **Claude Desktop:** Quit completely and restart
- **Cursor:** Restart the application
- **Claude Code:** Run `/mcp` to (re)connect — also do this after rebuilding the server

### Step 5: Verify Installation

Ask your AI assistant:

```
List available MCP tools
```

You should see tools like:
- `create_entity`
- `update_entity`
- `search_entities`
- `get_project_overview`
- `get_schema`
- etc.

---

## Plugin Installation

The plugin enables visual project management on Obsidian Canvas.

> 📦 **Current version: 1.8.94** — available from [GitHub Releases](https://github.com/ostanlabs/obsidian_plugin/releases), on npm as [@ostanlabs/canvas-project-manager](https://www.npmjs.com/package/@ostanlabs/canvas-project-manager), or built from source.

!!! tip "Already cloned the repo?"
    If you built the MCP server above, `npm run build` already produced the plugin files (`main.js`, `manifest.json`, `styles.css`) in the repo root — skip to Step 2 of Option C.

### Installation Methods

Choose the method that works best for you:

#### **Option A: GitHub Release (Recommended)**

No build required.

**Step 1: Download Release Assets**

Download `main.js`, `manifest.json`, and `styles.css` from the [latest release](https://github.com/ostanlabs/obsidian_plugin/releases).

**Step 2: Copy to Vault**

```bash
# Create plugin directory
mkdir -p /path/to/vault/.obsidian/plugins/canvas-project-manager

# Copy the downloaded files
cp ~/Downloads/{main.js,manifest.json,styles.css} \
  /path/to/vault/.obsidian/plugins/canvas-project-manager/
```

**Replace** `/path/to/vault` with your actual vault path.

**Step 3: Enable Plugin in Obsidian** (see below)

---

#### **Option B: Install from npm**

**Step 1: Install Package**

```bash
npm install @ostanlabs/canvas-project-manager
```

**Step 2: Copy to Vault**

```bash
# Create plugin directory
mkdir -p /path/to/vault/.obsidian/plugins/canvas-project-manager

# Copy built files from node_modules
cp node_modules/@ostanlabs/canvas-project-manager/{main.js,manifest.json,styles.css} \
  /path/to/vault/.obsidian/plugins/canvas-project-manager/
```

**Step 3: Enable Plugin in Obsidian**

1. Open Obsidian
2. Go to **Settings** (gear icon)
3. Navigate to **Community Plugins**
4. Click **Turn on community plugins** (if not already enabled)
5. Find **Canvas Project Manager** in the list
6. Toggle it **ON**

✅ Plugin is now active!

---

#### **Option C: Build from Source (For Development)**

Use this method if you want to contribute or modify the plugin.

> ⚠️ **Note:** Requires Git and build tools. Only use this if you need to modify the plugin code.

**Step 1: Clone Repository**

```bash
git clone https://github.com/ostanlabs/obsidian_plugin.git
cd obsidian_plugin
```

**Step 2: Install Dependencies**

```bash
npm install
```

**Step 3: Build Plugin**

```bash
npm run build
```

This creates three files:
- `main.js` - Plugin code
- `manifest.json` - Plugin metadata
- `styles.css` - Plugin styles

**Step 4: Copy to Vault**

```bash
# Create plugin directory
mkdir -p /path/to/vault/.obsidian/plugins/canvas-project-manager

# Copy built files
cp main.js manifest.json styles.css /path/to/vault/.obsidian/plugins/canvas-project-manager/
```

**Step 5: Enable Plugin in Obsidian

1. Open Obsidian
2. Go to **Settings** (gear icon)
3. Navigate to **Community Plugins**
4. Click **Turn on community plugins** (if not already enabled)
5. Find **Canvas Project Manager** in the list
6. Toggle it **ON**

✅ Plugin is now active!

---

## Vault Setup

### Create Required Folders

The system uses these folders for entity storage:

```bash
cd /path/to/vault

# Create entity folders
mkdir -p milestones
mkdir -p stories
mkdir -p tasks
mkdir -p decisions
mkdir -p documents
mkdir -p features

# Create archive folder
mkdir -p archive
```

### Create Main Canvas

Create a canvas file for visual project management:

1. In Obsidian, create new file: `main.canvas`
2. Or use command palette: "Create new canvas"

---

## Verification

### Test MCP Server

Ask your AI assistant:

```
Create a test milestone called "System Setup"
```

**Expected:**
- AI creates the milestone as `milestones/System_Setup.md` — filenames are **title-only**; the entity ID (`M-001`) lives in the YAML frontmatter, not the filename
- A `schema.json` appears in the project folder on first run (bootstrapped from the built-in default)

### Test Plugin

1. Open `main.canvas` in Obsidian
2. Open command palette (`Ctrl/Cmd+P`)
3. Run: **"Project Canvas: Populate from vault"**
4. Run: **"Project Canvas: Reposition nodes"**

**Expected:**
- M-001 appears on canvas
- Node is positioned and styled correctly

### Test Integration

1. Ask AI: `Create a story under M-001 called "Test Story"`
2. In Obsidian, run: **"Project Canvas: Populate from vault"**
3. Verify S-001 appears on canvas under M-001

✅ **Success!** Both components are working together.

---

## Next Steps

- **[Quick Start Guide](quick-start.md)** - Create your first project in 15 minutes
- **[User Guide](../user-guide/overview.md)** - Learn all features and workflows
- **[Sample Vault](../examples/sample-vault.md)** - Explore example project

---

## Troubleshooting

See [FAQ](../faq.md) for common issues and solutions.

