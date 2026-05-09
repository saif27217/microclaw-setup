---
name: learning-loop
description: Detects complex tasks and offers to create reusable skills. Tracks skill metrics and success rates for periodic improvement reviews. Adapted for Microclaw.
version: 2.0.0
platforms: []
deps: []
---

# Learning Loop v2.0 - Skill Creation System (Microclaw)

Automatically detects when complex tasks could become reusable skills and offers to capture them for future use.

## When to Trigger

Activate after completing tasks that involve:
- 3+ tool calls in sequence
- Multi-step reasoning or planning
- Repeated patterns that could be automated
- Complex workflows that might recur

## Detection Criteria

A task is a skill candidate when it has:
1. **Complexity**: Multiple steps with dependencies
2. **Reusability**: Could apply to similar future tasks
3. **Clarity**: Clear trigger conditions and outcomes
4. **Completeness**: Successfully executed end-to-end

## Workflow

### 1. Task Completion Detection
After completing a complex task:
- Count tool calls and reasoning steps
- Identify the core pattern or workflow
- Assess reusability potential

### 2. Skill Proposal
If criteria met, offer to create a skill:
```
I noticed this task involved [pattern description]. 
Would you like me to create a reusable skill for this?

Proposed skill:
- Name: [descriptive-name]
- Trigger: [when to use]
- Steps: [high-level workflow]
```

### 3. Skill Creation
If user agrees:
- Generate SKILL.md with YAML frontmatter
- Document trigger conditions
- Capture step-by-step instructions
- Include examples from current task
- Save to `~/microclaw/skills/` directory
- Log to `~/microclaw/memory/skill-metrics.json`

### 4. Skill Tracking
Maintain skill metrics in `~/microclaw/memory/skill-metrics.json`:

```json
{
  "skills": {
    "safe-file-editing": {
      "created": "2026-05-09T11:03:58Z",
      "activations": 0,
      "success_count": 0,
      "failure_count": 0,
      "success_rate": 0.0,
      "last_used": null,
      "source": "learning-loop",
      "evolved_from": ["grep-before-edit", "read-before-write", "verify-after-edit"]
    }
  },
  "last_updated": "2026-05-09T11:03:58Z"
}
```

## Skill Template

```markdown
---
name: skill-name
description: One-line description
version: 1.0.0
created: 2026-05-09
platforms: []
deps: []
---

# Skill Name

## When to Use
[Trigger conditions - be specific about when this skill applies]

## Prerequisites
[Any required tools, dependencies, or setup]

## Steps

1. **[Step 1 Title]**
   - Action details
   - Expected outcome

2. **[Step 2 Title]**
   - Action details
   - Expected outcome

## Examples

### Example 1: [Scenario]
[Concrete example from the original task]

## Notes
[Additional context, edge cases, limitations]

## Success Criteria
[How to verify the skill worked correctly]
```

## Periodic Review

**Scheduled Task** (Every 2 weeks):
```
Schedule: "0 0 0 */14 * *"
Prompt: "Activate learning-loop skill and run skill review"
```

Review:
- Underutilized skills (low activation count < 5 in 30 days)
- Failed skills (low success rate < 70%)
- Skill improvement opportunities
- Skill consolidation candidates (similar skills)

## Integration with Continuous Learning v2

This skill works alongside continuous-learning-v2:

| System | Focus | Output |
|--------|-------|--------|
| **Learning Loop v2** | Complex tasks (3+ tools) | Full skills |
| **Continuous Learning v2** | Observations & corrections | Atomic instincts |

Both systems feed into skill evolution:
```
Complex Task → Learning Loop → Skill (immediate)
     +
Observations → Instincts → Clustered Skills (over time)
```

## Commands (Conversational)

| User Says | Action |
|-----------|--------|
| "Review my skills" | Show skill metrics and usage statistics |
| "Improve [skill-name]" | Analyze and enhance existing skill |
| "Consolidate skills" | Merge related skills |
| "Show skill metrics" | Display activation counts and success rates |
| "Create skill from this task" | Manually trigger skill creation |

## Skill Metrics Tracking

After each skill activation, update metrics:

```bash
# Example: Update skill metrics after activation
# This would be done automatically by the system
{
  "skills": {
    "safe-file-editing": {
      "activations": 5,
      "success_count": 4,
      "failure_count": 1,
      "success_rate": 0.8,
      "last_used": "2026-05-09T11:03:58Z"
    }
  }
}
```

## Skill Review Criteria

### Underutilized Skills
- Activations < 5 in last 30 days
- Consider: Is the skill too specific? Should it be consolidated?

### Low Success Rate
- Success rate < 70%
- Consider: Are instructions unclear? Missing prerequisites?

### Consolidation Candidates
- Multiple skills with similar triggers
- Consider: Can they be merged into one comprehensive skill?

## Example: Creating a Skill

### Scenario
User completes a complex task involving:
1. `grep` to search for patterns
2. `read_file` to verify context
3. `edit_file` to make changes
4. `bash` to run tests

### Detection
Learning Loop detects:
- 4 tool calls in sequence
- Clear workflow pattern
- High reusability potential

### Proposal
```
I noticed this task involved a safe file editing workflow:
1. Search for patterns with grep
2. Read files to verify context
3. Edit files with changes
4. Verify with tests

Would you like me to create a "safe-file-editing" skill for this workflow?
```

### Creation
If approved, create `~/microclaw/skills/safe-file-editing/SKILL.md`:

```markdown
---
name: safe-file-editing
description: Safe workflow for editing files with verification steps
version: 1.0.0
created: 2026-05-09
platforms: []
deps: []
---

# Safe File Editing

## When to Use
When you need to edit files in a codebase and want to ensure accuracy and safety.

## Steps

1. **Search for patterns**
   - Use `grep` to find relevant code patterns
   - Identify all files that need changes

2. **Verify context**
   - Use `read_file` to understand surrounding code
   - Confirm the change is appropriate

3. **Make changes**
   - Use `edit_file` with precise edits
   - Apply changes to all identified files

4. **Verify changes**
   - Run tests with `bash`
   - Confirm no regressions

## Example
[Include the actual task that triggered this skill creation]

## Success Criteria
- All tests pass
- No unintended side effects
- Changes are consistent across files
```

## Privacy & Control

- All skill metrics stay local in `~/microclaw/memory/`
- User approves skill creation before it happens
- Skills can be edited or deleted anytime
- Metrics help identify which skills are actually useful

## Success Metrics

Track in `~/microclaw/memory/skill-metrics.json`:
- Total skills created via learning loop
- Average skill activation count
- Average skill success rate
- Skills consolidated or removed
- Skills evolved from instincts

---

*Teaching the system to teach itself, one task at a time.*
