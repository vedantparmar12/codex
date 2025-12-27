---
name: prd-track
description: Create new development track with specification and implementation plan - handles features, bugs, and chores
metadata:
  short-description: Create development track with spec and plan
  version: 2.0.0
  author: Vedant Parmar
  category: track-management
  tags: [prd, track, specification, planning, feature, bug, chore]
  usage: "Invoke with track description (optional): prd-track Add user authentication"
---

# PRD Track Creation Skill

Create a new development track through interactive spec and plan generation. This skill guides you through creating comprehensive specifications and implementation plans for features, bugs, and other work items.

## What This Does

Creates a complete development track including:
- **Specification** (spec.md) - Detailed requirements and acceptance criteria
- **Implementation Plan** (plan.md) - Phased breakdown of tasks and sub-tasks
- **Metadata** (metadata.json) - Track information and status
- **Registry Entry** - Updates tracks.md with new track

## When to Use

- Adding a new feature to your product
- Creating a bug fix task
- Planning a refactor or chore
- Need structured approach to development work
- Want clear acceptance criteria before coding

## How to Invoke

You can invoke this skill with or without a description:

```
$prd-track Add user authentication
```

or

```
$prd-track
```
(Will prompt for description)

## Instructions

### Step 1: Setup Check

**Verify required files exist:**
- `codex/tech-stack.md`
- `codex/workflow.md`
- `codex/product.md`
- `codex/tracks.md`

**If ANY missing:**
> "Codex is not set up. Please run prd-setup skill first."

HALT. Do NOT proceed.

---

### Step 2: Load Context

**Read these files to inform questions:**
1. `codex/product.md` - Product vision
2. `codex/product-guidelines.md` - Design principles
3. `codex/tech-stack.md` - Tech choices
4. `codex/workflow.md` - Development workflow
5. `codex/tracks.md` - Existing tracks

This context will help generate better questions and defaults.

---

### Step 3: Get Track Description

**If description provided as argument:**
- Use it as track description
- Example: `$prd-track "Add user authentication"`

**If no description provided:**
> "Please describe the track you want to create (feature, bug fix, chore, etc.)."

Wait for response.

---

### Step 4: Infer Track Type

Analyze description. Classify as:
- **feature**: New functionality
- **bug**: Fix incorrect behavior
- **chore**: Maintenance, refactoring
- **enhancement**: Improve existing feature
- **docs**: Documentation only

**Do NOT ask user** - infer from description automatically.

**Examples:**
- "Add dark mode" → feature
- "Fix login crash" → bug
- "Refactor auth module" → chore
- "Improve search speed" → enhancement
- "Update API docs" → docs

---

### Step 5: Interactive Spec Generation

#### 5.1 Announce

> "I'll guide you through creating a specification for this track."

#### 5.2 Question Rules

**Ask ONE question at a time. Wait for response.**

**Question Format:**
```
[Question]?

A) [Option A]
B) [Option B]
C) [Option C]
D) Type your own answer
E) Auto-generate and review spec.md
```

**Question Types:**
- **Additive** (multiple answers): Add "(Select all that apply)"
- **Exclusive** (single answer): No qualifier

#### 5.3 Questions for FEATURE Tracks (3-5 questions)

1. **User Impact** (Additive):
   > "Who will benefit from this feature? (Select all that apply)
   >
   > A) End users
   > B) Administrators
   > C) Developers (internal)
   > D) Type your own answer
   > E) Auto-generate and review spec.md"

2. **Core Functionality** (Exclusive):
   > "What is the primary functionality?
   >
   > A) [Inferred from description]
   > B) [Alternative interpretation]
   > C) [Another possibility]
   > D) Type your own answer
   > E) Auto-generate and review spec.md"

3. **User Interface** (Exclusive - if applicable):
   > "Where will users interact with this?
   >
   > A) New page/screen
   > B) Existing page modification
   > C) No UI (backend only)
   > D) Type your own answer
   > E) Auto-generate and review spec.md"

