# Protocol: PRD Generation

**Trigger:** User asks to "generate PRD", "/prd:generate", "/prd:refine", or "/prd:validate".

**Goal:** Create, refine, or validate Product Requirements Documents (PRDs) for products or major features.

---

## 1.0 OVERVIEW

PRD (Product Requirements Document) generation is for NEW products or MAJOR features that require comprehensive product-level documentation. This is different from track specs:

- **PRD**: WHAT and WHY - Product vision, market fit, user needs, business goals
- **Spec**: HOW - Technical implementation details for a specific track

---

## 2.0 PRD GENERATION (`/prd:generate`)

**When to use**: Starting a new product or defining a major feature set.

### 2.1 Initial Context Gathering

Ask the user:
> "I'll help you create a comprehensive PRD. What would you like to create a PRD for?
>
> A) New product
> B) Major feature or feature set
> C) Product redesign/pivot
>
> Please describe what you're building."

### 2.2 Interactive PRD Creation

Ask 10-15 questions across these categories:

**Product Vision** (3-4 questions):
- What problem does this solve?
- Who are the target users?
- What's the unique value proposition?
- What are the success metrics?

**Market & Users** (2-3 questions):
- What's the target market?
- Who are the competitors?
- What user personas exist?

**Features & Scope** (3-4 questions):
- What are the core features?
- What's in scope for V1/MVP?
- What's explicitly out of scope?

**Technical Considerations** (2-3 questions):
- What platforms/devices?
- What are the constraints?
- What are the dependencies?

**Business Model** (1-2 questions):
- How does this generate value?
- What's the go-to-market strategy?

### 2.3 Generate PRD

Create comprehensive PRD with this structure:

```markdown
# Product Requirements Document: [Product Name]

**Version**: 1.0
**Date**: [Date]
**Author**: [Name]
**Status**: Draft

---

## Executive Summary

[2-3 paragraph summary of the product]

## Problem Statement

### The Problem
[What problem are we solving?]

### Current Solutions
[How do people solve this today?]

### Why Now
[Why is this the right time?]

## Target Users

### Primary Persona
- **Name**: [Persona name]
- **Role**: [Job title/role]
- **Goals**: [What they want to achieve]
- **Pain Points**: [Current frustrations]

### Secondary Personas
[Additional user types]

## Product Vision

### Vision Statement
[Inspiring one-sentence vision]

### Success Metrics
- [Metric 1]: [Target]
- [Metric 2]: [Target]
- [Metric 3]: [Target]

## Market Analysis

### Target Market
- Size: [Market size]
- Growth: [Growth rate]
- Segments: [Market segments]

### Competitive Landscape
| Competitor | Strengths | Weaknesses | Differentiation |
|-----------|-----------|------------|-----------------|
| [Name 1]  | [...]     | [...]      | [Our advantage] |

## Product Overview

### Core Value Proposition
[What makes this product special?]

### Key Features (MVP)

#### Feature 1: [Name]
**Description**: [What it does]
**User Value**: [Why it matters]
**Priority**: [Must-have/Should-have/Nice-to-have]

#### Feature 2: [Name]
...

### Out of Scope (V1)
- [Feature deliberately excluded]
- [Feature for future versions]

## User Experience

### User Journeys
1. **[Journey Name]**: [Step-by-step flow]

### Key Screens/Pages
- [Screen 1]: [Purpose]
- [Screen 2]: [Purpose]

## Technical Requirements

### Platforms
- [Web/Mobile/Desktop]

### Performance Requirements
- Load time: [< X seconds]
- Uptime: [XX%]
- Concurrent users: [X users]

### Security & Privacy
- [Authentication method]
- [Data encryption]
- [Compliance requirements]

### Integrations
- [Third-party service 1]
- [Third-party service 2]

## Business Model

### Revenue Model
[How the product makes money]

### Pricing Strategy
[Pricing tiers and justification]

## Go-to-Market Strategy

### Launch Plan
- Phase 1: [Description]
- Phase 2: [Description]

### Marketing Channels
- [Channel 1]: [Strategy]
- [Channel 2]: [Strategy]

## Risks & Mitigation

| Risk | Impact | Probability | Mitigation Strategy |
|------|--------|------------|---------------------|
| [Risk 1] | High | Medium | [How to address] |

## Success Criteria

### Launch Criteria
- [ ] [Criterion 1]
- [ ] [Criterion 2]

### Success Metrics (6 months)
- [Metric]: [Target]

## Timeline & Milestones

- **MVP Launch**: [Date]
- **Beta**: [Date]
- **V1.0**: [Date]

## Open Questions

1. [Question requiring decision]
2. [Question requiring research]

## Appendix

### References
- [Link to research]
- [Link to competitive analysis]

### Glossary
- **Term**: Definition
```

### 2.4 Save PRD

Ask where to save:
> "Where should I save this PRD?
>
> A) As track PRD: `codex/tracks/[new-track]/prd.md` (creates new track)
> B) As product PRD: `codex/product-prd.md` (product-level)
> C) Custom location
>
> Please respond with A, B, or C."

---

## 3.0 PRD REFINEMENT (`/prd:refine`)

**When to use**: Improving existing PRD based on feedback or new information.

### 3.1 Load Existing PRD

Ask user for PRD location or auto-detect from context.

### 3.2 Identify Refinement Areas

> "What would you like to refine in the PRD?
>
> A) Add missing details to specific sections
> B) Update based on user feedback
> C) Revise scope or features
> D) Update market analysis
> E) Other (specify)
>
> Please describe what needs refinement."

### 3.3 Make Targeted Updates

Update only the specified sections, preserving the rest of the document.

### 3.4 Present Changes

Show diff of changes and get user approval before saving.

---

## 4.0 PRD VALIDATION (`/prd:validate`)

**When to use**: Ensuring PRD quality and completeness before implementation.

### 4.1 Completeness Check

Verify all required sections are present and filled:

```
[ ] Executive Summary exists and is clear
[ ] Problem Statement is well-defined
[ ] Target Users are specific
[ ] Success Metrics are measurable
[ ] Features are prioritized
[ ] Out of Scope is defined
[ ] Technical Requirements are specified
[ ] Risks are identified
[ ] Timeline exists
```

### 4.2 Quality Check

Verify quality of content:

```
[ ] Vision is inspiring and clear
[ ] Problem is validated (not assumed)
[ ] Users are specific personas (not "everyone")
[ ] Features have clear user value
[ ] Success metrics are SMART goals
[ ] Scope is realistic for timeline
[ ] Technical requirements are achievable
[ ] Risks have mitigation plans
```

### 4.3 Alignment Check

Check alignment with project context:

```
[ ] Aligns with product.md vision
[ ] Follows product-guidelines.md standards
[ ] Compatible with tech-stack.md choices
[ ] Feasible within workflow.md constraints
```

### 4.4 Generate Validation Report

> "📋 **PRD Validation Report**
>
> **Overall Score**: [X/10]
>
> **Completeness**: [Score]/9 sections complete
> **Quality**: [Score]/8 quality checks passed
> **Alignment**: [Score]/4 alignment checks passed
>
> **Issues Found**:
> - [Issue 1]
> - [Issue 2]
>
> **Recommendations**:
> 1. [Recommendation 1]
> 2. [Recommendation 2]
>
> **Status**: [Ready for Implementation / Needs Revision]"

---

**END OF PRD GENERATION PROTOCOL**
