---
name: prd-status
description: Display comprehensive project and track progress status with metrics and recommendations
metadata:
  short-description: View project and track status
  version: 2.0.0
  author: Vedant Parmar
  category: reporting
  tags: [prd, status, progress, metrics, reporting, dashboard]
---

# PRD Status Skill

Provide a comprehensive status overview of the project including track progress, current work, blockers, and recommendations for next steps.

## What This Does

Generates a detailed status report including:
- **Project Overview** - High-level project health
- **Tracks Summary** - Count of completed, in-progress, and pending tracks
- **Track Details** - Progress percentage, current phase, current task
- **Overall Progress** - Aggregate metrics with visual progress bar
- **Next Actions** - Recommended next steps
- **Blockers** - Any issues preventing progress
- **Recommendations** - Context-aware suggestions

## When to Use

- Check current project status
- Review track progress
- Identify what to work on next
- Find blockers or issues
- Get overview before starting work
- Report progress to stakeholders

## How to Invoke

```
$prd-status
```

## Instructions

### Step 1: Setup Check

**Verify tracks file exists:**
- Check: `codex/tracks.md` exists and is not empty

**If missing or empty:**
> "Project not set up or tracks.md is corrupted. Please run prd-setup skill to initialize."

HALT.

**Verify context files:**
- `codex/tech-stack.md`
- `codex/workflow.md`
- `codex/product.md`

**If ANY missing:**
> "Codex is not fully set up. Please run prd-setup skill to initialize."

HALT.

---

### Step 2: Read Project Data

#### 2.1 Read Tracks File

1. **Read:** `codex/tracks.md`
2. **Parse:** Split by `---` separator to identify track sections
3. **Extract for each track:**
   - Status: `[ ]` (pending), `[~]` (in progress), `[x]` (completed)
   - Track ID (from link)
   - Track description
   - Track type (if shown)
   - Creation date (if shown)

#### 2.2 Read Track Plans

1. **List track directories:** `ls codex/tracks/`
2. **For each track:**
   - Read: `codex/tracks/{track_id}/plan.md`
   - Parse phases, tasks, and sub-tasks
   - Count: Total, completed, in-progress, pending

3. **For each track (optional but recommended):**
   - Read: `codex/tracks/{track_id}/metadata.json`
   - Extract: type, status, created_at, updated_at

---

### Step 3: Analyze Status

#### 3.1 Calculate Metrics

**Per Track:**
- Total phases
- Total tasks
- Tasks completed: Count `[x]` tasks
- Tasks in progress: Count `[~]` tasks or tasks being worked on
- Tasks pending: Count `[ ]` tasks
- Progress percentage: (completed / total) * 100%

**Overall Project:**
- Total tracks
- Tracks completed: Count `[x]` in tracks.md
- Tracks in progress: Count `[~]` in tracks.md
- Tracks pending: Count `[ ]` in tracks.md
- Overall progress: Aggregate across all tracks

#### 3.2 Identify Current Work

**Current Phase:**
- Find first phase with incomplete tasks in active track

**Current Task:**
- Find first task marked `[~]` or first `[ ]` task in current phase

**Next Action:**
- Identify the next pending `[ ]` task

**Blockers:**
- Look for tasks marked as blocked or with failure notes

---

### Step 4: Present Status Report

#### 4.1 Report Format

Present status in this format:

