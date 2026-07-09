# Obsidian AI Project Management

> **Manage your projects by talking to AI, visualize them in Obsidian**

A complete AI-native project management system that combines the power of AI assistants with visual project organization in Obsidian.

---

## What is This?

This is a two-component system that lets you:

1. **Talk to AI to manage your project** - Create milestones, stories, tasks, track dependencies, make decisions
2. **Visualize everything in Obsidian** - See your project structure on a canvas, navigate relationships, review progress

```
┌─────────────────────────────────────────────────────────────────────┐
│                        Your Workflow                                │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│   You: "Create a milestone for Q1 launch"                          │
│   AI:  ✓ Created M-001 with 3 stories                              │
│                                                                     │
│   You: Opens Obsidian → Sees visual canvas with M-001              │
│        Reviews structure, checks dependencies                       │
│                                                                     │
│   You: "What's blocking the launch?"                               │
│   AI:  Analyzes dependencies → Reports blockers                     │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## Components

### 1. Canvas Project Manager Plugin
**Visual project management in Obsidian**

- Canvas-based visual organization
- Hierarchical entity layout
- Interactive navigation
- Relationship visualization
- Archive management

📦 **NPM Package:** [@ostanlabs/canvas-project-manager](https://www.npmjs.com/package/@ostanlabs/canvas-project-manager)
📂 **Repository & Releases:** [obsidian_plugin](https://github.com/ostanlabs/obsidian_plugin)

### 2. Bundled MCP Server
**AI-native project management via Model Context Protocol**

- Create and update entities via natural language
- Bulk operations and batch processing
- Project analysis and insights
- Dependency tracking and cycle detection
- Search and filtering

Built from the **same plugin repository** (`npm run build` produces `bin/mcp-server.mjs`) — the plugin and the MCP server share one entity engine and on-disk format. There is no separate MCP package to install.

---

## Quick Links

- **[Quick Start Guide](./guides/QUICK_START.md)** - Get up and running in 15 minutes
- **[Installation Guide](./guides/INSTALLATION.md)** - Detailed setup instructions
- **[User Guide](./guides/USER_GUIDE.md)** - Complete workflows and features
- **[Tutorials](./tutorials/)** - Step-by-step walkthroughs
- **[Sample Project](./examples/sample-vault/)** - Example vault to explore
- **[FAQ](./FAQ.md)** - Common questions and troubleshooting

---

## Key Features

### 🤖 AI-Powered Management
- Natural language project creation
- Intelligent dependency analysis
- Automated relationship syncing
- Bulk operations with dry-run preview

### 📊 Visual Organization
- Hierarchical canvas layout
- Workstream-based lanes
- Entity type differentiation
- Interactive navigation

### 🔗 Seamless Integration
- Shared entity model
- Bidirectional relationship sync
- Real-time updates
- Archive management

### 🎯 Smart Features
- Workstream normalization
- Transitive dependency removal
- Cycle detection
- Feature coverage tracking

---

## Use Cases

- **Solo Development** - Manage personal projects with AI assistance
- **Feature Planning** - Design and track product features
- **Technical Decisions** - Document and track architectural decisions
- **Spec-Driven Development** - AI creates specs, another AI implements
- **Progress Tracking** - Visual review of project status

---

## Documentation Structure

```
obsidian_docs/
├── README.md                    # This file
├── guides/
│   ├── QUICK_START.md          # 15-minute getting started
│   ├── INSTALLATION.md         # Detailed setup
│   └── USER_GUIDE.md           # Complete reference
├── tutorials/
│   ├── 01-first-project.md     # Create your first project
│   ├── 02-ai-workflows.md      # AI collaboration patterns
│   ├── 03-visual-review.md     # Using the canvas
│   └── 04-advanced.md          # Advanced features
├── examples/
│   └── sample-vault/           # Example project structure
├── reference/
│   ├── ENTITY_SCHEMAS.md       # Entity type definitions
│   ├── COMMANDS.md             # Available commands
│   └── WORKFLOWS.md            # Common workflow patterns
└── FAQ.md                       # Troubleshooting
```

---

## Getting Started

1. **[Install the MCP Server](./guides/INSTALLATION.md#mcp-server)** - Connect AI to your vault
2. **[Install the Plugin](./guides/INSTALLATION.md#plugin)** - Enable visual management
3. **[Follow the Quick Start](./guides/QUICK_START.md)** - Create your first project in 15 minutes

---

## Support

- **Issues:** [GitHub Issues](https://github.com/ostanlabs/obsidian_plugin/issues)
- **Discussions:** [GitHub Discussions](https://github.com/ostanlabs/obsidian_plugin/discussions)

---

## License

MIT License - See individual component repositories for details.

