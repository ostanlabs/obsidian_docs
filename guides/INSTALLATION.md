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

The MCP server enables AI assistants to manage your project entities. Install this **first**.

### Step 1: Locate Configuration File

The configuration file location depends on your AI assistant:

**Claude Desktop:**
- **macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Windows:** `%APPDATA%\Claude\claude_desktop_config.json`
- **Linux:** `~/.config/Claude/claude_desktop_config.json`

**Cursor:**
- Open Cursor → Settings → Features → MCP Servers
- Or edit: `~/.cursor/mcp_config.json`

### Step 2: Add MCP Server Configuration

Open the configuration file and add the obsidian MCP server:

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

**If you already have other MCP servers**, add the obsidian entry to the existing `mcpServers` object:

```json
{
  "mcpServers": {
    "existing-server": {
      "command": "...",
      "args": ["..."]
    },
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

### Step 3: Configure Environment Variables

Replace the placeholder values:

| Variable | Description | Example |
|----------|-------------|---------|
| `VAULT_PATH` | **Absolute path** to your Obsidian vault | `/Users/john/Documents/MyVault` |
| `DEFAULT_CANVAS` | Path to main canvas file (relative to vault) | `main.canvas` or `projects/main.canvas` |

**Important:**
- Use **absolute paths** for `VAULT_PATH`
- Use **forward slashes** (`/`) even on Windows
- No trailing slash on `VAULT_PATH`

**Examples:**

```json
// macOS
"VAULT_PATH": "/Users/john/Documents/ProjectVault"

// Windows (use forward slashes!)
"VAULT_PATH": "C:/Users/John/Documents/ProjectVault"

// Linux
"VAULT_PATH": "/home/john/vault"
```

### Step 4: Restart AI Assistant

- **Claude Desktop:** Quit completely and restart
- **Cursor:** Restart the application

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
- etc.

---

## Plugin Installation

The plugin enables visual project management on Obsidian Canvas.

> ⚠️ **Note:** The plugin is currently under review for the Obsidian Community Plugin directory. For now, install from source.

### Step 1: Clone Repository

```bash
git clone https://github.com/ostanlabs/obsidian_plugin.git
cd obsidian_plugin
```

### Step 2: Install Dependencies

```bash
npm install
```

### Step 3: Build Plugin

```bash
npm run build
```

This creates three files:
- `main.js` - Plugin code
- `manifest.json` - Plugin metadata
- `styles.css` - Plugin styles

### Step 4: Copy to Vault

Create plugin directory and copy files:

```bash
# Create plugin directory
mkdir -p /path/to/vault/.obsidian/plugins/canvas-project-manager

# Copy built files
cp main.js manifest.json styles.css /path/to/vault/.obsidian/plugins/canvas-project-manager/
```

**Replace** `/path/to/vault` with your actual vault path.

**Example:**

```bash
# macOS/Linux
mkdir -p ~/Documents/MyVault/.obsidian/plugins/canvas-project-manager
cp main.js manifest.json styles.css ~/Documents/MyVault/.obsidian/plugins/canvas-project-manager/

# Windows (PowerShell)
New-Item -ItemType Directory -Force -Path "C:\Users\John\Documents\MyVault\.obsidian\plugins\canvas-project-manager"
Copy-Item main.js,manifest.json,styles.css "C:\Users\John\Documents\MyVault\.obsidian\plugins\canvas-project-manager\"
```

### Step 5: Enable Plugin in Obsidian

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
Create a test milestone called "System Setup" with workstream "test"
```

**Expected:**
- AI creates `M-001` in `milestones/` folder
- File contains YAML frontmatter with entity data

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

- **[Quick Start Guide](./QUICK_START.md)** - Create your first project in 15 minutes
- **[User Guide](./USER_GUIDE.md)** - Learn all features and workflows
- **[Sample Vault](../examples/sample-vault/)** - Explore example project

---

## Troubleshooting

See [FAQ](../FAQ.md) for common issues and solutions.

