# Protocol: Track Management

**Trigger:** User asks to "start new track", "create feature", "/newTrack", or "check status" ("/status").

**Goal:** Create and manage development tracks (features, bugs, chores) with comprehensive specifications and detailed implementation plans.

---

## 1.0 SYSTEM DIRECTIVE

You are executing the **Track Management Protocol** for the Codex Agent framework. This protocol handles the creation, specification, planning, and status tracking of all development work.

**CRITICAL**: You must validate the success of every operation. If any operation fails, you MUST halt immediately, announce the failure to the user, and await further instructions.

---

## 2.0 NEW TRACK CREATION

**PROTOCOL: Create a new track with spec and plan through interactive dialogue.**

---

### 2.1 SETUP CHECK

**CRITICAL**: Before proceeding, verify the Codex environment is properly set up.

1. **Check for Required Files**:
   - `codex/tech-stack.md`
   - `codex/workflow.md`
   - `codex/product.md`
   - `codex/tracks.md`

2. **Handle Missing Files**:
   - If ANY required files are missing:
     > "Codex is not set up. Please run `/setup` to initialize the environment first."
   - HALT the operation
   - Do NOT proceed to track creation

3. **If All Files Exist**: Proceed to Section 2.2

---

### 2.2 GET TRACK DESCRIPTION AND DETERMINE TYPE

#### 2.2.1 Load Project Context

**Before asking any questions, read and understand:**
1. `codex/product.md` - Product vision and goals
2. `codex/product-guidelines.md` - Design principles
3. `codex/tech-stack.md` - Technology choices
4. `codex/workflow.md` - Development workflow
5. `codex/tracks.md` - Existing tracks

**This context informs all subsequent questions and planning.**

#### 2.2.2 Get Track Description

1. **If `{{args}}` contains a description**:
   - Use the content of `{{args}}` as the track description
   - Example: `/newTrack "Add user authentication"`

2. **If `{{args}}` is empty**:
   - Ask the user:
     > "Please describe the track you want to create (feature, bug fix, chore, refactor, etc.)."
   - **CRITICAL**: Wait for user response before proceeding
   - Use their response as the track description

#### 2.2.3 Infer Track Type

Analyze the description and classify as one of:
- **Feature**: New functionality or capability
- **Bug**: Fixing incorrect behavior
- **Chore**: Maintenance, refactoring, or non-functional improvement
- **Enhancement**: Improvement to existing feature
- **Documentation**: Documentation-only changes

**Do NOT ask the user to classify** - infer it from the description.

**Examples**:
- "Add dark mode toggle" → Feature
- "Fix login button not working" → Bug
- "Refactor authentication module" → Chore
- "Improve search performance" → Enhancement
- "Update API documentation" → Documentation

---

### 2.3 INTERACTIVE SPECIFICATION GENERATION (`spec.md`)

**GOAL**: Create a comprehensive specification through guided conversation.

#### 2.3.1 Announce Goal

> "I'll now guide you through creating a detailed specification for this track. This will ensure we have a clear understanding of what needs to be built."

#### 2.3.2 Questioning Phase

**CRITICAL RULES**:
1. **Ask questions ONE AT A TIME** - Wait for response after each question
2. **Tailor questions to track type** (Feature vs Bug vs Chore)
3. **Reference project context** in questions
4. **Provide multiple-choice options** when possible
5. **Always include**:
   - Option for custom answer
   - Option to auto-generate spec

#### 2.3.3 Question Guidelines

**Classify Each Question**:
- **Additive**: Allows multiple answers (use "Select all that apply")
- **Exclusive Choice**: Requires single answer

**Question Format**:

**For Additive Questions**:
> [Question]? **(Select all that apply)**
>
> A) [Option A]
> B) [Option B]
> C) [Option C]
> D) Type your own answer
> E) Auto-generate and review spec.md
>
> Please respond with your selection(s).

**For Exclusive Choice Questions**:
> [Question]?
>
> A) [Option A] (Recommended)
> B) [Option B]
> C) [Option C]
> D) Type your own answer
> E) Auto-generate and review spec.md
>
> Please respond with A, B, C, D, or E.

#### 2.3.4 Questions for FEATURE Tracks

**Ask 3-5 relevant questions** such as:

