---
name: prd-implement
description: Execute implementation tasks from a track's plan following the workflow methodology
metadata:
  short-description: Execute track implementation autonomously
  version: 2.0.0
  author: Vedant Parmar
  category: implementation
  tags: [prd, implement, execution, autonomous, tdd, workflow]
  usage: "Invoke with optional track name: prd-implement or prd-implement dark-mode"
---

# PRD Implementation Skill

Execute implementation tasks from a track's plan precisely according to your workflow methodology. This skill autonomously implements features, fixes bugs, and completes chores while following best practices defined in your workflow.

## What This Does

Executes a complete development track including:
- **Track Selection** - Auto-selects next incomplete track or uses specified track
- **Status Management** - Updates track status in tracks.md and metadata.json
- **Task Execution** - Follows workflow methodology (TDD, standard, etc.)
- **Quality Verification** - Runs tests, linters, and quality gates
- **Progress Tracking** - Updates plan.md after each task
- **Documentation Sync** - Updates product docs when track completes
- **Track Cleanup** - Archives or deletes completed tracks

## When to Use

- Ready to implement a planned track
- Want autonomous execution following your workflow
- Need systematic task-by-task implementation
- Want automatic quality verification and commits
- Ready to update product docs after completion

## How to Invoke

Auto-select next incomplete track:
```
$prd-implement
```

Or specify a track:
```
$prd-implement dark-mode
```

## Instructions

### Step 1: Setup Check

**Verify required files:**
- `codex/tech-stack.md`
- `codex/workflow.md`
- `codex/product.md`
- `codex/tracks.md`

**If ANY missing:**
> "Codex is not set up. Please run prd-setup skill first."

HALT.

---

### Step 2: Track Selection

#### 2.1 Parse Tracks File

1. **Read** `codex/tracks.md`
2. **Split** by `---` separator to identify track sections
3. **Extract** for each section:
   - Status: `[ ]` (pending), `[~]` (in progress), `[x]` (completed)
   - Track description (from `##` heading)
   - Track ID (from link or metadata)

**If no tracks found:**
> "The tracks file is empty. No tracks to implement."

HALT.

#### 2.2 Select Track

**If track name provided as argument:**
1. Perform case-insensitive match against parsed tracks
2. If unique match found:
   > "Found track '{track_description}'. Is this correct? (yes/no)"
3. Wait for confirmation
4. If no match or ambiguous:
   > "No exact match found. Showing available tracks..."
   Fall through to auto-select below

**If no track name provided (or match failed):**
1. **Find first incomplete track** (not `[x]`)
2. **If found:**
   > "Automatically selecting next incomplete track: '{track_description}'"
3. **If all complete:**
   > "All tracks are completed!"
   HALT.

---

### Step 3: Update Track Status

**Before starting work:**

1. **Update tracks.md:**
   - Find: `## [ ] Track: {description}`
   - Replace with: `## [~] Track: {description}`

2. **Update metadata.json:**
   - Read: `codex/tracks/{track_id}/metadata.json`
   - Update fields:
     ```json
     {
       "status": "in_progress",
       "updated_at": "{current ISO timestamp}"
     }
     ```
   - Write back

3. **Announce:**
   > "Starting implementation of track '{track_id}'"

---

### Step 4: Load Track Context

**Read these files into context:**
1. `codex/tracks/{track_id}/plan.md`
2. `codex/tracks/{track_id}/spec.md`
3. `codex/workflow.md`
4. `codex/product.md`
5. `codex/tech-stack.md`

**If any file fails to load:**
> "Error: Could not load {filename}. Track may be corrupted."

HALT.

---

### Step 5: Execute Tasks

#### 5.1 Workflow Execution

**CRITICAL: The `codex/workflow.md` is the single source of truth for task execution.**

Read the "Task Workflow" or "Development Commands" section in `workflow.md` and follow it precisely.

#### 5.2 Iterate Through Tasks

**For each task in `plan.md`:**

1. **Announce Task:**
   > "Starting: {Phase} > {Task}"

2. **Follow Workflow:** Execute according to `workflow.md` procedures:
   - **If TDD workflow:**
     1. Write test (expect failure)
     2. Run test (should fail)
     3. Implement feature
     4. Run test (should pass)
     5. Refactor if needed
   - **If standard workflow:**
     1. Implement feature
     2. Run tests
     3. Run linter
     4. Fix any issues

3. **Update plan.md:**
   - Change task status: `- [ ]` → `- [x]`
   - If using sub-tasks, update those too

4. **Commit (if workflow specifies):**
   - After each task OR after each phase
   - Follow commit message format from workflow
   - Example: `git commit -m "feat: {task description}"`
   - Add git note if workflow specifies:
     `git notes add -m "Task: {task summary}"`

5. **Verify:**
   - Run quality checks from workflow:
     - [ ] All tests pass
     - [ ] Lint/format checks pass
     - [ ] TypeScript (or language) has no errors
     - [ ] Build succeeds (if applicable)

6. **Phase Completion:**
   - If task is "User Manual Verification":
     > "Phase '{phase name}' complete. Please verify:
     > - [ ] All phase tasks completed
     > - [ ] Quality gates passed
     > - [ ] Manual testing done
     >
     > Ready to proceed to next phase? (yes/no)"
   - Wait for user confirmation

