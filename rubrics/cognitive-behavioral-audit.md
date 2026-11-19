# Cognitive Behavioral Audit: Diagnostic Protocols

This rubric audits the developer's cognitive execution, mental models, and debugging habits during real-time problem-solving.

---

## 1. Click Velocity Index (One-Shot Assimilation)
Measures how rapidly an engineer converts a novel architectural or runtime concept into accurate code:
* **Score A (Instant Click):** Grasps the technical mechanics on the first explanation. Immediately applies it without requiring re-explanation.
* **Score B (Guided Assimilation):** Requires a visual diagram (memory layout, execution timeline) before the concept stabilizes.
* **Score C (Frictional Drag):** Struggles across 3+ iterations. Indicates either missing foundational prerequisites or cognitive overload from too many simultaneous concepts.

---

## 2. Intent-Execution Drift (The "Sloppy Hands" Detector)
Measures the divergence between the engineer's high-level mental design and their actual typed code:
* **The Pattern:** The engineer verbally explains the exact correct architecture, but their typed code omits null safety guards, forgets asynchronous synchronization (`await`/promises), introduces off-by-one errors, or passes incorrectly wrapped types.
* **Audit Protocol:**
  1. Contrast the engineer's verbal claim against the raw git diff.
  2. Flag any discrepancy as "Intent Blindness."
  3. Prescribe the **Pen-and-Paper Trace Rule**: read code backwards line-by-line before running or committing.

---

## 3. Abstraction Stacking Ceiling
Measures how many layers of abstraction the engineer can maintain in working memory simultaneously:
* **Layer 1:** Procedural, sequential imperative flow (Line 1 $\rightarrow$ Line 2 $\rightarrow$ Line 3).
* **Layer 2:** Generic type parameters `<T>` or isolated higher-order callbacks.
* **Layer 3+:** Simultaneous combination of Generics + Callbacks + Asynchronous state machines + Thread/Worker boundaries.
* **Audit Protocol:**
  - Pinpoint the exact layer where the engineer freezes or introduces bugs.
  - Apply **Single-Concept Isolation (Cook the Bricks First)**: isolate each layer into a standalone testable script before combining them.

---

## 4. Architectural & Failure Intuition
Tracks the engineer's proactive identification of system failure modes, concurrency race conditions, and security risks:
* **Indicators of High Intuition:**
  - Independently questioning race conditions across asynchronous boundaries.
  - Spotting unhandled network failure states or missing timeouts.
  - Identifying tamper risks, unauthorized access, or unverified inputs.
  - Proposing OS-native event subscriptions over inefficient polling loops.
* **Audit Protocol:** Document these instincts as core technical strengths to reinforce confidence and design leadership.

---

## 5. Tooling, Shell & Git Hygiene
Audits operational professionalism across tools and environments:
* **Healthy Behaviors:**
  - Creating isolated feature branches before writing code (`git switch -c`).
  - Checking `git diff` or `git status` before staging.
  - Writing clear, imperative commit messages.
  - Executing pre-flight test suites and compilation checks before pushing.
* **Red Flags:**
  - Staging all files blindly (`git add .`) on dirty workspaces.
  - Committing broken builds or untracked temporary binaries.
  - Discomfort with command-line string escaping or process signals.
