# Engine 2: Intelligent Tutoring System (ITS)

> **Purpose:** An AI-powered tutor that maintains a live understanding of the learner's exact knowledge boundary and provides guidance precisely at the frontier — never too far above, never too far below.

---

## Core Principle: The Zone of Proximal Development (ZPD)

The ITS operates within the learner's **Zone of Proximal Development** — the narrow band between what they can do independently and what they cannot do even with help. Teaching below the ZPD wastes time. Teaching above the ZPD causes confusion and frustration.

```text
  ┌──────────────────────────────────────────┐
  │         CANNOT DO (even with help)        │  ← Above ceiling: DO NOT TEACH
  │          (Parking Lot concepts)           │
  ├──────────────────────────────────────────┤
  │   ZONE OF PROXIMAL DEVELOPMENT (ZPD)     │  ← TEACH HERE
  │   Can do WITH guided scaffolding         │
  │   (FRAGILE + UNVERIFIED concepts)        │
  ├──────────────────────────────────────────┤
  │        CAN DO INDEPENDENTLY              │  ← Below floor: DO NOT RE-TEACH
  │        (MASTERED concepts)               │
  └──────────────────────────────────────────┘
```

---

## Input Sources
- `memory/knowledge-frontier.json` → all 4 knowledge states (MASTERED, DEVELOPING, FRAGILE, UNVERIFIED_HUNCH)
- `memory/learning-path.json` → `parking_lot` (concepts above the ceiling)

## Output Targets
- `memory/knowledge-frontier.json` → state transitions after teaching interactions
- `memory/learning-path.json` → moves concepts from parking lot to active sequence when prerequisites are met

---

## State Transition Protocol

```text
UNVERIFIED_HUNCH ──(probe confirms)──► MASTERED
        │
        └──(probe reveals gap)──► FRAGILE
                                     │
FRAGILE ──(taught + 1 exercise)──► DEVELOPING
                                     │
DEVELOPING ──(punch test PASS)──► MASTERED
        │
        └──(punch test FAIL)──► FRAGILE (regression detected)

MASTERED ──(2-week decay test FAIL)──► FRAGILE (Engine 8 triggers this)
```

---

## Scaffolding Levels

The ITS provides 4 levels of support, progressively removed as the learner demonstrates mastery:

| Scaffolding Level | What the AI Provides | When to Use |
| :--- | :--- | :--- |
| **Level 4 (Full Support)** | Complete walkthrough with RAM diagrams, line-by-line trace, and explained output | Concept is FRAGILE or brand new |
| **Level 3 (Guided)** | Blueprint + algorithm + expected output. Learner writes the code. | Concept is DEVELOPING |
| **Level 2 (Hints Only)** | Problem statement + 1-2 directional hints. Learner designs and implements. | Concept is approaching MASTERED |
| **Level 1 (Independent)** | Problem statement only. No hints. Used as a verification test. | Punch test or decay test |

---

## Prerequisite Gate Rule

**NEVER teach Concept B if Concept A (its prerequisite) is still NOVICE or FRAGILE.**

Before presenting any concept from `learning-path.json`, check:
1. Read the concept's `prerequisite` field.
2. Look up the prerequisite in `knowledge-frontier.json`.
3. If the prerequisite is not at least DEVELOPING → skip this concept, teach the prerequisite first.
4. If the prerequisite is MASTERED → proceed with the concept.

---

## Contextual Guidance Rules

1. **Never explain the same concept the same way twice.** If a concept was taught via code example and the learner struggled, re-teach it via RAM diagram or execution timeline.
2. **Always connect theory to the learner's active project.** Abstract concepts should be grounded in the specific files, functions, or systems the learner is currently building.
3. **Ask before telling.** When possible, ask the learner to predict the behavior FIRST, then reveal the actual behavior. This engages active recall rather than passive reading.
