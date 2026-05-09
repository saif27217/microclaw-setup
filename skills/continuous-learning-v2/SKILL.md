---
name: continuous-learning-v2
description: Instinct-based learning system that observes sessions, creates atomic instincts with confidence scoring, and evolves them into skills. v2.1 adapted for nanobot architecture.
origin: ECC (adapted for nanobot)
version: 2.1.0
---

# Continuous Learning v2.1 - Instinct-Based Architecture

An advanced learning system that turns nanobot sessions into reusable knowledge through atomic "instincts" - small learned behaviors with confidence scoring.

## Architecture Adaptation for Nanobot

| Feature | Claude Code v2 | Nanobot Adaptation |
|---------|---------------|---------------------|
| Observation | PreToolUse/PostToolUse hooks | Session observation + HEARTBEAT |
| Analysis | Background Haiku agent | HEARTBEAT task (every 4 hours) |
| Storage | ~/.claude/homunculus/ | memory/instincts/ |
| Evolution | /evolve command | Auto-evolution on HEARTBEAT |
| Project scoping | git remote URL | Project context from conversation |

## The Instinct Model

An instinct is a small learned behavior:

```yaml
---
id: prefer-explicit-errors
trigger: "when handling errors in code"
confidence: 0.7
domain: "code-style"
source: "session-observation"
scope: global
created: 2026-04-05
observations: 3
---

# Prefer Explicit Errors

## Action
Use explicit error types and messages rather than generic catches.

## Evidence
- Observed 3 instances of explicit error handling preference
- User corrected generic catch to specific on 2026-04-05
- Pattern repeated in Python and JavaScript contexts
```

**Properties:**
- **Atomic** — one trigger, one action
- **Confidence-weighted** — 0.3 = tentative, 0.9 = near certain
- **Domain-tagged** — code-style, testing, git, debugging, workflow, etc.
- **Evidence-backed** — tracks what observations created it
- **Scope-aware** — `project` (default) or `global`

## How It Works

```
Session Activity
       |
       v
+--------------------------+
| OBSERVATION CAPTURE      |
| - Tool calls logged      |
| - User corrections noted |
| - Patterns detected     |
+--------------------------+
       |
       v
+--------------------------+
| memory/observations.jsonl|
| (prompts, tool calls,    |
|  outcomes, timestamps)   |
+--------------------------+
       |
        v (HEARTBEAT every 4 hours)
+--------------------------+
| PATTERN DETECTION        |
| * User corrections       |
| * Error resolutions      |
| * Repeated workflows     |
| * Scope decision         |
+--------------------------+
       |
       v
+--------------------------+
| INSTINCT CREATION        |
| memory/instincts/        |
| * prefer-functional.yaml |
| * always-validate.yaml   |
+--------------------------+
       |
       v (confidence >= 0.8)
+--------------------------+
| EVOLUTION                |
| Cluster related instincts|
| → Create skill/command   |
+--------------------------+
```

## File Structure

```
memory/
├── instincts/
│   ├── personal/           # Auto-learned instincts
│   │   ├── prefer-explicit-errors.yaml
│   │   └── grep-before-edit.yaml
│   └── inherited/          # Imported instincts
├── observations.jsonl      # Raw observations
└── instinct-registry.json  # Metadata

skills/
├── learning-loop/          # v1 skill (still active)
└── continuous-learning-v2/ # This skill
```

## Confidence Scoring

| Score | Meaning | Behavior |
|-------|---------|----------|
| 0.3 | Tentative | Suggested but not enforced |
| 0.5 | Moderate | Applied when relevant |
| 0.7 | Strong | Auto-approved for application |
| 0.9 | Near-certain | Core behavior |

**Confidence increases** when:
- Pattern is repeatedly observed (+0.1 per observation)
- User doesn't correct the suggested behavior
- Similar instincts from other sources agree

**Confidence decreases** when:
- User explicitly corrects the behavior (-0.2)
- Pattern isn't observed for extended periods
- Contradicting evidence appears

## Scope Decision Guide

| Pattern Type | Scope | Examples |
|--------------|-------|----------|
| Language/framework conventions | project | "Use React hooks", "Follow Django patterns" |
| File structure preferences | project | "Tests in __tests__/" |
| Code style | project | "Use functional style" |
| Security practices | global | "Validate user input", "Sanitize SQL" |
| General best practices | global | "Write tests first" |
| Tool workflow preferences | global | "Grep before Edit" |

## HEARTBEAT Tasks

### Task: Instinct Observation Analysis
1. Read `memory/observations.jsonl`
2. Detect patterns:
   - User corrections → create instinct
   - Error resolutions → create instinct
   - Repeated workflows → create instinct
3. Update existing instincts (confidence adjustment)
4. Check for evolution candidates (confidence >= 0.8)

### Task: Instinct Evolution
1. Find instincts with confidence >= 0.8
2. Cluster related instincts by domain
3. Generate skill/command proposal
4. Log to `memory/evolution-candidates.md`

## Commands (via conversation)

| Command | Description |
|---------|-------------|
| "Show my instincts" | Display all instincts with confidence |
| "Evolve instincts" | Cluster and propose skills |
| "Export instincts" | Export to file |
| "Import instincts" | Import from file |

## Integration with Existing Learning Loop

This v2 skill **enhances** the existing learning-loop skill:

| Feature | learning-loop (v1) | continuous-learning-v2 |
|---------|----------------------|-------------------------|
| Trigger | Complex task completion | Every session observation |
| Granularity | Full skills | Atomic instincts |
| Confidence | None | 0.3-0.9 weighted |
| Evolution | Direct to skill | Instincts → cluster → skill |
| Storage | skills/{name}/SKILL.md | memory/instincts/*.yaml |

Both run in parallel:
- v1: Offers skill creation after complex tasks
- v2: Continuously observes and builds instincts

## Privacy

- Observations stay local in `memory/`
- Only instincts (patterns) can be exported — not raw observations
- No actual code or conversation content is shared
- User controls what gets exported

---

*Instinct-based learning: teaching nanobot your patterns, one observation at a time.*