# Hi there, I'm Mykhailo Kishko 👋
### Go Backend Engineer

---

## 🚀 About Me

Backend Engineer focused on **fault-tolerant financial backends**, **high-concurrency systems**, and **low-latency Go services**. Ranked **#1 Academic Student** in Software Engineering at Lviv Polytechnic National University. 
Years of study - From September 2024 to Now.

- 🏦 **Domain Expertise**: Payment Gateways, Double-Entry Bookkeeping Ledgers, Idempotent Billing, Fraud Prevention, ISO/IEC 7812 Card Verification.
- ⚡ **Low-Latency & Concurrency**: GMP Scheduler tuning, 0-allocations hot paths, struct memory alignment (zero padding holes), Lock Contention & Deadlock elimination.
- 🏛 **Architecture**: Clean & Hexagonal Architecture, Strict Explicit DI (Composition Root), Transactional Outbox, Saga Pattern.

---

## 🛠 Tech Stack & Core Competencies

| Domain | Technologies & Deep Dives |
|---|---|
| **Languages & Runtime** | **Go 1.22+** (Goroutines, Channels, sync.Pool, Escape Analysis, Race Detector), SQL, Bash, C/C++ basics |
| **Databases & Cache** | **PostgreSQL 16** (MVCC, Row-Level Lock Ordering, `FOR UPDATE SKIP LOCKED`, pgx/v5, Connection Pools, Partial Indexes), **Redis** (Distributed Locks with Watchdog, Atomic Lua Scripts, Sliding Window Rate Limiting) |
| **Distributed Systems** | **Apache Kafka** (Commit Log, Partition Routing by `aggregate_id`, Idempotent Producer, Strict FIFO, Manual Offset Commit), **Transactional Outbox Pattern** |
| **Architecture & Design** | Clean / Hexagonal Architecture, Pure Explicit DI, Circuit Breakers, Exponential Backoff + Full Jitter, REST API, gRPC / Protobuf |
| **DevOps & Verification** | **testcontainers-go** (ephemeral PostgreSQL), Benchmark Profiling (`-benchmem`), Multi-stage Docker (`scratch` <20MB, unprivileged UID), CI/CD |

---

## 🏆 Key Engineering Projects

### 💳 [Card Validator API (Fintech Gateway)] (October 2026)
> *High-performance credit card verification microservice built for payment gateways (Solidgate test task specs).*
- **Clean Architecture & Strict Priority**: Format validation $\to$ Luhn checksum $\to$ Expiration verification against an abstracted `Clock` provider.
- **Zero-Allocation Luhn Algorithm**: Pure $O(N)$ right-to-left byte traversal achieving **0 B/op and 0 allocs/op** (~40ns/op).
- **Flexible DTO Parsing**: Custom JSON unmarshaling seamlessly accepting heterogeneous payload types (`string` / `number`) without precision degradation.
- **>90% Test Coverage**: Full table-driven test suite with deterministic Mock Clock testing and multi-stage containerization.

### 💰 [Wallet & Billing Core Engine] (February 2025 - June 2026)
> *Production-grade distributed ledger handling balance mutations, transactional outbox streaming, and strict idempotency.*
- **Zero-Deadlock Concurrency**: Implemented deterministic row-level locking order (`min(ID1, ID2) -> max(ID1, ID2)`), reducing DB round-trips from 5 to 2 and eliminating deadlocks under extreme contention.
- **Transactional Outbox & Strict FIFO**: Guaranteed at-least-once financial events publication to Apache Kafka partitions via an asynchronous worker using `FOR UPDATE SKIP LOCKED`.
- **Atomic Idempotency Engine**: `INSERT ... ON CONFLICT DO UPDATE` with TTL locks to prevent double-charging during network retries and payment gateway recovery storms.
- **Memory Optimization**: 64-bit struct memory alignment reaching **54 ns/op latency and 0 allocs/op** on core mutation methods (`Withdraw` / `Deposit`).

### 🛡️ [Distributed Rate Limiter & Concurrency Coordinator] (March 2026)
> *High-throughput concurrency toolkit protecting payment services from card testing attacks.*
- **Sliding Window Rate Limiter**: Built with Redis Sorted Sets (ZSET) and atomic Lua scripts, eliminating the $2\times$ burst vulnerability of fixed-window algorithms.
- **Distributed Locks with Watchdog**: Implemented atomic Redis distributed lock with UUID ownership checks and an asynchronous background goroutine watchdog extending lock TTL during prolonged tasks.

---

## 📬 Contact & Connect

- 📍 **Location**: Lviv, Ukraine
- 📧 **Email**: [mykhailokishko@gmail.com](mailto:mykhailokishko@gmail.com)
- 💼 **Telegram**: [my tg for contact](https://t.me/m_kishko)
- 🐙 **GitLab**: [gitlab.com/mykhailokishko](https://gitlab.com/mykhailokishko)
