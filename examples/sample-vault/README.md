# Sample Vault: Task Management SaaS

> **Example project demonstrating the full system capabilities**

This is a complete example vault showing how to manage a SaaS product development project using AI and visual organization.

---

## Project Overview

**Goal:** Build a task management SaaS application

**Timeline:** 12 weeks

**Workstreams:**
- `engineering` - Backend, frontend, infrastructure
- `business` - Product features, user experience
- `design` - UI/UX design

---

## Project Structure

```
Milestone M-001: MVP Foundation (Weeks 1-4)
├── Story S-001: User Authentication ✓ Complete
│   ├── Task T-001: Implement signup endpoint ✓
│   ├── Task T-002: Implement login endpoint ✓
│   └── Task T-003: Add password reset ✓
├── Story S-002: Database Setup ✓ Complete
│   ├── Task T-004: Set up PostgreSQL ✓
│   └── Task T-005: Create schema migrations ✓
└── Story S-003: Basic Task CRUD → In Progress
    ├── Task T-006: Create task endpoint ✓
    ├── Task T-007: Update task endpoint → InProgress
    └── Task T-008: Delete task endpoint → Open

Milestone M-002: Core Features (Weeks 5-8)
├── Story S-004: Project Management → Not Started
│   ├── Task T-009: Create project model → Open
│   ├── Task T-010: Project CRUD endpoints → Open
│   └── Task T-011: Project-task relationships → Open
├── Story S-005: Collaboration Features → Not Started
│   ├── Task T-012: Task assignment → Open
│   ├── Task T-013: Comments system → Open
│   └── Task T-014: Activity feed → Open
└── Story S-006: Search & Filtering → Not Started
    └── Task T-015: Implement search API → Open

Milestone M-003: Polish & Launch (Weeks 9-12)
└── Story S-007: Production Deployment → Not Started
    ├── Task T-016: Set up CI/CD → Open
    ├── Task T-017: Configure monitoring → Open
    └── Task T-018: Launch checklist → Open

Features:
├── Feature F-001: User Management (OSS, MVP) ✓ Complete
│   ├── Implemented by: M-001, S-001
│   └── Documented by: DOC-001
├── Feature F-002: Task Management (OSS, MVP) → In Progress
│   ├── Implemented by: S-003, S-004
│   └── Documented by: DOC-002
└── Feature F-003: Team Collaboration (Premium, Phase 1) → Planned
    ├── Implemented by: S-005
    └── Documented by: DOC-003

Decisions:
├── Decision DEC-001: Use PostgreSQL for database ✓ Decided
│   └── Affects: S-002, S-003, S-004
├── Decision DEC-002: Use JWT for authentication ✓ Decided
│   └── Affects: S-001
└── Decision DEC-003: REST API vs GraphQL → Pending
    └── Affects: S-003, S-004, S-005

Documents:
├── Document DOC-001: Authentication API Spec ✓ Approved
│   └── Implemented by: S-001
├── Document DOC-002: Task Management API Spec → Review
│   └── Implemented by: S-003
└── Document DOC-003: Collaboration Features Spec → Draft
    └── Implemented by: S-005
```

---

## How to Use This Sample

### Option 1: Explore in Place

1. Copy this folder to your Obsidian vault
2. Open `main.canvas` in Obsidian
3. Run "Populate from vault" to see the structure
4. Navigate relationships using the Entity Navigator

### Option 2: Use as Template

1. Copy the folder structure to your vault
2. Modify entity files to match your project
3. Update relationships and dependencies
4. Regenerate canvas layout

### Option 3: Let AI Recreate It

Ask your AI assistant:

```
Create a project structure for a task management SaaS with:
- M-001: MVP Foundation (4 weeks) with stories for auth, database, and basic CRUD
- M-002: Core Features (4 weeks) with stories for projects, collaboration, and search
- M-003: Polish & Launch (4 weeks) with deployment story
- Features for User Management (OSS, MVP), Task Management (OSS, MVP), and Team Collaboration (Premium, Phase 1)
- Decisions for database choice, authentication method
- Spec documents for each major feature
All in workstream "engineering"
```

---

## Key Relationships Demonstrated

### Hierarchy
- Milestones contain Stories
- Stories contain Tasks
- Clear parent-child structure

### Dependencies
- S-003 depends on S-002 (can't create tasks without database)
- T-007 depends on T-006 (update requires create)
- S-005 depends on S-001 (collaboration requires auth)

### Implementation
- Stories implement Features
- Stories implement Documents (specs)

### Decision Impact
- Decisions affect multiple entities
- Track why choices were made

### Documentation
- Features documented by spec documents
- Specs implemented by stories

---

## Files Included

```
sample-vault/
├── README.md                          # This file
├── main.canvas                        # Visual project canvas
├── milestones/
│   ├── M-001_MVP_Foundation.md
│   ├── M-002_Core_Features.md
│   └── M-003_Polish_Launch.md
├── stories/
│   ├── S-001_User_Authentication.md
│   ├── S-002_Database_Setup.md
│   ├── S-003_Basic_Task_CRUD.md
│   ├── S-004_Project_Management.md
│   ├── S-005_Collaboration_Features.md
│   ├── S-006_Search_Filtering.md
│   └── S-007_Production_Deployment.md
├── tasks/
│   ├── T-001_Implement_Signup.md
│   ├── T-002_Implement_Login.md
│   ├── T-003_Password_Reset.md
│   ├── T-004_Setup_PostgreSQL.md
│   ├── T-005_Schema_Migrations.md
│   ├── T-006_Create_Task_Endpoint.md
│   ├── T-007_Update_Task_Endpoint.md
│   ├── T-008_Delete_Task_Endpoint.md
│   └── ... (T-009 through T-018)
├── features/
│   ├── F-001_User_Management.md
│   ├── F-002_Task_Management.md
│   └── F-003_Team_Collaboration.md
├── decisions/
│   ├── DEC-001_PostgreSQL_Database.md
│   ├── DEC-002_JWT_Authentication.md
│   └── DEC-003_REST_vs_GraphQL.md
└── documents/
    ├── DOC-001_Auth_API_Spec.md
    ├── DOC-002_Task_API_Spec.md
    └── DOC-003_Collaboration_Spec.md
```

---

## What You Can Learn

### From This Example

1. **Project Structure** - How to organize a real project
2. **Relationships** - How entities connect and depend on each other
3. **Workstreams** - How to organize by functional area
4. **Features** - How to track product features separately from implementation
5. **Decisions** - How to document architectural choices
6. **Documentation** - How specs drive implementation

### Try These Commands

```
"What's the status of M-001?"
"What's blocking S-003?"
"Show me feature coverage"
"Which features are missing documentation?"
"What decisions affect S-003?"
"Show me all engineering tasks"
"What's left to complete M-002?"
```

---

## Next Steps

After exploring this sample:

1. **Create your own project** - Use this as a template
2. **Follow tutorials** - See [../tutorials/](../tutorials/) for step-by-step guides
3. **Read the User Guide** - Learn all features in [../guides/USER_GUIDE.md](../guides/USER_GUIDE.md)

---

## License

This sample project is provided as-is for learning purposes. Feel free to copy and modify for your own use.