4. **Integration Points** (Additive):
   > "What will this integrate with? (Select all that apply)
   >
   > A) [Component from codebase]
   > B) [Another component]
   > C) External API/service
   > D) Type your own answer
   > E) Auto-generate and review spec.md"

5. **Success Criteria** (Additive):
   > "How will we know this is successful? (Select all that apply)
   >
   > A) Users can complete [action]
   > B) Performance meets [metric]
   > C) Passes all tests
   > D) Type your own answer
   > E) Auto-generate and review spec.md"

#### 5.4 Questions for BUG Tracks (2-3 questions)

1. **Current Behavior**:
   > "What is the incorrect behavior?
   >
   > A) [Inferred from description]
   > B) [Alternative]
   > C) Type your own answer
   > D) Auto-generate and review spec.md"

2. **Expected Behavior**:
   > "What should happen instead?
   >
   > A) [Expected outcome]
   > B) [Alternative outcome]
   > C) Type your own answer
   > D) Auto-generate and review spec.md"

3. **Reproduction Steps**:
   > "How can we reproduce this?
   >
   > A) [Steps based on description]
   > B) Type your own answer
   > C) Auto-generate and review spec.md"

#### 5.5 Questions for CHORE Tracks (2-3 questions)

1. **Scope**:
   > "What is the scope of this work?
   >
   > A) [Inferred scope]
   > B) [Broader scope]
   > C) Type your own answer
   > D) Auto-generate and review spec.md"

2. **Completion Criteria**:
   > "How will we know this is complete?
   >
   > A) [Criteria based on description]
   > B) Type your own answer
   > C) Auto-generate and review spec.md"

#### 5.6 Auto-Generate Logic

**If user selects Option E:**
- Stop asking questions immediately
- Infer remaining details from context + previous answers
- Proceed to draft spec

#### 5.7 Draft & Approve spec.md

1. **Generate spec.md** with sections:
   - Overview
   - User Stories (if applicable)
   - Functional Requirements (Must Have / Should Have / Won't Have)
   - Non-Functional Requirements
   - User Interface (if applicable)
   - API/Integration (if applicable)
   - Acceptance Criteria
   - Technical Considerations
   - Dependencies
   - Risks and Mitigation

2. **Present draft:**
   > "I've drafted the specification. Please review:
   >
   > ```markdown
   > [spec content]
   > ```
   >
   > A) Approve - Proceed to planning
   > B) Suggest Changes"

3. **Loop until approved**

4. **Store spec** (don't write file yet - will write all together in Step 7)

---

### Step 6: Interactive Plan Generation

#### 6.1 Announce

> "Now I'll create an implementation plan based on the approved spec."

#### 6.2 Generate plan.md

**Requirements:**
1. Read approved spec
2. Read `codex/workflow.md` for methodology
3. Create hierarchical plan:
   - **Phases** (major milestones)
   - **Tasks** (within each phase)
   - **Sub-tasks** (within each task)

**Structure Example:**
```markdown
# Implementation Plan: [Track Title]

## Overview
[Brief summary]

## Phases

### Phase 1: [Phase Name]
**Objective**: [What this phase achieves]

#### Tasks
- [ ] **Task 1.1**: [Task name]
  - [ ] Sub-task: [Detail]
  - [ ] Sub-task: [Detail]

- [ ] **Task 1.2**: [Task name]
  - [ ] Sub-task: [Detail]

### Phase 2: [Phase Name]
...
```

**Workflow Integration:**
- If workflow specifies TDD:
  - Add "Write tests" sub-task BEFORE each feature implementation
  - Add "Run tests (expect failure)" → "Implement" → "Run tests (expect success)" → "Refactor"

- **Phase Completion Tasks:**
  - Read `codex/workflow.md` for "Phase Completion Verification Protocol"
  - If protocol exists: Add final task to each phase:
    `- [ ] Task: User Manual Verification '[Phase Name]' (Protocol in workflow.md)`

**Phases should include:**
- Phase objective statement
- Tasks with clear deliverables
- Sub-tasks with actionable steps
- Testing/verification steps
- Dependencies between phases

#### 6.3 Present & Approve

> "I've drafted the implementation plan. Please review:
>
> ```markdown
> [plan content]
> ```
>
> A) Approve - Create track
> B) Suggest Changes"

