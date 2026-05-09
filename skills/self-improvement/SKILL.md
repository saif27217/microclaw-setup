---
name: self-improvement
description: Master orchestrator for all learning loop components. Coordinates memory consolidation, skill improvement, user modeling, and continuous learning.
version: 1.0.0
---

# Self-Improvement - Master Learning Orchestrator

Coordinates all learning systems to build persistent memory, improve skills, and adapt to user patterns over time.

## Architecture

```
┌─────────────────────────────────────────┐
│      Self-Improvement Orchestrator       │
└─────────────────────────────────────────┘
                    |
        ┌───────────┼───────────┐
        |           |           |
        v           v           v
┌──────────┐ ┌──────────┐ ┌──────────┐
│Continuous│ │ Learning │ │   User   │
│Learning  │ │   Loop   │ │ Modeling │
│   v2.1   │ │   v1.0   │ │          │
└──────────┘ └──────────┘ └──────────┘
     |            |            |
     v            v            v
┌──────────────────────────────────────┐
│         Memory Systems               │
│  • Instincts (YAML)                 │
│  • Observations (JSONL)             │
│  • Knowledge Graph                  │
│  • Structured Memory                │
│  • USER.md (behavioral model)       │
└──────────────────────────────────────┘
```

## Core Components

### 1. Memory Consolidation (HEARTBEAT: Every 4 hours)

**Task: Consolidate Session Observations**
- Read observation logs from tmp/
- Extract patterns and corrections
- Update knowledge graph relationships
- Create/update instincts with confidence scores
- Archive processed observations

**Task: Instinct Evolution**
- Find high-confidence instincts (≥0.8)
- Cluster related instincts by domain
- Propose skill creation
- Log evolution candidates

### 2. User Modeling

Maintain `USER.md` with:

```markdown
# User Profile: [username]

## Behavioral Patterns
- Preferred workflows
- Common corrections
- Communication style
- Technical preferences

## Learning Model
- Confidence thresholds
- Feedback patterns
- Expertise areas

## Trust Indicators
- Correction frequency
- Acceptance rate of suggestions
- Autonomy preferences

## Active Instincts
[List of high-confidence instincts]

## Evolution History
[Skills created from instinct clusters]
```

### 3. Skill Improvement Reviews

**Periodic Review (Every 2 weeks)**
- Analyze skill activation metrics
- Identify underutilized skills
- Review failed skill executions
- Propose improvements or consolidation

### 4. Enhanced Search

Use structured_memory_search and knowledge_graph_query for:
- Context-aware skill activation
- Pattern-based recommendations
- Historical decision lookup
- User preference recall

## HEARTBEAT Tasks

### Task 1: Memory Consolidation (Every 4 hours)
```bash
# Pseudo-code workflow
1. Read tmp/session_observation_*.md files
2. For each observation:
   - Extract user corrections → create/update instincts
   - Extract patterns → update knowledge graph
   - Calculate confidence scores
3. Archive processed observations
4. Check for evolution candidates (confidence ≥ 0.8)
```

### Task 2: Instinct Evolution Check (Every 4 hours)
```bash
1. Query instincts with confidence ≥ 0.8
2. Group by domain (code-style, testing, workflow, etc.)
3. If cluster has 3+ related instincts:
   - Generate skill proposal
   - Log to memory/evolution-candidates.md
   - Notify user
```

### Task 3: Skill Metrics Update (Daily)
```bash
1. Scan skill activation logs
2. Update metrics:
   - Activation count
   - Success rate
   - Last used timestamp
3. Flag skills for review (low usage, low success)
```

## File Structure

```
~/.microclaw/
├── skills/
│   ├── continuous-learning-v2/
│   ├── learning-loop/
│   └── self-improvement/          # This skill
├── memory/
│   ├── instincts/
│   │   ├── personal/              # Auto-learned
│   │   └── inherited/             # Imported
│   ├── observations.jsonl         # Raw session data
│   ├── instinct-registry.json     # Metadata
│   ├── evolution-candidates.md    # Skill proposals
│   └── USER.md                    # User behavioral model
└── tmp/
    └── session_observation_*.md   # Session logs
```

## Integration Points

### With Continuous Learning v2
- Receives instinct creation triggers
- Manages instinct confidence updates
- Orchestrates evolution pipeline

### With Learning Loop v1
- Receives skill creation proposals
- Tracks skill metrics
- Coordinates skill improvement

### With Knowledge Graph
- Records user preferences as triples
- Tracks temporal relationships
- Enables context-aware recall

### With Structured Memory
- Stores high-level patterns
- Enables semantic search
- Maintains conversation context

## Commands

| Command | Action |
|---------|--------|
| "Show my learning status" | Display all learning metrics |
| "Consolidate memory now" | Force HEARTBEAT consolidation |
| "Show evolution candidates" | List instinct clusters ready for skills |
| "Update user model" | Refresh USER.md with latest patterns |
| "Export learning data" | Package instincts + observations |

## Privacy & Control

- All data stays local in ~/.microclaw/
- User can review/edit instincts before evolution
- Observations can be archived or deleted
- Export format excludes raw conversation content
- User controls confidence thresholds

## Success Metrics

Track over time:
- Instinct count and average confidence
- Skills created from instinct evolution
- User correction frequency (should decrease)
- Skill activation success rate (should increase)
- Memory recall accuracy

---

*The meta-skill that makes all other skills better.*
