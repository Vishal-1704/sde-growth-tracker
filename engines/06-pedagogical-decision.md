# Engine 6: Pedagogical Decision Engine

> **Purpose:** Decides WHAT should be taught next and HOW, based on knowledge gaps, project roadmap, prerequisite readiness, learner profile, and optimal timing.

---

## Core Principle

Teaching is not random. Every concept taught must be:
1. **Needed** — either for the learner's immediate project work or to fill a critical knowledge gap.
2. **Ready** — all prerequisites must be at least DEVELOPING.
3. **Timed** — introduced at the moment of maximum relevance (when the learner is about to use it).

---

## Input Sources
- `memory/knowledge-frontier.json` → gaps, fragile concepts, unverified hunches
- `memory/learning-path.json` → ordered sequence with prerequisites
- `memory/learner-model.json` → cognitive fingerprint, trajectory, stagnant domains
- Active workspace → current project files, upcoming features

## Output Targets
- `memory/wakeup-targets.json` → tomorrow's teaching mission
- `memory/learning-path.json` → reordered sequence based on new evidence

---

## Decision Algorithm: "What To Teach Next"

Execute this priority waterfall. Stop at the first match:

```text
Priority 1: REGRESSION REPAIR
  └── Any concept that REGRESSED from MASTERED to FRAGILE?
      └── YES → Teach this IMMEDIATELY. It is decaying.

Priority 2: FRAGILE REINFORCEMENT
  └── Any FRAGILE concept that hasn't been reinforced in 3+ sessions?
      └── YES → Schedule a reinforcement exercise.

Priority 3: PROJECT-ALIGNED TEACHING
  └── Does the learner's next coding task require a concept they don't have?
      └── YES → Teach just-in-time, right before they need it.
      Example: User is about to implement DeviceScheduler quota gate.
              They need to understand `if (count >= license.maxDevices)`.
              Teach license data model access patterns now.

Priority 4: STAGNANT DOMAIN INTERVENTION
  └── Any domain stuck at the same level for 5+ sessions?
      └── YES → Schedule a targeted intervention to break the plateau.

Priority 5: UNVERIFIED HUNCH PROBING
  └── Any UNVERIFIED_HUNCH concepts to casually test?
      └── YES → Weave a probe question into natural conversation.

Priority 6: NEXT IN SEQUENCE
  └── Take the next concept from learning-path.json whose prerequisites are met.

Priority 7: PARKING LOT REVIEW
  └── Check if any parked concepts now have met prerequisites.
      └── YES → Move to active sequence.
```

---

## Decision Algorithm: "HOW To Teach It"

After deciding WHAT to teach, select the delivery method:

```text
Step 1: Check learner-model.json → cognitive_fingerprint.optimal_explanation_modality
Step 2: Check knowledge-frontier.json → concept's current state

If concept is FRAGILE:
  → Use Level 4 scaffolding (full walkthrough with diagrams)
  → Switch modality from whatever was used last time (don't repeat a failed approach)

If concept is DEVELOPING:
  → Use Level 3 scaffolding (blueprint + algorithm, learner writes code)
  → Ground the example in the learner's active project files

If concept is UNVERIFIED_HUNCH:
  → Use Level 1 scaffolding (question only, no hints)
  → Frame as casual conversation, not a formal test

If concept is brand new (not in knowledge-frontier yet):
  → Check prerequisite status first (gate rule)
  → Use Level 4 scaffolding for first introduction
  → Follow the 5-Point Teaching Protocol:
     1. Origin & History
     2. Problem It Solves
     3. Core Mechanics & RAM Layout
     4. Current Application (in learner's project)
     5. Future Architectural Relevance
```

---

## Timing Rules

1. **Just-In-Time Teaching:** The most effective moment to teach a concept is immediately BEFORE the learner needs it in their project. Monitor the active coding task and pre-teach relevant concepts.
2. **Never interrupt flow for teaching.** If the learner is in deep focus on a coding task, teach during natural pauses (after a commit, after a build, after a debug session).
3. **Morning punch test.** Every new session starts with yesterday's queued punch test BEFORE any new teaching. This ensures retention before adding new knowledge.

---

## Wakeup Target Generation Protocol

At the end of every audit, generate tomorrow's `wakeup-targets.json`:

1. **teach_targets:** Select 1-2 concepts using the priority waterfall above.
2. **punch_test_queue:** Select 1 concept from today's newly taught material for tomorrow's active recall test.
3. **unverified_hunches_to_probe:** Select 1-2 hunches from `knowledge-frontier.json` to casually test during conversation.
