## Current Java Version

**Java SE 25 (JDK 25)** — Released September 2025  
Long-Term Support (LTS) release focused on performance, language improvements, and developer productivity.

### Key Features in Java 25

| Feature | JEP | What It Does |
| --- | --- | --- |
| **Primitive Types in Patterns & Switch** | - | Enables pattern matching directly with primitive types. Simplifies code and improves type safety with mixed object/primitive logic. |
| **Compact Object Headers** | JEP 469 | Uses smaller object headers to reduce memory overhead. Improves memory efficiency for large-scale apps. |
| **Structured Concurrency** | JEP 482 | New API to manage multiple concurrent tasks as a single unit. Makes multithreaded code simpler and safer. |
| **Vector API Enhancements** | JEP 459 | Better support for vectorized computations. Boosts performance for numerical and AI-related workloads. |
| **Scoped Values** | JEP 464 | Safer alternative to `ThreadLocal` for sharing immutable data within threads. Improves reliability in concurrent code. |
| **JFR CPU-Time Profiling on Linux** | - | Adds precise CPU-time profiling to Java Flight Recorder. Helps diagnose and optimize performance bottlenecks. |

### Why Java 25 Matters
- **LTS Release**: Gets security + bug-fix updates for several years. Good target for production.
- **Memory & Performance**: Compact headers + Vector API = faster, leaner apps.
- **Concurrency Upgrades**: Structured concurrency + Scoped Values = less error-prone multithreading.

> **Note**: To check your installed version: `java -version` or `javac -version`