1. **User Impact** (Additive):
   > "Who will benefit from this feature? (Select all that apply)"
   >
   > A) End users
   > B) Administrators
   > C) Developers (internal use)
   > D) Type your own answer
   > E) Auto-generate and review spec.md

2. **Core Functionality** (Exclusive):
   > "What is the primary functionality this feature provides?"
   >
   > A) [Inferred from description]
   > B) [Alternative interpretation]
   > C) [Another possibility]
   > D) Type your own answer
   > E) Auto-generate and review spec.md

3. **User Interface** (Exclusive - if applicable):
   > "Where will users interact with this feature?"
   >
   > A) New page/screen
   > B) Existing page/screen modification
   > C) No UI (backend only)
   > D) Type your own answer
   > E) Auto-generate and review spec.md

4. **Integration Points** (Additive):
   > "What existing systems or features will this integrate with? (Select all that apply)"
   >
   > A) [Component from codebase analysis]
   > B) [Another component]
   > C) External API/service
   > D) Type your own answer
   > E) Auto-generate and review spec.md

5. **Success Criteria** (Additive):
   > "How will we know this feature is successful? (Select all that apply)"
   >
   > A) Users can complete [specific action]
   > B) Performance meets [specific metric]
   > C) Passes all acceptance tests
   > D) Type your own answer
   > E) Auto-generate and review spec.md

#### 2.3.5 Questions for BUG Tracks

**Ask 2-3 relevant questions** such as:

1. **Bug Description** (Exclusive):
   > "What is the current incorrect behavior?"
   >
   > A) [Inferred from description]
   > B) [Alternative interpretation]
   > C) Type your own answer
   > D) Auto-generate and review spec.md

2. **Expected Behavior** (Exclusive):
   > "What should happen instead?"
   >
   > A) [Inferred expected behavior]
   > B) [Alternative expected behavior]
   > C) Type your own answer
   > D) Auto-generate and review spec.md

3. **Reproduction Steps** (Exclusive):
   > "How can we reproduce this bug?"
   >
   > A) [Steps inferred from description]
   > B) [Alternative reproduction path]
   > C) Type your own answer
   > D) Auto-generate and review spec.md

#### 2.3.6 Questions for CHORE Tracks

**Ask 2-3 relevant questions** such as:

1. **Scope** (Exclusive):
   > "What is the scope of this work?"
   >
   > A) Single file/component
   > B) Multiple related files
   > C) System-wide change
   > D) Type your own answer
   > E) Auto-generate and review spec.md

2. **Success Criteria** (Additive):
   > "How will we know this is complete? (Select all that apply)"
   >
   > A) Code is cleaner/more maintainable
   > B) Tests still pass
   > C) Performance improved
   > D) Type your own answer
   > E) Auto-generate and review spec.md

#### 2.3.7 Auto-Generation Logic

If user selects option **E** at any point:
1. **Stop asking questions immediately**
2. **Infer remaining details** from:
   - User's previous answers
   - Track description
   - Project context (product.md, tech-stack.md, etc.)
   - Existing codebase patterns
3. **Generate comprehensive spec.md**
4. **Skip to Section 2.3.9** (User Confirmation)

#### 2.3.8 Draft Specification

**CRITICAL**: Use ONLY the user's selected answers as source of truth. Do NOT include:
- The questions you asked
- Unselected options (A/B/C/D)
- Conversational markers

**Generate spec.md with structure based on track type:**

**For FEATURE**:
```markdown
# Specification: [Track Title]

## Overview

[High-level description of the feature and its purpose]

## User Stories

[User-focused descriptions of functionality]
- As a [user type], I want to [action], so that [benefit]

## Functional Requirements

### Must Have
1. [Requirement 1]
2. [Requirement 2]
3. [Requirement 3]

### Should Have
1. [Nice-to-have requirement 1]
2. [Nice-to-have requirement 2]

### Won't Have (Out of Scope)
1. [Explicitly excluded items]

## Non-Functional Requirements

- **Performance**: [Performance criteria]
- **Security**: [Security considerations]
- **Accessibility**: [Accessibility requirements]
- **Compatibility**: [Browser/device support]

## User Interface (if applicable)

[Description of UI changes, wireframes reference, etc.]

## API/Integration (if applicable)

[API endpoints, data models, integration points]

## Acceptance Criteria

- [ ] [Testable criterion 1]
- [ ] [Testable criterion 2]
- [ ] [Testable criterion 3]

## Technical Considerations

[Architecture decisions, technology choices, constraints]

## Dependencies

[Other tracks, external services, or systems this depends on]

## Risks and Mitigation

[Potential risks and how to address them]
```