Loop until approved.

---

### Step 7: Create Track Artifacts

Once spec and plan approved:

#### 7.1 Check for Duplicate

1. List existing tracks: `ls codex/tracks/`
2. Extract short names from track IDs (format: `shortname_YYYYMMDD`)
3. If proposed short name matches existing:
   > "A track with name '{shortname}' already exists. Please choose a different name or resume the existing track."
   HALT.

#### 7.2 Generate Track ID

Format: `{shortname}_{YYYYMMDD}`

**Examples:**
- "Add dark mode" → `dark-mode_20250115`
- "Fix login bug" → `fix-login_20250115`
- "Refactor auth" → `refactor-auth_20250115`

**Rules:**
- Lowercase
- Hyphens (no spaces/underscores in shortname)
- Max 30 chars for shortname
- Use current date

#### 7.3 Create Directory

`mkdir -p codex/tracks/{track_id}/`

#### 7.4 Write metadata.json

```json
{
  "track_id": "{track_id}",
  "type": "{feature|bug|chore|enhancement|docs}",
  "status": "new",
  "created_at": "{ISO 8601 timestamp}",
  "updated_at": "{ISO 8601 timestamp}",
  "description": "{original user description}"
}
```

Write to: `codex/tracks/{track_id}/metadata.json`

#### 7.5 Write spec.md

Write approved spec content to: `codex/tracks/{track_id}/spec.md`

#### 7.6 Write plan.md

Write approved plan content to: `codex/tracks/{track_id}/plan.md`

#### 7.7 Update tracks.md

**Read current `codex/tracks.md`**

**Append:**
```markdown

---

## [ ] Track: {Track Title}

**ID**: `{track_id}`
**Type**: {feature|bug|chore}
**Created**: {YYYY-MM-DD}
**Status**: New

{Brief 1-sentence description}

[View Track Details](./tracks/{track_id}/)
```

**Write updated content** back to `codex/tracks.md`

---

### Step 8: Finalize

#### 8.1 Announce Success

> "New track created successfully!
>
> **Track ID**: `{track_id}`
> **Type**: {type}
>
> Details:
> - Specification: codex/tracks/{track_id}/spec.md
> - Plan: codex/tracks/{track_id}/plan.md
> - Metadata: codex/tracks/{track_id}/metadata.json
>
> Next steps:
> 1. Start implementation: Use prd-implement skill
> 2. Check status: Use prd-status skill
> 3. View plan: Read codex/tracks/{track_id}/plan.md"

#### 8.2 Optional Git Commit

**If user wants to commit:**
```bash
git add codex/tracks/{track_id}/
git add codex/tracks.md
git commit -m "codex(track): Create {track_id}"
```

---

## Critical Rules

1. **Ask ONE question at a time**
2. **Wait for user response** before proceeding
3. **Use only selected answers** (ignore unselected A/B/C options)
4. **Option E stops questions** → auto-generate rest
5. **Max 5 questions** for features, 3 for bugs/chores
6. **Infer track type** - don't ask user
7. **Follow workflow methodology** when creating plans
8. **Check for duplicates** before creating track
9. **All artifacts in single directory**: `codex/tracks/{track_id}/`
10. **Update tracks.md** to register the new track

## References

- See `references/track_management.md` for detailed protocol

## Success Criteria

Track creation is complete when:
- [ ] Track ID generated
- [ ] Directory created: `codex/tracks/{track_id}/`
- [ ] `metadata.json` written with correct structure
- [ ] `spec.md` written with approved content
- [ ] `plan.md` written with approved content
- [ ] `tracks.md` updated with new track entry
- [ ] User notified of success

## Error Handling

- If setup files missing → Direct user to prd-setup skill
- If duplicate track name → Ask for different name
- If file write fails → Report error and retry once
- If context files unreadable → Warn and continue with generic questions
