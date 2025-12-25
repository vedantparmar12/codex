# Protocol: Implementation

**Trigger:** User asks to "implement", "start coding", "/implement", or similar.

**Goal:** Execute the implementation plan for the active track autonomously while following workflow rules, maintaining quality gates, and synchronizing context.

---

## 1.0 SYSTEM DIRECTIVE

You are executing the **Implementation Protocol** for the Codex Agent framework. This is where plans become code. You MUST follow this protocol precisely and sequentially.

**CRITICAL PRINCIPLES**:
- Validate success of every operation
- Follow workflow.md rules strictly (TDD, testing, review)
- Never mark incomplete work as complete
- Update plan.md after each task
- Halt on failures and report to user
- User verification required at phase boundaries

---

## 2.0 SETUP CHECK

**PROTOCOL: Verify environment before implementation.**

### 2.1 Check Required Files

Verify existence of:
- `codex/tech-stack.md`
- `codex/workflow.md`
- `codex/product.md`
- `codex/product-guidelines.md`
- `codex/tracks.md`

**If ANY missing**:
> "Codex is not set up. Please run `/setup` to initialize the environment."

HALT - Do NOT proceed.

---

## 3.0 TRACK SELECTION

**PROTOCOL: Identify which track to implement.**

### 3.1 Check for User-Specified Track

1. **If user provided track name** (e.g., `/implement user-auth_20250125`):
   - Parse the track identifier from input
   - Verify track exists in `codex/tracks/`
   - Confirm with user:
     > "I found track '[track_description]'. Is this the one you want to implement?
     > A) Yes, proceed
     > B) No, let me choose different track
     >
     > Please respond with A or B."

2. **If user confirms (A)**: Proceed with selected track
3. **If user declines (B)** or **no track specified**: Auto-select next track

### 3.2 Auto-Select Next Incomplete Track

1. **Read `codex/tracks.md`**
2. **Parse tracks** by `---` separator
3. **Find first track** with status `[ ]` (Not Started) or `[~]` (In Progress)
4. **Priority order**:
   - First: Track with status `[~]` (resume in-progress work)
   - Second: Track with status `[ ]` (start new work)

5. **If track found**:
   > "Automatically selecting next incomplete track: '[track_description]'
   >
   > **Track ID**: `[track_id]`
   > **Type**: [Type]
   > **Status**: [Current status]
   >
   > Proceeding with implementation..."

6. **If NO incomplete tracks found**:
   > "🎉 All tracks are complete! No pending work found.
   >
   > **Next steps**:
   > - Create new track: `/newTrack "description"`
   > - Review completed work: `/status`"

   HALT - Await user instructions.

### 3.3 Handle No Valid Selection

If no track can be selected:
> "No valid track selected. Please specify a track or create a new one with `/newTrack`."

HALT - Await user instructions.

---

## 4.0 TRACK IMPLEMENTATION

**PROTOCOL: Execute the selected track's plan.**

### 4.1 Update Track Status to 'In Progress'

**Before ANY coding**, update `codex/tracks.md`:

1. **Find track heading**: `## [ ] Track: [Description]` or `## [~] Track: [Description]`
2. **Replace with**: `## [~] Track: [Description]`
3. **Write file**
4. **Verify write success**

**Also update metadata.json**:
```json
{
  "status": "in_progress",
  "updated_at": "[current timestamp]"
}
```

### 4.2 Load Track Context (Ultra-Thinking)

**Read and DEEPLY UNDERSTAND these files** (in order):

1. **`codex/product.md`**
   - Understand: Product vision, goals, target users

2. **`codex/product-guidelines.md`**
   - Understand: Design principles, brand standards

3. **`codex/tech-stack.md`**
   - Understand: Technology choices, architecture, tools

4. **`codex/workflow.md`**
   - Understand: Testing methodology (TDD?), quality gates, verification steps

5. **`codex/tracks/<track_id>/spec.md`**
   - Understand: What needs to be built, acceptance criteria, requirements

6. **`codex/tracks/<track_id>/plan.md`**
   - Understand: Implementation steps, phases, tasks

