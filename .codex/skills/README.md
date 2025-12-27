# PRD-to-Product Agent Skills

**Version 3.0.0** - Agent Skills Standard Implementation

Transform product ideas into shipping code through systematic, PRD-driven development using the [Agent Skills standard](https://agentskills.io).

## What Are These Skills?

A collection of Agent Skills that enable efficient product development from PRD to production:

- **prd-setup** - Initialize project with product context and workflow
- **prd-track** - Create development tracks with specs and plans
- **prd-implement** - Execute implementation autonomously
- **prd-status** - View project and track progress
- **prd-generate** - Generate comprehensive PRD documents

## Why Use Agent Skills?

**Problems Solved:**
- Jumping to code without clear specifications
- Losing context between coding sessions
- Unclear requirements and scope creep
- No structured workflow or quality gates
- Difficulty tracking progress across features

**Benefits:**
- **Context-Driven**: Define product vision, tech stack, workflow once
- **Spec-First**: Create detailed specs before coding
- **Organized**: Track-based feature management
- **Autonomous**: AI follows your workflow precisely
- **Progressive Disclosure**: Skills load only what's needed
- **Standard-Based**: Compatible with any tool supporting Agent Skills

## What Changed from v2.0?

| Aspect | v2.0 (Extension) | v3.0 (Agent Skills) |
|--------|-----------------|---------------------|
| Format | TOML files | SKILL.md with YAML frontmatter |
| Standard | Codex-specific | Agent Skills (agentskills.io) |
| Compatibility | Codex CLI only | Any Agent Skills-compatible tool |
| Discovery | Manual config | Auto-discovered by AI |
| Invocation | `$setup` | `$prd-setup` |
| Structure | Flat | Organized with references/assets |
| Portability | Limited | High (cross-tool compatible) |

## Installation

### Prerequisites

1. **Claude Code CLI** (or any Agent Skills-compatible AI tool)
2. **Git** (for version control)

### Option 1: User-Scoped (Recommended)

Available in all your projects:

```bash
# Clone or download this repository
git clone https://github.com/your-username/prd-to-product-skills.git

# Copy skills to user directory
cp -r prd-to-product-skills/.codex/skills/* ~/.codex/skills/

# Verify installation
ls ~/.codex/skills/
# Should show: prd-setup, prd-track, prd-implement, prd-status, prd-generate
```

### Option 2: Project-Scoped

Available only in current project:

```bash
# In your project directory
mkdir -p .codex/skills
cp -r /path/to/prd-to-product-skills/.codex/skills/* .codex/skills/
```

### Option 3: Copy Individual Skills

Pick only the skills you need:

```bash
# Just the essentials
cp -r prd-to-product-skills/.codex/skills/prd-setup ~/.codex/skills/
cp -r prd-to-product-skills/.codex/skills/prd-track ~/.codex/skills/
cp -r prd-to-product-skills/.codex/skills/prd-implement ~/.codex/skills/
```

## Quick Start

### 1. Initialize Your Project

```bash
cd /path/to/your/project
claude  # or your AI tool
```

In the AI chat:
```
$prd-setup
```

**This will:**
- Detect greenfield vs brownfield project
- Ask max 5 questions per section (or auto-generate with Option E)
- Create `codex/` directory with context files
- Generate your first development track

**Fast Path:**
```
What do you want to build?
> A task management app

Who are your target users?
E) Auto-generate and review product.md  ← Select this to skip questions!

Setup complete in 30 seconds!
```

### 2. Create a Development Track

```
$prd-track Add user authentication
```

**This will:**
- Ask 3-5 context-aware questions
- Generate detailed `spec.md` with requirements
- Generate phased `plan.md` with tasks
- Update `tracks.md` registry

### 3. Implement the Track

```
$prd-implement
```

**This will:**
- Auto-select next incomplete track
- Follow your workflow methodology (TDD, standard, etc.)
- Execute tasks phase by phase
- Run quality checks and tests
- Commit after each task (configurable)
- Update progress in real-time

### 4. Check Progress

```
$prd-status
```

**This shows:**
- Project health and track summary
- Per-track progress with percentages
- Current phase and task
- Visual progress bars
- Blockers (if any)
- Recommended next actions

## Skills Reference

### prd-setup

**Initialize project with PRD-driven context**

**Creates:**
- `codex/product.md` - Product vision and goals
- `codex/product-guidelines.md` - Design and brand standards
- `codex/tech-stack.md` - Technologies and architecture
- `codex/workflow.md` - Development workflow and quality gates
- `codex/code_styleguides/` - Language-specific style guides
- `codex/tracks/` - Track directory with first track (greenfield)

**Features:**
- Auto-detects greenfield vs brownfield
- Max 5 questions per section
- Option E to auto-generate
- Resume support (interrupted setup)
- State tracking via `setup_state.json`

**Usage:**
```
$prd-setup
```

---

### prd-track

**Create development track with spec and plan**

**Creates:**
- `codex/tracks/{track-id}/spec.md` - Detailed specification
- `codex/tracks/{track-id}/plan.md` - Phased implementation plan
- `codex/tracks/{track-id}/metadata.json` - Track metadata
- Updates `codex/tracks.md` registry

**Features:**
- Auto-infers track type (feature/bug/chore)
- Context-aware questions (3-5 max)
- Option E to auto-generate
- Follows workflow methodology
- Duplicate detection

**Usage:**
```
$prd-track Add dark mode
$prd-track Fix login crash
$prd-track  # Will prompt for description
```

---

### prd-implement

**Execute implementation autonomously**

**Features:**
- Auto-selects next incomplete track
- Updates status in tracks.md and metadata.json
- Follows workflow.md precisely (TDD, standard, etc.)
- Runs quality gates (tests, linters, builds)
- Updates plan.md after each task
- Commits per workflow specification
- Phase-gate confirmations
- Doc synchronization after completion
- Track cleanup (archive/delete/keep)

**Usage:**
```
$prd-implement              # Auto-select next track
$prd-implement dark-mode    # Implement specific track
```

---

### prd-status

**View comprehensive project status**

**Shows:**
- Project overview and health
- Tracks summary (completed/in-progress/pending)
- Per-track progress with percentages
- Current phase and task
- Overall progress with visual bar
- Blockers and issues
- Recommended next actions

**Usage:**
```
$prd-status
```

---

### prd-generate

**Generate comprehensive PRD documents**

**Actions:**
- **new** - Create PRD through 10-15 interactive questions
- **refine** - Update existing PRD sections
- **validate** - Check PRD quality and completeness

**Features:**
- Comprehensive PRD structure
- Market analysis and competitive landscape
- User personas and journeys
- Success metrics and launch criteria
- Option E to auto-generate
- Quality validation with scoring
- Multiple save locations

**Usage:**
```
$prd-generate new       # Create new PRD
$prd-generate refine    # Update existing PRD
$prd-generate validate  # Validate PRD quality
```

---

## File Structure

After running `prd-setup` and creating tracks:

```
your-project/
├── .codex/
│   └── skills/              # Agent Skills (this folder)
│       ├── prd-setup/
│       │   ├── SKILL.md
│       │   ├── references/
│       │   │   └── setup.md
│       │   └── assets/
│       │       ├── workflow.md
│       │       └── code_styleguides/
│       ├── prd-track/
│       ├── prd-implement/
│       ├── prd-status/
│       └── prd-generate/
├── codex/                   # Project context (created by prd-setup)
│   ├── product.md
│   ├── product-guidelines.md
│   ├── tech-stack.md
│   ├── workflow.md
│   ├── tracks.md
│   ├── code_styleguides/
│   └── tracks/
│       └── feature-name_20250127/
│           ├── spec.md
│           ├── plan.md
│           └── metadata.json
├── src/
└── README.md
```

## Workflow Example

```bash
# 1. Setup project (first time)
$prd-setup
> What do you want to build?
> A SaaS application
>
> Who are your target users?
E  # Auto-generate remaining sections!

# 2. Create track
$prd-track Build authentication system
E  # Auto-generate spec and plan!

# 3. Implement
$prd-implement
# AI autonomously implements following your workflow

# 4. Check status
$prd-status
# See progress, current task, blockers

# 5. Create next track
$prd-track Add user dashboard

# 6. Continue
$prd-implement
```

## Advanced Features

### Auto-Generate (Option E)

Every question in setup, track creation, and PRD generation offers **Option E**:
```
A) Option A
B) Option B
C) Option C
D) Type your own
E) Auto-generate and review [artifact]  ← Skip all remaining questions!
```

Selecting E:
- Stops asking questions immediately
- Infers remaining answers from context
- Generates complete artifact
- Presents for review and approval

### Resume Support

If `prd-setup` is interrupted, just run it again:
- Reads `codex/setup_state.json`
- Resumes from last successful step
- No need to re-answer questions

### State Tracking

- `codex/setup_state.json` - Setup progress
- `codex/tracks/{track-id}/metadata.json` - Track status
- `codex/tracks.md` - Track registry with status indicators

### Progressive Disclosure

Skills use progressive disclosure for efficiency:
1. **Startup**: AI loads only skill names and descriptions
2. **Selection**: AI loads full instructions when skill is invoked
3. **References**: Supporting docs loaded only when needed

### Brownfield Detection

`prd-setup` automatically detects existing projects:
- Checks for `.git`, package manifests, source directories
- Scans codebase to infer tech stack
- Asks permission before analyzing files
- Respects `.gitignore` and `.claudeignore`
- Smart file reading (first/last 20 lines for large files)

## Skill Precedence

Skills are loaded from these locations (high to low priority):

1. **REPO**: `.codex/skills` in current working directory
2. **REPO**: `.codex/skills` in parent directory (if in git repo)
3. **REPO**: `.codex/skills` in repository root (if in git repo)
4. **USER**: `~/.codex/skills` (your personal skills)
5. **ADMIN**: `/etc/codex/skills` (system-wide)
6. **SYSTEM**: Bundled with AI tool

Skills with the same name from higher precedence overwrite lower precedence.

## Customization

### Modify Existing Skills

Edit SKILL.md files to customize behavior:

```bash
# Example: Modify prd-setup to ask different questions
nano ~/.codex/skills/prd-setup/SKILL.md
```

### Create Custom Skills

1. Create new folder: `~/.codex/skills/my-skill/`
2. Add `SKILL.md` with YAML frontmatter:
   ```yaml
   ---
   name: my-skill
   description: What this skill does
   ---

   # Instructions
   ...
   ```
3. Add references: `~/.codex/skills/my-skill/references/`
4. Add assets: `~/.codex/skills/my-skill/assets/`

### Override Skills

To override a skill for a specific project:
```bash
# Copy skill to project
cp -r ~/.codex/skills/prd-setup .codex/skills/

# Modify project-specific version
nano .codex/skills/prd-setup/SKILL.md
```

## Troubleshooting

### Skills Not Found

```bash
# Verify installation
ls ~/.codex/skills/
# Should show: prd-setup, prd-track, prd-implement, prd-status, prd-generate

# Reinstall if missing
cp -r /path/to/skills/* ~/.codex/skills/
```

### Skills Not Loading

Check if your AI tool supports Agent Skills:
- Claude Code CLI: Native support
- Other tools: Check documentation for Agent Skills support

### Setup Interrupted

Just run `$prd-setup` again - it will resume from where you left off.

### Too Many Questions

Select **Option E** on any question to auto-generate the rest!

### Context Files Missing

If `codex/` directory is missing:
```
$prd-setup
```

If individual files missing:
- Re-run `$prd-setup` (it will detect and regenerate)
- Or manually create missing files

## Migration from v2.0

If you're using the old TOML-based Codex extension:

### What to Keep

Keep your existing `codex/` directory:
- `codex/product.md`
- `codex/tech-stack.md`
- `codex/workflow.md`
- `codex/tracks/`

These files are compatible with v3.0!

### What to Replace

Replace old commands:

| Old (v2.0) | New (v3.0) |
|-----------|-----------|
| `$setup` | `$prd-setup` |
| `$newTrack` | `$prd-track` |
| `$implement` | `$prd-implement` |
| `$status` | `$prd-status` |
| `/prd:generate` | `$prd-generate new` |

### Migration Steps

1. **Install new skills:**
   ```bash
   cp -r .codex/skills/* ~/.codex/skills/
   ```

2. **Keep your data:**
   - Don't delete `codex/` directory
   - All context and tracks work with v3.0

3. **Update workflows:**
   - Use new skill names (`$prd-*`)
   - Everything else works the same

## Contributing

Want to improve these skills?

1. Fork the repository
2. Make your changes to SKILL.md files
3. Test with your AI tool
4. Submit a pull request

## License

MIT License - see LICENSE file

## Credits

- **Author**: Vedant Parmar
- **Inspired by**: Conductor (Gemini CLI extension)
- **Standard**: [Agent Skills](https://agentskills.io) (agentskills.io)
- **Compatible with**: Claude Code CLI and other Agent Skills-compatible tools

## Support

- **Issues**: https://github.com/your-username/prd-to-product-skills/issues
- **Documentation**: This README and individual SKILL.md files
- **Examples**: See Quick Start section above

---

**Version**: 3.0.0
**Last Updated**: 2025-12-27
**Standard**: Agent Skills (agentskills.io)
