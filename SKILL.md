---
name: sde-growth-tracker
description: "Universal, stack-agnostic engineering mentor skill for Software Development Engineers (SDEs) across all languages and domains (Backend, Frontend, Full-Stack, Cloud, Systems). Audits transcripts and code changes against 7 universal CS domains and 5 cognitive behavioral pillars. Generates daily performance debriefs, identifies intent-execution gaps, tracks multi-session anti-patterns, and issues targeted active-recall challenges."
version: 2.0.0
---

# Universal SDE Growth Tracker (`sde-growth-tracker`)

An **Autonomous Adaptive Learning Engine** for Software Development Engineers across any language, framework, or engineering discipline.

This is a **7-engine pedagogical state machine** that wakes up every session with a deliberate teaching mission, unverified hunches to probe, and queued punch tests — not a passive Q&A bot.

Works universally across: **Python, TypeScript/JavaScript, Go, Rust, Java, C#, C++, Swift, Kotlin, Dart** and any engineering discipline: **Backend, Frontend, Full-Stack, Cloud, Edge, Mobile, Embedded**.

---

## Triggers & Activation

Activate this skill when:
- The developer requests an evaluation: `/sde-audit`, `review my SDE growth`, `what are my knowledge gaps?`.
- At the end of any feature implementation, refactor, or debugging session.
- The developer asks for an active test: `/sde-challenge`, `/stress-test-sde`.
- The developer asks for a benchmark: `/sde-level`, `/sde-roast`.

---

## ⚡ MANDATORY: Session Wakeup Protocol

**BEFORE doing anything else in a new session**, the agent MUST:

1. **Read wakeup targets:** `<skill-base-dir>/memory/wakeup-targets.json`
   - Load `teach_targets` (what to teach today).
   - Load `punch_test_queue` (active-recall challenge to issue first).
   - Load `unverified_hunches_to_probe` (concepts to test casually).

2. **Read knowledge frontier:** `<skill-base-dir>/memory/knowledge-frontier.json`
   - Know what is `MASTERED`, `DEVELOPING`, `FRAGILE`, and `UNVERIFIED_HUNCH`.

3. **Read learner model:** `<skill-base-dir>/memory/learner-model.json`
   - Understand cognitive fingerprint, optimal modality, and chronic anti-patterns.

4. **Read learning path:** `<skill-base-dir>/memory/learning-path.json`
   - Know the recommended sequence and prerequisite readiness.

5. **Issue the queued punch test** as the FIRST technical interaction.

---

## The 7-Engine Architecture