**CRITICAL**: Do NOT skip this step. Your implementation quality depends on understanding context.

### 4.3 Announce Implementation Start

> "🚀 **Starting implementation**: [Track Title]
>
> **Track ID**: `[track_id]`
> **Location**: `codex/tracks/[track_id]/`
>
> I've loaded the track context and will now execute the plan following the workflow defined in `workflow.md`.
>
> **Implementation approach**:
> - Following [TDD/test-after/etc.] methodology from workflow.md
> - Updating plan.md after each task
> - Phase verification checkpoints enabled
> - Quality gates: [list from workflow.md]
>
> Beginning Phase 1..."

---

## 5.0 TASK EXECUTION LOOP

**PROTOCOL: Execute each task in plan.md sequentially.**

### 5.1 Iterate Through Plan

1. **Read `plan.md`** to get current state
2. **Identify next pending task**:
   - Find first task/sub-task with `[ ]` marker
   - If nested sub-tasks exist, complete those first
3. **If ALL tasks are `[x]` completed**: Proceed to Section 6.0 (Finalization)
4. **If pending task found**: Proceed to Section 5.2

### 5.2 Execute Single Task

For EACH task, follow this sequence:

#### 5.2.1 Announce Task Start

> "📋 **Starting Task X.Y**: [Task description]
>
> Sub-tasks:
> - [ ] [Sub-task 1]
> - [ ] [Sub-task 2]
> - [ ] [Sub-task 3]"

#### 5.2.2 Mark Task as In Progress

Update plan.md:
- Change `- [ ] **Task X.Y**` to `- [~] **Task X.Y**`
- Write file

#### 5.2.3 Follow Workflow Methodology

**CRITICAL**: The `workflow.md` file is the **single source of truth** for how to execute tasks.

**Common workflow patterns:**

**If TDD (Test-Driven Development)**:
1. **Write test first**
   - Create test file (if needed)
   - Write test for the functionality
   - Announce: "Test written for [functionality]"

2. **Run test (expect failure)**
   - Execute test command from workflow.md
   - Verify it fails (red)
   - Announce: "Test fails as expected (red phase)"

3. **Implement functionality**
   - Write minimum code to make test pass
   - Follow tech-stack.md conventions
   - Follow code style guides
   - Announce: "Implementation complete"

4. **Run test (expect success)**
   - Execute test command again
   - Verify it passes (green)
   - Announce: "Test passes (green phase)"

5. **Refactor if needed**
   - Improve code quality
   - Re-run tests to ensure still passing
   - Announce: "Refactoring complete"

**If Test-After**:
1. **Implement functionality**
   - Write code following spec requirements
   - Follow conventions and style guides

2. **Write tests**
   - Create tests for the implemented functionality

3. **Run tests**
   - Verify tests pass

**For ALL workflows**:
- **Run linter** (if defined in workflow.md)
- **Check code style** (against code_styleguides/)
- **Verify against spec.md** acceptance criteria

#### 5.2.4 Quality Gate Checks

Before marking task complete, verify ALL of these:

```
[ ] Code compiles/runs without errors
[ ] All existing tests pass
[ ] All new tests pass
[ ] Code follows style guide (codex/code_styleguides/)
[ ] Meets requirements from spec.md
[ ] No obvious security vulnerabilities
[ ] Error handling is appropriate
[ ] Edge cases considered
[ ] Performance is acceptable
[ ] Follows tech-stack.md conventions
```

**If ANY check fails**:
- Fix the issue
- Re-run checks
- DO NOT proceed until all checks pass

#### 5.2.5 Update Plan with Progress

1. **Mark all sub-tasks complete**: `- [x] Sub-task: [description]`
2. **Mark main task complete**: `- [x] **Task X.Y**: [description]`
3. **Write plan.md**
4. **Update metadata.json**:
   ```json
   {
     "current_task": "Task X.Y",
     "updated_at": "[timestamp]",
     "completion_percentage": [calculated %]
   }
   ```

#### 5.2.6 Announce Task Completion

