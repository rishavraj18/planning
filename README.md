# Planning

# 3-Month Lead / Architect Interview Prep Plan
### Built around one capstone project: **Cloud-Native Order Management Platform**

---

## Why a single project

At 10 YOE, interviewers aren't testing whether you *know* Kubernetes or Kafka — they're testing whether you can **make and defend architectural trade-offs** under real constraints. A single, evolving project lets you accumulate a design doc, an ADR (architecture decision record) log, and war stories you can talk through in interviews — which is far more convincing than a list of tutorials completed.

**The project:** A multi-service order management platform (e.g., "OrderFlow") — generic enough to map to e-commerce, insurance, or fintech domains, which covers most Lead/Architect JD patterns you shared earlier.

Core services to build:
- **Order Service** (C#/.NET Core, REST + gRPC)
- **Inventory Service** (event-driven, consumes Kafka events)
- **Notification Service** (async, queue-based)
- **API Gateway / BFF** (Ocelot or YARP, or cloud-native API Gateway)
- **React/Next.js frontend** (order dashboard)

---

## Month 1 — Core Architecture & Microservices Foundation

**Goal:** Working multi-service system with clean architecture, deployed locally via Docker Compose.

| Week | Focus | Deliverable |
|---|---|---|
| 1 | Domain-Driven Design: bounded contexts, define Order/Inventory/Notification domains. Set up Clean Architecture + CQRS in .NET Core (MediatR, FluentValidation) | Solution skeleton + domain model diagram |
| 2 | Build Order Service + Inventory Service. REST APIs (Swagger/OpenAPI), SQL Server + EF Core, Polly for resilience | Two working services with API contracts |
| 3 | Event-driven integration: Kafka (or RabbitMQ) between Order → Inventory → Notification. Add gRPC for one synchronous internal call | Working async event flow + sequence diagram |
| 4 | Dockerize all services, docker-compose orchestration, unit + integration tests (XUnit, TestContainers) | Fully containerized system, test suite passing |

**Interview prep (parallel, ~3 hrs/week):**
- Review SOLID, GoF patterns, Clean Architecture vs. layered — be ready to explain *why*, not just *what*.
- Start an **ADR log** (Architecture Decision Records) — write one every time you make a non-trivial call (e.g., "Why Kafka over RabbitMQ here"). This becomes real interview material.

---

## Month 2 — Cloud-Native, Scale & Observability

**Goal:** Production-grade deployment on cloud with observability, security, and IaC.

| Week | Focus | Deliverable |
|---|---|---|
| 5 | Deploy to Kubernetes (AKS or EKS — pick one, go deep). Helm charts for each service | Running K8s cluster, Helm-based deploys |
| 6 | IaC: Terraform for cluster, networking, managed DB, secrets. CI/CD: GitHub Actions pipeline (build → test → deploy) | Terraform repo + working CI/CD pipeline |
| 7 | Observability: Prometheus + Grafana dashboards, OpenTelemetry tracing across services, structured logging (ELK or cloud-native equivalent) | Dashboard showing latency/error rates across services |
| 8 | Security & auth: OAuth2/JWT on API Gateway, secrets management (Key Vault/Secrets Manager), rate limiting, add caching layer (Redis) | Secured, cached, rate-limited system |

**Interview prep (parallel):**
- Practice 2–3 **system design problems per week** on paper/whiteboard (e.g., "design a ride-sharing dispatch system," "design a multi-tenant SaaS billing system") — timebox to 45 min each, then critique your own trade-offs.
- Read up on failure modes: circuit breakers, retries, idempotency, saga pattern for distributed transactions — you'll likely be asked to reason about failure, not just happy path.

---

## Month 3 — Architecture Depth, Leadership Narrative & Interview Simulation

**Goal:** Be able to *present* the system as if briefing stakeholders, and reframe your project work into leadership stories.

| Week | Focus | Deliverable |
|---|---|---|
| 9 | Write a formal **architecture document**: context diagram, component diagram, data flow, trade-off matrix (build vs buy, sync vs async, SQL vs NoSQL per service). Use Lucidchart/Draw.io | Polished architecture doc (this doubles as an interview artifact) |
| 10 | Scalability deep dive: load test with k6 or JMeter, identify bottlenecks, document how you'd scale to 10x traffic (sharding, read replicas, CQRS read models, multi-region) | Load test report + scaling plan |
| 11 | Behavioral/leadership prep: convert real past experiences into STAR stories for: mentoring, driving a technical decision against pushback, handling production incidents, influencing without authority | Written STAR story bank (8–10 stories) |
| 12 | Mock interviews: 2–3 system design mocks, 2 architecture "whiteboard and defend" sessions, 1 behavioral mock. Refine based on feedback | Interview-ready |

---

## Weekly Time Budget (assuming full-time job)

- **Hands-on building:** 6–8 hrs/week (weekends + 1 weekday evening)
- **System design practice:** 2–3 hrs/week
- **Reading/theory (patterns, failure modes, leadership frameworks):** 1–2 hrs/week
- **Total:** ~10–12 hrs/week — sustainable alongside a full-time role

---

## What This Plan Deliberately Skips

- Learning new languages/frameworks from scratch — you don't need Angular *and* React *and* Vue; pick what's on your target JDs and go deep.
- Chasing every cloud platform — depth on one (AWS or Azure) beats shallow coverage of three.
- Certifications — for Lead/Architect interviews, a working system + articulate trade-off reasoning outweighs a cert badge almost every time.

---

## Interview Framing Cheat Sheet

When asked "tell me about a project," don't describe *what* you built — describe:
1. **The constraint** (traffic, team size, legacy system, deadline)
2. **The alternatives you considered** and why you rejected them
3. **The trade-off you accepted** and what you'd revisit at 10x scale
4. **How you'd lead a team** through building it (code review culture, mentoring, unblocking others)

This project gives you real material for all four, across every category in your original tech list — instead of reciting tool names.