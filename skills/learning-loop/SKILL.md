---
name: learning-loop
description: Detects complex tasks and offers to create reusable skills. Tracks skill metrics and success rates for periodic improvement reviews.
version: 1.0.0
---

# Learning Loop - Skill Creation System

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
- Save to skills/ directory

### 4. Skill Tracking
Maintain skill metrics:
- Creation date
- Times activated
- Success rate
- Last used date
- User feedback

## Skill Template

```markdown
---
name: skill-name
description: One-line description
version: 1.0.0
created: YYYY-MM-DD
metrics:
  activations: 0
  success_rate: 0.0
  last_used: null
---

# Skill Name

## When to Use
[Trigger conditions]

## Steps
1. [Step 1]
2. [Step 2]
...

## Examples
[Examples from original task]

## Notes
[Additional context]
```

## Periodic Review

Every 2 weeks or 50 tasks, review:
- Underutilized skills (low activation count)
- Failed skills (low success rate)
- Skill improvement opportunities
- Skill consolidation candidates

## Integration with Continuous Learning v2

This skill works alongside continuous-learning-v2:
- **Learning Loop (v1)**: Creates full skills after complex tasks
- **Continuous Learning (v2)**: Builds atomic instincts from observations

Both systems feed into skill evolution:
```
Complex Task → Learning Loop → Skill
     +
Observations → Instincts → Clustered Skills
```

## Commands

| Command | Action |
|---------|--------|
| "Review my skills" | Show skill metrics and usage |
| "Improve [skill-name]" | Analyze and enhance existing skill |
| "Consolidate skills" | Merge related skills |

---

*Teaching the system to teach itself, one task at a time.*