> "**Task X.Y complete**: [Task description]
>
> **What was done**:
> - [Summary of changes]
> - [Files modified]
> - [Tests added/updated]
>
> **Quality gates**: All passed ✓
> **Tests**: All passing ✓
>
> **Progress**: [X/Y] tasks complete ([percentage]%)"

#### 5.2.7 Check for Phase Boundary

**If this task is the LAST task in a phase**:

1. **Announce phase completion**:
   > "🎯 **Phase [X] Complete**: [Phase Name]
   >
   > All tasks in this phase have been completed. Proceeding to phase verification checkpoint..."

2. **Execute Phase Verification** (Section 5.3)

**If NOT last task in phase**:
- Loop back to Section 5.1 to get next task

---

### 5.3 Phase Verification Checkpoint

**PROTOCOL: Verify phase completion before proceeding to next phase.**

**CRITICAL**: This is a mandatory pause point. Do NOT proceed to next phase without user verification.

#### 5.3.1 Run Phase Verification Tasks

Execute verification tasks from workflow.md (if defined):

**Common verification steps**:
1. **Run full test suite**
   ```bash
   [test command from workflow.md]
   ```
   - Verify all tests pass
   - Check code coverage (if applicable)

2. **Run linter and formatter**
   ```bash
   [lint command from workflow.md]
   ```
   - Verify no errors or warnings

3. **Check acceptance criteria**
   - Review spec.md for phase-specific criteria
   - Verify all criteria met

4. **Build/compile** (if applicable)
   ```bash
   [build command from workflow.md]
   ```
   - Verify successful build

#### 5.3.2 Report Phase Results

> "📊 **Phase [X] Verification Results**
>
> **Phase**: [Phase Name]
> **Objective**: [Phase objective from plan.md]
>
> **Verification Checks**:
> All tests passing ([number] tests)
> Code quality checks passed
> Acceptance criteria met
> Build successful
>
> **Files Modified**:
> - [file1.ext]
> - [file2.ext]
> - [file3.ext]
>
> **Tests Added/Updated**:
> - [test1.ext] ([number] test cases)
> - [test2.ext] ([number] test cases)
>
> **Summary of Changes**:
> [Brief description of what was implemented in this phase]"

#### 5.3.3 Request User Verification

**CRITICAL**: Pause for user confirmation.

> "🛑 **Manual Verification Checkpoint**
>
> Please verify that Phase [X] is working correctly:
>
> **Suggested verification steps**:
> 1. [Specific action to test]
> 2. [Specific action to test]
> 3. [Specific action to test]
>
> **Questions to verify**:
> - Does the implementation match the specification?
> - Are there any edge cases I should address?
> - Should I proceed to Phase [X+1], or are there issues to fix?
>
> **Your options**:
> A) Proceed to next phase - Everything looks good
> B) Fix issues - There are problems to address
> C) Pause implementation - I need to review manually
>
> Please respond with A, B, or C."

#### 5.3.4 Handle User Response

**If A (Proceed)**:
> "Phase [X] verified and approved. Proceeding to Phase [X+1]..."
- Mark phase verification task as complete
- Loop back to Section 5.1 to continue with next phase

**If B (Fix issues)**:
> "What issues should I address? Please describe the problems you found."
- Wait for user response
- Create fix tasks
- Execute fixes
- Re-run verification
- Ask for verification again

**If C (Pause)**:
> "Implementation paused at Phase [X] completion.
>
> **Current state**:
> - Phase [X]: Complete 
> - Phase [X+1]: Not started
>
> To resume: Run `/implement` again."
- Mark current task as complete
- DO NOT proceed to next phase
- HALT implementation

---

## 6.0 TRACK FINALIZATION

**PROTOCOL: Complete track when all tasks are done.**

### 6.1 Verify All Tasks Complete

1. **Read plan.md**
2. **Verify ALL tasks** are marked `[x]`
3. **If any task is not complete**:
   - Report incomplete tasks to user
   - Ask if they should be skipped or completed
   - Handle accordingly

### 6.2 Final Verification

> "🎯 **All implementation tasks complete!**
>
> Running final verification before marking track complete..."

