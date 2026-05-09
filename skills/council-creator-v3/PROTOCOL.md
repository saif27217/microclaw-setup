# Council Engine Operational Protocol (v3)

## Invocation Patterns

### Pattern A — Direct Idea

User: “Create a council for X.”

Response:

Expand scope 2 levels wider

Inject cross-domain analogy

Run scoring model

Form council

### Pattern B — Validation

User: “Validate this idea.”

Protocol:

Stress test via asymmetry equation

Identify hidden dependencies

Simulate competition density

Return EXECUTE / PROBE / QUEUE / DISCARD

### Pattern C — Open Exploration

User: “What high ROI plays exist?”

Protocol:

Run Multi-Surface Scan

Rank top 5 asymmetry vectors

Form only highest-scoring council

List 2 alternates briefly

## Autonomous Protocol

### Cron Layers

Instead of one daily run:

1. Trend Pulse Scan

Detect velocity spikes.

2. Structural Weakness Scan

Detect competitor failures, price shifts.

3. Arbitrage Scan

Detect timing windows.

Only if convergence occurs → council formed.

#### Cron Configuration Example
{
  "name": "Council Engine Multi-Scan",
  "schedule": {
    "kind": "cron",
    "expr": "0 */8 * * *",
    "tz": "Asia/Kolkata"
  },
  "payload": {
    "kind": "agentTurn",
    "message": "Run multi-surface friction scan. Calculate asymmetry. Inject entropy. If threshold met, form council. Else log silent.",
    "timeoutSeconds": 1800
  },
  "sessionTarget": "isolated",
  "delivery": {
    "mode": "digest",
    "channel": "discord"
  },
  "enabled": true
}

Runs every 8 hours.

## Discord Workflow (Simplified)

Post council charter

Tag required specialists only if needed

Set 24h decision window

If no decision:

Downgrade to QUEUE

Archive

## Time Budgets

Phase	Max Time  
Scan	1.5 hrs  
Formation	30 min  
Specialist feedback	Async  
Synthesis	45 min

Total active time: < 3 hours.

## Priority Escalation Rules

Immediate escalation if:

Structural arbitrage window < 14 days

Competitor collapse detected

Regulatory change with monetizable impact

Tool deprecation affecting workflow

## Weekly Evolution Protocol

Every 7 days:

Ban highest-used archetype

Force unfamiliar vertical

Run contrarian thesis:
“What if the opposite trend wins?”