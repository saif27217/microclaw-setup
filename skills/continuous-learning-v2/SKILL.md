---
name: continuous-learning-v2
description: Instinct-based learning system that observes sessions, creates atomic instincts with confidence scoring, and evolves them into skills. Adapted for Microclaw architecture.
origin: ECC (adapted for Microclaw)
version: 2.2.0
platforms: []
deps: []
---

# Continuous Learning v2.2 - Instinct-Based Architecture (Microclaw)

An advanced learning system that turns Microclaw sessions into reusable knowledge through atomic "instincts" - small learned behaviors with confidence scoring.

## Architecture Adaptation for Microclaw

| Feature | Original (Nanobot) | Microclaw Adaptation |
|---------|-------------------|---------------------|
| Observation | PreToolUse/PostToolUse hooks | Manual logging after tool sequences |
| Analysis | Background agent | Scheduled task (every 4 hours) |
| Storage | ~/.claude/homunculus/ | ~/microclaw/memory/instincts/ |
| Evolution | /evolve command | "Show evolution candidates" |
| HEARTBEAT | Background process | `schedule_task` with cron |

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
created: 2026-05-09
observations: 3
last_observed: 2026-05-09
---

# Prefer Explicit Errors

## Action
Use explicit error types and messages rather than generic catches.

## Evidence
- Observed 3 instances of explicit error handling preference
- User corrected generic catch to specific on 2026-05-09
- Pattern repeated in Python and JavaScript contexts

## Confidence History
- 2026-05-09: 0.5 (initial observation)
- 2026-05-09: 0.7 (repeated pattern)
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
| MANUAL OBSERVATION LOG   |
| After tool sequences:    |
| - Log to observations.jsonl |
| - Note corrections       |
| - Detect patterns        |
+--------------------------+
       |
       v
+--------------------------+
| memory/observations.jsonl|
| (tool calls, corrections,|
|  patterns, timestamps)   |
+--------------------------+
       |
        v (Scheduled task every 4 hours)
+--------------------------+
| HEARTBEAT ANALYSIS       |
| * Read observations      |
| * Detect patterns        |
| * User corrections       |
| * Error resolutions      |
| * Repeated workflows     |
+--------------------------+
       |
       v
+--------------------------+
| INSTINCT CREATION        |
| memory/instincts/personal/|
| * prefer-functional.yaml |
| * always-validate.yaml   |
+--------------------------+
       |
       v (confidence >= 0.8)
+--------------------------+
| EVOLUTION                |
| Cluster related instincts|
| → Create skill proposal  |
+--------------------------+
```

## File Structure

```
~/microclaw/memory/
├── instincts/
│   ├── personal/           # Auto-learned instincts
│   │   ├── prefer-explicit-errors.yaml
│   │   └── grep-before-edit.yaml
│   └── inherited/          # Imported instincts
├── observations.jsonl      # Raw observations
├── instinct-registry.json  # Metadata
├── evolution-candidates.md # Skill proposals
└── USER.md                 # User behavioral model
```

## Observation Logging

### When to Log

Log observations after:
- Completing a tool sequence (3+ tools)
- User provides a correction
- Detecting a repeated pattern
- Resolving an error

### Observation Format

Append to `~/microclaw/memory/observations.jsonl`:

```json
{
  "timestamp": "2026-05-09T11:02:27Z",
  "chat_id": 524753197,
  "observation_type": "user_correction",
  "tool_sequence": ["read_file", "edit_file", "bash"],
  "user_correction": "Use grep before editing files",
  "pattern_detected": "grep-before-edit",
  "confidence_delta": 0.2,
  "domain": "workflow",
  "scope": "global",
  "context": "User corrected file editing workflow"
}
```

### Observation Types

- `user_correction` - User explicitly corrects behavior
- `pattern_repeated` - Same pattern observed multiple times
- `error_resolution` - Successful error fix
- `workflow_preference` - Preferred tool sequence
- `code_style` - Code formatting/style preference

### Logging Helper

After completing a significant tool sequence, log the observation:

```bash
# Append observation to log
echo '{"timestamp":"'$(date -u +%Y-%m-%dT%H:%M:%SZ)'","chat_id":524753197,"observation_type":"user_correction","pattern_detected":"grep-before-edit","confidence_delta":0.2,"domain":"workflow","scope":"global"}' >> ~/microclaw/memory/observations.jsonl
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
- Pattern isn't observed for extended periods (-0.05 per week)
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

## HEARTBEAT Task (Scheduled Every 4 Hours)

