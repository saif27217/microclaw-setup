---
name: concise-planning
description: 'Use when a user asks for a plan for a coding task, to generate a clear, actionable, and atomic checklist. Creates minimal, focused plans with 6-10 ordered steps.'
---

# Concise Planning

Turn a user request into a single, actionable plan with atomic steps.

## When to Use This Skill

- User asks for a plan, roadmap, or checklist
- Breaking down a feature request
- Planning refactoring or migration work
- Estimating implementation scope

## Workflow

### 1. Scan Context

- Read `README.md`, docs, and relevant code files
- Identify constraints (language, frameworks, tests)
- Understand existing architecture

### 2. Minimal Interaction

- Ask at most 1-2 questions and only if truly blocking
- Make reasonable assumptions for non-blocking unknowns
- Proceed with sensible defaults when information is incomplete

### 3. Generate Plan

Create a plan with:

- **Approach**: 1-3 sentences on what and why
- **Scope**: Bullet points for "In" and "Out"
- **Action Items**: 6-10 atomic, ordered tasks (verb-first)
- **Validation**: At least one item for testing

## Plan Template

```markdown
# Plan

<High-level approach>

## Scope

- In:
  - <Feature/change being implemented>
- Out:
  - <Explicitly not included>

## Action Items

[ ] <Step 1: Discovery/Research>
[ ] <Step 2: Implementation>
[ ] <Step 3: Implementation>
[ ] <Step 4: Implementation>
[ ] <Step 5: Validation/Testing>
[ ] <Step 6: Rollout/Commit>

## Open Questions

- <Question 1 (max 3)>
```

## Checklist Guidelines

### Atomic

Each step should be a single logical unit of work:

- [ ] "Add user authentication" (single feature)
- [ ] NOT "Add auth, write tests, deploy" (three things)

### Verb-First

Start each item with an action verb:

- [ ] "Create database schema for users"
- [ ] "Implement login endpoint"
- [ ] "Add input validation"
- [ ] "Verify authentication works"

### Concrete

Name specific files or modules when possible:

- [ ] "Update src/auth/login.ts"
- [ ] "Add validation to api/users.ts"
- [ ] "Create migration 001_add_users.sql"

## Examples

### Good Plan

```markdown
# Plan

Add JWT authentication to the API using existing user database.

## Scope

- In:
  - JWT token generation
  - Token validation middleware
  - Login endpoint
- Out:
  - Password reset flow
  - OAuth/social login

## Action Items

[ ] Read existing user model and auth configuration
[ ] Create JWT utility functions in lib/auth.ts
[ ] Add token validation middleware
[ ] Update login endpoint to return JWT
[ ] Add auth tests for login flow
[ ] Document API authentication

## Open Questions

- Should refresh tokens be implemented?
```

### Poor Plan (Avoid)

```markdown
# Plan

Add authentication to the app.

## Action Items

[ ] Do auth stuff
[ ] Make it work
[ ] Test everything

## Open Questions

- What kind of auth?
- What database?
```

## Validation

Always include at least one validation step:

- [ ] "Test the feature manually"
- [ ] "Run existing test suite"
- [ ] "Verify with curl/Postman"
- [ ] "Code review changes"

## Principles

1. **Stay focused**: Keep scope tight
2. **Be realistic**: 6-10 items max
3. **Order matters**: Put discovery before implementation
4. **Validate**: Always include testing
5. **Be explicit**: "Out" scope prevents scope creep
