# Protocol: Validation

**Trigger:** User asks to "validate", "/validate", or during implementation verification.

**Goal:** Validate code implementation against specifications, PRDs, and quality standards.

---

## 1.0 VALIDATION TYPES

### 1.1 Code vs. Spec Validation
Verify implementation matches track specification.

### 1.2 Code vs. PRD Validation
Verify implementation aligns with product requirements.

### 1.3 Quality Validation
Verify code meets quality standards from workflow.md and style guides.

---

## 2.0 SPEC VALIDATION

**PROTOCOL: Verify code matches track spec.md**

### 2.1 Load Context

Read:
1. `codex/tracks/<track_id>/spec.md`
2. Implemented code files
3. Test files

### 2.2 Functional Requirements Check

For each requirement in spec.md:

```
[ ] Requirement is implemented
[ ] Implementation matches description
[ ] Edge cases are handled
[ ] Error handling is appropriate
```

### 2.3 Acceptance Criteria Check

For each criterion in spec.md:

```
[ ] Criterion is met
[ ] Tests verify criterion
[ ] User can verify manually
```

### 2.4 Non-Functional Requirements Check

```
[ ] Performance requirements met
[ ] Security requirements met
[ ] Accessibility requirements met
[ ] Compatibility requirements met
```

### 2.5 Generate Report

> "**Spec Validation Report**
>
> **Track**: [Track ID]
> **Spec**: [Track title]
>
> **Functional Requirements**: [X/Y] met
> **Acceptance Criteria**: [X/Y] met
> **Non-Functional Requirements**: [X/Y] met
>
> **Issues**:
> - [Issue 1]
>
> **Status**: [Pass/Fail]"

---

## 3.0 PRD VALIDATION

**PROTOCOL: Verify implementation aligns with PRD**

### 3.1 Load PRD

Read product-level PRD if exists.

### 3.2 Alignment Checks

```
[ ] Implements PRD feature correctly
[ ] Follows PRD user experience guidelines
[ ] Meets PRD performance targets
[ ] Aligns with PRD technical requirements
[ ] Supports PRD user journeys
```

### 3.3 Generate Report

Similar to spec validation but at product level.

---

## 4.0 QUALITY VALIDATION

**PROTOCOL: Verify code quality standards**

### 4.1 Code Style Check

```
[ ] Follows code_styleguides/ for language
[ ] Naming conventions consistent
[ ] Formatting is correct
[ ] No linter errors
```

### 4.2 Test Coverage Check

```
[ ] Tests exist for new code
[ ] Tests cover edge cases
[ ] Tests are meaningful
[ ] All tests pass
```

### 4.3 Security Check

```
[ ] No obvious vulnerabilities
[ ] Input validation present
[ ] Authentication/authorization correct
[ ] Sensitive data protected
```

### 4.4 Performance Check

```
[ ] No obvious performance issues
[ ] Efficient algorithms used
[ ] Resource usage acceptable
```

---

## 5.0 COMPREHENSIVE VALIDATION

**Run all validation types and generate comprehensive report.**

> "🔍 **Comprehensive Validation Report**
>
> **Spec Compliance**: [Score]/10
> **PRD Alignment**: [Score]/10
> **Code Quality**: [Score]/10
> **Test Coverage**: [X]%
> **Security**: [Pass/Fail]
>
> **Overall Status**: [Ready/Needs Work]
>
> **Critical Issues**: [Number]
> **Warnings**: [Number]
>
> **Recommendations**:
> 1. [Recommendation]
> 2. [Recommendation]"

---

**END OF VALIDATION PROTOCOL**