```
# Project Status Report
Generated: {current timestamp}

## Overview
**Project:** {product name from product.md}
**Status:** {On Track | Behind Schedule | Blocked | Completed}

## Tracks Summary
- Total Tracks: {count}
- Completed: {count}
- In Progress: {count}
- Pending: {count}

## Track Details

### [~] {Track 1 Title} ({track_id})
**Type:** {feature|bug|chore}
**Created:** {date}
**Progress:** {tasks_completed}/{tasks_total} ({percentage}%)

**Current Phase:** {Phase X: Phase Name}
**Current Task:** {Task description}
**Status:** {In Progress | Blocked | On Track}

{If blocked:}
**Blocker:** {blocker description}

---

### [ ] {Track 2 Title} ({track_id})
**Type:** {feature|bug|chore}
**Created:** {date}
**Status:** Not started

---

### [x] {Track 3 Title} ({track_id})
**Type:** {feature|bug|chore}
**Completed:** {date}
**Status:** Completed

## Overall Progress
**Total Tasks Across All Tracks:** {total_tasks}
**Completed Tasks:** {completed_tasks}
**Progress:** {percentage}%

{Progress bar visualization}
█████████░░░░░░░░░░░ 45%

## Next Actions
1. {Next pending task from active track}
2. {Following task or "Create new track with prd-track skill"}

## Blockers
{List any blockers, or "None"}

## Recommendations
{Suggest next steps based on status}
```

#### 4.2 Status Determination Logic

**Completed:**
- All tracks are `[x]`
- All tasks in all tracks are complete

**On Track:**
- At least one track `[~]` in progress
- No blockers
- Progress is being made

**Behind Schedule:**
- Multiple tracks in progress
- Many pending tasks
- (Optional: compare against estimated timeline if available)

**Blocked:**
- Any track has blockers
- Implementation halted
- Requires user intervention

#### 4.3 Progress Bar

Generate ASCII progress bar based on overall completion:

**0-10%:** `░░░░░░░░░░░░░░░░░░░░ 5%`
**11-25%:** `███░░░░░░░░░░░░░░░░░ 15%`
**26-50%:** `█████████░░░░░░░░░░░ 40%`
**51-75%:** `███████████████░░░░░ 65%`
**76-99%:** `██████████████████░░ 90%`
**100%:** `████████████████████ 100%`

---

### Step 5: Provide Recommendations

Based on status analysis, suggest next actions:

**If no tracks in progress:**
> "Recommendation: Start implementing with prd-implement skill or create a new track with prd-track skill"

**If track is in progress:**
> "Recommendation: Continue current track with prd-implement skill"

**If all tracks complete:**
> "All tracks are complete! Create a new track with prd-track skill to continue development."

**If blockers exist:**
> "Recommendation: Resolve blockers before proceeding. Review the blocker description above."

**If many tracks pending:**
> "Recommendation: Focus on completing in-progress tracks before starting new ones."

---

### Step 6: Additional Insights (Optional)

If useful, include:

**Recent Activity:**
- Last updated track and timestamp
- Most recent commits (from git log if available)

**Quality Metrics:**
- Test coverage (if available from workflow)
- Build status (if applicable)

**Context Summary:**
- Tech stack summary (from tech-stack.md)
- Product goals (from product.md)

---

## Critical Rules

1. **Always show current timestamp**
2. **Parse tracks.md correctly** using `---` separator
3. **Calculate accurate percentages** for each track
4. **Identify blockers** prominently
5. **Provide actionable next steps**
6. **Use clear visual indicators**
7. **Show progress bars** for visual clarity
8. **List tracks in order**: In Progress → Pending → Completed
9. **Validate tool calls** before proceeding
10. **Handle missing data gracefully** (e.g., if metadata.json missing)

## Success Criteria

Status report is complete when:
- [ ] Current timestamp shown
- [ ] All tracks parsed from tracks.md
- [ ] Progress calculated for each track
- [ ] Overall progress calculated
- [ ] Current work identified
- [ ] Blockers identified (if any)
- [ ] Recommendations provided
- [ ] Progress bar displayed
- [ ] Report formatted clearly

## Error Handling

- If tracks.md missing → Direct to prd-setup skill
- If track directory missing → Note in report and continue
- If plan.md unreadable → Show track but mark as "Unable to calculate progress"
- If metadata.json missing → Use data from tracks.md only
- If all reads fail → Report error and suggest checking file integrity
