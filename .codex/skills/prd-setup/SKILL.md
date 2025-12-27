---
name: prd-setup
description: Initialize project with PRD-driven development context - creates product.md, tech-stack.md, workflow.md, and initial track
metadata:
  short-description: Initialize PRD-driven project context
  version: 2.0.0
  author: Vedant Parmar
  category: project-initialization
  tags: [prd, setup, initialization, context, greenfield, brownfield]
---

# PRD Setup Skill

Initialize a project with Context-Driven Development using the PRD-first methodology. This skill creates the foundational context files needed for efficient product development.

## What This Does

Sets up a comprehensive project context including:
- **Product Vision** (product.md) - What you're building and why
- **Design Guidelines** (product-guidelines.md) - Brand and UX standards
- **Tech Stack** (tech-stack.md) - Technologies and architecture
- **Code Standards** (code_styleguides/) - Language-specific style guides
- **Workflow** (workflow.md) - Development and testing process
- **First Track** - Initial development track with spec and plan

## When to Use

- Starting a new project (greenfield)
- Adding PRD structure to existing project (brownfield)
- Need to establish clear product context
- Want spec-driven development workflow

## Instructions

### CRITICAL: Resume Support

Before starting, check for `codex/setup_state.json`:

1. **Read State File:**
   - If missing: New setup. Proceed to Welcome.
   - If exists: Read `last_successful_step` value and resume from that step.

2. **Resume Logic:**
   - `"2.1_product_guide"` → Resume at Product Guidelines
   - `"2.2_product_guidelines"` → Resume at Tech Stack
   - `"2.3_tech_stack"` → Resume at Code Styleguides
   - `"2.4_code_styleguides"` → Resume at Workflow
   - `"2.5_workflow"` → Resume at Track Generation
   - `"3.3_initial_track"` → Announce complete, suggest next steps

---

### Step 1: Welcome

Present overview:

> "Welcome to PRD Setup! I'll guide you through:
>
> **Phase 1: Project Discovery**
> - Detect if this is a new or existing project
>
> **Phase 2: Context Definition**
> - Define product vision and goals
> - Establish design guidelines
> - Document tech stack and architecture
>
> **Phase 3: Track Creation**
> - Create your first development track with detailed plan
>
> Let's begin!"

---

### Step 2: Project Detection

**Brownfield (Existing) Indicators:**
- `.git`, `.svn`, or `.hg` directory exists
- Package manifests: `package.json`, `pom.xml`, `requirements.txt`, `go.mod`, `Cargo.toml`, etc.
- Source directories: `src/`, `app/`, `lib/` with code files

**Classification:**
- If ANY brownfield indicator found → **Brownfield**
- Otherwise → **Greenfield**

#### For Greenfield:
1. Initialize Git if no `.git`: `git init`
2. Ask: "What do you want to build? (Describe in 1-2 sentences)"
3. Wait for response, then:
   - Create `codex/` directory
   - Write `codex/setup_state.json`: `{"last_successful_step": ""}`
   - Write response to `codex/product.md` under `# Initial Concept`

#### For Brownfield:
1. Announce: "Existing project detected."
2. Check for uncommitted changes - warn user to commit/stash if dirty
3. Ask permission to scan:
   > "I detected an existing project. May I scan it to understand context?
   > A) Yes
   > B) No"
4. If approved, scan project:
   - Read `.gitignore` and `.claudeignore` first (if exist)
   - Prioritize: `README.md`, package manifests
   - For files >1MB: Read only first/last 20 lines
   - Extract: Tech stack, architecture, project goal
5. Create state file: `mkdir -p codex && echo '{"last_successful_step": ""}' > codex/setup_state.json`

---

### Step 3: Product Guide (Interactive)

**Maximum 5 questions. Ask ONE at a time. Wait for response.**

#### Question Format

Every question must offer:
```
A) [Option A]
B) [Option B]
C) [Option C]
D) Type your own answer
E) Auto-generate and review product.md
```

#### Sample Questions

For **Greenfield:**
1. "Who are your target users? (Select all that apply)"
2. "What are the core goals?"
3. "What are must-have MVP features? (Select all that apply)"
4. "What problem does this solve?"
5. "What makes this unique?"

For **Brownfield:**
Ask context-aware questions based on scan results.

#### Auto-Generate Logic

If user selects **E**, stop questions. Infer remaining details from context and previous answers.

#### Draft & Approval Loop

1. Generate `product.md` from answers (expand and polish)
2. Present draft:
   > "I've drafted the product guide. Please review:
   >
   > ```markdown
   > [content]
   > ```
   >
   > A) Approve - Proceed
   > B) Suggest Changes"

3. Loop until approved
4. Write file: Append to `codex/product.md`
5. Update state: `{"last_successful_step": "2.1_product_guide"}`

---

### Step 4: Product Guidelines (Interactive)

**Maximum 5 questions. Same format as Step 3.**

#### Sample Questions

1. "What's your brand tone?" (Exclusive)
2. "What design principles guide UI/UX? (Select all that apply)"
3. "Brand colors or visual identity?"
4. "Content standards?"
5. "UX interaction standards?"

#### Workflow

Same as Step 3:
- Ask sequentially (one by one)
- Option E = auto-generate
- Draft → Approve → Write `codex/product-guidelines.md`
- Update state: `{"last_successful_step": "2.2_product_guidelines"}`