**For BUG**:
```markdown
# Bug Fix: [Bug Title]

## Problem Description

[Clear description of the incorrect behavior]

## Expected Behavior

[What should happen instead]

## Current Behavior

[What actually happens]

## Reproduction Steps

1. [Step 1]
2. [Step 2]
3. [Step 3]

## Root Cause (if known)

[Analysis of why the bug occurs]

## Proposed Solution

[How the bug will be fixed]

## Acceptance Criteria

- [ ] Bug is no longer reproducible
- [ ] All existing tests pass
- [ ] New test added to prevent regression
- [ ] Related functionality still works

## Impact

- **Severity**: [Critical/High/Medium/Low]
- **Affected Users**: [Who is impacted]
- **Workaround**: [Temporary workaround if available]
```

**For CHORE**:
```markdown
# Chore: [Task Title]

## Objective

[What is being improved/refactored/maintained]

## Motivation

[Why this work is necessary]

## Scope

[What will be changed]

## Out of Scope

[What will NOT be changed]

## Success Criteria

- [ ] [Criterion 1]
- [ ] [Criterion 2]
- [ ] All tests pass
- [ ] No functional regressions

## Technical Approach

[How the work will be done]

## Risks

[Potential issues and mitigation strategies]
```

#### 2.3.9 User Confirmation Loop

Present the drafted specification:

> "I've drafted the specification for this track. Please review:
>
> ```markdown
> [Full drafted spec.md content]
> ```
>
> What would you like to do?
> A) Approve - This looks good, proceed to planning
> B) Suggest Changes - I'll tell you what to modify
>
> Please respond with A or B."

**Loop Logic**:
- If **B**:
  - Ask: "What changes would you like me to make?"
  - Wait for response
  - Apply changes to spec
  - Re-present the updated spec
  - Repeat until user approves
- If **A**: Proceed to Section 2.4

---

### 2.4 INTERACTIVE PLAN GENERATION (`plan.md`)

**GOAL**: Create a detailed, actionable implementation plan.

#### 2.4.1 Announce Goal

> "Now I'll create an implementation plan based on the approved specification. This plan will break down the work into phases, tasks, and sub-tasks."

#### 2.4.2 Read Context

**Before generating the plan, read:**
1. The confirmed `spec.md` content (from previous step)
2. `codex/workflow.md` - To understand testing methodology (TDD, etc.)
3. `codex/tech-stack.md` - To understand technology choices
4. `codex/product-guidelines.md` - To understand quality standards

#### 2.4.3 Generate Plan Structure

**CRITICAL**: The plan MUST follow the workflow defined in `codex/workflow.md`.

**Example for TDD Workflow**:
- Each feature task should have:
  1. Write test for feature
  2. Run test (should fail)
  3. Implement feature
  4. Run test (should pass)
  5. Refactor if needed

**Plan Hierarchy**:
```
Phase (Major milestone)
├── Task (Specific deliverable)
│   ├── Sub-task (Implementation step)
│   ├── Sub-task (Implementation step)
│   └── Sub-task (Implementation step)
└── Task (Verification/Checkpoint)
```

**Status Markers**:
- `[ ]` - Not started
- `[~]` - In progress
- `[x]` - Completed

#### 2.4.4 Plan Template Structure

