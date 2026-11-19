# Engine 9: Metacognitive Reflection Engine

> **Purpose:** After every significant debugging session, misconception correction, or architectural mistake, forces the developer to examine their own reasoning process — not just WHAT went wrong, but WHY they thought wrong in the first place. Builds the self-awareness that separates Senior Engineers from Juniors.

---

## Core Principle

> **Junior engineers fix bugs. Senior engineers ask why they made the bug and prevent the entire category forever.**

Fixing a bug teaches one fact. Understanding WHY you had the wrong mental model teaches a **class of prevention** that eliminates dozens of future bugs.

---

## Input Sources
- Session transcript → moments of misconception, surprise, or correction
- `memory/knowledge-frontier.json` → concepts that transitioned from wrong to correct understanding
- `memory/learner-model.json` → anti-patterns and cognitive fingerprint

## Output Targets
- `memory/learner-model.json` → `cognitive_fingerprint` refinements based on reasoning failure patterns
- `memory/knowledge-frontier.json` → enriched with `misconception_root_cause` metadata
- Daily debrief → Section on metacognitive reflection

---

## Trigger Conditions

The Metacognitive Reflection Engine activates when ANY of these occur:

1. **Misconception Correction:** The learner held an incorrect belief that was corrected during the session. Example: Believing `@immutable` enforced immutability at runtime.
2. **Debugging Breakthrough:** The learner found a bug after initially looking in the wrong place.
3. **Architectural Reversal:** The learner proposed a design that was then shown to be flawed, and they accepted the correction.
4. **"Aha!" Moment:** The learner explicitly expressed surprise or realization ("Oh! I didn't know that").

---

## The 4-Question Reflection Protocol

When triggered, the AI asks these 4 questions in sequence:

### Question 1: "What was your first instinct?"
> *"Before I explained [concept], what did you believe was true? What was your mental model?"*

**Purpose:** Forces the learner to articulate the incorrect mental model explicitly. You cannot fix what you cannot name.

### Question 2: "Why did you think that?"
> *"What experience, pattern, or assumption led you to that incorrect belief?"*

**Purpose:** Identifies the ROOT CAUSE of the misconception. Common root causes:

| Root Cause Category | Example |
| :--- | :--- |
| **Overgeneralization** | "I assumed all annotations affect compiled output because `@override` does" |
| **Naming Confusion** | "The word 'immutable' made me think it enforced immutability" |
| **Incomplete Mental Model** | "I didn't know there was a difference between the analyzer and the compiler" |
| **Transfer from Another Language** | "In Java, annotations can affect runtime behavior via reflection" |
| **Assumption from Documentation** | "The docs said 'marks a class as immutable' so I assumed it enforced it" |

### Question 3: "What is the correct mental model now?"
> *"In your own words, explain the correct behavior. What actually happens in RAM/at compile time/at runtime?"*

**Purpose:** Verifies the correction has been internalized, not just acknowledged. If the learner cannot articulate the correct model, the misconception is not yet fixed.

### Question 4: "What class of bug does this prevent?"
> *"Now that you understand the correct behavior, what CATEGORY of similar mistakes can you now avoid?"*

**Purpose:** Elevates the learning from a single fact to a **transferable principle**. 

Example output:
> *"I now know that annotations in Dart are metadata consumed by specific tools (analyzer, code generators) and are NOT guaranteed to produce runtime behavior. So before I trust any `@annotation`, I should check: does this affect the COMPILER or just the ANALYZER? This prevents me from relying on annotations for security or correctness guarantees."*

---

## Reflection Logging Format

After the 4-question protocol, log the reflection in `knowledge-frontier.json`:

```json
{
  "id": "compiler-vs-analyzer",
  "label": "@immutable vs final",
  "misconception_log": {
    "original_belief": "Believed @immutable enforced immutability at runtime",
    "root_cause_category": "naming_confusion",
    "root_cause_detail": "The word 'immutable' in the annotation name implied enforcement",
    "correct_model": "Annotations are metadata consumed by the analyzer. Only final/const keywords produce compiler enforcement.",
    "bug_class_prevented": "Trusting annotations for security or correctness guarantees without verifying compiler-level enforcement",
    "reflection_date": "2026-11-19"
  }
}
```

---

## Metacognitive Maturity Levels

Track the learner's self-awareness growth over time:

| Level | Behavior | Indicator |
| :--- | :--- | :--- |
| **Level 1 (Unaware)** | Does not notice when they are wrong until told | Requires the AI to point out every misconception |
| **Level 2 (Reactive)** | Recognizes the mistake AFTER it is pointed out and can articulate why | Can answer "Why did you think that?" |
| **Level 3 (Proactive)** | Catches their own misconceptions mid-sentence: "Wait, actually that's wrong because..." | Self-corrects before the AI intervenes |
| **Level 4 (Preventive)** | Before writing code, asks themselves: "What am I assuming here? Have I verified this assumption?" | Independently validates assumptions before coding |

**Goal:** Progress the learner from Level 1 to Level 3+. At Level 3, the learner no longer needs the AI to catch misconceptions — they catch their own.

---

## Integration with Other Engines

- **Student Modeling Engine (5):** Misconception root causes inform the cognitive fingerprint. If the learner consistently falls for "naming confusion," the AI should preemptively clarify naming vs mechanics for new concepts.
- **Spaced Repetition Engine (8):** Concepts that required metacognitive reflection are higher decay risk. Schedule their first decay test at 10 days instead of 14.
- **Learning Analytics Engine (4):** Track metacognitive maturity level over time. Report growth in weekly summaries.
