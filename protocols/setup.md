# Protocol: Project Setup

**Trigger:** User asks to "setup project", "initialize", or `/setup`.

**Goal:** Establish the `codex/` context directory and create all core definition files through an interactive, intelligent process.

---

## 1.0 SYSTEM DIRECTIVE

You are executing the **Setup Protocol** for the Codex Agent framework. This protocol establishes the foundational context that will drive all future development. Adhere to these instructions precisely and sequentially.

**CRITICAL**: You must validate the success of every file operation. If any operation fails, you MUST halt immediately, announce the failure to the user, and await further instructions.

---

## 1.1 PRE-SETUP: STATE MANAGEMENT CHECK

**PROTOCOL: Before starting setup, determine if this is a fresh setup or a resumption.**

### 1.1.1 Check for Existing State

1. **Check for `codex/setup_state.json`**:
   - If it **does NOT exist**: This is a fresh setup. Proceed to Section 1.2.
   - If it **exists**: Read its content and resume from the last successful step.

2. **Resume Logic**:
   - Let `last_successful_step` in the JSON be `STEP`.
   - Based on `STEP`, jump to the next logical section:
     - If `STEP` is `"1.3_project_type_detected"`: Proceed to Section 2.1 (Product Guide)
     - If `STEP` is `"2.1_product_guide"`: Announce "Resuming setup: Product Guide complete. Next: Product Guidelines." → Proceed to Section 2.2
     - If `STEP` is `"2.2_product_guidelines"`: Announce "Resuming setup: Guidelines complete. Next: Tech Stack." → Proceed to Section 2.3
     - If `STEP` is `"2.3_tech_stack"`: Announce "Resuming setup: Tech Stack defined. Next: Code Styleguides." → Proceed to Section 2.4
     - If `STEP` is `"2.4_code_styleguides"`: Announce "Resuming setup: Styleguides selected. Next: Workflow." → Proceed to Section 2.5
     - If `STEP` is `"2.5_workflow"`: Announce "Resuming setup: Workflow defined. Next: Initial Track." → Proceed to Section 3.0
     - If `STEP` is `"3.1_tracks_registry_created"`: Announce "Setup complete! You can now create tracks with `/newTrack` or check status with `/status`." → Halt.
     - If `STEP` is unrecognized: Announce error and halt.

---

## 1.2 PRE-INITIALIZATION OVERVIEW

### 1.2.1 Welcome Message

Present the following overview to the user:

> **Welcome to Codex Agent!**
>
> I will guide you through the following steps to set up your project for Context-Driven Development:
>
> **Phase 1: Project Discovery**
> - Analyze your current directory to determine if this is a new or existing project
> - Detect version control, dependencies, and existing code
>
> **Phase 2: Context Definition**
> - Collaboratively define your product vision and goals
> - Establish design guidelines and brand principles
> - Document your technology stack and architecture
>
> **Phase 3: Workflow Configuration**
> - Select appropriate code style guides for your languages
> - Customize your development workflow (testing, review, deployment)
>
> **Phase 4: Initialization Complete**
> - Create track registry for managing features and bugs
> - Ready to start development with `/newTrack`
>
> Let's get started!

---

## 1.3 PROJECT TYPE DETECTION

**PROTOCOL: Determine if this is a "Brownfield" (existing) or "Greenfield" (new) project.**

### 1.3.1 Detection Logic

Execute the following checks:

1. **Version Control Check**:
   - Check for `.git`, `.svn`, or `.hg` directories
   - If `.git` exists, run `git status --porcelain`
   - If output is not empty, classify as **Brownfield** (dirty repo)

2. **Dependency Manifest Check**:
   - Look for: `package.json`, `pom.xml`, `requirements.txt`, `go.mod`, `Cargo.toml`, `composer.json`, `Gemfile`, `build.gradle`, `pyproject.toml`
   - If ANY found, classify as **Brownfield**

3. **Source Code Check**:
   - Look for directories: `src/`, `app/`, `lib/`, `pkg/`, `internal/`, `cmd/`
   - Check if they contain code files (`.js`, `.ts`, `.py`, `.go`, `.java`, `.rb`, etc.)
   - If found, classify as **Brownfield**

4. **Greenfield Condition**:
   - Classify as **Greenfield** ONLY if:
     - NO version control directories found
     - NO dependency manifests found
     - NO source code directories with code files found
     - Directory is empty OR contains only documentation (e.g., single `README.md`)

### 1.3.2 Proceed Based on Project Type

#### If BROWNFIELD (Existing Project):

