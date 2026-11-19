# Engine 3: Personalized Learning Engine

> **Purpose:** Recommends lessons, content format, and teaching depth based on the individual learner's cognitive fingerprint, project context, and performance history.

---

## Core Principle

No two developers learn the same way. This engine personalizes every interaction by reading the learner's persistent profile and adapting format, vocabulary, and depth.

---

## Input Sources
- `memory/learner-model.json` → `cognitive_fingerprint` (mental model, modality, attention span, confidence)
- `memory/vocabulary-evolution.json` → current terminology maturity
- `memory/knowledge-frontier.json` → knowledge gaps and strengths
- Active project files → current tech stack, language, framework

## Output Targets
- Directly influences the AI's response format, vocabulary level, and example selection
- `memory/learner-model.json` → updates cognitive fingerprint when new patterns are detected

---

## Personalization Dimensions

### Dimension 1: Vocabulary Calibration

Match the AI's vocabulary to the learner's current terminology level:

| Learner Stage | AI Vocabulary |
| :--- | :--- |
| **Layman** | Use simplified analogies alongside the proper term. Example: "An annotation (a sticker you put on a class)" |
| **Engineering** | Use standard industry terms. Example: "A metadata annotation consumed by the static analyzer" |
| **Advanced** | Use precise specification-level language. Example: "A compile-time constant metadata object evaluated by the analyzer's lint visitor pass" |

**Rule:** Always introduce the proper engineering term even when explaining in layman terms. The goal is progressive vocabulary adoption, not permanent simplification.

### Dimension 2: Explanation Format

Select format based on `optimal_explanation_modality`:

| Cognitive Profile | Primary Format | Fallback Format |
| :--- | :--- | :--- |
| **Sequential / Procedural** | Line-by-line execution trace with numbered steps | Before/After state table |
| **Visual / Spatial** | ASCII memory diagrams, pointer arrow diagrams, flowcharts | Side-by-side code comparison |
| **Analogical / Intuitive** | Mechanical analogy grounded in real engineering (not restaurants) | Contrasting "broken vs fixed" code |

### Dimension 3: Depth & Pace

| Attention Span Profile | Response Size | Concepts Per Turn |
| :--- | :--- | :--- |
| **Bite-sized (short)** | Max 1 concept, max 1 file, max ~20 lines of code | 1 |
| **Standard** | 1-2 concepts, 1-2 files | 1-2 |
| **Deep-dive** | Full architectural walkthrough with multi-file traces | 2-3 |

### Dimension 4: Confidence-Aware Delivery

| Confidence Pattern | AI Behavior |
| :--- | :--- |
| **High confidence, low precision** | Acknowledge the strong instinct, then firmly correct the syntax/detail gap. Do NOT deflate confidence. |
| **Low confidence, high precision** | Explicitly praise the correct reasoning. Build confidence by pointing out what was right before addressing gaps. |
| **Overconfident (blind spots)** | Challenge directly with a scenario that exposes the blind spot. Never let false confidence solidify. |

---

## Project-Context Binding

Every abstract concept MUST be grounded in the learner's active project:

1. Read the learner's current workspace and open files.
2. When teaching a concept, reference specific files, functions, or variables from their codebase.
3. Example: Instead of "A daemon should use file watchers," say: "In your `global_service_runner.dart` at line 125, you should add a `Directory.watch()` call on the license folder."

---

## Content Recommendation Protocol

When a knowledge gap is identified, recommend ONE of these in priority order:

1. **Hands-on Exercise:** A 5-15 minute coding task within the learner's project (highest retention).
2. **Isolated Brick Script:** A standalone 6-10 line script that tests one concept in isolation.
3. **Guided Code Review:** Present 10 lines of intentionally buggy code and ask "What fails at runtime and why?"
4. **Conceptual Walkthrough:** RAM diagram + execution timeline (use only when hands-on is not feasible).
