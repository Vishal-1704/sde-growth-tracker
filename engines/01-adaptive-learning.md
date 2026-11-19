# Engine 1: Adaptive Learning Engine

> **Purpose:** Automatically adjusts teaching depth, pace, modality, and complexity in real-time based on the learner's live performance signals during the current session.

---

## Core Principle

The engine observes how the learner responds to each concept and dynamically shifts strategy. It never locks into a fixed teaching pace.

---

## Input Sources
- `memory/learner-model.json` → `cognitive_fingerprint` (dominant mental model, optimal modality, attention span)
- Live session transcript → current turn's response quality

## Output Targets
- `memory/learner-model.json` → updates `cognitive_fingerprint` and `performance_trajectory`
- Directly influences the AI's next response format and depth

---

## Adaptation Decision Tree

```text
Concept Presented to Learner
        │
        ▼
   Did they grasp it on Turn 1?
        │
   ┌────┴────┐
   YES       NO
   │         │
   ▼         ▼
ACCELERATE   Did they grasp it on Turn 2 with a diagram/trace?
   │              │
   │         ┌────┴────┐
   │         YES       NO
   │         │         │
   │         ▼         ▼
   │      CONTINUE   Did they grasp it on Turn 3?
   │      (standard      │
   │       pace)    ┌────┴────┐
   │                YES       NO
   │                │         │
   │                ▼         ▼
   │             REINFORCE  DECOMPOSE
   │             (extra     (Rule 17: Cook
   │              exercise)  the Bricks)
   │
   ▼
Skip to next concept.
Queue a punch test for
tomorrow to verify retention.
```

---

## Modality Switching Rules

When friction is detected (Turn 2+), switch teaching format based on the learner's `optimal_explanation_modality`:

| Learner Profile | Turn 1 (Default) | Turn 2 (Friction) | Turn 3 (Decompose) |
| :--- | :--- | :--- | :--- |
| **Visual / Spatial** | Code snippet | ASCII RAM diagram | Isolated 4-line brick script |
| **Sequential / Procedural** | Step-by-step execution trace | Before/After state comparison | Single-variable trace through 6 lines |
| **Analogical** | Real-world mechanical parallel | Contrasting "wrong way vs right way" | Side-by-side code: broken vs fixed |

---

## Pace Calibration Rules

1. **ACCELERATE (1-shot click):** Skip supplementary examples. Move directly to the next concept in `learning-path.json`. Queue a punch test for 48 hours later.
2. **CONTINUE (2-shot with diagram):** Provide one reinforcing exercise. Proceed to next concept next turn.
3. **REINFORCE (3-shot):** Provide a hands-on coding exercise. Do NOT proceed to next concept until the exercise is completed and reviewed.
4. **DECOMPOSE (3+ shots, still friction):** The concept has stacked prerequisites. Invoke Rule 17:
   - Identify which sub-bricks are causing friction.
   - Isolate each sub-brick into a standalone 4-8 line script.
   - Teach each sub-brick independently.
   - Only recombine after each sub-brick is independently DEVELOPING or higher.

---

## Anti-Flooding Rule

**Never teach more than 2 new concepts per session** when the learner's attention span is calibrated as "bite-sized" in `learner-model.json`. Quality of understanding beats quantity of topics covered.