1. **Announce Detection**:
   > "I've detected an existing project (Brownfield). I'll analyze the codebase to understand the current state."

2. **Check for Uncommitted Changes**:
   - If `git status --porcelain` showed uncommitted changes:
     > "WARNING: You have uncommitted changes in your Git repository. Please commit or stash your changes before proceeding, as Codex will be making modifications."

3. **Request Permission for Analysis**:
   > "I need to perform a read-only scan of your project to understand its structure, technology stack, and purpose. This will help me create accurate context files.
   >
   > May I proceed with the analysis?
   > A) Yes, proceed with analysis
   > B) No, I'll provide context manually
   >
   > Please respond with A or B."

4. **Handle Response**:
   - If **A (Yes)**: Proceed to Section 1.4 (Brownfield Analysis)
   - If **B (No)**: Skip to Section 2.1 (Product Guide) and rely on user input

#### If GREENFIELD (New Project):

1. **Announce Detection**:
   > "I've detected a new project (Greenfield). I'll help you set up the foundation for your product."

2. **Initialize Git Repository**:
   - If `.git` does not exist:
     - Execute: `git init`
     - Announce: "Initialized new Git repository."

3. **Create Codex Directory and State File**:
   - Execute: `mkdir -p codex`
   - Create `codex/setup_state.json`:
     ```json
     {"last_successful_step": ""}
     ```

4. **Ask Initial Question**:
   > "What do you want to build? Please describe your product idea in a few sentences."
   - **CRITICAL**: Wait for user response before proceeding
   - Once received, create `codex/product.md`:
     ```markdown
     # Initial Concept

     [User's response here]
     ```

5. **Update State**:
   - Write to `codex/setup_state.json`:
     ```json
     {"last_successful_step": "1.3_project_type_detected"}
     ```

6. **Proceed to Section 2.1** (Product Guide)

---

## 1.4 BROWNFIELD PROJECT ANALYSIS

**PROTOCOL: Intelligently analyze existing codebase to extract context.**

### 1.4.1 Respect Ignore Files

**CRITICAL**: Before scanning any files, check for `.gitignore` and `.codexignore` files.

1. **Read Ignore Patterns**:
   - If `.codexignore` exists, use its patterns (takes precedence)
   - If `.gitignore` exists, use its patterns
   - Combine patterns from both files

2. **Common Ignore Patterns** (fallback if no ignore files):
   - `node_modules/`, `.m2/`, `build/`, `dist/`, `bin/`, `target/`
   - `.git/`, `.idea/`, `.vscode/`, `.vs/`
   - `*.log`, `*.tmp`, `*.cache`
   - `vendor/`, `deps/`, `_build/`

### 1.4.2 Efficient File Listing

1. **Use Git-Aware Commands** (if `.git` exists):
   ```bash
   git ls-files --exclude-standard -co
   ```

2. **Use Find with Ignore Patterns** (if no Git):
   ```bash
   find . -type f -not -path "*/node_modules/*" -not -path "*/.git/*" ...
   ```

### 1.4.3 Prioritized File Analysis

Analyze files in this priority order:

1. **README and Documentation**:
   - `README.md`, `README.txt`, `CONTRIBUTING.md`, `ARCHITECTURE.md`
   - Extract: Project purpose, goals, architecture overview

2. **Dependency Manifests**:
   - `package.json`, `pom.xml`, `requirements.txt`, `go.mod`, etc.
   - Extract: Languages, frameworks, libraries, database drivers

3. **Configuration Files**:
   - `.env.example`, `config.yaml`, `application.properties`
   - Extract: External services, deployment targets, environment variables

4. **Source Directory Structure**:
   - List top 2-3 levels of `src/`, `app/`, `lib/` directories
   - Extract: Architecture pattern (MVC, microservices, monorepo, etc.)

5. **Key Source Files** (sample only):
   - Main entry point (e.g., `index.js`, `main.py`, `Main.java`)
   - For large files (>1MB), read only first and last 20 lines

### 1.4.4 Extract Project Context

Based SOLELY on the analyzed files, extract:

1. **Programming Language(s)**:
   - Primary: [Language]
   - Secondary: [Languages if multi-language project]

2. **Frameworks**:
   - Frontend: [Framework or None]
   - Backend: [Framework or None]

3. **Database/Storage**:
   - Database: [DB type inferred from drivers/config]
   - Caching: [Redis, Memcached, etc. if detected]

4. **Architecture Pattern**:
   - Type: [Monolith, Microservices, Monorepo, MVC, etc.]
   - Evidence: [Brief justification from directory structure]