---

### Step 5: Tech Stack (Interactive)

**Maximum 5 questions.**

#### For Greenfield

Ask about:
- Primary language(s)
- Frontend framework
- Backend framework
- Database
- Deployment platform

#### For Brownfield

State inferred stack from scan. Ask:
> "Based on analysis, your stack is:
> - Language: [detected]
> - Framework: [detected]
> ...
>
> A) Correct
> B) I'll provide the correct stack"

#### Workflow

- Draft → Approve → Write `codex/tech-stack.md`
- Update state: `{"last_successful_step": "2.3_tech_stack"}`

---

### Step 6: Code Styleguides (Automated)

1. List available guides from references/code_styleguides/
2. **For Greenfield:**
   - Recommend guides based on tech stack
   - Ask: "A) Use recommended | B) Customize selection"
   - If B: Show numbered list, ask which to copy
3. **For Brownfield:**
   - Auto-select based on detected languages
   - Ask: "A) Use suggested guides | B) Add more"
4. Execute: Copy selected guides to `codex/code_styleguides/`
5. Update state: `{"last_successful_step": "2.4_code_styleguides"}`

---

### Step 7: Workflow (Interactive)

1. Copy workflow template from references/workflow.md to `codex/workflow.md`
2. Ask: "Use default workflow or customize?

   Default includes:
   - >80% test coverage
   - Commit after each task
   - Git notes for summaries

   A) Default
   B) Customize"

3. **If Customize:**
   - Q1: "Change test coverage from 80%? A) No | B) Yes (type %)"
   - Q2: "Commit timing? A) After each task | B) After each phase"
   - Q3: "Task summaries? A) Git notes | B) Commit messages"

4. Update workflow.md with choices
5. Update state: `{"last_successful_step": "2.5_workflow"}`

---

### Step 8: Summarize Setup

Present summary:
> "Setup complete! Created:
> - codex/product.md
> - codex/product-guidelines.md
> - codex/tech-stack.md
> - codex/code_styleguides/
> - codex/workflow.md
>
> Next: I'll help you create your first development track."

---

### Step 9: Initial Track (For Greenfield Only)

#### 9.1 Gather Requirements (Max 5 questions)

Ask about:
1. "What user stories define the MVP?"
2. "What are functional requirements?"
3. "What are non-functional requirements?"
4. "Success criteria?"
5. "Acceptance criteria?"

Option E = auto-generate from context.

#### 9.2 Propose Track

Announce:
> "To create the MVP, I suggest:
>
> - [Track Title]"

Ask: "A) Approve | B) Suggest changes"

#### 9.3 Create Track Artifacts

Once approved:

1. **Generate track ID:** `{shortname}_{YYYYMMDD}` format
2. **Create directory:** `mkdir -p codex/tracks/{track_id}/`
3. **Write metadata.json:**
   ```json
   {
     "track_id": "{track_id}",
     "type": "feature",
     "status": "new",
     "created_at": "{ISO timestamp}",
     "updated_at": "{ISO timestamp}",
     "description": "{Track Title}"
   }
   ```

4. **Generate spec.md:** Detailed specification with acceptance criteria
5. **Generate plan.md:**
   - Follow workflow from `codex/workflow.md`
   - If TDD: Add "Write Tests" sub-task before each feature
   - Add phase completion tasks: `- [ ] Task: User Manual Verification '{Phase Name}' (Protocol in workflow.md)`

6. **Create tracks.md:**
   ```markdown
   # Project Tracks

   ## [ ] Track: {Track Title}
   *Link: [./codex/tracks/{track_id}/](./codex/tracks/{track_id}/)*
   ```

7. **Update state:** `{"last_successful_step": "3.3_initial_track"}`

#### 9.4 Finalize

1. Commit: `git add codex/ && git commit -m "codex(setup): Initialize project context"`
2. Announce:
   > "Setup complete! Your first track is ready.
   >
   > Next steps:
   > - Start implementing: Use prd-implement skill
   > - Check status: Use prd-status skill
   > - Create new track: Use prd-track skill"

---

## Critical Rules

1. **Ask ONE question at a time**
2. **Wait for user response** before next question
3. **Use only user's selected answers** (ignore unselected options A/B/C)
4. **Update state file** after each major section
5. **Validate all tool calls** before proceeding
6. **Option E stops questions** → auto-generate remaining
7. **Maximum 5 questions** per section

## References

- See `references/setup.md` for detailed protocol
- See `assets/workflow.md` for workflow template
- See `assets/code_styleguides/` for style guide templates

## Success Criteria

Setup is complete when:
- [ ] `codex/` directory exists
- [ ] `codex/product.md` created
- [ ] `codex/product-guidelines.md` created
- [ ] `codex/tech-stack.md` created
- [ ] `codex/code_styleguides/` populated
- [ ] `codex/workflow.md` created
- [ ] `codex/tracks.md` created (greenfield)
- [ ] First track created (greenfield)
- [ ] Git commit made

## Error Handling

- If tool call fails → Halt and await instructions
- If user denies brownfield scan → Ask for manual input
- If file write fails → Report error and retry once
- If git fails → Warn but continue (user can commit later)
