# Engine 4: Learning Analytics Engine

> **Purpose:** Analyzes learner behavior, performance metrics, and progress trends across sessions to identify patterns, stagnation, acceleration, and regression.

---

## Core Principle

Raw performance snapshots are noise. Trends across sessions reveal the truth. This engine looks at **multi-session trajectories**, not single-turn successes or failures.

---

## Input Sources
- `memory/debriefs/*.md` → historical daily debriefs (the primary evidence archive)
- `memory/knowledge-frontier.json` → concept state history
- `memory/learner-model.json` → anti-pattern records with frequency counts
- Session transcript → current turn interactions

## Output Targets
- `memory/learner-model.json` → `performance_trajectory`, `anti_patterns_chronic`
- Daily debrief artifact → Section 5 (Chronic Habit Watchlist)

---

## Metric 1: Click Velocity Trend

Track how many turns it takes to absorb new concepts across sessions:

```text
Session 1: Concept A = 3 turns (friction)
Session 2: Concept B = 2 turns (guided)
Session 3: Concept C = 1 turn  (instant click)
Session 4: Concept D = 1 turn  (instant click)

TREND: Accelerating ↑ (learning efficiency improving)
```

| Trend | Interpretation | Action |
| :--- | :--- | :--- |
| **Accelerating** (avg turns decreasing) | Learner is building stronger foundational mental models | Increase concept complexity; introduce stacked abstractions |
| **Stable** (avg turns constant) | Learner is at a plateau | Introduce concepts from a different domain to break the plateau |
| **Decelerating** (avg turns increasing) | Cognitive overload or missing prerequisites | Pause new concepts; reinforce FRAGILE foundations |

---

## Metric 2: Intent-Execution Drift Frequency

Count instances per session where the learner's verbal design was correct but typed code contained bugs:

```text
Session 1: 4 drift events (high)
Session 2: 3 drift events
Session 3: 2 drift events
Session 4: 1 drift event

TREND: Improving ↓ (execution discipline tightening)
```

**Alert Threshold:** If drift events are ≥ 3 per session for 3 consecutive sessions → flag as **Chronic Intent Blindness** and prescribe the Pen-and-Paper Trace Rule.

---

## Metric 3: Domain Stagnation Detector

For each of the 7 CS domains, track the number of sessions since the last level-up:

| Domain | Current Level | Sessions Since Last Level-Up | Status |
| :--- | :--- | :--- | :--- |
| OS & Memory | 2 (Developing) | 2 sessions | Active Growth |
| Applied DSA | 1 (Novice) | 8 sessions | **STAGNANT** |
| Craftsmanship | 1 (Novice) | 5 sessions | **AT RISK** |

**Stagnation Rule:** If a domain has not leveled up in **5+ sessions** where it was relevant, flag it as STAGNANT in the debrief and schedule a targeted teaching intervention via the Pedagogical Decision Engine.

---

## Metric 4: Regression Detector

Track concepts that were previously MASTERED but failed a later punch test or spaced repetition test:

```text
Concept: "Post-await mounted guard"
  - Mastered on: 2026-11-10
  - Failed punch test on: 2026-11-19
  - Status: REGRESSION → moved back to FRAGILE
```

**Regression Rule:** Any concept that regresses from MASTERED to FRAGILE gets flagged with HIGH priority and scheduled for immediate re-teaching in the next session's `wakeup-targets.json`.

---

## Metric 5: Anti-Pattern Recurrence

Track named anti-patterns with rolling frequency:

```json
{
  "pattern_id": "missing-await",
  "occurrences": [
    {"session": "2026-11-15", "file": "license_service.dart", "line": 134},
    {"session": "2026-11-17", "file": "status_card.dart", "line": 45},
    {"session": "2026-11-19", "file": "config_guard.dart", "line": 22}
  ],
  "frequency": "3 times in 5 sessions",
  "severity": "HIGH",
  "status": "CHRONIC"
}
```

**Chronic Threshold:** If the same anti-pattern appears in **3+ sessions within a 7-day window**, elevate from WATCH to CHRONIC and mandate a targeted intervention.

---

## Weekly Summary Protocol

Every 7 days (or when the learner asks `/sde-audit`), generate a weekly trend summary:

1. **Velocity Graph:** Click velocity trend over the week (accelerating/stable/decelerating).
2. **Domain Heat Map:** Which domains grew, which stagnated, which regressed.
3. **Top 3 Wins:** Concepts that clicked fastest.
4. **Top 3 Risks:** Concepts most likely to decay or regress.
5. **Anti-Pattern Report:** Chronic habits with frequency and prescribed fixes.