5. **Project Goal** (one sentence):
   - Summarize based on README header or package.json description

### 1.4.5 Present Findings

> "Based on my analysis, here's what I found:
>
> **Language**: [Primary language]
> **Frameworks**: [Frontend] / [Backend]
> **Database**: [Database type]
> **Architecture**: [Pattern]
> **Project Goal**: [One-sentence summary]
>
> Is this accurate?
> A) Yes, proceed with this understanding
> B) No, let me provide corrections
>
> Please respond with A or B."

- If **A**: Create `codex/` directory and `setup_state.json`, proceed to Section 2.1
- If **B**: Ask user for corrections, then proceed

---

## 2.0 PHASE 1: INTERACTIVE CONTEXT CREATION

**PROTOCOL: Create comprehensive context files through guided interaction.**

---

## 2.1 GENERATE PRODUCT GUIDE (`product.md`)

### 2.1.1 Introduction

Announce:
> "Let's define your product vision. I'll ask you up to 5 questions to understand what you're building and why."

### 2.1.2 Question Guidelines

**CRITICAL RULES**:
1. **Ask questions ONE AT A TIME** - Wait for user response after each question
2. **Classify each question** as either:
   - **Additive**: Allows multiple answers (e.g., target users, features, goals)
   - **Exclusive Choice**: Requires single answer (e.g., pricing model, primary platform)
3. **Provide 3 suggested answers** for each question (A, B, C)
4. **Always include options**:
   - D) Type your own answer
   - E) Auto-generate and review product.md
5. **Format as vertical list**

**For Additive Questions**:
> [Question text]? **(Select all that apply)**
>
> A) [Option A]
> B) [Option B]
> C) [Option C]
> D) Type your own answer
> E) Auto-generate and review product.md

**For Exclusive Choice Questions**:
> [Question text]?
>
> A) [Option A] (Recommended)
> B) [Option B]
> C) [Option C]
> D) Type your own answer
> E) Auto-generate and review product.md

### 2.1.3 Example Questions

Tailor these based on Greenfield vs. Brownfield:

**For Greenfield**:
1. "Who are your target users?" (Additive)
2. "What are the core goals of this product?" (Additive)
3. "What are the must-have features for the MVP?" (Additive)
4. "What problem does this product solve?" (Exclusive)
5. "What makes this product unique?" (Exclusive)

**For Brownfield**:
1. "Based on the code, I see [inferred purpose]. Is this accurate, or should I adjust?" (Exclusive)
2. "Who are the primary users of this product?" (Additive)
3. "What are the current focus areas for development?" (Additive)

### 2.1.4 Auto-Generation Logic

If user selects option **E** at any point:
1. **Stop asking questions**
2. **Infer remaining details** from:
   - Previous answers
   - Brownfield analysis (if applicable)
   - Industry best practices
3. **Generate comprehensive `product.md`**
4. **Present for review** (skip to 2.1.6)

### 2.1.5 Draft Generation

**CRITICAL**: Use ONLY the user's selected answers as source of truth. Do NOT include:
- The questions you asked
- Unselected options (A/B/C)
- Conversational markers (A), B), C), etc.)

Generate `product.md` with this structure:

```markdown
# Product Vision

[High-level product purpose and mission]

## Target Users

[Detailed user personas based on answers]

## Core Goals

[Product objectives and success criteria]

## Key Features

[Must-have features for this product]

## Unique Value Proposition

[What makes this product special]

## Success Metrics

[How you'll measure product success]
```

### 2.1.6 User Confirmation Loop

Present the drafted content:
> "I've drafted the product guide. Please review:
>
> ```markdown
> [Drafted product.md content]
> ```
>
> What would you like to do?
> A) Approve - This looks good, proceed
> B) Suggest Changes - I'll tell you what to modify
>
> Please respond with A or B."

**Loop Logic**:
- If **B**: Ask "What changes would you like?" → Apply changes → Re-present → Repeat
- If **A**: Proceed to 2.1.7

### 2.1.7 Write File and Update State

1. **Write to `codex/product.md`** (append to existing if Greenfield, or create new)
2. **Update `codex/setup_state.json`**:
   ```json
   {"last_successful_step": "2.1_product_guide"}
   ```
3. **Proceed immediately to Section 2.2**

---

## 2.2 GENERATE PRODUCT GUIDELINES (`product-guidelines.md`)

### 2.2.1 Introduction

Announce:
> "Now let's define your product guidelines - the design principles and brand standards that will guide development."

### 2.2.2 Interactive Question Process

