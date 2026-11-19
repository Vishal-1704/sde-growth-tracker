# 7 Universal CS Domains: SDE Competency Radar

A stack-agnostic rubric defining engineering competencies across 7 fundamental computer science domains. Applicable to any programming language, runtime, or architectural paradigm.

---

## Rating Scale

* **Level 1 — Novice:** Writes syntax that functions on happy paths; lacks visibility into runtime memory layout, edge cases, and failure modes.
* **Level 2 — Developing:** Understands mechanics when traced sequentially (Line A -> Line B); writes implementations from structured blueprints; requires guidance on subtle race conditions and leak prevention.
* **Level 3 — Proficient:** Autonomously writes robust, production-grade code; anticipates failure modes, manages concurrency safely, and optimizes data flow without prompting.
* **Level 4 — Senior / Staff SDE:** Defines system architecture; models worst-case failure cascades; balances trade-offs across latency, throughput, memory, and operational complexity; mentors others.

---

## 1. Execution & Memory Models
*The engine: How code executes, uses memory, and interacts with the host environment.*

* **Level 1:** Unaware of stack vs heap differences; treats memory as infinite; confuses synchronous execution with asynchronous promises/futures; ignores CPU spinning vs sleeping.
* **Level 2:** Distinguishes stack allocation from heap reference pointers; understands how the runtime event loop or thread pool schedules work; grasps garbage collection pauses or reference counting lifecycles.
* **Level 3:** Understands thread safety, atomic operations, race conditions, memory visibility across cores/threads, non-blocking I/O event loops, and OS process/worker memory isolation.
* **Level 4:** Analyzes CPU cache locality (L1/L2/L3), memory fragmentation, zero-copy buffers, memory-mapped files (`mmap`), lock-free synchronization, and kernel context switch overhead.

---

## 2. Networking & Distributed Communications
*The wire: How software communicates across processes, machines, and networks.*

* **Level 1:** Treats network calls as instantaneous, reliable local function calls; unaware of latency, timeouts, or connection failures; only understands basic HTTP GET/POST.
* **Level 2:** Understands connection handshakes, timeouts, HTTP status codes, socket lifecycles, and serialization formats (JSON, Protobuf, binary framing).
* **Level 3:** Implements resilient communication: backpressure, connection pooling, idempotency keys, exponential backoff with jitter, WebSockets/gRPC streaming, and rate limiting.
* **Level 4:** Architects distributed consensus, models split-brain scenarios, designs custom wire framing protocols, analyzes MTU/packet fragmentation, and mitigates thundering herd problems.

---

## 3. Applied Data Structures & Algorithmic Efficiency
*The mechanics: How data is stored in memory and transformed efficiently.*

* **Level 1:** Defaults to a single generic list/array for everything; unaware of $O(1)$ vs $O(N)$ lookup costs; introduces unintentional nested loops ($O(N^2)$).
* **Level 2:** Distinguishes between contiguous arrays (cache-friendly) and node-based linked structures; selects Hash Maps vs Lists appropriately; reasons about space and time complexity.
* **Level 3:** Employs optimal collections for specific workloads: Priority Queues (heaps) for scheduling, Ring Buffers for streaming data, Sets for fast dedup, and B-trees/LSM trees for indexed storage.
* **Level 4:** Implements custom cache-conscious data structures; analyzes algorithmic cache line utilization; designs probabilistic structures (Bloom Filters, HyperLogLog) for massive scale.

---

## 4. System Architecture & Modularity
*The blueprint: How components are isolated, composed, and evolved.*

* **Level 1:** Writes tightly coupled "god files" mixing UI, business logic, networking, and storage; state mutations occur unpredictably across global scopes.
* **Level 2:** Enforces clear separation of concerns: decouples presentations/controllers from domain logic and data access; uses dependency injection or inversion of control.
* **Level 3:** Designs around clean boundaries: uses explicit finite state machines, event-driven pipelines, offline-first sync patterns, and fault-isolated worker components.
* **Level 4:** Architects distributed systems with clear bounded contexts; models eventual consistency vs strong consistency; designs resilient distributed systems tolerant of partial network partitions.

---

## 5. Application Security & Defensive Engineering
*The shield: How systems protect against malicious input, data leakage, and attacks.*

* **Level 1:** Hardcodes secrets or API keys; trusts client input blindly; ignores SQL/Command injection vulnerabilities; confuses hashing with encryption.
* **Level 2:** Implements proper authentication/authorization checks; uses secure environment variables; understands symmetric (AES) vs asymmetric (RSA/Ed25519) cryptography; sanitizes external input.
* **Level 3:** Implements defense-in-depth: Encrypt-then-Sign paradigms, replay attack defense (nonces/timestamps), principle of least privilege, secure token lifecycles, and threat modeling.
* **Level 4:** Designs zero-trust architectures; models advanced threat vectors (timing attacks, side-channel leaks, binary patching, MITM); conducts cryptographic audits and security threat modeling.

---

## 6. Code Craftsmanship & Testing Rigor
*The standard: How reliable, readable, and maintainable the code is in production.*

* **Level 1:** Unhandled exceptions; empty catch blocks; fragile null handling; untested code paths; changes introduce regressions in unrelated features.
* **Level 2:** Clear naming conventions; comprehensive error handling (`try/finally` resource cleanup); writes basic unit tests for happy paths and simple edge cases.
* **Level 3:** Enforces strict immutability; writes deterministic unit, integration, and property-based tests; guards against asynchronous lifecycle race conditions (unmounted views, cancelled requests).
* **Level 4:** Authors self-documenting domain models; establishes zero-regression test harnesses and architectural linting; authors libraries with intuitive, impossible-to-misuse public APIs.

---

## 7. AI-Native Engineering & Agent Steering
*The modern multiplier: How effectively the engineer steers, reviews, and collaborates with AI.*

* **Level 1:** Passive consumer; copy-pastes complete AI outputs without reading diffs; relies on AI to guess requirements without constraints; gets trapped in hallucination loops.
* **Level 2:** Provides targeted problem descriptions; reviews generated code for obvious syntax bugs; uses AI for boilerplate and test case generation.
* **Level 3:** Acts as a Senior Code Reviewer to the AI: decomposes complex features into isolated architectural bricks; rejects fragile AI patterns; rigorously validates edge cases.
* **Level 4:** Orchestrates agentic workflows; designs deterministic prompt/eval pipelines; uses AI to stress-test architecture and explore trade-offs; operates at 10x engineering velocity.
