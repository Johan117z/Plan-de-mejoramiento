# ADR-003 — Database per Service Pattern Strategy

- **ID:** ADR-003
- **Date:** 2026-09-18
- **Status:** Accepted
- **Authors:** Johan Andrés Liñan Esquivel, Juan David Romero Calderón, Gabriel Tijaro Jiménez, Mateo Esteban Ramírez Garzón

---

## Context

The FixGo ecosystem comprises multiple distinct business domains: identity management (IAM), roadside emergency dispatch with a geospatial engine, real-time location telemetry, and automotive workshop catalogs. Sharing a single relational database instance or unified schema across these contexts introduces tight coupling, risk of cascading failures, and resource lockups (e.g., heavy PostGIS spatial queries blocking authentication transactions).

A clear persistence strategy is required to enforce domain boundaries, enable independent service deployments, and support horizontal scaling.

**Known constraints:**

- The emergency dispatch service requires advanced spatial data types and indexing (PostGIS).
- Location telemetry requires high-throughput, low-latency writes and fast spatial lookups.
- Infrastructure budget constraints require optimizing database instances during early development without violating logical bounded context boundaries.

---

## Decision

**We decided:** Adopt the **Database per Service** pattern. Each microservice within the FixGo platform manages its own dedicated persistence engine or logical schema, completely inaccessible to other microservices.

**Justification:**
This pattern enforces zero data-level coupling. `fixgo-dispatch-service` can execute complex PostGIS query extensions without impacting authentication workloads in `fixgo-iam-service` or real-time location streaming in `fixgo-location-service`.

---

## Evaluated alternatives

| Alternative                                     | Pros                                                                                                                      | Cons                                                                                 | Reason for discarding                                                             |
| ----------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------- |
| **Database per Service (Chosen)**               | Complete isolation, independent scaling, technology choice per service (e.g., Redis for telemetry, PostGIS for dispatch). | Increased operational complexity and distributed transaction management.             | — (chosen)                                                                        |
| **Shared Database (Single Schema)**             | Direct SQL JOINs, simple ACID transactions across domain tables.                                                          | Single point of failure, severe coupling between domain models, global outage risks. | Violates microservice independence and hinders schema evolution.                  |
| **Shared Database Instance / Separate Schemas** | Lower initial cloud hosting costs, logical schema segregation.                                                            | Risk of CPU/RAM resource contention during heavy analytical or spatial queries.      | Discarded as target architecture; acceptable only as a temporary local dev setup. |

---

## Consequences

**Positive:**

- Independent database schema migrations without cross-service coordination.
- Ability to select optimal storage engines per service (PostgreSQL + PostGIS vs. Redis Spatial).
- Fault isolation: database degradation in the workshop module does not disrupt user authentication.

**Negative / Trade-offs:**

- Cross-service SQL `JOIN` queries are impossible (e.g., mapping a Driver to an Assistance Request requires API orchestration or event consumption).
- Eventual consistency mechanisms must be introduced via event-driven messaging.

**Impact on the system:**

- Affected services: `fixgo-iam-service`, `fixgo-dispatch-service`, `fixgo-location-service`, `fixgo-workshop-service`.
- Documents to update: `05-architecture/overview.md`, `05-architecture/pattern-guide.md`, `06-data/models.md`.

---

## Risks

| Risk                             | Probability | Impact | Mitigation                                                                                    |
| -------------------------------- | ----------- | ------ | --------------------------------------------------------------------------------------------- |
| Cross-service data inconsistency | Medium      | High   | Implement transactional Outbox Pattern and choreographed Sagas over RabbitMQ.                 |
| Infrastructure cost overhead     | Medium      | Medium | Utilize logical isolated database schemas inside shared containers during initial dev phases. |

---

## References

- Pattern: Database per Service (Microservices.io)
- Related to: ADR-001 (Documentation Language), ADR-002 (Stateless JWT Authentication)
