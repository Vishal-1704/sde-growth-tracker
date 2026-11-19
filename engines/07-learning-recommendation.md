# Engine 7: Learning Recommendation Engine

> **Purpose:** Recommends the next lesson, exercise, coding challenge, or learning resource based on the learner's current knowledge state, project context, and growth trajectory.

---

## Core Principle

Recommendations must be **actionable within the learner's current session**. Never recommend a vague "go study algorithms." Instead, recommend a specific, bounded, 5-15 minute exercise grounded in their codebase.

---

## Input Sources
- `memory/learning-path.json` → recommended sequence and parking lot
- `memory/knowledge-frontier.json` → knowledge states
- `memory/learner-model.json` → stagnant domains, anti-patterns
- Active workspace → current project, open files, recent diffs

## Output Targets
- `memory/wakeup-targets.json` → queued exercises and challenges
- `memory/learning-path.json` → updated parking lot and sequence
- Daily debrief → Section 7 (Tomorrow's challenge)

---

## Recommendation Types (Priority Order)

### Type 1: In-Project Hands-On Exercise (Highest Retention)
**When:** A concept needs reinforcement AND the learner's active project has a natural application point.

Format:
```text
EXERCISE: [Title]
CONCEPT: [What this tests]
LOCATION: [Specific file and line range in the learner's project]
TASK: [Concrete 5-15 minute coding task]
SUCCESS CRITERIA: [What the code should do when correct]
```

Example:
```text
EXERCISE: License Gate Guard
CONCEPT: Async while-loop with Future.delayed
LOCATION: global_service_runner.dart, line 124 (before database init)
TASK: Write a while(true) loop that calls loadAndVerifyActiveLicense(), 
      breaks on valid, and sleeps 30s with Future.delayed on invalid.
SUCCESS CRITERIA: Service waits in standby until a valid .lic file exists on disk.
```

### Type 2: Isolated Brick Script (Concept Isolation)
**When:** A concept is FRAGILE or has stacked prerequisites that need decomposition.

Format:
```text
BRICK: [Title]
CONCEPT: [Single isolated concept]
SCRIPT: [4-10 line standalone script]
PREDICT: [Ask learner to predict the output BEFORE running]
VERIFY: [Run the script and compare]
```

### Type 3: Buggy Code Review (Error Detection)
**When:** The learner has an anti-pattern for missing specific bug types (e.g., missing await, type mismatches).

Format:
```text
CODE REVIEW: [Title]
BUGS HIDDEN: [Number of intentional bugs]
CODE: [10-20 lines with planted bugs]
TASK: Find all bugs and explain the runtime consequence of each.
```

### Type 4: Architecture Design Challenge (System Thinking)
**When:** System Design domain needs growth or a new feature requires architectural planning.

Format:
```text
DESIGN: [Title]
SCENARIO: [Real-world system design problem relevant to the learner's project]
CONSTRAINTS: [Memory, latency, throughput, or reliability constraints]
DELIVERABLE: [ASCII diagram + data flow + trade-off analysis]
```

### Type 5: Explain-to-Rubber-Duck (Deepest Understanding Test)
**When:** A concept is marked MASTERED but hasn't been verbally articulated by the learner.

Format:
```text
EXPLAIN: [Concept]
PROMPT: "Explain [concept] to a junior developer who has never seen it. 
         Use no code. Only use a whiteboard diagram and plain English."
PASS CRITERIA: The explanation must include:
  - WHY it exists (what problem it solves)
  - HOW it works mechanically (not just what it does)
  - WHEN to use it vs alternatives
```

---

## Recommendation Scheduling

| Priority | Timing | Type |
| :--- | :--- | :--- |
| **Immediate** | Before next coding task | Type 1 (In-Project) for just-in-time teaching |
| **Session Start** | First interaction tomorrow | Type 2 or 3 (Brick or Bug Review) as punch test |
| **Weekly** | End of week audit | Type 4 (Design Challenge) for domain growth |
| **On Demand** | When learner asks `/sde-challenge` | Type 5 (Explain) for deepest verification |

---

## Parking Lot Management

Concepts that are too advanced for the learner's current state are stored in `learning-path.json` → `parking_lot`:

1. **Entry Rule:** A concept enters the parking lot when its prerequisites are not at DEVELOPING or higher.
2. **Exit Rule:** Every time `knowledge-frontier.json` is updated, re-check parking lot prerequisites. If met, move the concept to the active `recommended_sequence`.
3. **Notification:** When a parked concept becomes teachable, mention it in the next debrief: *"New unlock: You now have the prerequisites to learn [concept]."*
