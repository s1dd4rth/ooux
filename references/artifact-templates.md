# ORCA Artifact Templates

## 1. Nested Object Matrix (NOM)

A grid where both axes list all objects. Each cell describes the relationship between the row object and column object.

```
             | User          | Project       | Task          | Comment       |
-------------|---------------|---------------|---------------|---------------|
User         | —             | has many      | has many      | has many      |
Project      | belongs to 1  | —             | has many      | has many      |
Task         | assigned to 1 | belongs to 1  | —             | has many      |
Comment      | authored by 1 | belongs to 1  | belongs to 1  | —             |
```

### How to Fill It
- Read each cell as: "[Row Object] _____ [Column Object]"
- Use natural language: "has many", "belongs to one", "has one", "relates to many"
- Note cardinality: 1:1, 1:many, many:many
- Mark empty cells with "—" (no direct relationship)
- Highlight interesting/unexpected relationships

### What It Reveals
- Navigation paths (if A has many Bs, users probably navigate from A → B list)
- Join tables needed (many-to-many = needs association object)
- Missing objects (if two objects need a relationship but it feels forced, there might be a mediating object)

---

## 2. CTA Matrix

A grid where rows are objects and columns are user roles. Each cell lists the actions that role can perform on that object.

```
             | Admin              | Member             | Guest              |
-------------|--------------------|--------------------|---------------------|
Project      | Create, Edit,      | View, Edit,        | View                |
             | Delete, Archive    | Comment             |                     |
Task         | Create, Assign,    | Create, Edit,      | View                |
             | Delete, Reassign   | Complete, Comment   |                     |
Comment      | Create, Edit,      | Create, Edit own,  | —                   |
             | Delete any, Pin    | Delete own          |                     |
```

### CTA Categories to Consider
- **CRUD**: Create, Read, Update, Delete
- **Lifecycle**: Submit, Approve, Reject, Publish, Archive, Restore
- **Social**: Share, Follow, Like, Comment, Mention, Invite
- **Organization**: Pin, Star, Bookmark, Tag, Move, Sort, Filter
- **Export**: Download, Print, Export, Copy Link
- **Contra-CTAs**: Undo, Cancel, Unsubscribe, Report, Block, Mute

### Enriching CTAs (Requirements Round)
For each CTA, document:
```
CTA: [Action] a [Object]
Role: [Who can do this]
Why: [User goal / job-to-be-done]
When: [Trigger or prerequisite state]
Where: [Which view/screen]
Edge cases: [Error states, conflicts, limits]
```

---

## 3. Object Map (Card Format)

Each object gets a card. This is the "holy grail" artifact.

```
┌─────────────────────────────────────┐
│  🟦 PROJECT                         │
├─────────────────────────────────────┤
│  Core Attributes:                   │
│  • Name (text, required)            │
│  • Description (rich text)          │
│  • Status (Draft|Active|Archived)   │
│  • Due Date (date)                  │
│  • Cover Image (image)             │
│  • Owner (→ User)                   │
├─────────────────────────────────────┤
│  Nested Objects:                    │
│  1. Tasks (many)                    │
│  2. Comments (many)                 │
│  3. Files (many)                    │
│  4. Members (many → Users)          │
├─────────────────────────────────────┤
│  CTAs:                              │
│  [Primary] Edit | [Secondary] Share │
│  [Tertiary] Archive · Duplicate     │
└─────────────────────────────────────┘
```

### Color Coding Convention
- 🟦 Blue — Core/primary objects
- 🟩 Green — Supporting objects
- 🟨 Yellow — Transactional objects (orders, payments, logs)
- 🟪 Purple — User/role objects
- 🟧 Orange — Content objects

### Attribute Notation
For each attribute, note:
- Name
- Data type (text, number, date, boolean, enum, rich text, image, reference)
- Required? (mark with *)
- Source: user-input | system-generated | computed | imported

---

## 4. Object Guide (Requirements Round)

A structured document for each object. Think of it as a hyped-up glossary entry.

```markdown
## Object: Project

**Definition:** A container for organized work toward a specific goal, 
owned by a user and containing tasks, files, and discussions.

**What makes it unique:** Project name + Owner (composite key in UX terms)

**Lifecycle:**
- Created by: Admin or Member
- States: Draft → Active → On Hold → Completed → Archived
- Deleted: Soft delete, recoverable for 30 days

**Permissions:**
| Action    | Admin | Member | Guest |
|-----------|-------|--------|-------|
| Create    | ✓     | ✓      | ✗     |
| View      | ✓     | ✓      | ✓     |
| Edit      | ✓     | ✓*     | ✗     |
| Delete    | ✓     | ✗      | ✗     |

*Members can only edit projects they own

**Edge Cases:**
- Empty project (no tasks): Show empty state with CTA to create first task
- At scale (1000+ tasks): Needs pagination/filtering
- Shared across teams: Permission inheritance model needed
- Naming conflicts: Allow duplicate names? Or enforce uniqueness per owner?

**Related Questions (Parking Lot):**
- Can a project belong to multiple teams?
- Do archived projects still show in search?
- What happens to tasks when a project is deleted?
```

---

## 5. Prioritized Object Map

After the Prioritization Round, annotate each object with:

```
Object: PROJECT ✅ Keep (P1 — Core)
Object: TASK ✅ Keep (P1 — Core)  
Object: COMMENT ✅ Keep (P2 — Important)
Object: TAG ⬇️ Downgrade to attribute (metadata on objects)
Object: TEMPLATE 🚫 Eliminate (V2 — defer)
Object: ACTIVITY LOG ⬇️ Downgrade to metadata (system-generated)
```

Priority levels:
- **P1 Core**: Must have for MVP. Product doesn't work without it.
- **P2 Important**: Significantly improves experience. Target for launch.
- **P3 Nice-to-have**: Can defer. V2 or later.
- **Eliminate**: Not needed. Remove from scope.
- **Downgrade**: Was an object, now treat as attribute/metadata.
- **Combine**: Merge with another object (e.g., "Note" + "Comment" → "Comment").