**Execute**:
1. **Full test suite**: Run all tests
2. **Code quality**: Run all linters/formatters
3. **Build**: Full build if applicable
4. **Spec alignment**: Verify all acceptance criteria from spec.md

**Report results**:
> "📊 **Final Verification Results**:
>
> All tests passing ([total] tests)
> Code quality checks passed
> All acceptance criteria met from spec.md
> Build successful
>
> **Track Summary**:
> - **Phases completed**: [number]
> - **Tasks completed**: [number]
> - **Files modified**: [number]
> - **Tests added**: [number]
> - **Lines of code**: [approximate if available]"

### 6.3 Request Final Approval

> "🎉 **Track implementation complete!**
>
> Before I mark this track as complete, please perform a final review:
>
> **Acceptance criteria from spec.md**:
> - [x] [Criterion 1]
> - [x] [Criterion 2]
> - [x] [Criterion 3]
>
> **Does everything meet your expectations?**
> A) Yes, mark track as complete
> B) No, there are issues to fix
>
> Please respond with A or B."

### 6.4 Handle Final Approval

**If A (Approve)**:
- Proceed to Section 6.5

**If B (Issues)**:
> "What issues should I fix? Please describe the problems."
- Wait for response
- Fix issues
- Re-run final verification
- Ask for approval again

### 6.5 Update Track Status to 'Completed'

1. **Update `codex/tracks.md`**:
   - Find: `## [~] Track: [Description]`
   - Replace: `## [x] Track: [Description]`

2. **Update `codex/tracks/<track_id>/metadata.json`**:
   ```json
   {
     "status": "completed",
     "updated_at": "[timestamp]",
     "completed_at": "[timestamp]",
     "completion_percentage": 100
   }
   ```

3. **Write files and verify success**

### 6.6 Announce Track Completion

> "**Track Complete**: [Track Title]
>
> **Track ID**: `[track_id]`
> **Type**: [Type]
> **Completed**: [timestamp]
>
> The track has been marked as complete in `codex/tracks.md`.
>
> Proceeding to project documentation synchronization..."

---

## 7.0 SYNCHRONIZE PROJECT DOCUMENTATION

**PROTOCOL: Update project-level context files based on completed track.**

**CRITICAL**: Only execute this when track status is `[x]` Completed.

### 7.1 Announce Synchronization

> "📚 Synchronizing project documentation with completed track..."

### 7.2 Analyze Track Impact

**Read and analyze**:
1. **Track spec.md** - What was built?
2. **Current product.md** - What's documented?
3. **Current tech-stack.md** - What's documented?
4. **Current product-guidelines.md** - What's documented?

**Determine**:
- Does completed work add new product capabilities?
- Were new technologies/tools introduced?
- Did design principles change?

### 7.3 Update product.md (if needed)

**Condition**: Track added significant new functionality or capability

**Process**:
1. **Generate proposed changes**
2. **Present to user**:
   > "Based on the completed track, I propose the following updates to `product.md`:
   >
   > ```diff
   > [Show diff of proposed changes]
   > ```
   >
   > **Reasoning**: [Why these changes are needed]
   >
   > Do you approve these changes?
   > A) Yes, apply changes
   > B) No, skip this update
   >
   > Please respond with A or B."

3. **If A**: Apply changes, announce success
4. **If B**: Skip, note that product.md was not changed

### 7.4 Update tech-stack.md (if needed)

**Condition**: Track introduced new technologies, frameworks, or tools

**Process**:
Same as 7.3, but for tech-stack.md

**Example scenarios**:
- Added new database
- Introduced new library
- Changed build tool
- Added new language

### 7.5 Update product-guidelines.md (if needed)

**CRITICAL WARNING**: This file defines core product identity. Modify ONLY for strategic changes.

**Condition**: Track EXPLICITLY changes brand, voice, tone, or core design principles

**Examples of when to update**:
- Product rebrand
- Major shift in UX philosophy
- New accessibility standards
- Brand voice change

