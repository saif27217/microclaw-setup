---
name: self-improvement
description: Master orchestrator for all learning loop components. Coordinates memory consolidation, skill improvement, user modeling, and continuous learning. Adapted for Microclaw.
version: 2.2.0
platforms: []
deps: []
---

# Self-Improvement - Master Learning Orchestrator (Microclaw)

Coordinates all learning systems to build persistent memory, improve skills, and adapt to user patterns over time.

## Architecture

```
┌─────────────────────────────────────────┐
│      Self-Improvement Orchestrator       │
│         (Microclaw Adapted)              │
└─────────────────────────────────────────┘
                    |
        ┌───────────┼───────────┐
        |           |           |
        v           v           v
┌──────────┐ ┌──────────┐ ┌──────────┐
│Continuous│ │ Learning │ │   User   │
│Learning  │ │   Loop   │ │ Modeling │
│  v2.2    │ │   v2.0   │ │          │
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

### 1. Memory Consolidation (Scheduled: Every 4 hours)

**Scheduled Task**: 
```
schedule_task(
  chat_id=524753197,
  prompt="Activate self-improvement skill and run memory consolidation",
  schedule_type="cron",
  schedule_value="0 0 */4 * * *"
)
```

**Consolidation Workflow**:
1. Read observation logs from `~/microclaw/memory/observations.jsonl`
2. Extract patterns and corrections
3. Update knowledge graph relationships
4. Create/update instincts with confidence scores
5. Archive processed observations
6. Update USER.md with new patterns

### 2. User Modeling

Maintain `~/microclaw/memory/USER.md` with:

```markdown
# User Profile: saifdkhan

**Last Updated**: 2026-05-09T11:03:07Z

## Behavioral Patterns
- Prefers grep before edit workflow
- Uses explicit error handling
- Follows test-driven development
- Prefers functional programming style

## Learning Model
- Confidence threshold for auto-apply: 0.7
- Correction frequency: Low (improving)
- Feedback style: Direct and specific
- Expertise areas: Python, JavaScript, DevOps

## Trust Indicators
- Correction frequency: 2 per week (decreasing)
- Acceptance rate of suggestions: 85%
- Autonomy preference: High (prefers action over asking)

## Active Instincts (confidence ≥ 0.7)
1. grep-before-edit (0.9) - workflow
2. prefer-explicit-errors (0.8) - code-style
3. test-first-development (0.7) - testing

## Evolution History
- 2026-05-09: Created safe-file-editing skill from workflow instincts
- 2026-05-08: Created error-handling-patterns skill

