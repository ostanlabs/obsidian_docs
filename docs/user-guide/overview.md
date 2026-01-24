# User Guide

> **Complete reference for AI-native project management with Obsidian**

---

## Table of Contents

1. [Core Concepts](#core-concepts)
2. [Entity Types](#entity-types)
3. [Workflow Patterns](#workflow-patterns)
4. [AI Commands](#ai-commands)
5. [Plugin Features](#plugin-features)
6. [Killer Features](#killer-features)
7. [Best Practices](#best-practices)

---

## Core Concepts

### The AI-Human Collaboration Model

This system is designed around a specific workflow:

```
┌─────────────────────────────────────────────────────────────────┐
│                    Recommended Workflow                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  1. Human iterates on design with AI                           │
│     ↓                                                           │
│  2. AI plans the project structure                             │
│     ↓                                                           │
│  3. AI creates specifications (on demand)                       │
│     ↓                                                           │
│  4. Another AI agent implements according to spec              │
│     ↓                                                           │
│  5. Human reviews progress and structure visually              │
│     ↓                                                           │
│  6. Repeat                                                      │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**Key Principle:**
- **AI does the work** (creating, updating, analyzing)
- **Human reviews visually** (canvas, relationships, progress)

### Entity Hierarchy

Projects are organized in a 3-tier hierarchy:

```
Milestone (M-xxx)     ← High-level goals, deliverables
└── Story (S-xxx)     ← Features, user stories
    └── Task (T-xxx)  ← Actionable work items

Plus supporting entities:

Feature (F-xxx)       ← Product features with tier/phase tracking
Decision (DEC-xxx)    ← Architectural decisions
Document (DOC-xxx)    ← Specs, ADRs, guides
```

### Workstreams

All entities belong to a **workstream** (organizational grouping):

- `engineering` - Technical development
- `business` - Business logic, product
- `infra` - Infrastructure, DevOps
- `research` - R&D, experiments
- `design` - UX/UI design
- `marketing` - Marketing, content

**Workstream Normalization:**
The system automatically normalizes workstream names:
- `infrastructure`, `infra` → `infra`
- `eng`, `dev`, `development` → `engineering`
- `biz` → `business`
- `ops`, `operations` → `operations`
- `r&d`, `rnd` → `research`
- `ux`, `ui` → `design`
- `mktg` → `marketing`

### Relationships

Entities can have multiple types of relationships:

| Relationship | Forward | Reverse | Description |
|--------------|---------|---------|-------------|
| **Hierarchy** | `parent` | `children` | Parent-child structure |
| **Dependency** | `depends_on` | `blocks` | Task dependencies |
| **Implementation** | `implements` | `implemented_by` | Story implements Feature/Doc |
| **Documentation** | `documents` | `documented_by` | Doc describes Feature |
| **Decision Impact** | - | `affects` | Decision affects entities |
| **Supersession** | `supersedes` | `superseded_by` | Replaces older version |

**All relationships are bidirectional and auto-synced!**

When you set `S-001.parent = M-001`, the system automatically adds `S-001` to `M-001.children`.

---

## Entity Types

### Milestone (M-xxx)

High-level project goals and deliverables.

**Key Fields:**
```yaml
id: M-001
type: milestone
title: "Q1 Product Launch"
status: In Progress              # Not Started | In Progress | Completed | Blocked
workstream: engineering
priority: High                   # Low | Medium | High | Critical
target_date: 2026-03-31
owner: john
children: [S-001, S-002]        # Auto-synced
```

**Use Cases:**
- Quarterly goals
- Product releases
- Major features
- Project phases

### Story (S-xxx)

User stories or feature descriptions.

**Key Fields:**
```yaml
id: S-001
type: story
title: "User Authentication"
status: In Progress
workstream: engineering
priority: High
parent: M-001                    # Parent milestone
children: [T-001, T-002]        # Auto-synced
depends_on: [S-002]             # Dependencies
acceptance_criteria:
  - Users can sign up with email
  - Users can log in
  - Password reset works
```

**Use Cases:**
- User stories
- Feature implementations
- Technical improvements
- Bug fixes

### Task (T-xxx)

Actionable work items.

**Key Fields:**
```yaml
id: T-001
type: task
title: "Implement login endpoint"
status: InProgress              # Open | InProgress | Complete | OnHold
workstream: engineering
parent: S-001                   # Parent story
assignee: john
estimate_hrs: 4
depends_on: [T-002]
```

**Use Cases:**
- Implementation tasks
- Testing tasks
- Documentation tasks
- Code reviews

### Decision (DEC-xxx)

Architectural or design decisions.

**Key Fields:**
```yaml
id: DEC-001
type: decision
title: "Use PostgreSQL for primary database"
status: Decided                 # Pending | Decided | Superseded
workstream: engineering
decided_by: john
decided_on: 2026-01-15
affects: [S-001, S-003, DOC-001]  # Entities affected by this decision
context: "Need reliable ACID transactions..."
decision: "Use PostgreSQL 15..."
consequences: "Team needs to learn SQL..."
```

**Use Cases:**
- Technology choices
- Architecture decisions
- Design patterns
- Process changes

### Document (DOC-xxx)

Technical specs, ADRs, guides.

**Key Fields:**
```yaml
id: DOC-001
type: document
title: "API Design Specification"
status: Approved                # Draft | Review | Approved | Superseded
workstream: engineering
doc_type: spec                  # spec | adr | guide | reference
version: "1.0"
implemented_by: [S-001, S-002]  # Stories implementing this doc
documents: [F-001]              # Features this documents
```

**Use Cases:**
- API specifications
- Architecture Decision Records (ADRs)
- Design documents
- Technical guides

### Feature (F-xxx)

Product features with tier/phase classification.

**Key Fields:**
```yaml
id: F-001
type: feature
title: "Workflow Execution"
status: Complete                # Planned | In Progress | Complete | Deferred
workstream: engineering
tier: OSS                       # OSS | Premium
phase: MVP                      # MVP | 0 | 1 | 2 | 3 | 4 | 5
priority: High
user_story: "As a developer, I want to..."
implemented_by: [M-001, S-001]  # Implementation
documented_by: [DOC-001]        # Documentation
```

**Use Cases:**
- Product roadmap planning
- Feature tracking
- Coverage analysis
- Release planning

---

## Workflow Patterns

### Pattern 1: Design Iteration

**Goal:** Refine project design with AI before implementation

```
Human: "I want to build a task management app"
AI:    "Let me help you design it. What are the core features?"
Human: "Users, tasks, projects, and tags"
AI:    "Should we include collaboration features?"
Human: "Yes, but keep it simple for MVP"
AI:    "Here's a proposed structure..."
       [AI suggests milestones and stories]
Human: "Looks good, but split authentication into its own milestone"
AI:    "Updated structure..."
```

**Review:** Open canvas to see proposed structure visually

### Pattern 2: AI Planning

**Goal:** Let AI create the full project structure

```
Human: "Create a project for building a REST API with authentication,
        user management, and data persistence. Target completion in 8 weeks."

AI:    ✓ Created M-001: Authentication System (Week 1-2)
       ✓ Created M-002: User Management (Week 3-4)
       ✓ Created M-003: Data Layer (Week 5-6)
       ✓ Created M-004: API Endpoints (Week 7-8)
       ✓ Created 12 stories across milestones
       ✓ Created 45 tasks with dependencies
```

**Review:** Open canvas, run "Populate from vault" and "Reposition nodes"

### Pattern 3: Spec-Driven Development

**Goal:** AI creates specs, another AI implements

```
Human: "Create a spec for the authentication API"
AI:    ✓ Created DOC-001: Authentication API Specification
       - Endpoints defined
       - Request/response schemas
       - Error handling
       - Security considerations

[Switch to implementation AI agent]

Human: "Implement DOC-001"
AI:    [Reads spec, implements code]
       ✓ Created S-001: Implement auth endpoints (implements DOC-001)
       ✓ Created T-001: POST /auth/signup
       ✓ Created T-002: POST /auth/login
       ✓ Created T-003: POST /auth/refresh
       ✓ All tasks linked to S-001
```

**Review:** Check canvas to see implementation structure

### Pattern 4: Progress Tracking

**Goal:** Visual review of project status

```
Human: "What's the status of M-001?"
AI:    M-001: Authentication System
       Status: In Progress (60% complete)
       - S-001: Complete ✓
       - S-002: In Progress (2/3 tasks done)
       - S-003: Not Started

       Blockers: None
```

**Review:** Open canvas to see visual progress, check dependencies

---

## AI Commands

### Project Overview

```
"What's the status of my project?"
"Show me the engineering workstream status"
"What items are blocked?"
"Analyze the project and identify risks"
```

### Creating Entities

```
"Create a milestone for Q1 launch with target date March 31"
"Create a story under M-001 for user authentication"
"Add a task to S-003 for writing unit tests"
"Create a decision about using PostgreSQL vs MongoDB"
"Create a spec document for the API design"
```

### Updating Entities

```
"Mark T-005 as completed"
"Update S-002 status to In Progress"
"Assign T-003 to john"
"Set M-001 priority to Critical"
"Archive M-002"
```

### Dependencies

```
"S-004 depends on S-002 and S-003"
"What's blocking S-006?"
"Show me the dependency graph for M-001"
"Remove the dependency between T-001 and T-002"
```

### Decisions & Documents

```
"What decisions have been made about authentication?"
"Is DOC-003 up to date with recent decisions?"
"Create an ADR for the database choice"
"Link DEC-001 to S-005"
```

### Features

```
"What's the feature coverage for Phase 2?"
"Which features are missing documentation?"
"Show me F-001's implementation status"
"Create a feature for user onboarding in tier OSS, phase MVP"
```

### Batch Operations

```
"Archive all completed stories in M-001"
"Create 5 tasks for S-002: [list of tasks]"
"Update all engineering tasks to assign them to john"
```

### Analysis

```
"What are the biggest risks in this project?"
"Which workstream has the most blocked items?"
"Show me features without implementation"
"Find circular dependencies"
```

---

## Plugin Features

### Canvas Commands

Access via Command Palette (`Ctrl/Cmd+P`):

| Command | Description |
|---------|-------------|
| **Populate from vault** | Scan vault and add entities to canvas |
| **Reposition nodes** | Apply hierarchical layout algorithm |
| **Sync edges to dependencies** | Update frontmatter from canvas edges |
| **Remove duplicate nodes** | Clean up duplicate entity nodes |
| **Auto-layout features** | Layout feature entities by tier/phase |

### Entity Navigator

Navigate relationships via context menu (right-click on entity):

```
┌─────────────────────────────────┐
│ S-001: User Authentication      │
├─────────────────────────────────┤
│ Go to Parent (M-001)         ▶  │
│ Go to Children               ▶  │
│   └─ T-001: Login endpoint      │
│   └─ T-002: Signup endpoint     │
│ Go to Dependencies           ▶  │
│   └─ S-002: Database setup      │
│ Go to Documents              ▶  │
│   └─ DOC-001: Auth spec         │
│ Go to Decisions              ▶  │
│   └─ DEC-001: JWT tokens        │
└─────────────────────────────────┘
```

**Keyboard Shortcuts:**
- `Ctrl/Cmd+Shift+P` - Go to parent
- `Ctrl/Cmd+Shift+D` - Go to documents
- `Ctrl/Cmd+Shift+E` - Go to decisions

### Visual Styling

Entities are visually differentiated on canvas:

| Entity | Border | Color | Icon |
|--------|--------|-------|------|
| Milestone | 3px solid | Purple | 🎯 |
| Story | 2px solid | Blue | 📖 |
| Task | 1px solid | Green | ✓ |
| Decision | 2px dashed | Orange | ⚖️ |
| Document | 1px dotted | Yellow | 📄 |
| Feature | 2px solid | Cyan | ⭐ |

**Workstream Lanes:**
Entities are organized in horizontal lanes by workstream:

```
┌─────────────────────────────────────────────────────────┐
│                   Engineering Lane                      │
│  [M-001] ──┬── [S-001] ──┬── [T-001]                   │
│            └── [S-002]   └── [T-002]                    │
├─────────────────────────────────────────────────────────┤
│                   Business Lane                         │
│  [M-002] ──┬── [S-003]                                  │
│            └── [S-004]                                  │
└─────────────────────────────────────────────────────────┘
```

### Archive System

Entities with `status: archived` are automatically:
- Moved to `archive/{type}/` folders (e.g., `archive/milestones/`)
- Removed from canvas
- Excluded from searches and vault scans

**Archive via AI:**
```
"Archive M-001"
"Archive all completed stories"
```

**Restore via AI:**
```
"Restore M-001"
```

---

## Killer Features

### 1. Automatic Relationship Syncing

**What it does:** Maintains bidirectional relationships automatically

**Example:**
```
AI: "Set S-001 parent to M-001"
```

**Behind the scenes:**
- Updates `S-001.parent = M-001`
- Automatically adds `S-001` to `M-001.children`
- Updates both markdown files
- Syncs canvas edges

**Why it's killer:** You never have to manually maintain both sides of a relationship!

### 2. Transitive Dependency Removal

**What it does:** Removes redundant dependencies in the graph

**Example:**
```
Before:
  C depends on B
  C depends on A
  B depends on A

After (automatic cleanup):
  C depends on B
  B depends on A
  (C→A removed as redundant)
```

**Why it's killer:** Keeps dependency graphs clean and understandable!

### 3. Workstream Normalization

**What it does:** Automatically normalizes workstream names

**Example:**
```
You say: "Create milestone in workstream infrastructure"
System:  ✓ Created with workstream "infra"
         (Normalized: infrastructure → infra)
```

**Why it's killer:** Consistent organization without thinking about it!

### 4. Cycle Detection

**What it does:** Prevents circular dependencies

**Example:**
```
AI: "Make S-001 depend on S-002"
    [S-002 already depends on S-001]

System: ❌ Error: This would create a cycle!
        S-001 → S-002 → S-001
```

**Why it's killer:** Catches logical errors before they cause problems!

### 5. Feature Coverage Tracking

**What it does:** Tracks which features have implementation and documentation

**Example:**
```
AI: "Show feature coverage for Phase MVP"

Response:
  F-001: Workflow Execution
    ✓ Implementation: M-001, S-001, S-002
    ✓ Documentation: DOC-001
    ✓ Tests: 3 files
    Coverage: 100%

  F-002: Tool Invocation
    ✓ Implementation: S-003
    ❌ Documentation: Missing
    ⚠️  Tests: 1 file
    Coverage: 60%
```

**Why it's killer:** Know exactly what's done and what's missing!

### 6. Bulk Operations with Dry-Run

**What it does:** Preview bulk changes before applying

**Example:**
```
AI: "Archive all completed stories in M-001 (dry run)"

Preview:
  Would archive:
  - S-001: User Authentication
  - S-003: Database Setup

  Would update:
  - M-001.children (remove S-001, S-003)

Confirm? (yes/no)
```

**Why it's killer:** Safe bulk operations with preview!

---

## Best Practices

### 1. Use AI for Creation, Plugin for Review

✅ **Good:**
```
1. Ask AI to create project structure
2. Open canvas to review visually
3. Ask AI to adjust based on what you see
4. Refresh canvas to verify
```

❌ **Avoid:**
```
Manually creating entities in Obsidian
(Use AI instead - it's faster and maintains relationships)
```

### 2. Start with High-Level, Let AI Fill Details

✅ **Good:**
```
"Create a milestone for user authentication with stories for
signup, login, and password reset"
```

❌ **Avoid:**
```
"Create milestone M-001"
"Create story S-001"
"Create story S-002"
...
(Let AI create the structure in one go)
```

### 3. Use Workstreams Consistently

✅ **Good:**
```
All auth-related work: workstream "engineering"
All product decisions: workstream "business"
```

❌ **Avoid:**
```
Mixing workstreams randomly
(Makes visual organization confusing)
```

### 4. Document Decisions as You Make Them

✅ **Good:**
```
"Create a decision about using JWT for authentication,
affecting S-001 and S-002"
```

❌ **Avoid:**
```
Making decisions without documenting them
(You'll forget why you chose something)
```

### 5. Use Dependencies to Show Blockers

✅ **Good:**
```
"S-003 depends on S-001 and S-002"
(Now you can ask "What's blocking S-003?")
```

❌ **Avoid:**
```
Not linking dependencies
(Can't track what's blocking what)
```

### 6. Archive Completed Work

✅ **Good:**
```
"Archive all completed stories in M-001"
(Keeps canvas clean and focused)
```

❌ **Avoid:**
```
Leaving completed work on canvas
(Visual clutter makes it hard to see what's active)
```

### 7. Use Features for Product Planning

✅ **Good:**
```
Create features for product roadmap
Link stories to features via "implements"
Track coverage with "Show feature coverage"
```

❌ **Avoid:**
```
Planning features without tracking implementation
(Can't see what's done vs planned)
```

---

## Next Steps

- **[AI Workflows](ai-workflows.md)** - Learn AI collaboration patterns
- **[Sample Project](../examples/sample-vault.md)** - Explore example vault
- **[FAQ](../faq.md)** - Common questions and troubleshooting

---

## Need Help?

- **Issues:** [GitHub Issues](https://github.com/ostanlabs/obsidian_plugin/issues)
- **Discussions:** [GitHub Discussions](https://github.com/ostanlabs/obsidian_plugin/discussions)