#### 5.3 Error Handling

**If any step fails:**
1. **Do NOT mark task complete**
2. **Announce failure:**
   > "Task failed: {error description}
   >
   > Options:
   > A) Fix the issue and retry
   > B) Skip this task (mark as blocked)
   > C) Abort implementation"
3. **Wait for user decision**

---

### Step 6: Finalize Track

**After all tasks in plan.md are completed:**

#### 6.1 Update Status

1. **Update tracks.md:**
   - Find: `## [~] Track: {description}`
   - Replace: `## [x] Track: {description}`

2. **Update metadata.json:**
   ```json
   {
     "status": "completed",
     "updated_at": "{current ISO timestamp}"
   }
   ```

3. **Announce:**
   > "Track '{track_id}' is now complete!"

---

### Step 7: Synchronize Documentation

**Execute ONLY when track reaches `[x]` status.**

#### 7.1 Analyze Impact

1. **Read completed spec:** `codex/tracks/{track_id}/spec.md`
2. **Read project docs:**
   - `codex/product.md`
   - `codex/product-guidelines.md`
   - `codex/tech-stack.md`

#### 7.2 Propose Updates

**For product.md:**
1. **Determine if update needed:**
   - Did we add significant features?
   - Did product capabilities change?
2. **If yes:**
   > "Based on the completed track, I propose these updates to `product.md`:
   >
   > ```diff
   > [Show diff of proposed changes]
   > ```
   >
   > Approve? (yes/no)"
3. **Wait for approval** before updating

**For tech-stack.md:**
1. **Determine if update needed:**
   - Did we add new technologies?
   - Did dependencies change?
2. **If yes:**
   > "I propose these updates to `tech-stack.md`:
   >
   > ```diff
   > [Show diff]
   > ```
   >
   > Approve? (yes/no)"
3. **Wait for approval** before updating

**For product-guidelines.md:**
1. **WARNING: Update RARELY**
   - Only if branding, voice, or core identity changed
2. **If update needed:**
   > "WARNING: Track suggests changes to core product guidelines.
   > Review carefully:
   >
   > ```diff
   > [Show diff]
   > ```
   >
   > Approve these critical changes? (yes/no)"
3. **Wait for approval** before updating

#### 7.3 Report Synchronization

> "Documentation synchronization complete:
> - product.md: {Updated | No changes needed}
> - tech-stack.md: {Updated | No changes needed}
> - product-guidelines.md: {Updated | No changes needed}"

---

### Step 8: Track Cleanup

**Execute ONLY after track is complete and docs are synced.**

#### 8.1 Ask User Preference

> "Track '{track_description}' is complete. What would you like to do?
>
> A) **Archive**: Move track folder to `codex/archive/` and remove from tracks.md
> B) **Delete**: Permanently delete track folder and remove from tracks.md
> C) **Skip**: Keep track in tracks.md for reference
>
> Please respond with A, B, or C."

#### 8.2 Execute Choice

**If A (Archive):**
1. `mkdir -p codex/archive/`
2. `mv codex/tracks/{track_id}/ codex/archive/`
3. Remove track section from `codex/tracks.md`
4. Announce: "Track archived to codex/archive/{track_id}/"

**If B (Delete):**
1. `rm -rf codex/tracks/{track_id}/`
2. Remove track section from `codex/tracks.md`
3. Announce: "Track deleted permanently."

**If C (Skip):**
1. Do nothing
2. Announce: "Track marked complete in tracks.md."

---

### Step 9: Next Steps

> "Implementation complete!
>
> Next options:
> - Create new track: Use prd-track skill
> - Check status: Use prd-status skill
> - Implement next track: Use prd-implement skill again"

---

## Critical Rules

1. **Follow workflow.md exactly** - it's the source of truth
2. **Update plan.md** after each task
3. **Commit regularly** per workflow specification
4. **Validate all tools** before proceeding
5. **Wait for user confirmation** at phase gates
6. **Don't mark tasks complete** if they fail
7. **Update status** in both tracks.md and metadata.json
8. **Only sync docs** after track is fully complete
9. **Get user approval** before updating project docs
10. **Respect workflow.md protocols** for verification and checkpoints

## References

- See `references/implementation.md` for detailed protocol

## Success Criteria

Implementation is complete when:
- [ ] All tasks in plan.md marked as complete
- [ ] All quality checks pass
- [ ] Track status updated to completed in tracks.md and metadata.json
- [ ] Product docs synchronized (if needed)
- [ ] Track archived/deleted/kept per user preference
- [ ] User notified of completion

## Autonomous Operation

This skill operates autonomously but with safety mechanisms:

**May Autonomously:**
- Read any project files
- Write code according to approved plans
- Run tests and verification commands
- Update plan.md and tracks.md
- Create new files as specified in plan
- Refactor code within approved scope

**Must Ask Permission To:**
- Deviate from approved plan
- Delete existing files
- Modify core context files (product.md, tech-stack.md, workflow.md)
- Install new dependencies
- Change git configuration
- Execute destructive operations
- Modify database schemas

## Error Handling

- If setup files missing → Direct user to prd-setup skill
- If no tracks found → Suggest creating track with prd-track skill
- If task fails → Pause and ask user for decision
- If quality gate fails → Do not proceed until fixed
- If file write fails → Report error and retry once