The HEARTBEAT task runs automatically via `schedule_task`:

```
Schedule: "0 0 */4 * * *" (every 4 hours)
Chat ID: 524753197
Prompt: "Activate continuous-learning-v2 and run HEARTBEAT analysis"
```

### HEARTBEAT Workflow

When activated with "run HEARTBEAT analysis":

1. **Read Observations**
   ```bash
   cat ~/microclaw/memory/observations.jsonl
   ```

2. **Detect Patterns**
   - User corrections → create/update instinct
   - Error resolutions → create/update instinct
   - Repeated workflows → create/update instinct
   - Calculate confidence deltas

3. **Create/Update Instincts**
   - Check if instinct exists in `memory/instincts/personal/`
   - If exists: update confidence, add evidence
   - If new: create YAML file with initial confidence 0.3-0.5

4. **Check Evolution Candidates**
   - Find instincts with confidence ≥ 0.8
   - Group by domain
   - If 3+ related instincts: add to `evolution-candidates.md`

5. **Update Registry**
   - Update `memory/instinct-registry.json` with metadata
   - Track total instincts, average confidence, last update

6. **Archive Processed Observations**
   - Move processed observations to archive
   - Keep last 100 observations in main file

### Manual HEARTBEAT Trigger

User can request: "Run HEARTBEAT analysis now" or "Consolidate memory"

## Commands (Conversational)

| User Says | Action |
|-----------|--------|
| "Show my instincts" | Display all instincts with confidence scores |
| "Show evolution candidates" | List instinct clusters ready for skills |
| "Evolve instincts" | Cluster and propose skills from high-confidence instincts |
| "Export instincts" | Export instincts to file |
| "Import instincts from [file]" | Import instincts from file |
| "Run HEARTBEAT now" | Force immediate HEARTBEAT analysis |

## Integration with Knowledge Graph

High-confidence instincts (≥0.7) are also stored in the knowledge graph:

```
knowledge_graph_add(
  subject="saifdkhan",
  predicate="prefers",
  object="grep-before-edit workflow"
)
```

## Integration with Structured Memory

Instinct summaries are stored in structured memory for semantic search:

```
User prefers explicit error handling in code (confidence: 0.8)
User follows grep-before-edit workflow (confidence: 0.9)
```

## Privacy & Control

- All observations stay local in `~/microclaw/memory/`
- User can review/edit instincts before evolution
- Observations can be archived or deleted
- Export format excludes raw conversation content
- User controls confidence thresholds

## Example: Creating an Instinct

### 1. Observation
User corrects: "Please use grep to search before editing files"

### 2. Log Observation
```json
{
  "timestamp": "2026-05-09T11:02:27Z",
  "chat_id": 524753197,
  "observation_type": "user_correction",
  "user_correction": "Use grep to search before editing files",
  "pattern_detected": "grep-before-edit",
  "confidence_delta": 0.2,
  "domain": "workflow",
  "scope": "global"
}
```

### 3. HEARTBEAT Creates Instinct

File: `~/microclaw/memory/instincts/personal/grep-before-edit.yaml`

```yaml
---
id: grep-before-edit
trigger: "when searching for code patterns before editing"
confidence: 0.5
domain: "workflow"
source: "user-correction"
scope: global
created: 2026-05-09
observations: 1
last_observed: 2026-05-09
---

# Grep Before Edit

## Action
Use grep to search for patterns in files before using edit_file.

## Evidence
- User correction on 2026-05-09: "Use grep to search before editing files"

## Confidence History
- 2026-05-09: 0.5 (initial user correction)
```

### 4. Pattern Repeats

User doesn't correct this behavior in next 3 sessions.

HEARTBEAT updates:
```yaml
confidence: 0.8
observations: 4
last_observed: 2026-05-10
```

### 5. Evolution Candidate

Confidence ≥ 0.8 → Added to `evolution-candidates.md`:

```markdown
## Workflow Domain Cluster

**Instincts:**
- grep-before-edit (0.8)
- read-before-write (0.8)
- verify-after-edit (0.7)

**Proposed Skill:** safe-file-editing
**Description:** Workflow for safely editing files with verification steps
```

## Success Metrics

Track in `memory/instinct-registry.json`:
- Total instincts created
- Average confidence score
- Instincts evolved into skills
- User correction frequency (should decrease over time)
- HEARTBEAT run count

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
```

Initialize registry:
```json
{
  "last_updated": "2026-05-09T11:02:27Z",
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

*Instinct-based learning: teaching Microclaw your patterns, one observation at a time.*
