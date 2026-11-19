# Universal SDE Growth Tracker 🌐
> **The Autonomous, Stack-Agnostic AI Engineering Mentor for Software Development Engineers**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Antigravity Skill](https://img.shields.io/badge/Antigravity-Skill-orange.svg)](https://github.com)
[![Multi-Stack](https://img.shields.io/badge/Stack-Universal-purple.svg)](rubrics/stack-mappings.md)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

**Universal SDE Growth Tracker** is an open-source AI skill that transforms passive AI assistants into an **unvarnished, objective engineering mentor**. 

Designed to accelerate any developer's journey from **Junior/Mid-level to Senior SDE / Principal Architect**, it audits your **conversation transcripts**, **code diffs**, and **architectural decisions** across 7 universal computer science domains and 5 cognitive behavioral habits.

---

## 💡 Why This Exists

Modern developers face two major hurdles when building with AI:
1. **The "Copy-Paste" Illusion:** You feel productive because the AI generates code, but you struggle to explain memory layout, concurrency hazards, or network failure modes in technical interviews.
2. **Intent Blindness:** You have brilliant high-level ideas, but your code introduces silent bugs, forgotten asynchronous synchronization, race conditions, or memory leaks because nobody audits your execution discipline.

**Universal SDE Growth Tracker** actively audits **HOW** you think and **WHAT** you master.

---

## 🧩 The Two-Tier Diagnostic Framework

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   UNIVERSAL SDE GROWTH FRAMEWORK                       │
├────────────────────────────────────────────────────────────────────────┤
│ TIER 1: COGNITIVE HABITS (How the Engineer Reasons)                    │
│    • Click Velocity (One-shot conceptual grasp vs friction cycles)     │
│    • Intent-Execution Drift (Design clarity vs syntax precision gap)   │
│    • Abstraction Stacking Ceiling (Procedural vs Stacked Generics/Fn)  │
│    • Architectural Intuition (Independent threat & failure detection)  │
│    • Tooling & Git Hygiene (Branching, clean diffs, CLI fluency)       │
├────────────────────────────────────────────────────────────────────────┤
│ TIER 2: 7 UNIVERSAL CS DOMAINS (What the Engineer Masters)             │
│    1. Execution & Memory Models (Stack/Heap, Concurrency, Runtimes)    │
│    2. Networking & Distributed Systems (Protocols, Serialization)      │
│    3. Applied Data Structures & Algorithms (Access patterns, Big-O)     │
│    4. System Architecture & Modularity (Decoupling, State flow, Scale) │
│    5. Security & Defensive Engineering (AuthN/AuthZ, Cryptography)     │
│    6. Code Craftsmanship & Rigor (Async safety, Resource lifecycle)    │
│    7. AI-Native Engineering & Steering (Decomposition, Code review)    │
├────────────────────────────────────────────────────────────────────────┤
│ TIER 3: RETENTION & ACCELERATION ENGINES                               │
│    • Active Recall Challenge (1-Question targeted retention test)       │
│    • Multi-Session Anti-Pattern Watchlist (Habit loop detection)        │
│    • Intent vs. Diff Verifier (Promised design vs committed code)       │
│    • Industry Hiring Benchmark (SDE I -> SDE II -> Senior SDE / Lead)   │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 🌍 Works Across Any Tech Stack

The rubrics map directly to any programming language or runtime:
* **TypeScript / JavaScript:** V8 execution contexts, Event Loop, Promises, Microtasks, Workers.
* **Python:** GIL, `asyncio`, Memory management, Multiprocessing.
* **Go:** Goroutines, Channels, Memory escape analysis, Pointers, Slices.
* **Rust:** Ownership, Lifetimes, Borrow Checker, Zero-cost abstractions.
* **Java / C#:** Managed Heap, Virtual Threads, Generics, Concurrency primitives.
* **C / C++:** Pointer arithmetic, Stack vs Heap, RAII, Cache lines.
* **Dart / Flutter:** Isolates, Event loop, State lifecycles, Pure Dart vs UI boundaries.

*(See [rubrics/stack-mappings.md](rubrics/stack-mappings.md) for the complete multi-language guide).*

---

## ⚡ Installation & Quick Start

### Global Installation in Antigravity / Gemini CLI
Clone this repository directly into your global skills directory:

```bash
# Windows
git clone https://github.com/<your-username>/sde-growth-tracker.git "%USERPROFILE%\.gemini\config\skills\sde-growth-tracker"

# Linux / macOS
git clone https://github.com/<your-username>/sde-growth-tracker.git "$HOME/.gemini/config/skills/sde-growth-tracker"
```

### Per-Project Workspace Installation
Copy the folder into your project's `.agents/skills/` directory:

```bash
mkdir -p .agents/skills/
cp -r sde-growth-tracker .agents/skills/
```

---

## 🕹️ Usage & Commands

| Command | Action |
| :--- | :--- |
| `/sde-audit` | Audits the current session's code diffs and chat history; generates a comprehensive `sde_debrief_YYYY_MM_DD.md` artifact. |
| `/sde-challenge` | Runs a 1-question scenario challenge testing your weakest concept from yesterday. |
| `/sde-level` | Benchmarks your current execution against SDE I, SDE II, and Senior SDE industry hiring standards. |

---

## 🤝 Community & Contributing

We welcome contributions from engineers across all languages, disciplines, and platforms!
- **Add new Stack Mappings:** Help map the 7 CS domains to Elixir, Haskell, Zig, Swift, Kotlin, etc.
- **Contribute Anti-Pattern Detectors:** Help catalog emerging developer habits in AI-assisted workflows.
- **Improve Socratic Challenges:** Share high-signal interview and debugging scenarios.

### License
Released under the permissive [MIT License](LICENSE).
