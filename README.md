# Codex PRD Extension

**Developed by Vedant Parmar**

**Measure twice, code once.**

Codex PRD Extension is a comprehensive framework specifically designed for **OpenAI Codex CLI**. It enables **Context-Driven Development** with **Ultra-Thinking** capabilities, transforming your Codex CLI session into an autonomous, agentic project manager that follows strict protocols to specify, plan, and implement software features with PRD-first methodology.

Instead of just writing code, Codex PRD Extension ensures a consistent, high-quality lifecycle for every task: **Context → PRD/Spec → Plan → Implement → Validate**.

---

## Table of Contents

- [Features](#features)
- [Integration with Codex CLI](#integration-with-codex-cli)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Customization Guide](#customization-guide)
- [Commands Reference](#commands-reference)
- [Project Structure](#project-structure)
- [Workflow](#workflow)
- [Team Setup & Sharing](#team-setup--sharing)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

---

## Features

- **Codex CLI Specific**: Built exclusively for OpenAI Codex CLI integration
- **Ultra-Thinking Context Engineering**: Multi-level planning with deep context analysis
- **PRD-First Development**: Generate comprehensive Product Requirements Documents
- **Plan before you build**: Create detailed specs and implementation plans
- **Maintain context**: Ensure AI follows style guides, tech stack choices, and product goals
- **Iterate safely**: Review plans before code is written
- **Team collaboration**: Share context across your team via git
- **Brownfield & Greenfield**: Works with existing codebases and new projects
- **Autonomous Implementation**: AI executes plans autonomously with quality gates
- **Protocol-Based**: 767-851 line protocols for every operation
- **Quality Gates**: Never proceed without validation
- **Phase Verification**: User verification at every major milestone

---

## Integration with Codex CLI

### How It Works

This extension integrates with Codex CLI using a **context-injection approach**:

1. **AGENT.md** contains all system instructions for Codex CLI
2. **Protocol files** define detailed operational procedures
3. **Codex CLI** loads the context when you start a session
4. **Virtual commands** like `/setup`, `/newTrack`, `/implement` are recognized

### Architecture

```
┌─────────────────────────────────────────────────────────┐
│                  OpenAI Codex CLI                       │
└────────────────────┬────────────────────────────────────┘
                     │
                     │ codex --context ./codex/AGENT.md
                     ▼
┌─────────────────────────────────────────────────────────┐
│                    AGENT.md                             │
│  • Ultra-Thinking Principles                            │
│  • Command Recognition (/setup, /newTrack, etc.)       │
│  • Protocol Mapping                                     │
│  • Quality Gates & Validation Rules                    │
└────────────────────┬────────────────────────────────────┘
                     │
                     │ Loads protocols when executing commands
                     ▼
┌─────────────────────────────────────────────────────────┐
│                   protocols/                            │
│  • setup.md (767 lines)                                 │
│  • track_management.md (851 lines)                      │
│  • implementation.md (747 lines)                        │
│  • prd_generation.md                                    │
│  • validation.md                                        │
└─────────────────────────────────────────────────────────┘
```

### Why Context-Injection?

**For Codex CLI**: Unlike some AI CLIs with native extension systems, Codex CLI uses context loading via the `--context` flag. This approach:
- Works natively with Codex CLI's architecture
- Keeps all protocols readable and modifiable
- Enables version control and team sharing
- Allows full customization without CLI restrictions

---

1.  **Enforcing Structure:** The AI cannot write code without first generating a Specification (`spec.md`) and a phased Plan (`plan.md`).
2.  **Persistent Context:** It maintains a `codex/` directory in your project root which acts as the "Source of Truth" for your Product Vision, Tech Stack, and Workflow.
3.  **Autonomous Implementation:** Using the `/implement` command, the agent works through your task list, verifies its own work, and only marks tasks as complete when they meet the project standards.
4.  **Native Integration:** Built specifically for the Codex CLI ecosystem, utilizing native features like `/diff`, `/compact`, and `codex cloud`.

### Prerequisites

1. **OpenAI Codex CLI** installed and configured
2. **Git** (for version control and team sharing)
3. Your project directory

### Step-by-Step Installation

#### 1. Clone into Your Project

```bash
# Navigate to your project root
cd /path/to/your/project

# Clone this repository into a 'codex' directory
git clone https://github.com/your-username/codex-prd.git codex

# Remove the .git directory to make it part of your project
cd codex && rm -rf .git && cd ..
```

#### 2. Verify Installation

```bash
# Check that all files are present
ls -la codex/

# Should show:
# AGENT.md
# codex-extension.json
# protocols/
# templates/
# README.md
```

#### 3. Test the Integration

```bash
# Start Codex CLI with the extension context
codex --context ./codex/AGENT.md

# In the Codex CLI session, type:
# /setup
```

If the agent responds with setup instructions, the integration is working!

---

## Quick Start

### 1. Start Codex CLI with Context

**Every time you start Codex CLI**, load the context:

```bash
codex --context ./codex/AGENT.md
```

**Pro tip**: Create a shell alias to make this easier:

```bash
# Add to your ~/.bashrc or ~/.zshrc
alias codex-prd='codex --context ./codex/AGENT.md'

# Then simply run:
codex-prd
```

### 2. Initialize Your Project

In the Codex CLI session:
```
/setup
```

**For existing projects (Brownfield):**
```
Agent: "I've detected an existing project. I'll analyze the codebase..."
Agent: "Based on my analysis:
        Language: JavaScript/TypeScript
        Frameworks: React / Node.js
        Database: PostgreSQL

        Is this accurate?
        A) Yes, proceed
        B) No, let me provide corrections"

You: A

Agent: "Let's define your product vision. I'll ask up to 5 questions..."
```

**For new projects (Greenfield):**
```
Agent: "I've detected a new project. I'll help you set up the foundation."
Agent: "What do you want to build? Please describe your product idea."

You: A task management app for development teams

Agent: "Let's define your product vision. I'll ask up to 5 questions..."
```

### 3. Create a Track

```
/newTrack "Add user authentication"
```

The agent will:
- Ask 3-5 clarifying questions
- Generate specification (spec.md)
- Generate implementation plan (plan.md)
- Add to tracks registry

### 4. Implement the Track

```
/implement
```

The agent will:
- Load all context
- Execute plan step-by-step
- Follow your workflow (TDD, testing, etc.)
- Pause at phase boundaries for verification
- Update documentation when complete

### 5. Check Progress

```
/status
```

---

## Customization Guide

### 1. Customize Workflow Template

The workflow template defines how the AI implements code (TDD, testing strategy, etc.).

**Location**: `codex/templates/workflow.md`

**Steps to customize:**

1. **Copy the template** (if not already done):
```bash
cp codex/templates/workflow.md codex/templates/workflow-custom.md
```

2. **Edit the workflow file**:
```bash
# Open in your editor
vim codex/templates/workflow.md
# or
code codex/templates/workflow.md
```

3. **Customize these sections**:

```markdown
# Development Workflow

## Testing Strategy
TDD (Test-Driven Development)  # Change to: "Test-After" or "Manual Testing"

### Test Commands
- **Run all tests**: `npm test`          # Change to your test command
- **Run unit tests**: `npm run test:unit`
- **Run integration tests**: `npm run test:integration`
- **Coverage report**: `npm run coverage`

## Code Review Process
PR review required before merge  # Change to your process

## Git Workflow
- **Branch naming**: `feature/`, `bugfix/`, `hotfix/`
- **Commit messages**: Conventional Commits  # Change to your convention
- **Merge strategy**: Squash and merge      # Change to your strategy

## Quality Gates
Before marking any task complete:
- [ ] All tests pass
- [ ] Code follows style guide
- [ ] No lint errors
- [ ] Code reviewed (if applicable)  # Add/remove checks as needed
- [ ] Documentation updated

## Deployment Process
Automatic deployment to staging on merge to develop  # Customize
Production deployment requires manual approval       # Customize
```

4. **When you run `/setup`**, the agent will use this template

**Example customizations:**

**For solo developers:**
```markdown
## Code Review Process
Solo development - no formal review required

## Quality Gates
- [ ] All tests pass
- [ ] Code follows style guide
- [ ] Self-review complete
```

**For teams with strict process:**
```markdown
## Code Review Process
- Minimum 2 reviewers required
- Security review for auth/payment code
- Architecture review for major changes

## Quality Gates
- [ ] All tests pass (minimum 80% coverage)
- [ ] Code follows style guide
- [ ] No lint errors or warnings
- [ ] Security scan passed
- [ ] Performance benchmarks met
- [ ] 2 approvals received
- [ ] Documentation updated
- [ ] Changelog entry added
```

### 2. Add Language-Specific Styleguides

Create styleguides for each language your project uses.

**Steps:**

#### Step 1: Create the Template Directory

```bash
mkdir -p codex/templates/code_styleguides
```

#### Step 2: Create Styleguide Files

**Example: JavaScript/TypeScript Styleguide**

Create `codex/templates/code_styleguides/javascript.md`:

```bash
cat > codex/templates/code_styleguides/javascript.md << 'EOF'
# JavaScript/TypeScript Style Guide

## General Principles
- Prefer readability over cleverness
- Write self-documenting code
- Use TypeScript for type safety

## Naming Conventions
- **Variables/Functions**: camelCase
  ```javascript
  const userName = "John";
  function getUserData() {}
  ```

- **Classes/Types**: PascalCase
  ```typescript
  class UserService {}
  interface UserData {}
  ```

- **Constants**: UPPER_SNAKE_CASE
  ```javascript
  const MAX_RETRIES = 3;
  const API_BASE_URL = "https://api.example.com";
  ```

- **Private properties**: prefix with underscore
  ```javascript
  class User {
    _password = "";
  }
  ```

## Code Structure
- One component/class per file
- Group imports: external, internal, relative
- Export at the bottom of file

## Formatting
- **Indentation**: 2 spaces
- **Line length**: 100 characters max
- **Quotes**: Single quotes for strings, double for JSX
- **Semicolons**: Required
- **Trailing commas**: Always in multiline

## Functions
- Arrow functions for callbacks and short functions
- Named functions for main logic
- Async/await over promises

```javascript
// Good
const processUsers = async (users) => {
  return await Promise.all(users.map(user => processUser(user)));
};

// Avoid
function processUsers(users) {
  return users.map(function(user) {
    return processUser(user);
  });
}
```

## Error Handling
- Always handle errors explicitly
- Use try-catch for async operations
- Provide meaningful error messages

```javascript
try {
  const data = await fetchData();
  return processData(data);
} catch (error) {
  logger.error('Failed to process data', { error });
  throw new ProcessingError('Data processing failed', { cause: error });
}
```

## Testing
- Test file naming: `*.test.ts` or `*.spec.ts`
- One describe block per function/component
- Arrange-Act-Assert pattern

## Comments
- Use JSDoc for public APIs
- Inline comments for complex logic only
- Avoid obvious comments

```javascript
/**
 * Processes user payment with retry logic
 * @param {string} userId - User identifier
 * @param {number} amount - Payment amount in cents
 * @returns {Promise<PaymentResult>}
 */
async function processPayment(userId, amount) {
  // Retry up to 3 times due to payment gateway flakiness
  return await retryOperation(() => gateway.charge(userId, amount), 3);
}
```

## Tools
- **Linter**: ESLint with TypeScript plugin
- **Formatter**: Prettier
- **Type checker**: TypeScript strict mode
EOF
```

**Example: Python Styleguide**

Create `codex/templates/code_styleguides/python.md`:

```bash
cat > codex/templates/code_styleguides/python.md << 'EOF'
# Python Style Guide

## General Principles
- Follow PEP 8
- Explicit is better than implicit
- Readability counts

## Naming Conventions
- **Variables/Functions**: snake_case
  ```python
  user_name = "John"
  def get_user_data():
      pass
  ```

- **Classes**: PascalCase
  ```python
  class UserService:
      pass
  ```

- **Constants**: UPPER_SNAKE_CASE
  ```python
  MAX_RETRIES = 3
  API_BASE_URL = "https://api.example.com"
  ```

- **Private**: prefix with underscore
  ```python
  class User:
      def __init__(self):
          self._password = ""
  ```

## Code Structure
- One class per file (unless closely related)
- Imports at top: standard lib, third-party, local
- Two blank lines between top-level definitions

## Formatting
- **Indentation**: 4 spaces
- **Line length**: 88 characters (Black formatter)
- **Quotes**: Double quotes for strings
- **Trailing commas**: In multiline structures

## Functions
- Type hints for all public functions
- Docstrings for all public APIs
- Keep functions small and focused

```python
def process_users(users: list[User]) -> list[ProcessedUser]:
    """
    Process a list of users and return processed results.

    Args:
        users: List of User objects to process

    Returns:
        List of ProcessedUser objects

    Raises:
        ProcessingError: If user processing fails
    """
    return [process_user(user) for user in users]
```

## Error Handling
- Specific exceptions over generic ones
- Use context managers for resources
- Document exceptions in docstrings

```python
try:
    data = fetch_data()
    return process_data(data)
except NetworkError as e:
    logger.error(f"Failed to fetch data: {e}")
    raise ProcessingError("Data processing failed") from e
```

## Testing
- Test file naming: `test_*.py`
- Use pytest
- Fixtures for setup

## Tools
- **Linter**: Pylint or Ruff
- **Formatter**: Black
- **Type checker**: mypy
EOF
```

#### Step 3: During Setup

When you run `/setup`, the agent will:
1. Detect languages from your tech-stack.md
2. Look for matching styleguides in `templates/code_styleguides/`
3. Copy them to `codex/code_styleguides/`
4. Use them during implementation

#### Step 4: Verify

```bash
# After setup, check that styleguides were copied
ls -la codex/code_styleguides/

# Should show files like:
# javascript.md
# python.md
```

### 3. Create Custom PRD Templates

**Location**: `codex/templates/prd/`

**Steps:**

```bash
# Create a SaaS-specific PRD template
cat > codex/templates/prd/saas-template.md << 'EOF'
# SaaS Product Requirements Document: [Product Name]

## Product Overview
[Brief description of the SaaS product]

## Target Market
- **Market Size**: [TAM/SAM/SOM]
- **Ideal Customer Profile**: [Description]
- **Pricing Tier**: [Freemium/Paid/Enterprise]

## Core Features
### Feature 1: [Name]
- **User Value**: [Why users need this]
- **Competitive Advantage**: [How this differentiates us]

## Subscription Model
- **Free Tier**: [What's included]
- **Pro Tier**: [What's included, price]
- **Enterprise Tier**: [What's included, custom pricing]

## Metrics
- **MRR Goal**: [Monthly Recurring Revenue target]
- **Churn Target**: [Acceptable churn rate]
- **CAC**: [Customer Acquisition Cost]
- **LTV**: [Lifetime Value]

[Rest of standard PRD sections...]
EOF
```

---

## Commands Reference

| Command | Description | Example |
| :--- | :--- | :--- |
| `/setup` | Initialize project with context files | `/setup` |
| `/newTrack` | Create new feature/bug track | `/newTrack "Add dark mode"` |
| `/implement` | Execute autonomous implementation | `/implement` |
| `/status` | Display project and track status | `/status` |
| `/validate` | Validate code against specs/PRD | `/validate` |
| `/prd:generate` | Generate Product Requirements Document | `/prd:generate` |
| `/prd:refine` | Refine existing PRD | `/prd:refine` |
| `/prd:validate` | Validate PRD quality | `/prd:validate` |

---

## Project Structure

```
your-project/
├── codex/                          # Extension directory
│   ├── AGENT.md                    # Load this with --context flag
│   ├── codex-extension.json        # Extension metadata
│   ├── protocols/                  # Detailed operation protocols
│   │   ├── setup.md               # 767 lines
│   │   ├── track_management.md    # 851 lines
│   │   ├── implementation.md      # 747 lines
│   │   ├── prd_generation.md
│   │   └── validation.md
│   ├── templates/                  # Templates (customize these!)
│   │   ├── workflow.md            # YOUR workflow definition
│   │   ├── code_styleguides/      # YOUR language styleguides
│   │   │   ├── javascript.md
│   │   │   ├── python.md
│   │   │   └── ...
│   │   └── prd/                   # PRD templates
│   │       └── product-prd-template.md
│   ├── product.md                  # Created by /setup
│   ├── product-guidelines.md       # Created by /setup
│   ├── tech-stack.md              # Created by /setup
│   ├── workflow.md                # Created by /setup
│   ├── code_styleguides/          # Created by /setup
│   │   └── [language].md
│   └── tracks/                    # Created by /setup
│       ├── tracks.md              # Track registry
│       └── [track-id]/
│           ├── spec.md
│           ├── plan.md
│           ├── prd.md (optional)
│           └── metadata.json
├── src/                            # Your source code
├── tests/                          # Your tests
└── ...
```

---

To use this framework, you need to load the `AGENT.md` file as the system context when launching your Codex session.

The development hierarchy:

```
1. PRODUCT VISION (product.md)
   ↓
2. PRD (optional for major features)
   ↓
3. SPEC (spec.md for each track)
   ↓
4. PLAN (plan.md for each track)
   ↓
5. IMPLEMENT (autonomous execution)
   ↓
6. VALIDATE (quality verification)
```

**Typical Flow:**
1. `/setup` - Initialize project
2. `/newTrack "feature"` - Create feature track
3. Review and approve spec and plan
4. `/implement` - AI builds the feature
5. Verify at phase checkpoints
6. `/validate` - Final quality check
7. Repeat for next feature

---

## Team Setup & Sharing

### Setup for Team Collaboration

#### 1. Initial Setup (Team Lead)

```bash
# 1. Install the extension in your project
cd your-project
git clone https://github.com/your-username/codex-prd.git codex
cd codex && rm -rf .git && cd ..

# 2. Customize for your team
# Edit codex/templates/workflow.md with team workflow
vim codex/templates/workflow.md

# 3. Add language styleguides your team uses
vim codex/templates/code_styleguides/javascript.md
vim codex/templates/code_styleguides/python.md

# 4. Run setup to initialize project context
codex --context ./codex/AGENT.md
# Then in Codex CLI: /setup

# 5. Commit everything to git
git add codex/
git commit -m "Add Codex PRD Extension for team

- Initialized project context
- Configured team workflow
- Added language styleguides
- Ready for team use"

git push origin main
```

#### 2. Team Member Setup

```bash
# 1. Clone the project (extension is already there)
git clone https://github.com/your-team/your-project.git
cd your-project

# 2. Verify codex directory exists
ls -la codex/
# Should show: AGENT.md, protocols/, templates/, product.md, etc.

# 3. Create shell alias for easy access
echo "alias codex-prd='codex --context ./codex/AGENT.md'" >> ~/.bashrc
source ~/.bashrc

# 4. Start using it
codex-prd
# In Codex CLI: /status (to see project state)
```

#### 3. Keeping Team in Sync

**When making changes to context:**

```bash
# Team lead updates workflow
vim codex/workflow.md

# Commit and push
git add codex/workflow.md
git commit -m "Update workflow: Add security review step"
git push

# Team members pull changes
git pull
# Changes automatically available next time they start Codex CLI
```

**When creating tracks:**

```bash
# Developer creates a track
codex-prd
# /newTrack "Add payment processing"
# Spec and plan are created in codex/tracks/payment-processing_20250125/

# Commit the track
git add codex/tracks/
git commit -m "Add track: Payment processing

- Created spec for Stripe integration
- Generated implementation plan"
git push

# Other team members can see the track
codex-prd
# /status (shows all tracks including new one)
```

### .gitignore Recommendations

Add this to your `.gitignore`:

```gitignore
# Codex - Keep most files, ignore these:
codex/setup_state.json          # Setup progress (project-specific)
codex/tracks/*/metadata.json    # Track metadata (auto-generated)

# Keep these committed for team:
# codex/AGENT.md
# codex/protocols/
# codex/templates/
# codex/product.md
# codex/product-guidelines.md
# codex/tech-stack.md
# codex/workflow.md
# codex/tracks/
```

### Team Workflow Example

**Scenario: Team building a web app**

```bash
# 1. Team lead sets up project (one time)
codex --context ./codex/AGENT.md
/setup
# Answers questions about product vision, tech stack, workflow
git add codex/ && git commit -m "Initialize Codex context" && git push

# 2. Developer A creates authentication feature
git pull  # Get latest context
codex --context ./codex/AGENT.md
/newTrack "Add user authentication"
# Reviews spec and plan, approves
/implement
# Track implemented with quality gates
git add . && git commit -m "Implement: User authentication" && git push

# 3. Developer B creates payment feature
git pull  # Gets latest code + context
codex --context ./codex/AGENT.md
/status  # Sees authentication track is complete
/newTrack "Add payment processing"
/implement
git add . && git commit -m "Implement: Payment processing" && git push

# 4. Developer C reviews project status
git pull
codex --context ./codex/AGENT.md
/status
# Sees: Authentication (complete), Payment (complete)
# Creates next track...
```

---

## Best Practices

### 1. Always Load Context

```bash
# Create an alias for convenience
alias codex-prd='codex --context ./codex/AGENT.md'

# Use it every time
codex-prd
```

### 2. Commit Context Files

```bash
# Commit all context for team sharing
git add codex/
git commit -m "Update project context"
git push
```

### 3. Review Before Approving

Always review generated specs and plans before approving. The AI generates based on your answers.

### 4. Keep Workflow Updated

As your team process evolves, update `codex/workflow.md`:

```bash
vim codex/workflow.md
git add codex/workflow.md
git commit -m "Update workflow: Add performance testing"
git push
```

### 5. Use Styleguides

Create and maintain styleguides for consistency:

```bash
vim codex/code_styleguides/javascript.md
# Update with team conventions
git commit -am "Update JS styleguide: Prefer async/await"
```

### 6. Use PRDs for Major Features

```bash
codex-prd
/prd:generate
# For new products or major feature sets
```

### 7. Verify at Checkpoints

Never skip phase verification checkpoints. They catch issues early.

---

## Troubleshooting

### "Commands not recognized"

**Problem**: Codex CLI doesn't respond to `/setup`, `/newTrack`, etc.

**Solution**: Ensure context is loaded:
```bash
codex --context ./codex/AGENT.md
```

### "Codex is not set up"

**Problem**: Trying to use `/newTrack` before running `/setup`.

**Solution**: Run `/setup` first.

### "Template not found"

**Problem**: Agent can't find workflow or styleguide templates.

**Solution**: Ensure templates exist:
```bash
ls codex/templates/workflow.md
ls codex/templates/code_styleguides/
```

### Implementation not following your workflow

**Problem**: AI not following TDD or your process.

**Solution**: Check `codex/workflow.md` - the AI follows it exactly:
```bash
cat codex/workflow.md
# Verify it matches your expectations
# Edit if needed
```

### Team member has outdated context

**Problem**: Team member's Codex using old workflow.

**Solution**: Pull latest changes:
```bash
git pull
# Context is automatically updated
```

---

## Advanced Tips

### Create Project-Specific Aliases

```bash
# Add to your project's .bashrc or team docs
cat >> ~/.bashrc << 'EOF'
# Codex PRD Extension aliases
alias cprd='cd /path/to/project && codex --context ./codex/AGENT.md'
alias cprd-setup='cd /path/to/project && codex --context ./codex/AGENT.md -e "/setup"'
alias cprd-status='cd /path/to/project && codex --context ./codex/AGENT.md -e "/status"'
EOF
```

### Quick Template Updates

```bash
# Update workflow
make update-workflow:
	vim codex/templates/workflow.md && \
	git add codex/templates/workflow.md && \
	git commit -m "Update workflow template"

# Add new styleguide
make add-styleguide:
	vim codex/templates/code_styleguides/$(LANG).md && \
	git add codex/templates/code_styleguides/ && \
	git commit -m "Add $(LANG) styleguide"
```

---

## FAQ

**Q: Do I need to load context every time?**

A: Yes. The `--context` flag tells Codex CLI to load AGENT.md. Create an alias to make it easier:
```bash
alias codex-prd='codex --context ./codex/AGENT.md'
```

**Q: Can I modify the protocols?**

A: Yes! All protocols are markdown files in `codex/protocols/`. Edit them to customize behavior.

**Q: How do I update the extension?**

A: Download new version and replace `codex/AGENT.md` and `codex/protocols/`. Keep your `product.md`, `tech-stack.md`, `workflow.md`, etc.

**Q: What if my team uses different languages?**

A: Create styleguides for each language in `codex/templates/code_styleguides/`. The setup will copy the ones matching your tech stack.

---

## Credits

**Developed by Vedant Parmar**

Specifically designed for OpenAI Codex CLI integration.

Inspired by [Conductor](https://github.com/gemini-cli-extensions/conductor) by the Gemini CLI community.

---

## Support

For issues or questions:
- Check `codex/protocols/` for detailed documentation
- Review `codex/AGENT.md` for system behavior
- Open an issue on GitHub

---

**Remember**: Measure twice, code once. Let Codex PRD Extension handle the thinking with Codex CLI.