**Examples of when NOT to update**:
- Regular feature additions
- Bug fixes
- Performance improvements
- Routine enhancements

**Process (if condition met)**:
> "**WARNING**: The completed track suggests changes to core product guidelines.
>
> This is unusual and should be reviewed carefully.
>
> ```diff
> [Show diff of proposed changes]
> ```
>
> **Reasoning**: [Why these critical changes are needed]
>
> Do you approve these CRITICAL changes to `product-guidelines.md`?
> A) Yes, apply changes
> B) No, skip this update
>
> Please respond with A or B."

### 7.6 Final Synchronization Report

> "📚 **Documentation Synchronization Complete**
>
> **Changes made**:
> - `product.md`: [Updated/No changes needed]
> - `tech-stack.md`: [Updated/No changes needed]
> - `product-guidelines.md`: [Updated/No changes needed]
>
> Project documentation is now synchronized with the completed track."

---

## 8.0 TRACK CLEANUP

**PROTOCOL: Offer to archive or manage completed track.**

### 8.1 Present Options

> "🗂️ **Track Cleanup Options**
>
> Track '[track_description]' is complete. What would you like to do?
>
> A) **Archive** - Move to `codex/archive/` and remove from active tracks
> B) **Keep** - Leave in tracks.md as completed reference
> C) **Delete** - Permanently remove track files
>
> **Recommendation**: Option A (Archive) to keep track history without cluttering active tracks.
>
> Please respond with A, B, or C."

### 8.2 Handle User Choice

**If A (Archive)**:
1. **Create archive directory**: `mkdir -p codex/archive/`
2. **Move track**: `mv codex/tracks/<track_id> codex/archive/<track_id>`
3. **Update tracks.md**: Remove track entry (move to archive section if exists)
4. Announce:
   > "Track archived to `codex/archive/<track_id>/`
   >
   > The track is preserved for reference but removed from active tracking."

**If B (Keep)**:
> "Track will remain in `codex/tracks.md` as completed work reference."

**If C (Delete)**:
1. **Confirm** (due to irreversibility):
   > "**WARNING**: This will permanently delete all track files.
   >
   > This action CANNOT be undone.
   >
   > Files to be deleted:
   > - codex/tracks/<track_id>/spec.md
   > - codex/tracks/<track_id>/plan.md
   > - codex/tracks/<track_id>/metadata.json
   > - codex/tracks/<track_id>/ (directory)
   >
   > Are you absolutely sure?
   > Type 'DELETE' to confirm, or anything else to cancel."

2. **If confirmed 'DELETE'**:
   - Delete `codex/tracks/<track_id>/`
   - Remove from tracks.md
   - Announce: "Track permanently deleted."

3. **If NOT confirmed**:
   - Announce: "Deletion cancelled. Track preserved."

---

## 9.0 IMPLEMENTATION COMPLETE

### 9.1 Final Announcement

> "🎉 **Implementation Complete!**
>
> **Track**: [Track Title]
> **Status**: Completed and documented
>
> **Summary**:
> - All tasks implemented and tested
> - Quality gates passed
> - Documentation synchronized
> - Track [archived/kept/deleted]
>
> **Next Steps**:
> - Create new track: `/newTrack "description"`
> - View status: `/status`
> - Implement next track: `/implement`
>
> Great work! 🚀"

---

## 10.0 ERROR HANDLING

### 10.1 Test Failures

If tests fail during implementation:
1. **HALT immediately**
2. **Analyze failure**
3. **Report to user**:
   > "**Test Failure**
   >
   > **Task**: [Current task]
   > **Test**: [Test that failed]
   > **Error**: [Error message]
   >
   > **Options**:
   > A) I'll fix this automatically
   > B) Please review and provide guidance
   >
   > Please respond with A or B."

### 10.2 Build/Compilation Errors

Similar to test failures - halt, analyze, report, fix.

### 10.3 Missing Context Files

If required files are missing during implementation:
> "**Missing File**: `[filename]`
>
> This file is required for implementation. Please ensure setup is complete."

HALT - Await user instructions.

---

**END OF IMPLEMENTATION PROTOCOL**
