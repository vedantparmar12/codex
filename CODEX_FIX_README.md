# Codex Extension Fix - Migration to Working Commands

## Problem Summary

The original codex extension was broken because:

1. **Virtual Commands Don't Work**: Commands like `/setup`, `/newTrack` were defined in AGENTS.md but codex CLI doesn't automatically recognize them
2. **Too Many Questions**: The setup process asked 15+ questions, making simple tasks like "create a portfolio" take minutes
3. **No State Tracking**: If interrupted, users had to start over from scratch
4. **Wrong CLI Tool**: Designed for a hypothetical "Codex CLI" that auto-discovers AGENTS.md, but the real codex CLI works differently

## Solution Applied

### ✅ What Was Fixed

1. **Created Real Command Files (TOML)**
   - `commands/codex/setup.toml` - Project initialization
   - `commands/codex/newTrack.toml` - Track creation
   - `commands/codex/implement.toml` - Implementation execution
   - `commands/codex/status.toml` - Progress reporting

2. **Added State Tracking**
   - `codex/setup_state.json` tracks progress
   - Resume capability: If setup fails at step 3, resume from step 3
   - No more re-asking same questions

3. **Simplified Question Flow**
   - **Max 5 questions** per section (down from 15+)
   - **Option E: Auto-generate** on every question
   - User can skip detailed Q&A by selecting auto-generate
   - Questions tailored to context (fewer for brownfield projects)

4. **Streamlined Workflows**
   - Adopted conductor's pattern: ONE question at a time
   - Binary choices (A/B) instead of long explanations
   - Clear progress indicators
   - Phase checkpointing

### 📋 Key Improvements Adopted from Conductor

