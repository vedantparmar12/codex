# Codex PRD Extension

**Version 2.0.0** - Context-Driven Development for Codex CLI

> ⚠️ **Codex CLI Only**: This extension is designed exclusively for [OpenAI's Codex CLI](https://openai.com/codex). It will NOT work with Claude Code, GitHub Copilot, or other AI coding tools.

## What is This?

A Codex CLI extension that simplifies product development through:
- **Context-Driven Development**: Define product vision, tech stack, and workflow once
- **Spec-Driven Implementation**: Create detailed specs before coding
- **Track Management**: Organize work into trackable features/bugs
- **State Tracking**: Resume work without re-answering questions
- **Auto-Generate**: Skip long Q&A sessions with smart defaults

## Why Use This?

**Problem**: Traditional development jumps straight to code without clear specs, leading to scope creep and confusion.

**Solution**: This extension enforces a simple workflow:
1. **Setup** → Define product context once
2. **Track** → Create spec for each feature
3. **Implement** → Follow the generated plan
4. **Status** → Track progress

**Result**: Clear specs, organized work, trackable progress.

## Installation

### Prerequisites

1. **Install Codex CLI** from OpenAI
2. **Authenticate**: `codex login`
3. **Enable Skills**: Add to `~/.codex/config.toml`:
   ```toml
   [features]
   skills = true
   ```

### Install Extension

**User-Scoped (Recommended - Available in All Projects)**

```bash
# Clone or download this repository
cd /path/to/codex-prd

# Copy skills to user directory
cp -r commands/codex ~/.codex/skills/

# Verify installation
ls ~/.codex/skills/codex/
# Should show: setup.toml, newTrack.toml, implement.toml, status.toml
```

**Repo-Scoped (Only for This Project)**

```bash
# In your project directory
cp -r /path/to/codex-prd/commands/codex .codex/skills/
```

See [INSTALLATION.md](INSTALLATION.md) for detailed instructions.

## Quick Start

### 1. Initialize Your Project

```bash
cd /path/to/your/project
codex
```

In the Codex TUI:
```
$setup
```

**The setup will:**
- Detect if this is a new or existing project
- Ask max 5 questions (or auto-generate with Option E)
- Create `codex/` directory with product.md, tech-stack.md, workflow.md, etc.

**Fast Path Example:**
```
What do you want to build?
> A portfolio website

Who are your target users?
E) Auto-generate and review product.md  ← Select this!

✅ Setup complete in 30 seconds!
```

### 2. Create Your First Track

```
$newTrack Build portfolio MVP
```

**The track creation will:**
- Ask 3-5 questions (or auto-generate with Option E)
- Generate `spec.md` and `plan.md`
- Update `tracks.md` registry

### 3. Implement the Track

```
$implement
```

**This will:**
- Auto-select the next incomplete track
- Execute tasks according to workflow
- Update progress in plan.md
- Commit after each task

### 4. Check Progress

```
$status
```

**Shows:**
- Overall project progress
- Track-by-track status
- Current phase and task
- Next actions

## Skills Reference

### `$setup` - Initialize Project

```bash
$setup
```

**Creates:**
- `codex/product.md` - Product vision
- `codex/product-guidelines.md` - Design guidelines
- `codex/tech-stack.md` - Technology stack
- `codex/workflow.md` - Development workflow
- `codex/code_styleguides/` - Code standards

**Features:**
- Detects greenfield vs brownfield automatically
- Max 5 questions per section
- Option E to auto-generate
- Resumes from last step if interrupted

---

### `$newTrack` - Create Development Track

```bash
$newTrack "Add user authentication"
```

**Creates:**
- `codex/tracks/{track-id}/spec.md` - Specification
- `codex/tracks/{track-id}/plan.md` - Implementation plan
- `codex/tracks/{track-id}/metadata.json` - Metadata
- Updates `codex/tracks.md` registry

**Features:**
- Infers track type (feature/bug/chore)
- Asks 3-5 context-aware questions
- Option E to auto-generate
- Follows workflow methodology

---

### `$implement` - Execute Implementation

```bash
$implement
```

**Features:**
- Auto-selects next incomplete track
- Updates track status to "in progress"
- Follows workflow precisely
- Commits after tasks (per workflow)
- Syncs docs after completion

---

### `$status` - View Progress

```bash
$status
```

**Shows:**
- Project overview
- Track summary with percentages
- Current phase and task
- Progress bars
- Recommendations

## File Structure

After running `$setup` and `$newTrack`:

```
your-project/
├── codex/
│   ├── product.md
│   ├── product-guidelines.md
│   ├── tech-stack.md
│   ├── workflow.md
│   ├── tracks.md
│   ├── setup_state.json
│   ├── code_styleguides/
│   │   ├── typescript.md
│   │   └── ...
│   └── tracks/
│       └── portfolio-mvp_20250115/
│           ├── spec.md
│           ├── plan.md
│           └── metadata.json
├── src/
└── README.md
```

## Workflow Example

```bash
codex

# 1. Setup (first time)
$setup
> What do you want to build?
> A SaaS application

> Who are your target users?
E  # Auto-generate!

# 2. Create track
$newTrack "Build MVP"
E  # Auto-generate spec!

# 3. Implement
$implement

# 4. Check status
$status

# 5. Create next track
$newTrack "Add authentication"

# 6. Continue
$implement
```

## Key Features

### 1. State Tracking & Resume

Setup interrupted at step 5? Resume from step 5, not step 1!

```json
// codex/setup_state.json
{"last_successful_step": "2.3_tech_stack"}
```

### 2. Auto-Generate (Option E)

Every question has Option E to auto-generate:
```
A) Option A
B) Option B
C) Option C
D) Type your own
E) Auto-generate and review [artifact]  ← Skip questions!
```

### 3. Brownfield Detection

Automatically detects existing projects based on:
- `.git` directory
- Package manifests
- Source directories

No manual classification needed!

### 4. Smart File Scanning

- Reads `.gitignore` first
- Prioritizes manifests
- For files >1MB: reads first/last 20 lines only

### 5. One Question at a Time

Sequential questioning for clarity:
- Ask ONE question
- Wait for response
- Then ask next

## Comparison

| Aspect | Old (v1.0) | New (v2.0) |
|--------|-----------|-----------|
| Commands | Virtual ❌ | Real skills ✅ |
| Questions | 15+ | Max 5 |
| Auto-generate | No | Yes (Option E) |
| Resume | No | Yes |
| Setup time | 5+ min | 30 sec |
| Platform | Multiple | Codex only |

## Troubleshooting

### Skills Not Found

```bash
# Verify location
ls ~/.codex/skills/codex/

# Reinstall
cp -r commands/codex ~/.codex/skills/
```

### Skills Not Loading

```bash
# Check config
cat ~/.codex/config.toml | grep skills

# Should show: skills = true
# If not, add it:
echo -e "\n[features]\nskills = true" >> ~/.codex/config.toml
```

### Too Many Questions

Select Option E on any question to auto-generate the rest!

### Setup Interrupted

Just run `$setup` again - it resumes from where you left off.

## Documentation

- **[INSTALLATION.md](INSTALLATION.md)** - Detailed installation guide
- **[CODEX_FIX_README.md](CODEX_FIX_README.md)** - What was fixed and why

## FAQ

**Q: Can I use this with Claude Code?**
A: No. **Codex CLI only**.

**Q: Do I need to answer all questions?**
A: No! Select Option E to auto-generate.

**Q: Can I edit generated files?**
A: Yes! All files in `codex/` are editable.

**Q: What if I interrupt setup?**
A: Run `$setup` again - it resumes automatically.

**Q: Can I use in existing projects?**
A: Yes! Setup auto-detects existing projects.

## License

MIT License

## Credits

- **Author**: Vedant Parmar
- **Inspired by**: Conductor (Gemini CLI extension)
- **Platform**: OpenAI Codex CLI

---

**Made for**: OpenAI Codex CLI **only**
**Version**: 2.0.0