Follow the same process as 2.1.2, but focus on:

**Example Topics**:
1. "What's your brand's tone of voice?" (Exclusive)
   - A) Professional and formal
   - B) Friendly and conversational
   - C) Technical and precise
   - D) Type your own
   - E) Auto-generate

2. "What design principles should guide the UI/UX?" (Additive)
   - A) Simplicity and minimalism
   - B) Accessibility-first
   - C) Mobile-first responsive design
   - D) Type your own
   - E) Auto-generate

3. "What are your brand colors or visual identity guidelines?" (Exclusive/Additive)

**For Brownfield**: Analyze existing UI/UX patterns and ask for confirmation.

### 2.2.3 Draft Structure

```markdown
# Product Guidelines

## Brand Voice and Tone

[Communication style guidelines]

## Design Principles

[UI/UX guiding principles]

## Visual Identity

[Colors, typography, spacing, etc.]

## Content Standards

[Writing style, terminology, accessibility]

## User Experience Standards

[Interaction patterns, error handling, feedback]
```

### 2.2.4 Confirmation and State Update

- Follow same confirmation loop as 2.1.6
- Write to `codex/product-guidelines.md`
- Update state:
  ```json
  {"last_successful_step": "2.2_product_guidelines"}
  ```
- Proceed to Section 2.3

---

## 2.3 GENERATE TECH STACK (`tech-stack.md`)

### 2.3.1 Introduction

Announce:
> "Let's document your technology stack - the languages, frameworks, and tools that power your product."

### 2.3.2 Brownfield Special Handling

**If Brownfield**:
1. **State inferred stack** from analysis:
   > "Based on my code analysis, I've detected:
   > - **Language**: [Detected]
   > - **Frontend**: [Detected or None]
   > - **Backend**: [Detected or None]
   > - **Database**: [Detected or None]
   >
   > Is this correct?
   > A) Yes, proceed with this stack
   > B) No, I need to make corrections
   >
   > Please respond with A or B."

2. **If A**: Use detected stack, skip to draft generation
3. **If B**: Ask correction questions, then proceed

### 2.3.3 Greenfield Interactive Questions

**Example Questions** (max 5):
1. "What is your primary programming language?" (Exclusive)
   - A) JavaScript/TypeScript (Recommended for web)
   - B) Python (Recommended for AI/data)
   - C) Go (Recommended for backend services)
   - D) Type your own
   - E) Auto-generate

2. "What frontend framework will you use?" (Exclusive)
   - A) React
   - B) Vue
   - C) None (vanilla HTML/CSS/JS)
   - D) Type your own
   - E) Auto-generate

3. "What backend framework will you use?" (Exclusive)
4. "What database will you use?" (Exclusive)
5. "What additional tools or services do you plan to use?" (Additive)
   - A) Redis for caching
   - B) Docker for containerization
   - C) CI/CD (GitHub Actions, etc.)
   - D) Type your own
   - E) Auto-generate

### 2.3.4 Draft Structure

```markdown
# Technology Stack

## Programming Languages

- **Primary**: [Language and version]
- **Secondary**: [Other languages if applicable]

## Frontend

- **Framework**: [Framework or None]
- **UI Library**: [Component library if any]
- **State Management**: [If applicable]
- **Build Tool**: [Webpack, Vite, etc.]

## Backend

- **Framework**: [Express, Django, etc.]
- **API Style**: [REST, GraphQL, gRPC]
- **Authentication**: [Strategy]

## Database & Storage

- **Primary Database**: [PostgreSQL, MongoDB, etc.]
- **Caching**: [Redis, Memcached, etc.]
- **File Storage**: [S3, local, etc.]

## DevOps & Tools

- **Version Control**: Git
- **CI/CD**: [GitHub Actions, etc.]
- **Containerization**: [Docker, Kubernetes, etc.]
- **Monitoring**: [Tools if applicable]

## Code Quality

- **Linting**: [ESLint, Pylint, etc.]
- **Formatting**: [Prettier, Black, etc.]
- **Testing**: [Jest, Pytest, etc.]

## Architecture

- **Pattern**: [Monolith, Microservices, Serverless, etc.]
- **Deployment**: [Cloud provider, on-prem, etc.]
```

### 2.3.5 Confirmation and State Update

- Follow confirmation loop
- Write to `codex/tech-stack.md`
- Update state:
  ```json
  {"last_successful_step": "2.3_tech_stack"}
  ```
- Proceed to Section 2.4

---

## 2.4 SELECT CODE STYLEGUIDES