## Session Statistics
- Total sessions: 47
- Average tools per session: 8.3
- Most used tools: bash, read_file, edit_file
- Learning rate: High (new instincts forming quickly)
```

### 3. Skill Improvement Reviews

**Scheduled Task** (Every 2 weeks):
```
schedule_task(
  chat_id=524753197,
  prompt="Activate self-improvement skill and run skill improvement review",
  schedule_type="cron",
  schedule_value="0 0 0 */14 * *"
)
```

**Review Workflow**:
1. Analyze skill activation metrics
2. Identify underutilized skills (low activation count)
3. Review failed skill executions
4. Propose improvements or consolidation
5. Update skill documentation

### 4. Enhanced Search

Use Microclaw's built-in tools for context-aware recall:
- `structured_memory_search` - semantic search across memories
- `knowledge_graph_query` - relationship and temporal queries
- Pattern matching from instinct registry

## Scheduled Tasks

### Task 1: Memory Consolidation (Every 4 hours)

**Schedule**: `0 0 */4 * * *`
**Prompt**: "Activate self-improvement skill and run memory consolidation"

**Actions**:
1. Read `~/microclaw/memory/observations.jsonl`
2. For each observation:
   - Extract user corrections → create/update instincts
   - Extract patterns → update knowledge graph
   - Calculate confidence scores
3. Archive processed observations to `~/microclaw/memory/archive/observations-YYYY-MM-DD.jsonl`
4. Check for evolution candidates (confidence ≥ 0.8)
5. Update `~/microclaw/memory/USER.md`
6. Update `~/microclaw/memory/instinct-registry.json`

### Task 2: Instinct Evolution Check (Every 4 hours)

**Schedule**: `0 0 */4 * * *` (runs with consolidation)
**Prompt**: "Activate continuous-learning-v2 and check for evolution candidates"

**Actions**:
1. Query instincts with confidence ≥ 0.8
2. Group by domain (code-style, testing, workflow, etc.)
3. If cluster has 3+ related instincts:
   - Generate skill proposal
   - Log to `~/microclaw/memory/evolution-candidates.md`
   - Notify user via structured memory

### Task 3: Skill Metrics Update (Daily)

**Schedule**: `0 0 0 * * *`
**Prompt**: "Activate self-improvement skill and update skill metrics"

**Actions**:
1. Scan skill activation logs (if available)
2. Update metrics:
   - Activation count
   - Success rate
   - Last used timestamp
3. Flag skills for review (low usage < 5 in 30 days, low success < 70%)
4. Update `~/microclaw/memory/skill-metrics.json`

### Task 4: Skill Improvement Review (Every 2 weeks)

**Schedule**: `0 0 0 */14 * *`
**Prompt**: "Activate self-improvement skill and run comprehensive skill review"

**Actions**:
1. Review all skill metrics
2. Identify improvement opportunities
3. Propose skill consolidation (similar skills)
4. Generate improvement report
5. Update USER.md with recommendations

## File Structure

```
~/microclaw/
├── skills/
│   ├── continuous-learning-v2/
│   ├── learning-loop/
│   └── self-improvement/          # This skill
├── memory/
│   ├── instincts/
│   │   ├── personal/              # Auto-learned
│   │   └── inherited/             # Imported
│   ├── archive/                   # Archived observations
│   │   └── observations-2026-05-09.jsonl
│   ├── observations.jsonl         # Current observations
│   ├── instinct-registry.json     # Metadata
│   ├── evolution-candidates.md    # Skill proposals
│   ├── skill-metrics.json         # Skill usage stats
│   └── USER.md                    # User behavioral model
```

## Integration Points

### With Continuous Learning v2
- Orchestrates HEARTBEAT tasks
- Manages instinct confidence updates
- Coordinates evolution pipeline
- Archives processed observations

### With Learning Loop v2
- Receives skill creation proposals
- Tracks skill metrics
- Coordinates skill improvement
- Manages skill lifecycle

### With Knowledge Graph
- Records user preferences as triples
- Tracks temporal relationships
- Enables context-aware recall
- Stores high-confidence instincts

### With Structured Memory
- Stores high-level patterns
- Enables semantic search
- Maintains conversation context
- Archives learning milestones

## Commands (Conversational)

| User Says | Action |
|-----------|--------|
| "Show my learning status" | Display all learning metrics, instincts, and evolution candidates |
| "Consolidate memory now" | Force immediate memory consolidation |
| "Show evolution candidates" | List instinct clusters ready for skills |
| "Update user model" | Refresh USER.md with latest patterns |
| "Export learning data" | Package instincts + observations for backup |
| "Review my skills" | Show skill metrics and improvement opportunities |
| "Show HEARTBEAT status" | Display last run time and next scheduled run |

## Manual Consolidation Workflow

When user requests "Consolidate memory now":

1. **Read Observations**
   ```bash
   cat ~/microclaw/memory/observations.jsonl
   ```

2. **Process Each Observation**
   - Identify pattern type (correction, workflow, error-resolution)
   - Calculate confidence delta
   - Determine scope (project vs global)

3. **Update/Create Instincts**
   - Check if instinct exists
   - Update confidence and evidence
   - Create new YAML if needed

4. **Update Knowledge Graph**
   ```
   knowledge_graph_add(
     subject="saifdkhan",
     predicate="prefers",
     object="[pattern]"
   )
   ```

5. **Update USER.md**
   - Add new behavioral patterns
   - Update trust indicators
   - Refresh active instincts list

6. **Archive Observations**
   ```bash
   mv ~/microclaw/memory/observations.jsonl ~/microclaw/memory/archive/observations-$(date +%Y-%m-%d).jsonl
   touch ~/microclaw/memory/observations.jsonl
   ```

## Privacy & Control

- All data stays local in `~/microclaw/memory/`
- User can review/edit instincts before evolution
- Observations can be archived or deleted
- Export format excludes raw conversation content
- User controls confidence thresholds and automation level

## Success Metrics

Track in `~/microclaw/memory/instinct-registry.json`:

```json
{
  "last_updated": "2026-05-09T11:03:07Z",
  "total_instincts": 12,
  "average_confidence": 0.68,
  "instincts_evolved": 2,
  "skills_created": 2,
  "user_correction_frequency": 0.04,
  "heartbeat_runs": 47,
  "last_consolidation": "2026-05-09T08:00:00Z",
  "next_consolidation": "2026-05-09T12:00:00Z"
}
```

## Initial Setup

When first activated, create the memory structure:

```bash
mkdir -p ~/microclaw/memory/instincts/personal
mkdir -p ~/microclaw/memory/instincts/inherited
mkdir -p ~/microclaw/memory/archive
touch ~/microclaw/memory/observations.jsonl
touch ~/microclaw/memory/instinct-registry.json
touch ~/microclaw/memory/evolution-candidates.md
touch ~/microclaw/memory/USER.md
touch ~/microclaw/memory/skill-metrics.json
```

Initialize registry:
```json
{
  "last_updated": "2026-05-09T11:03:07Z",
  "total_instincts": 0,
  "average_confidence": 0.0,
  "instincts_evolved": 0,
  "skills_created": 0,
  "user_correction_frequency": 0.0,
  "heartbeat_runs": 0,
  "last_consolidation": null,
  "next_consolidation": null
}
```

---

*The meta-skill that makes all other skills better - now adapted for Microclaw.*
