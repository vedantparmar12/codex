# Installation Guide - Codex PRD Extension

> ⚠️ **For Codex CLI Only**: This extension requires [OpenAI's Codex CLI](https://openai.com/codex).

## Prerequisites

### 1. Install Codex CLI

Follow OpenAI's official installation instructions:
- Visit: https://openai.com/codex
- Download and install Codex CLI for your platform
- Verify installation: `codex --version`

### 2. Authenticate

```bash
codex login
```

Follow the prompts to authenticate with your OpenAI account.

**Verify authentication:**
```bash
codex login status
# Should show: "Logged in as: your@email.com"
```

### 3. Enable Skills Feature

**Check if skills are enabled:**
```bash
cat ~/.codex/config.toml | grep -A 2 "\[features\]"
```

**If skills are not enabled, add to config:**
```bash
# Edit config file
nano ~/.codex/config.toml

# Add these lines:
[features]
skills = true
```

**Or via command:**
```bash
echo -e "\n[features]\nskills = true" >> ~/.codex/config.toml
```

## Installation Methods

### Method 1: User-Scoped (Recommended)

**Install for all your projects:**

```bash
# 1. Clone or download this repository
git clone https://github.com/your-repo/codex-prd.git
cd codex-prd

# 2. Copy skills to user directory
cp -r commands/codex ~/.codex/skills/

# 3. Verify installation
ls ~/.codex/skills/codex/
```

**Expected output:**
```
setup.toml
newTrack.toml
implement.toml
status.toml
```

**Advantages:**
- ✅ Available in all projects
- ✅ Install once, use everywhere
- ✅ Easier to update

### Method 2: Repo-Scoped

**Install for a specific project only:**

```bash
# 1. Navigate to your project
cd /path/to/your/project

# 2. Clone or download extension
git clone https://github.com/your-repo/codex-prd.git /tmp/codex-prd

# 3. Copy skills to project
cp -r /tmp/codex-prd/commands/codex .codex/skills/

# 4. Verify installation
ls .codex/skills/codex/
```

**Expected output:**
```
setup.toml
newTrack.toml
implement.toml
status.toml
```

**Advantages:**
- ✅ Project-specific customization
- ✅ Version control with your repo
- ✅ Team members get same skills

**Commit to version control:**
```bash
git add .codex/
git commit -m "Add codex PRD extension"
git push
```

## Verification

### 1. Test Skills Discovery

```bash
codex
```

In the Codex TUI, type:
```
$
```

**You should see autocomplete suggestions:**
```
$setup
$newTrack
$implement
$status
```

### 2. Test Setup Command

```bash
# In Codex TUI:
$setup
```

**Expected behavior:**
```
Welcome to Codex! I'll guide you through:
- Project Discovery
- Context Definition
- Track Creation

Let's begin!
```

If you see this, installation succeeded! ✅

### 3. Verify File Structure

**User-scoped installation:**
```bash
tree ~/.codex/skills/codex/
```

**Expected:**
```
~/.codex/skills/codex/
├── setup.toml
├── newTrack.toml
├── implement.toml
└── status.toml
```

**Repo-scoped installation:**
```bash
tree .codex/skills/codex/
```

**Expected:**
```
.codex/skills/codex/
├── setup.toml
├── newTrack.toml
├── implement.toml
└── status.toml
```

## Configuration

### Optional: Customize Config

Edit `~/.codex/config.toml` to customize Codex behavior:

```toml
[features]
skills = true

[defaults]
model = "gpt-4.1"  # or your preferred model
approvals = "suggest"  # or "auto", "full-auto", "read-only"

[experimental]
# Add experimental features if needed
```

## Troubleshooting

### Issue: Skills Not Found

**Symptom:**
```
> $setup
Error: Unrecognized command '$setup'
```

**Solutions:**

1. **Verify skills are enabled:**
   ```bash
   cat ~/.codex/config.toml | grep skills
   # Should show: skills = true
   ```

2. **Check skills location:**
   ```bash
   ls ~/.codex/skills/codex/
   # Should list .toml files
   ```

3. **Reinstall skills:**
   ```bash
   rm -rf ~/.codex/skills/codex/
   cp -r commands/codex ~/.codex/skills/
   ```

4. **Restart Codex:**
   ```bash
   # Exit current session
   /quit

   # Start new session
   codex
   ```

### Issue: Permission Denied

**Symptom:**
```bash
cp: cannot create directory '~/.codex/skills/': Permission denied
```

**Solution:**
```bash
# Create directory first
mkdir -p ~/.codex/skills/

# Then copy
cp -r commands/codex ~/.codex/skills/
```

### Issue: Skills Directory Not Found

**Symptom:**
```bash
ls: ~/.codex/skills: No such file or directory
```

**Solution:**
```bash
# Create skills directory
mkdir -p ~/.codex/skills/

# Copy skills
cp -r commands/codex ~/.codex/skills/
```

### Issue: Codex Not Installed

**Symptom:**
```bash
codex: command not found
```

**Solution:**
1. Visit https://openai.com/codex
2. Download and install Codex CLI
3. Follow OpenAI's installation guide
4. Verify: `codex --version`

### Issue: Not Authenticated

**Symptom:**
```bash
> codex
Error: Not authenticated. Please run 'codex login'
```

**Solution:**
```bash
codex login
# Follow authentication prompts
```

### Issue: Skills Not Autocompleting

**Symptom:**
Typing `$` doesn't show autocomplete

**Solutions:**

1. **Enable skills in config:**
   ```bash
   echo -e "\n[features]\nskills = true" >> ~/.codex/config.toml
   ```

2. **Check skill file format:**
   ```bash
   head -n 5 ~/.codex/skills/codex/setup.toml
   ```

   **Should show:**
   ```toml
   description = "..."
   prompt = """
   ...
   ```

3. **Verify file permissions:**
   ```bash
   chmod 644 ~/.codex/skills/codex/*.toml
   ```

## Updating

### Update User-Scoped Installation

```bash
# 1. Pull latest changes
cd /path/to/codex-prd
git pull origin main

# 2. Copy updated skills
cp -r commands/codex ~/.codex/skills/

# 3. Restart Codex
codex
```

### Update Repo-Scoped Installation

```bash
# 1. Pull extension updates
cd /path/to/codex-prd
git pull origin main

# 2. Copy to your project
cp -r commands/codex /path/to/your/project/.codex/skills/

# 3. Commit changes
cd /path/to/your/project
git add .codex/skills/codex/
git commit -m "Update codex PRD extension"
```

## Uninstallation

### Remove User-Scoped

```bash
rm -rf ~/.codex/skills/codex/
```

### Remove Repo-Scoped

```bash
cd /path/to/your/project
rm -rf .codex/skills/codex/
git add .codex/
git commit -m "Remove codex PRD extension"
```

### Remove All Extension Files

```bash
# Remove skills
rm -rf ~/.codex/skills/codex/
rm -rf .codex/skills/codex/

# Remove generated context (if desired)
rm -rf codex/
```

## Next Steps

After successful installation:

1. **Run setup:**
   ```bash
   codex
   $setup
   ```

2. **Create your first track:**
   ```bash
   $newTrack "Build MVP"
   ```

3. **Start implementing:**
   ```bash
   $implement
   ```

4. **Check progress:**
   ```bash
   $status
   ```

## Support

**Issues?**
- Check [Troubleshooting](#troubleshooting) section above
- Review README.md for usage examples
- Check Codex CLI documentation

**Feature requests or bugs?**
- Open an issue on GitHub
- Include your Codex CLI version: `codex --version`
- Include error messages if applicable

---

**Platform**: OpenAI Codex CLI only
**Version**: 2.0.0
**Last Updated**: 2025-01-15
