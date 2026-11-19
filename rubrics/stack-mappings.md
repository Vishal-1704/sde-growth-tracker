# Multi-Stack Mapping Guide: Translating Universal CS Concepts

This guide maps the 7 Universal CS Domains across major programming languages and runtimes.

---

## 1. Execution & Memory Models

| Universal Concept | TypeScript / JavaScript | Python | Go | Rust | Java / C# | C / C++ |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Call Stack vs Heap** | V8 Execution Context, Call Stack vs Heap memory | Frame objects on stack, Heap-allocated PyObjects | Stack frames with dynamic growth vs Escape analysis to heap | Zero-cost stack allocation vs `Box<T>` heap allocation | Thread stack frames vs JVM/CLR managed heap | Automatic storage (stack) vs `malloc`/`new` (heap) |
| **Concurrency Model** | Single-threaded Event Loop + Worker Threads | GIL + `threading` / `asyncio` / `multiprocessing` | Goroutines + Channels (M:N scheduler) | OS Threads + Async runtime (`tokio`, `async/await`) | OS Threads + Virtual Threads (Project Loom) / Tasks | OS Threads (`std::thread`, `pthreads`), Atomics |
| **Memory Management** | Generational Mark-and-Sweep GC | Reference Counting + Cyclical GC | Concurrent Tricolor Mark-and-Sweep GC | Compile-time Ownership & Borrow Checker (Zero GC) | Generational / ZGC / G1 Garbage Collector | Manual (`free`/`delete`) or RAII (`std::unique_ptr`) |

---

## 2. Asynchronous Lifecycle & Safety

| Universal Concept | TypeScript / JavaScript | Python | Go | Rust | Dart / Flutter |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Async Primitive** | `Promise<T>` | `asyncio.Future` / Task | Goroutine + `chan T` | `Future<Output = T>` | `Future<T>` |
| **Async Cancellation** | `AbortController` / `AbortSignal` | `asyncio.Task.cancel()` | `context.Context` (`WithCancel`) | Drop future or `tokio::select!` cancellation | `CancelableOperation` |
| **Post-Async State Guard** | Check `component.isMounted` / Ref check | Check task cancellation flag | Check `ctx.Err() != nil` | Check channel status / state lock | `if (!mounted) return;` |
| **Resource Cleanup** | `try...finally` or `using` (ES2023) | `try...finally` or `async with` | `defer file.Close()` | `Drop` trait implementation | `try...finally` |

---

## 3. Applied Data Structures

| Universal Workload | TypeScript | Python | Go | Rust | Java / C# |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **O(1) Key-Value Lookup** | `Map<K, V>` | `dict` | `map[K]V` | `HashMap<K, V>` | `HashMap` / `Dictionary` |
| **O(1) Unique Set** | `Set<T>` | `set` | `map[T]struct{}` | `HashSet<T>` | `HashSet` |
| **Priority Queue / Heap** | Custom Heap / MinPriorityQueue | `heapq` module | `container/heap` | `BinaryHeap<T>` | `PriorityQueue` |
| **Streaming / Ring Buffer** | Circular TypedArray buffer | `collections.deque(maxlen=N)` | Ring buffer slice | `VecDeque<T>` | `ArrayDeque` |

---

## 4. System Modularity & State Management

| Architectural Principle | Backend Web / Cloud | Mobile / Frontend | Systems / Edge Daemons |
| :--- | :--- | :--- | :--- |
| **Decoupling Boundaries** | Controller $\rightarrow$ Service $\rightarrow$ Repository pattern | View $\rightarrow$ ViewModel/Notifier $\rightarrow$ Domain Entity | CLI/Daemon Driver $\rightarrow$ Engine $\rightarrow$ Storage/Port |
| **State Machine Pattern** | Explicit order/billing status state machine | UI State (Loading, Data, Error) union/sealed classes | Service lifecycle (Boot, Standby, Active, Draining) |
| **Failure Isolation** | Circuit Breaker + Dead Letter Queue | Error Boundary widgets + Fallback UI | Worker isolation (Process/Isolate restart on panic) |

---

## 5. Security & Cryptography

| Security Practice | Web API / Microservices | Mobile / Desktop Clients | Edge / IoT Devices |
| :--- | :--- | :--- | :--- |
| **Secret Management** | Vault, AWS Secrets Manager, Env vars | OS Keystore / Keychain / DPAPI | TPM / Secure Enclave / Encrypted files |
| **Data Integrity** | HMAC-SHA256, JWT Signature validation | Code Signing, App Transport Security | Ed25519 asymmetric signatures, Secure Boot |
| **Confidentiality** | TLS 1.3, AES-GCM at-rest encryption | SQLCipher, Encrypted Preferences | AES-256-CBC/GCM payload encryption |