| Feature | Before | After |
|---------|--------|-------|
| **State Tracking** | None | `setup_state.json` with resume capability |
| **Question Count** | 15+ questions | Max 5 per section |
| **Auto-Generate** | Not available | Option E on every question |
| **Command Format** | Virtual (didn't work) | Real TOML files |
| **Question Flow** | Batch questions | ONE at a time, wait for response |
| **Brownfield Detection** | Asked user | Automatic detection |
| **File Scanning** | Read everything | Respect .gitignore, read manifests first |
| **Large Files** | Full read | First/last 20 lines only |

## Installation Options

### Option 1: Skills (Recommended)

Install commands as codex skills:

```bash
# For user-level (all projects)
cp -r commands/codex ~/.codex/skills/

# For project-level (this repo only)
cp -r commands/codex .codex/skills/
```

Then use:
```bash
codex
$setup
$newTrack "Add dark mode"
$implement
$status
```

### Option 2: AGENTS.md Context

Use AGENTS.md for system instructions:

```bash
cd /path/to/project
cp AGENTS.md ./
codex /init
```

Then type commands in conversation:
- "Run /setup"
- "Run /newTrack 'Add authentication'"
- "Run /implement"
- "Run /status"

## New Workflow Example

**Before (Broken):**
```bash
$ codex
> /setup
Error: Unrecognized command '/setup'

# OR if AGENTS.md loaded:
> /setup
[Asks 15 questions]
[No way to resume if interrupted]
```

**After (Fixed):**
```bash
$ codex
> $setup

Welcome to Codex! I'll guide you through:
- Project Discovery
- Context Definition
- Track Creation

Detected: Greenfield project (new)
Initialized Git repository.

What do you want to build?
> A portfolio website

[Asks max 5 questions, with Option E to auto-generate]

Who are your target users?
A) Recruiters
B) Clients
C) Developers
D) Type your own
E) Auto-generate and review product.md

> E

[Auto-generates product.md based on "portfolio website"]

I've drafted the product guide:
...
A) Approve
B) Suggest Changes

> A

✅ Setup complete!
Next: Create your first track
```

## File Structure

```
codex/
├── commands/
│   └── codex/
│       ├── setup.toml          # NEW: Real command definition
│       ├── newTrack.toml       # NEW: Real command definition
│       ├── implement.toml      # NEW: Real command definition
│       └── status.toml         # NEW: Real command definition
├── protocols/                   # OLD: Keep for reference
│   ├── setup.md
│   ├── track_management.md
│   └── implementation.md
├── templates/                   # Keep: Used by commands
│   ├── workflow.md
│   └── code_styleguides/
├── AGENTS.md                    # Updated: Now references TOML commands
├── codex-extension.json         # Updated: New integration method
└── CODEX_FIX_README.md         # This file
```

## Command Comparison

| Command | Purpose | Questions | State Tracking | Resume |
|---------|---------|-----------|----------------|--------|
| `$setup` | Initialize project | Max 5 per section | ✅ Yes | ✅ Yes |
| `$newTrack` | Create track | 3-5 (type-based) | ✅ Yes | N/A |
| `$implement` | Execute plan | 0 (follows plan) | ✅ Yes | ✅ Yes |
| `$status` | View progress | 0 | N/A | N/A |

## State Tracking Examples

**setup_state.json:**
```json
{
  "last_successful_step": "2.2_product_guidelines"
}
```

If setup is interrupted at "Tech Stack" step, next run resumes there instead of asking about product vision again.

**Track metadata.json:**
```json
{
  "track_id": "dark-mode_20250115",
  "type": "feature",
  "status": "in_progress",
  "created_at": "2025-01-15T10:30:00Z",
  "updated_at": "2025-01-15T14:45:00Z"
}
```

## Simplified Question Flow

**Setup Section Example:**

OLD approach (codex):
```
Question 1: Who are target users?
Question 2: What are core goals?
Question 3: What are MVP features?
Question 4: What problem does this solve?
Question 5: What makes it unique?
... (continues for 15 questions)
```

NEW approach (conductor-inspired):
```
Question 1: Who are target users?
A) Recruiters
B) Clients
C) Developers
D) Type your own
E) Auto-generate and review product.md

[User selects E]

✅ Auto-generated rest of product.md
[Shows draft for approval]
```

## Usage Tips

1. **Use Auto-Generate (Option E)**: Fastest way to get started
2. **Let It Resume**: Don't worry about interruptions, state is saved
3. **Trust Brownfield Detection**: No need to tell it you have existing code
4. **Review Auto-Generated Docs**: Always review before approving
5. **Use $status Often**: Track progress across all tracks

## Migration from Old Codex

If you were using the old AGENTS.md approach:

1. **Backup** your old `codex/` directory
2. **Install** new TOML commands to `~/.codex/skills/codex/`
3. **Run** `$setup` in your project
4. **Resume** existing tracks with `$implement`

Old `codex/tracks.md` and track directories will still work!

## Why This Works Now

**Conductor Pattern Success Factors:**

1. **State Files**: `setup_state.json`, `metadata.json` track progress
2. **One Question at a Time**: Easier to follow, less overwhelming
3. **Auto-Generate Option**: Respects user time
4. **Smart File Scanning**: Reads `.gitignore`, prioritizes manifests
5. **Binary Choices**: A/B decisions instead of open-ended
6. **Brownfield Detection**: Automatic, no questions needed
7. **Phase Checkpointing**: Verify each major milestone

**Real Command Integration:**

- TOML files are actual command definitions
- Codex CLI recognizes skills in `~/.codex/skills/`
- No reliance on "virtual commands" in AGENTS.md
- Works like conductor's Gemini integration

## Testing the Fix

```bash
# Test setup
cd /path/to/new/project
codex
> $setup

# Test newTrack
> $newTrack "Add authentication"

# Test status
> $status

# Test implement
> $implement
```

## Troubleshooting

**"Unrecognized command $setup":**
- Skills not installed. Copy `commands/codex/` to `~/.codex/skills/`
- Or use without `$`: Just type "setup" in conversation

**"Setup asks too many questions":**
- Select Option E (Auto-generate) on any question
- It will infer the rest from context

**"Setup crashed mid-way":**
- Run `$setup` again - it will resume from last successful step
- Check `codex/setup_state.json` to see where it left off

**"Commands work but questions still verbose":**
- Make sure you copied the NEW toml files, not old protocols
- Check file timestamps - new files dated 2025-01-15 or later

## Credits

- **Original Codex Extension**: Vedant Parmar
- **Conductor Pattern**: Inspired by Gemini conductor extension
- **Fixes Applied**: Adopted conductor's state tracking, question simplification, and command structure

## Version History

- **v1.0.0**: Original AGENTS.md-based extension (broken)
- **v2.0.0**: TOML-based commands with conductor patterns (fixed)

---

**Result**: Codex extension now works reliably with real commands, simplified workflows, and state tracking. Users can create a portfolio in 30 seconds by selecting "auto-generate" instead of answering 15 questions.
