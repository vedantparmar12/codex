# PRD-to-Product Agent Skills

**Transform product ideas into shipping code** through systematic, PRD-driven development.

Version 3.0.0 - Now using the [Agent Skills standard](https://agentskills.io)!

## Overview

A collection of Agent Skills that enable efficient product development from PRD to production. Compatible with Claude Code CLI and any tool supporting the Agent Skills standard.

## Features

- **prd-setup** - Initialize project with product context and workflow
- **prd-track** - Create development tracks with specs and plans
- **prd-implement** - Execute implementation autonomously
- **prd-status** - View project and track progress
- **prd-generate** - Generate comprehensive PRD documents

## Installation

### Quick Install (User-Scoped)

Available in all your projects:

```bash
# Clone this repository
git clone <your-repo-url>

# Copy skills to user directory
cp -r .codex/skills/* ~/.codex/skills/

# Verify
ls ~/.codex/skills/
```

### Project-Scoped Install

Available only in current project:

```bash
# In your project
cp -r /path/to/this/repo/.codex/skills .codex/
```

## Quick Start

```bash
# 1. Initialize project
$prd-setup

# 2. Create your first track
$prd-track Build MVP

# 3. Implement it
$prd-implement

# 4. Check progress
$prd-status
```

## Documentation

See [.codex/skills/README.md](.codex/skills/README.md) for:
- Detailed installation instructions
- Complete skills reference
- Workflow examples
- Advanced features
- Troubleshooting guide
- Migration from v2.0

## File Structure

```
.
├── .codex/skills/           # Agent Skills (install these!)
│   ├── prd-setup/
│   │   ├── SKILL.md
│   │   ├── references/
│   │   └── assets/
│   ├── prd-track/
│   ├── prd-implement/
│   ├── prd-status/
│   ├── prd-generate/
│   └── README.md           # Full documentation
│
├── codex/                  # Original extension (for reference)
│   ├── commands/           # Old TOML files (deprecated)
│   ├── protocols/          # Documentation (now in skills/references)
│   └── templates/          # Templates (now in skills/assets)
│
└── README.md              # This file
```

## What's New in v3.0?

**Major Changes:**
- Converted from proprietary TOML format to **Agent Skills standard**
- Skills now work with any Agent Skills-compatible tool
- Better organization with references and assets
- Improved progressive disclosure
- Cross-tool portability

**Skill Name Changes:**
- `$setup` → `$prd-setup`
- `$newTrack` → `$prd-track`
- `$implement` → `$prd-implement`
- `$status` → `$prd-status`
- `/prd:generate` → `$prd-generate`

## Compatibility

- **Claude Code CLI** ✅ (native support)
- **OpenAI Codex CLI** ✅ (if it adopts Agent Skills standard)
- **Any Agent Skills-compatible tool** ✅

## Migration from v2.0

Your existing `codex/` directory with product context and tracks is fully compatible!

Just:
1. Install new skills to `~/.codex/skills/`
2. Use new skill names (`$prd-*`)
3. Everything else works the same

## Comparison

| Aspect | v2.0 (Extension) | v3.0 (Agent Skills) |
|--------|-----------------|---------------------|
| Standard | Codex-specific | Agent Skills (universal) |
| Format | TOML | SKILL.md |
| Portability | Low | High |
| Invocation | `$setup` | `$prd-setup` |
| Discovery | Manual | Auto-discovery |
| Data | Compatible ✅ | Compatible ✅ |

## Examples

### Initialize New Project

```
$prd-setup
> What do you want to build?
> A task management app
>
> E) Auto-generate ← Skip questions!
```

### Create Track

```
$prd-track Add user authentication
> E) Auto-generate ← Skip questions!
```

### Implement Track

```
$prd-implement
```

The AI will:
- Follow your workflow (TDD, standard, etc.)
- Run tests and quality checks
- Commit after each task
- Update progress automatically

## Why Use This?

**Before (Traditional Development):**
- Jump straight to code
- Unclear requirements
- Scope creep
- Lost context between sessions
- Hard to track progress

**After (PRD-Driven Development):**
- Clear specifications first
- Organized track-based workflow
- Systematic implementation
- Automatic progress tracking
- Context preserved between sessions

## License

MIT License

## Author

**Vedant Parmar**

Inspired by Conductor (Gemini CLI extension)

## Support

- **Full Documentation**: [.codex/skills/README.md](.codex/skills/README.md)
- **Issues**: Open an issue on GitHub
- **Standard**: [Agent Skills](https://agentskills.io)

---

**Start building better products faster!**

```bash
# Get started in 30 seconds
cp -r .codex/skills/* ~/.codex/skills/
cd your-project
$prd-setup
```