```markdown
# Implementation Plan: [Track Title]

## Overview

[Brief summary of implementation approach]

## Phases

### Phase 1: [Phase Name - e.g., Setup and Preparation]

**Objective**: [What this phase accomplishes]

#### Tasks

- [ ] **Task 1.1**: [Task description]
  - [ ] Sub-task: [Detailed step]
  - [ ] Sub-task: [Detailed step]
  - [ ] Sub-task: [Detailed step]

- [ ] **Task 1.2**: [Task description]
  - [ ] Sub-task: [Detailed step]
  - [ ] Sub-task: [Detailed step]

- [ ] **Task 1.3**: Phase 1 Verification
  - [ ] Sub-task: Run all tests
  - [ ] Sub-task: Verify acceptance criteria from spec
  - [ ] Sub-task: Manual verification checkpoint (user)

---

### Phase 2: [Phase Name - e.g., Core Implementation]

**Objective**: [What this phase accomplishes]

#### Tasks

- [ ] **Task 2.1**: [Task description following workflow]
  - [ ] Sub-task: Write test for [functionality]
  - [ ] Sub-task: Run test (expect failure)
  - [ ] Sub-task: Implement [functionality]
  - [ ] Sub-task: Run test (expect success)
  - [ ] Sub-task: Refactor if needed

- [ ] **Task 2.2**: [Next task]
  - [ ] Sub-task: [Step]
  - [ ] Sub-task: [Step]

- [ ] **Task 2.3**: Phase 2 Verification
  - [ ] Sub-task: Run full test suite
  - [ ] Sub-task: Verify phase objectives met
  - [ ] Sub-task: Manual verification checkpoint (user)

---

### Phase 3: [Phase Name - e.g., Integration and Testing]

**Objective**: [What this phase accomplishes]

#### Tasks

- [ ] **Task 3.1**: [Integration task]
  - [ ] Sub-task: [Step]
  - [ ] Sub-task: [Step]

- [ ] **Task 3.2**: [Testing task]
  - [ ] Sub-task: [Step]
  - [ ] Sub-task: [Step]

- [ ] **Task 3.3**: Phase 3 Verification
  - [ ] Sub-task: Run all tests
  - [ ] Sub-task: Verify all acceptance criteria
  - [ ] Sub-task: Manual verification checkpoint (user)

---

### Phase 4: [Phase Name - e.g., Documentation and Cleanup]

**Objective**: [What this phase accomplishes]

#### Tasks

- [ ] **Task 4.1**: Update documentation
  - [ ] Sub-task: Update README if needed
  - [ ] Sub-task: Add inline code comments
  - [ ] Sub-task: Update API docs if applicable

- [ ] **Task 4.2**: Final verification
  - [ ] Sub-task: Run full test suite
  - [ ] Sub-task: Verify all spec requirements met
  - [ ] Sub-task: Code review (if workflow requires)
  - [ ] Sub-task: Final user acceptance

---

## Notes

- Follow TDD workflow: Write test → Fail → Implement → Pass → Refactor
- Each phase ends with verification checkpoint
- Do not proceed to next phase until current phase verified
- User verification required at phase boundaries

## Estimated Complexity

- **Phases**: [Number]
- **Tasks**: [Number]
- **Estimated Effort**: [Low/Medium/High]
```

#### 2.4.5 Phase Completion Verification

**CRITICAL**: If `codex/workflow.md` contains a "Phase Completion Verification Protocol", you MUST inject a verification task at the end of each phase.

**Format**:
```markdown
- [ ] **Task X.Y**: Phase X Verification (Protocol in workflow.md)
  - [ ] Sub-task: Run verification commands from workflow.md
  - [ ] Sub-task: Check all phase objectives completed
  - [ ] Sub-task: User manual verification checkpoint
```

#### 2.4.6 User Confirmation

Present the drafted plan:

> "I've drafted the implementation plan. Please review:
>
> ```markdown
> [Full drafted plan.md content]
> ```
>
> Does this plan cover all necessary steps and align with the specification?
> A) Approve - Proceed to create the track
> B) Suggest Changes - I'll modify the plan
>
> Please respond with A or B."

**Loop Logic**:
- If **B**: Get feedback, modify plan, re-present
- If **A**: Proceed to Section 2.5

---

### 2.5 CREATE TRACK ARTIFACTS AND UPDATE REGISTRY

#### 2.5.1 Check for Duplicate Track Names

1. **List existing tracks**:
   - Read `codex/tracks/` directory
   - Extract all track IDs

2. **Generate short name** from track description:
   - Example: "Add user authentication" → `user-auth`
   - Convert to lowercase, replace spaces with hyphens

3. **Check for duplicates**:
   - If a track with same short name exists:
     > "A track with similar name already exists: `[existing-track-id]`
     >
     > Would you like to:
     > A) Create this track anyway with a different name
     > B) Cancel and review the existing track
     >
     > Please respond with A or B."

4. **Handle response**:
   - If **A**: Ask for alternative name, then proceed
   - If **B**: Halt track creation, show existing track details

#### 2.5.2 Generate Track ID

Create unique track ID in format: `shortname_YYYYMMDD`

**Examples**:
- `user-auth_20250125`
- `fix-login-bug_20250125`
- `refactor-api_20250125`

