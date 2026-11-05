# Mykhailo Kishko
**Go Backend Engineer | High-Concurrency & Distributed Systems**  
📍 Lviv, Ukraine | 📞 +380 68 781 9423 | ✉️ kishkomyhajlo@gmail.com  
🔗 [github.com/mykhailokishko](https://github.com/mykhailokishko) | 🔗 [gitlab.com/mykhailokishko](https://gitlab.com/mykhailokishko)

---

## 🚀 Professional Summary
Software Engineering student ranked #1 academically at Lviv Polytechnic National University. Backend Engineer focused on fault-tolerant financial ledgers, low-latency Go microservices, and high-concurrency event-driven systems. Deep practical expertise in Clean/Hexagonal Architecture, transactional outbox streaming with PostgreSQL and Apache Kafka, zero-allocation hot paths, and deadlock-free SQL concurrency patterns.

---

## 🎓 Education

**Lviv Polytechnic National University**  
*Bachelor of Software Engineering (Specialty 121)* — **Ranked #1 Student in Department** | *Sep. 2024 – Present*
- **Top-1 Academic Excellence:** Ranked 1st overall in academic standing across the entire Software Engineering specialty.
- **Relevant Coursework:** Operating Systems & Systems Programming, Relational Database Internals, High-Performance Computing, Computer Architecture, Distributed Systems.

---

## 🛠️ Technical Competencies

- **Languages & Core Runtime:** Go 1.26+ (Goroutines, Channels, `sync.Pool`, Escape Analysis, Race Detector), SQL (PostgreSQL 16 dialect), C/C++ basics, Bash scripting.
- **Databases & Caching:** PostgreSQL 16 (`pgx/v5`, MVCC, Row-Level Locks, `FOR UPDATE SKIP LOCKED`, Connection Pools, Partial Indexes), Redis 7 (Distributed Locks + Watchdog, Atomic Lua Scripts, ZSET Rate Limiting, Cache-Aside).
- **Distributed Systems & Messaging:** Apache Kafka (KRaft mode, Partition routing by `aggregate_id`, Strict FIFO Ordering, `acks=all`, Idempotent Producer, Manual Offset Commit), Transactional Outbox Pattern.
- **Architecture & Design Patterns:** Clean / Hexagonal Architecture, Pure Explicit DI (Composition Root), Finite State Machine (FSM), Unit of Work, Circuit Breaker, Exponential Backoff + Full Jitter, REST API, gRPC / Protobuf.
- **Testing & DevOps:** `testcontainers-go` (ephemeral PostgreSQL), Benchmark Profiling (`-benchmem`, 0 allocs/op), Race Detector (`-race`), Docker (Multi-stage `scratch` <20MB, unprivileged UID), GitLab CI/CD, Linux (`epoll`, `syscalls`, `cgroups v2`).

---

## 🏆 Key Engineering Projects

### 🖨️ PrintFlow — Industrial ERP/MES Backend Platform
*Go 1.26 | PostgreSQL 16 | Apache Kafka (KRaft) | pgx/v5 | Singleflight | Docker Compose* | *October 2026 – Present*
- **Clean Domain Engine & FSM:** Architected an isolated domain model enforcing order state machine transitions (`DRAFT` → `SHIPPED`) and zero IEEE 754 precision loss using fixed-point integer units (`int64` grams) and Basis Points (`AllowedWasteBps`).
- **Zero-Allocation Data Access Layer (DAL):** Implemented a `pgx/v5` connection pool layer with a generic `Querier` interface abstracting `*pgxpool.Pool` and `*pgx.Tx`, eliminating allocation overheads during query execution and state scanning.
- **Transactional Outbox & Unit of Work:** Engineered an atomic `CreateOrder` use-case within `OrderService` executing `INSERT INTO orders` and `INSERT INTO outbox_events` inside a single PostgreSQL ACID transaction, eliminating dual-write anomalies.
- **Asynchronous Kafka Worker:** Developed a high-throughput event worker polling pending outbox events using `FOR UPDATE SKIP LOCKED`, featuring exponential backoff (up to 30s) and a 5-retry threshold to prevent retry storms.
- **Singleflight Coalescer & Index Optimization:** Built a thread-safe `Singleflight` batching dispatcher to eliminate thundering herd connection starvation. Designed partial composite B-Tree indexes (`WHERE status = 'PENDING'`), reducing active index bloat by >95%.
- **Testing Pyramid & Concurrency Guard:** Established a 4-tier testing strategy achieving 75.6%+ code coverage, including 50-goroutine concurrent stress tests validated under `go test -race` with zero race conditions.

---

### 💰 Wallet & Billing Core Engine
*Go 1.22+ | PostgreSQL 16 | Apache Kafka | Redis 7 | Docker | GitLab CI* | *March 2025 – June 2026*
- **Clean / Hexagonal Architecture:** Implemented pure dependency inversion (`Domain` ← `UseCase` ← `Adapters`) with explicit Composition Root DI in `main.go`, isolating financial invariants from infrastructure frameworks.
- **Zero-Deadlock Concurrency:** Designed a deterministic row-level locking strategy (`min(ID1, ID2)` → `max(ID1, ID2)`) combining locking and state retrieval in a single query, reducing DB network round-trips from 5 to 2 and eliminating Deadlocks under heavy contention.
- **Transactional Outbox Pattern:** Guaranteed At-Least-Once delivery by atomically recording balance mutations and domain events in a single PostgreSQL ACID transaction, polled via an asynchronous worker using `FOR UPDATE SKIP LOCKED`.
- **Strict FIFO Event Streaming:** Integrated an Apache Kafka publisher routing financial mutations by `aggregate_id` (Account ID) to ensure deterministic message ordering within Kafka partitions.
- **Idempotent Billing Guard:** Built an atomic idempotency engine (`INSERT ... ON CONFLICT DO UPDATE` with TTL locks) preventing double charges during network retries and payment gateway recovery storms.
- **Zero-Allocation Hot Path Optimization:** Enforced 64-bit struct memory alignment (zero padding holes), achieving 54 ns/op latency and 0 allocs/op on core financial mutation methods (`Withdraw` / `Deposit`).
- **Rigorous Verification with Testcontainers:** Authored integration stress tests spinning up real ephemeral `postgres:16-alpine` containers via `testcontainers-go`; validated zero deadlocks and 100% balance conservation across 100 concurrent cross-transfers with Race Detector (`-race`).
- **Production Hardening & CI/CD:** Configured a minimal multi-stage Dockerfile on `scratch` (<20MB, unprivileged UID 10001) and an enterprise `.gitlab-ci.yml` pipeline covering linting, unit tests, and integration tests.

---

### 🛡️ Distributed Rate Limiter & Concurrency Coordinator
*Go | Redis 7 | Lua Scripts | epoll* | *March 2026*
- **Sliding Window Rate Limiter:** Engineered a sliding window rate limiter in Go using Redis Sorted Sets (`ZSET`) and atomic Lua scripts, eliminating the 2x burst vulnerability of Fixed Window algorithms at window boundaries.
- **Distributed Lock with Watchdog:** Implemented an atomic Distributed Lock pattern with UUID ownership verification and an asynchronous background Watchdog goroutine to periodically extend lock TTL during long-running background tasks.

---

### 💳 Card Validator API (Fintech Gateway)
*Go 1.22+ | REST API | Clean Architecture | Docker* | *October 2026*
- **Zero-Allocation Luhn Traversal:** Built a high-performance credit card verification microservice featuring pure $O(N)$ right-to-left byte traversal achieving 0 B/op and 0 allocs/op (~40ns/op).
- **Flexible DTO Parsing:** Custom JSON unmarshaling seamlessly accepting heterogeneous payload types (string / number) without precision degradation.
- **Table-Driven Verification:** >90% test coverage with deterministic Mock Clock testing and multi-stage containerization.