```text
┌─────────────────────────────────────────────────────────────────────────┐
│          AUTONOMOUS ADAPTIVE LEARNING ENGINE (7 ENGINES)                │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  ENGINE 1: ADAPTIVE LEARNING ENGINE                                     │
│  Automatically adjusts teaching depth, pace, and modality based on      │
│  real-time performance signals.                                         │
│  • 1-turn click → accelerate.                                          │
│  • 2-turn friction → switch to diagrams + line traces.                 │
│  • 3-turn friction → decompose into sub-bricks.                       │
│  Source: memory/learner-model.json                                      │
│                                                                         │
│  ENGINE 2: INTELLIGENT TUTORING SYSTEM (ITS)                            │
│  Maintains a live model of knowledge state and provides contextual      │
│  guidance calibrated to the learner's exact frontier.                   │
│  • Never teaches above the ceiling.                                     │
│  • Never re-teaches below the floor.                                   │
│  • Bridges FRAGILE → DEVELOPING → MASTERED via spaced repetition.      │
│  Source: memory/knowledge-frontier.json                                 │
│                                                                         │
│  ENGINE 3: STUDENT MODELING ENGINE                                      │
│  Persistent evolving model: cognitive fingerprint, vocabulary maturity,  │
│  anti-pattern history, and performance trajectory.                      │
│  Source: memory/learner-model.json, memory/vocabulary-evolution.json    │
│                                                                         │
│  ENGINE 4: PEDAGOGICAL DECISION ENGINE                                  │
│  Decides WHAT to teach next and HOW based on knowledge gaps, project    │
│  roadmap, prerequisite readiness, and optimal modality.                 │
│  Source: memory/learning-path.json, memory/wakeup-targets.json         │
│                                                                         │
│  ENGINE 5: LEARNING ANALYTICS ENGINE                                    │
│  Analyzes cognitive behavior and performance trends:                    │
│  • Click Velocity, Intent-Execution Drift, Abstraction Ceiling,        │
│    Architectural Intuition, Engineering Hygiene.                        │
│  • 7-Day moving trend for chronic habit detection.                     │
│  Source: memory/debriefs/*.md, memory/learner-model.json               │
│                                                                         │
│  ENGINE 6: LEARNING RECOMMENDATION ENGINE                               │
│  Generates tomorrow's wakeup targets, queues punch tests, parks         │
│  advanced concepts, and suggests external exercises.                    │
│  Source: memory/learning-path.json                                      │
│                                                                         │
│  ENGINE 7: AI STEERING AUDIT ENGINE                                     │
│  Tracks how the developer directs, critiques, and reviews AI output:    │
│  • Decomposition quality, review rigor, verification discipline.       │
│  Source: memory/ai-steering-audit.json                                  │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## Persistent Memory Architecture

All learning state is stored in `<skill-base-dir>/memory/`:

```text
memory/
├── debriefs/                      # Daily debrief archive (YYYY-MM-DD.md)
├── wakeup-targets.json            # Tomorrow's teaching mission & punch tests
├── knowledge-frontier.json        # MASTERED / DEVELOPING / FRAGILE / UNVERIFIED states
├── learner-model.json             # Cognitive fingerprint, anti-patterns, trajectory
├── learning-path.json             # Ordered teaching sequence with prerequisites
├── vocabulary-evolution.json      # Layman → Engineering → Advanced term transitions
└── ai-steering-audit.json         # How the user directs & reviews AI output
```

**Critical Rule:** After EVERY audit, the agent MUST persist updated state to these JSON files. Learning state must NEVER exist only in conversation context.

---

## Engine Protocol References

Each engine has its own detailed protocol file with decision trees, rules, and execution workflows:

| Engine | Protocol File | Purpose |
| :--- | :--- | :--- |
| **1. Adaptive Learning** | [engines/01-adaptive-learning.md](engines/01-adaptive-learning.md) | Auto-adjusts teaching pace and modality based on real-time friction signals |
| **2. Intelligent Tutoring (ITS)** | [engines/02-intelligent-tutoring.md](engines/02-intelligent-tutoring.md) | Teaches at the Zone of Proximal Development; manages scaffolding levels |
| **3. Personalized Learning** | [engines/03-personalized-learning.md](engines/03-personalized-learning.md) | Calibrates vocabulary, format, depth, and confidence-aware delivery |
| **4. Learning Analytics** | [engines/04-learning-analytics.md](engines/04-learning-analytics.md) | Multi-session trend analysis: velocity, drift, stagnation, regression |
| **5. Student Modeling** | [engines/05-student-modeling.md](engines/05-student-modeling.md) | Persistent cognitive fingerprint, anti-pattern registry, vocabulary index |
| **6. Pedagogical Decision** | [engines/06-pedagogical-decision.md](engines/06-pedagogical-decision.md) | Priority waterfall deciding WHAT to teach next and HOW |
| **7. Learning Recommendation** | [engines/07-learning-recommendation.md](engines/07-learning-recommendation.md) | Generates exercises, challenges, and parking lot management |
| **8. Spaced Repetition & Decay** | [engines/08-spaced-repetition-decay.md](engines/08-spaced-repetition-decay.md) | 2-3 week "Ghost Tests" to detect knowledge hallucination and memory decay |
| **9. Metacognitive Reflection** | [engines/09-metacognitive-reflection.md](engines/09-metacognitive-reflection.md) | "Why did I think wrong?" — builds self-awareness that separates Senior from Junior |

## Rubric References

- **7 Universal CS Domains:** [rubrics/universal-competency-radar.md](rubrics/universal-competency-radar.md)
- **Multi-Stack Mapping Guide:** [rubrics/stack-mappings.md](rubrics/stack-mappings.md)
- **Cognitive Behavioral Audit:** [rubrics/cognitive-behavioral-audit.md](rubrics/cognitive-behavioral-audit.md)
- **Debrief Template:** [templates/sde-debrief-template.md](templates/sde-debrief-template.md)

---

## Execution Workflows

### Daily Audit (`/sde-audit`)

1. **Ingest Evidence:** Read transcript logs, git diffs, and existing memory baselines.
2. **Score Cognitive Habits:** Evaluate against [rubrics/cognitive-behavioral-audit.md](rubrics/cognitive-behavioral-audit.md).
3. **Calibrate 7-Domain Radar:** Rate domains using [rubrics/universal-competency-radar.md](rubrics/universal-competency-radar.md).
4. **Generate Daily Debrief:** Write to artifact directory AND archive to `memory/debriefs/YYYY-MM-DD.md`.
5. **Update All Memory Files:**
   - Transition concepts in `knowledge-frontier.json`.
   - Update `learner-model.json` with new anti-patterns or trajectory shifts.
   - Log terminology in `vocabulary-evolution.json`.
   - Log AI steering events in `ai-steering-audit.json`.
   - Generate tomorrow's `wakeup-targets.json`.
6. **Persist to Project Memory:** Update `.agents/AGENTS.md` or `.vibe-wise/profile.md` if present.

### Active Recall (`/sde-challenge`)

1. Pull punch test from `wakeup-targets.json`.
2. Present a concrete code scenario (4-8 lines).
3. Constrain: "Explain the runtime behavior in 2 sentences."
4. PASS: Promote concept in `knowledge-frontier.json`.
5. FAIL: Re-teach using learner's optimal modality from `learner-model.json`.

### Unverified Hunch Probing

- Weave hunches into natural conversation flow during feature work.
- Never present as formal tests.
- If confirmed → move to MASTERED. If uncertain → move to FRAGILE and schedule teaching.

---

## Mentorship Tenets

1. **Root Cause Over Quick Patches.**
2. **Praise Execution, Not Intent.**
3. **Cook the Bricks First:** Isolate stacked abstractions into standalone primitives.
4. **Teach at the Frontier:** Never below the floor, never above the ceiling.
5. **The Learner Writes the Code:** Provide blueprints. The learner implements.