#### 2.5.3 Create Track Directory Structure

```bash
mkdir -p codex/tracks/<track_id>/
```

#### 2.5.4 Create metadata.json

Create `codex/tracks/<track_id>/metadata.json`:

```json
{
  "track_id": "<track_id>",
  "title": "<Track title>",
  "type": "feature|bug|chore|enhancement|documentation",
  "status": "new",
  "created_at": "2025-01-25T12:00:00Z",
  "updated_at": "2025-01-25T12:00:00Z",
  "description": "<Original user description>",
  "current_phase": null,
  "current_task": null,
  "completion_percentage": 0
}
```

Use actual values and current timestamp.

#### 2.5.5 Write Spec and Plan Files

1. **Write spec.md**:
   - Write confirmed spec content to: `codex/tracks/<track_id>/spec.md`
   - Verify write success

2. **Write plan.md**:
   - Write confirmed plan content to: `codex/tracks/<track_id>/plan.md`
   - Verify write success

#### 2.5.6 Update Tracks Registry

1. **Read `codex/tracks.md`**

2. **Append new track** to the "Active Tracks" section:

```markdown

---

## [ ] Track: [Track Title]

**ID**: `[track_id]`
**Type**: [Feature/Bug/Chore/Enhancement/Documentation]
**Created**: [Date]
**Status**: New

[Brief one-line description]

[View Track Details](./tracks/[track_id]/)
```

3. **Write updated `codex/tracks.md`**

#### 2.5.7 Announce Completion

> "**Track created successfully!**
>
> **Track ID**: `[track_id]`
> **Location**: `codex/tracks/[track_id]/`
>
> **Files created**:
> - spec.md - Detailed specification
> - plan.md - Implementation plan
> - metadata.json - Track metadata
>
> **Next steps**:
> - Review the track: Check `codex/tracks/[track_id]/`
> - Start implementation: Run `/implement`
> - Check status: Run `/status`
>
> The track has been added to your tracks registry and is ready for implementation!"

---

## 3.0 STATUS REPORTING

**PROTOCOL: Report current project and track status.**

---

### 3.1 Read Tracks Registry

1. **Read `codex/tracks.md`**
2. **Parse all tracks** and their statuses:
   - `[ ]` = Not Started
   - `[~]` = In Progress
   - `[x]` = Completed

### 3.2 Identify Active Track

1. **Check metadata for all tracks** in `codex/tracks/`
2. **Find tracks with status**: `"in_progress"` or `"new"`
3. **Identify the currently active track** (most recently updated with status `"in_progress"`)

### 3.3 Generate Status Report

**Format**:

```markdown
# Project Status

**Project**: [From product.md title]
**Last Updated**: [Current timestamp]

---

## Summary

- **Total Tracks**: [Number]
- **Completed**: [Number] ([Percentage]%)
- **In Progress**: [Number]
- **Not Started**: [Number]

---

## Active Track

### [~] [Track Title]

**ID**: `[track_id]`
**Type**: [Type]
**Progress**: [X/Y] tasks complete ([Percentage]%)

**Current Phase**: [Phase name]
**Current Task**: [Task description]

**Next Steps**:
1. [Next pending task]
2. [Following task]

[View Track](./codex/tracks/[track_id]/)

---

## Recent Activity

[List of recently updated tracks]

---

## All Tracks

### Completed 

[List of completed tracks]

### In Progress

[List of in-progress tracks]

### Not Started

[List of not-started tracks]

---

## Quick Actions

- Start implementation: `/implement`
- Create new track: `/newTrack "description"`
- View specific track: Check `codex/tracks/[track-id]/`
```

### 3.4 Display Status

Present the formatted status report to the user.

---

## 4.0 ERROR HANDLING

### 4.1 Missing Setup

If required files don't exist:
> "Codex is not initialized. Run `/setup` first."

### 4.2 Duplicate Track

If track name conflicts:
> "Track with similar name exists. Choose different name or review existing track."

### 4.3 Invalid Response

If user provides invalid input during Q&A:
> "I didn't understand that response. Please select A, B, C, D, or E."

### 4.4 File Write Failure

If file write fails:
> "Failed to write [filename]. Error: [error message]
>
> Please check permissions and try again."

---

**END OF TRACK MANAGEMENT PROTOCOL**
