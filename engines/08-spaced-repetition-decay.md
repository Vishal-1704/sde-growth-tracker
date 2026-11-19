# Engine 8: Spaced Repetition & Memory Decay Detector

> **Purpose:** Randomly tests concepts that were previously MASTERED after 2-3 weeks of inactivity to detect knowledge decay. When decay is detected, the concept is flagged as "hallucinating knowledge" (the learner thinks they know it but cannot reproduce it) and re-entered into the teaching pipeline.

---

## Core Principle: The Forgetting Curve

Human memory is not permanent storage. Without reinforcement, knowledge decays predictably:

```text
100% ┤ ●  Concept Learned
     │  ╲
 80% ┤   ╲
     │    ╲
 60% ┤     ╲
     │      ╲───── Without review: ~50% forgotten in 2 weeks
 40% ┤        ╲
     │         ╲
 20% ┤          ╲──── Without review: ~80% forgotten in 4 weeks
     │            ╲
  0% ┼──────────────────────────
     Day 1    Week 1   Week 2   Week 3   Week 4
```

**Each successful recall RESETS and FLATTENS the curve.** A concept tested and passed at Week 2 will retain longer before the next test is needed.

---

## Input Sources
- `memory/knowledge-frontier.json` → all MASTERED concepts with `mastered_on` dates
- `memory/learner-model.json` → anti-patterns and regression history
- System clock → current date for calculating decay intervals

## Output Targets
- `memory/knowledge-frontier.json` → state transitions (MASTERED → FRAGILE on failure)
- `memory/wakeup-targets.json` → queued decay tests
- `memory/learner-model.json` → regression records

---

## Decay Test Scheduling Algorithm

```text
For each concept in knowledge_states.MASTERED:
  │
  ├── Calculate: days_since_mastered = today - mastered_on
  ├── Calculate: days_since_last_tested = today - last_decay_test_date
  │
  ├── Is days_since_last_tested >= decay_interval?
  │     │
  │     YES → Queue a decay test in wakeup-targets.json
  │     NO  → Skip (not due yet)
  │
  └── Determine decay_interval based on reinforcement_count:
        │
        ├── reinforcement_count == 0 → decay_interval = 14 days (2 weeks)
        ├── reinforcement_count == 1 → decay_interval = 21 days (3 weeks)
        ├── reinforcement_count == 2 → decay_interval = 35 days (5 weeks)
        ├── reinforcement_count >= 3 → decay_interval = 60 days (2 months)
        │
        └── Each successful decay test increments reinforcement_count
            (exponential backoff: concepts proven durable are tested less often)
```

---

## Knowledge Frontier Schema Extension

Each MASTERED concept must include these decay-tracking fields:

```json
{
  "id": "compiler-vs-analyzer",
  "label": "@immutable (0-byte analyzer lint) vs final (compiler RAM lock)",
  "domain": "Execution & Memory",
  "mastered_on": "2026-11-19",
  "last_decay_test_date": null,
  "reinforcement_count": 0,
  "decay_interval_days": 14,
  "decay_status": "NOT_DUE | DUE | TESTED_PASS | TESTED_FAIL"
}
```

---

## Decay Test Execution Protocol

### Step 1: Select the Test
Pick the MASTERED concept with the **oldest `last_decay_test_date`** that is past its `decay_interval_days`.

### Step 2: Frame as a Casual "Ghost Test"
Do NOT announce: *"I am now testing whether you remember X."*

Instead, weave it naturally into conversation:
- During a code review: *"Quick question — in this model class, what would happen if we removed `final` from that field?"*
- During architecture discussion: *"Why did we use Ed25519 for the license instead of RSA?"*
- During debugging: *"Before we fix this, predict: what state is this variable in after the await?"*

### Step 3: Evaluate the Response

| Response Quality | Verdict | Action |
| :--- | :--- | :--- |
| **Correct with mechanical explanation** | PASS | Increment `reinforcement_count`, update `last_decay_test_date`, increase `decay_interval_days` |
| **Correct answer but vague/uncertain reasoning** | SOFT PASS | Update `last_decay_test_date` but do NOT increment reinforcement. Keep the same interval. |
| **Incorrect or confused** | FAIL (DECAY DETECTED) | Move concept from MASTERED → FRAGILE. Flag as "Knowledge Hallucination." Schedule immediate re-teaching. |

### Step 4: Handle "Knowledge Hallucination"

When a learner FAILS a decay test on a previously MASTERED concept:

1. **Log it explicitly:** *"Decay detected: You mastered [concept] on [date], but today's test revealed the memory has degraded."*
2. **Move to FRAGILE** in `knowledge-frontier.json`.
3. **Re-teach using a DIFFERENT modality** than the original teaching (the first approach clearly didn't create durable memory).
4. **Schedule aggressive follow-up:** Test again in 7 days (not 14).

---

## "Hallucinating Knowledge" Detection Heuristics

The most dangerous state is when a learner **believes** they know something but actually doesn't. Signs:

1. **Confident but wrong:** Answers immediately without hesitation, but the answer is incorrect.
2. **Partial recall:** Remembers the conclusion but not the mechanics. Example: *"Yeah, @immutable does nothing"* but cannot explain WHY (analyzer vs compiler).
3. **Confusion with similar concepts:** Mixes up `final` (Dart) with `const` (Dart) or `readonly` (C#) or `val` (Kotlin).

**Rule:** If a hallucination is detected, the concept MUST be re-taught from scratch using the 5-Point Teaching Protocol (Origin → Problem → Mechanics → Application → Future).

---

## Decay Test Frequency Caps

To avoid turning every session into an exam:
- **Maximum 1 decay test per session** (the learner should spend most time building, not testing).
- **Never test on the same day as teaching the same concept** (allow consolidation time).
- **Prioritize concepts relevant to the current project work** (relevance increases retention).
