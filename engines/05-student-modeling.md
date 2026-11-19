# Engine 5: Student Modeling Engine

> **Purpose:** Maintains a persistent, evolving model of the learner's cognitive fingerprint, knowledge abilities, learning preferences, vocabulary maturity, and behavioral anti-patterns across all sessions.

---

## Core Principle

The student model is the **single source of truth** about who the learner is as an engineer. Every other engine reads from it. It must be accurate, current, and brutally honest — never flattering, never stale.

---

## Input Sources
- Session transcript → communication patterns, vocabulary, reasoning quality
- Code diffs → implementation quality, bug patterns, naming conventions
- `memory/knowledge-frontier.json` → concept mastery states
- `memory/vocabulary-evolution.json` → terminology progression
- `memory/ai-steering-audit.json` → AI interaction quality

## Output Targets
- `memory/learner-model.json` → ALL sections updated after every audit
- `memory/vocabulary-evolution.json` → new terminology observations

---

## Model Components

### Component 1: Cognitive Fingerprint

A persistent profile of HOW the learner thinks and learns:

```json
{
  "dominant_mental_model": "imperative_sequential | visual_spatial | analogical | abstract_theoretical",
  "optimal_explanation_modality": "ram_diagrams | line_traces | code_comparison | socratic_questions",
  "attention_span_per_concept": "bite_sized | standard | deep_dive",
  "learning_velocity": "Description of speed patterns across different concept types",
  "confidence_pattern": "high_confidence_low_precision | low_confidence_high_precision | calibrated | overconfident"
}
```

**Update Rule:** Re-evaluate the cognitive fingerprint every 5 sessions. If the learner's patterns have shifted (e.g., they now handle diagrams faster than line traces), update the profile.

### Component 2: Performance Trajectory

A rolling trend indicator:

```json
{
  "trajectory_direction": "ASCENDING | PLATEAU | DESCENDING",
  "sessions_analyzed": 12,
  "trend_summary": "Free-text description of growth pattern",
  "weekly_velocity": {
    "fastest_growing_domain": "System Design",
    "slowest_growing_domain": "Applied DSA",
    "stagnant_domains": ["Software Craftsmanship"]
  }
}
```

**Update Rule:** Recalculate after every daily audit. Compare last 3 debriefs to determine trajectory direction.

### Component 3: Chronic Anti-Pattern Registry

Named, tracked behavioral anti-patterns with recurrence data:

```json
{
  "pattern_id": "intent-blindness",
  "description": "Understands design correctly but types code with missing checks",
  "first_observed": "2026-11-10",
  "occurrences_last_7_days": 3,
  "severity": "LOW | MEDIUM | HIGH | CRITICAL",
  "status": "WATCH | CHRONIC | RESOLVING | RESOLVED",
  "prescribed_intervention": "Specific action to address this pattern"
}
```

**Severity Escalation Rules:**
- 1 occurrence → LOW (note it)
- 2 occurrences in 7 days → MEDIUM (mention in debrief)
- 3+ occurrences in 7 days → HIGH (mandate intervention)
- Persists across 3+ weeks → CRITICAL (restructure learning path to address root cause)

**Resolution Rules:**
- If a CHRONIC pattern has 0 occurrences for 2 consecutive weeks → move to RESOLVING
- If RESOLVING has 0 occurrences for another 2 weeks → move to RESOLVED
- If RESOLVED re-occurs → immediately escalate back to HIGH

### Component 4: Vocabulary Maturity Index

Track the learner's progression from casual language to precise engineering terminology:

| Stage | Example | Indicator |
| :--- | :--- | :--- |
| **Layman** | "make it sleep" | Uses everyday language for technical concepts |
| **Transitional** | "polling loop" | Knows some terms but mixes with casual phrasing |
| **Engineering** | "non-blocking I/O event subscription" | Uses standard industry vocabulary consistently |
| **Specification** | "kernel inotify file descriptor watch" | Uses implementation-level precision |

**Update Protocol:**
1. After every session, scan the transcript for new terminology the learner used.
2. Compare against `vocabulary-evolution.json` observed transitions.
3. If the learner spontaneously used an engineering term that was previously only taught (not self-generated), log the transition.
4. If the learner reverts to layman phrasing for a concept they previously used engineering terms for, flag potential regression.

---

## Model Integrity Rules

1. **Never invent data.** Only record observations with concrete evidence from transcripts or code diffs.
2. **Never soften assessments.** If the learner is NOVICE, record NOVICE. Inflation helps nobody.
3. **Always timestamp.** Every model update must include the date it was observed.
4. **Conflict resolution:** If a single session shows contradictory signals (e.g., brilliant architecture + sloppy syntax), record BOTH signals with their respective evidence. Do not average them into a meaningless middle score.
