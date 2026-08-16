# 3-Month Lead / Architect Interview Prep Plan
### Built around one capstone project: **Real-Time Fraud & Risk Detection Pipeline**

---

## Why this project

Fraud/risk detection is a high-signal domain for Lead/Architect interviews because it forces you to reason about **throughput, latency, and correctness under uncertainty** simultaneously — not just CRUD-and-scale. It's also a very common system design prompt ("design a system that scores transactions for fraud in real time"), so building it yourself gives you a rehearsed, defensible answer instead of a whiteboard guess.

**The project:** "RiskGuard" — a system that ingests a stream of transactions/events, scores each for fraud risk in near-real-time, and exposes both a synchronous scoring API and an async investigation/dashboard workflow.

Core components to build:
- **Ingestion Service** (C#/.NET Core, REST API + event producer)
- **Stream Processing Layer** (Kafka + Kafka Streams, or Flink if you want a differentiator)
- **Feature Store** (real-time features: transaction velocity, account age, geo-mismatch — Redis for hot path, Postgres for cold/historical)
- **Rules Engine + Scoring Service** (combines static rules with a simple ML/heuristic score)
- **Case Management API + React Dashboard** (analysts review flagged transactions)
- **Notification Service** (async alerts for high-risk events)

---

## Month 1 — Core Pipeline & Domain Modeling

**Goal:** Working end-to-end pipeline: transaction in → scored → flagged/stored, running locally via Docker Compose.

| Week | Focus | Deliverable |
|---|---|---|
| 1 | Domain modeling: define transaction schema, risk factors, bounded contexts (Ingestion, Scoring, Case Management). Set up Clean Architecture + CQRS in .NET Core (MediatR, FluentValidation) | Domain model + context diagram |
| 2 | Build Ingestion Service (REST API accepting transactions, publishes to Kafka) + Case Management Service (Postgres-backed, stores flagged cases) | Two working services, Swagger/OpenAPI docs |
| 3 | Build Feature Store: Redis for real-time features (rolling transaction count, velocity checks) + Postgres for historical account data. Wire into a Kafka consumer | Feature extraction pipeline consuming live events |
| 4 | Build Rules Engine + Scoring Service: combine deterministic rules (e.g., "3+ transactions in 60s") with a weighted score. Dockerize everything, integration tests (XUnit, TestContainers) | Fully containerized pipeline scoring live transactions |

**Interview prep (parallel, ~3 hrs/week):**
- Review event-driven patterns: at-least-once vs exactly-once delivery, idempotency keys, dedup strategies — fraud systems can't double-count or double-alert.
- Start your **ADR log** — first entries: "Why Kafka Streams over a custom consumer," "Why Redis for hot-path features vs. querying Postgres directly."

---

## Month 2 — Scale, Low Latency & Observability

**Goal:** Production-grade deployment that can be reasoned about under load, with full observability into scoring latency and accuracy.

| Week | Focus | Deliverable |
|---|---|---|
| 5 | Deploy to Kubernetes (AKS or EKS). Helm charts for each service. Separate hot path (scoring, must be low-latency) from cold path (batch analytics, historical model retraining data) | Running K8s cluster with hot/cold path separation |
| 6 | IaC: Terraform for cluster, Kafka (MSK/Confluent Cloud or self-managed), managed Redis/Postgres. CI/CD: GitHub Actions pipeline | Terraform repo + CI/CD pipeline |
| 7 | Observability: Prometheus + Grafana for scoring latency (p50/p95/p99), false-positive rate tracking, OpenTelemetry tracing from ingestion → score → alert, structured logging | Dashboard showing scoring latency + throughput |
| 8 | Load testing (k6) to find throughput ceiling; add caching, batch feature lookups, and horizontal scaling for the scoring service. Add auth (OAuth2/JWT) on the case management API | Load test report + documented scaling limits |

**Interview prep (parallel):**
- Practice 2–3 system design prompts/week centered on **stream processing and low-latency scoring**: "design a real-time bidding system," "design a rate limiter," "design a live leaderboard" — these share the same bones as fraud scoring.
- Study failure modes specific to this domain: what happens when Kafka lags, when the feature store is stale, when the scoring service is slow — how do you fail safe (block transaction) vs fail open (allow + flag for review)? This trade-off is a great interview talking point.

---

## Month 3 — Architecture Depth, Leadership Narrative & Interview Simulation

**Goal:** Present RiskGuard as if briefing risk/fraud stakeholders, and convert the build into leadership stories.

| Week | Focus | Deliverable |
|---|---|---|
| 9 | Write a formal **architecture document**: context diagram, data flow (hot vs cold path), trade-off matrix (fail-safe vs fail-open, sync vs async scoring, rules engine vs ML model), scaling strategy | Polished architecture doc (interview artifact) |
| 10 | Scalability deep dive: model how the system behaves at 10x transaction volume — sharding Kafka topics, read replicas, feature store partitioning, multi-region failover for the scoring path | Scaling plan document |
| 11 | Behavioral/leadership prep: convert build experience into STAR stories — e.g., "how I decided fail-safe vs fail-open," "how I'd onboard a new engineer to this system," "how I'd handle a production incident where false positives spiked" | Written STAR story bank (8–10 stories) |
| 12 | Mock interviews: 2–3 system design mocks (bias toward streaming/real-time systems), 2 "whiteboard and defend the architecture" sessions, 1 behavioral mock | Interview-ready |

---

## Weekly Time Budget (assuming full-time job)

- **Hands-on building:** 6–8 hrs/week
- **System design practice:** 2–3 hrs/week
- **Reading/theory (streaming patterns, failure modes, leadership frameworks):** 1–2 hrs/week
- **Total:** ~10–12 hrs/week

---

## What Makes This Project Interview-Ready

The fraud/risk domain gives you built-in answers to the questions Architect interviews love to ask:

1. **"How do you handle a slow dependency in the critical path?"** → You'll have a real answer: fail-safe vs fail-open decision in the scoring service.
2. **"How do you scale a stateful stream processor?"** → You'll have partitioning and hot/cold path separation to point to.
3. **"How do you avoid data loss in an event-driven system?"** → You'll have your Kafka delivery-semantics decision and idempotency strategy.
4. **"How would you evolve this system as requirements change?"** → You'll have the rules engine (easy to extend) vs. ML scoring (harder to explain/audit) trade-off ready.

---

## Interview Framing Cheat Sheet

When describing this project, structure it as:
1. **The constraint** — transactions must be scored in under X ms, false positives cost customer trust, false negatives cost money.
2. **The alternatives you considered** — e.g., synchronous DB lookup vs. pre-computed feature store; rules engine vs. ML model.
3. **The trade-off you accepted** — and what you'd revisit at 10x scale or with a dedicated ML team.
4. **How you'd lead a team through it** — code review standards for the scoring logic (high-stakes, needs strong review), mentoring on distributed systems concepts, handling the on-call rotation for a system where downtime = unscored transactions.