### 2.4.1 Introduction

Announce:
> "I'll now set up code style guides for your languages to ensure consistent code quality."

### 2.4.2 Language Detection

Based on `tech-stack.md`, identify primary language(s):
- JavaScript/TypeScript → `javascript.md`
- Python → `python.md`
- Go → `go.md`
- Java → `java.md`
- Ruby → `ruby.md`
- etc.

### 2.4.3 Copy from Templates

1. **Check if templates exist**:
   - Look in `codex/templates/code_styleguides/`

2. **For each detected language**:
   - Copy template to `codex/code_styleguides/[language].md`
   - Announce: "Created style guide for [Language]"

3. **If template doesn't exist**:
   - Create basic styleguide with industry standards
   - Announce: "Created basic style guide for [Language] - you can customize this later"

### 2.4.4 State Update

- Update state:
  ```json
  {"last_successful_step": "2.4_code_styleguides"}
  ```
- Proceed to Section 2.5

---

## 2.5 GENERATE WORKFLOW (`workflow.md`)

### 2.5.1 Introduction

Announce:
> "Let's define your development workflow - how you'll write, test, review, and deploy code."

### 2.5.2 Interactive Questions

**Example Questions**:
1. "What testing approach will you follow?" (Exclusive)
   - A) TDD (Test-Driven Development) - Write tests first (Recommended)
   - B) Test after implementation
   - C) Manual testing only
   - D) Type your own
   - E) Auto-generate

2. "What code review process will you use?" (Exclusive)
   - A) PR review required before merge
   - B) Pair programming
   - C) Solo development (no formal review)
   - D) Type your own
   - E) Auto-generate

3. "What CI/CD checks should run automatically?" (Additive)
   - A) Run tests on every commit
   - B) Lint and format checks
   - C) Security scanning
   - D) Type your own
   - E) Auto-generate

### 2.5.3 Draft Structure

Use template from `codex/templates/workflow.md` if exists, or create:

```markdown
# Development Workflow

## Testing Strategy

[TDD, test-after, or manual approach]

### Test Commands

- **Run all tests**: `[command]`
- **Run unit tests**: `[command]`
- **Run integration tests**: `[command]`
- **Coverage report**: `[command]`

## Code Review Process

[PR review, pair programming, etc.]

## Git Workflow

- **Branch naming**: `feature/`, `bugfix/`, `hotfix/`
- **Commit messages**: [Convention]
- **Merge strategy**: [Squash, rebase, merge commits]

## Quality Gates

Before marking any task complete:
- [ ] All tests pass
- [ ] Code follows style guide
- [ ] No lint errors
- [ ] Code reviewed (if applicable)
- [ ] Documentation updated

## Deployment Process

[How code goes to production]

## Phase Completion Verification

At the end of each implementation phase:
1. Run full test suite
2. Verify all acceptance criteria met
3. Manual smoke testing (if applicable)
4. User verification checkpoint
```

### 2.5.4 Confirmation and State Update

- Follow confirmation loop
- Write to `codex/workflow.md`
- Update state:
  ```json
  {"last_successful_step": "2.5_workflow"}
  ```
- Proceed to Section 3.0

---

## 3.0 FINALIZATION

### 3.1 Create Tracks Registry

1. **Create `codex/tracks.md`**:
   ```markdown
   # Tracks Registry

   This file tracks all features, bugs, and tasks in the project.

   ## Active Tracks

   _No active tracks yet. Create one with `/newTrack`._

   ## Completed Tracks

   _No completed tracks yet._

   ## Legend

   - `[ ]` Not Started
   - `[~]` In Progress
   - `[x]` Completed
   ```

2. **Update state**:
   ```json
   {"last_successful_step": "3.1_tracks_registry_created"}
   ```

### 3.2 Final Announcement

> "**Project setup complete!**
>
> I've created the following context files in `codex/`:
> - `product.md` - Your product vision and goals
> - `product-guidelines.md` - Design and brand standards
> - `tech-stack.md` - Technology choices
> - `workflow.md` - Development process
> - `code_styleguides/` - Language-specific style guides
> - `tracks.md` - Feature/bug tracking registry
>
> **Next Steps**:
> - Create your first track: `/newTrack "description"`
> - Check project status: `/status`
>
> All future development will be guided by this context. You can edit these files anytime to update your project's direction."

### 3.3 Session Ready

- Codex Agent is now fully initialized
- Ready to accept `/newTrack`, `/status`, and other commands
- Context-driven development enabled

---

**END OF SETUP PROTOCOL**
