# 06 — Data Architecture & Persistence Policies: FixGo

> **What is this?** The master directory for data persistence, database schemas, dictionary specifications, and schema evolution strategies for FixGo microservices.

---

## Why this section exists

Data governance in a microservices architecture requires strict boundaries. In FixGo, emergency dispatch, location tracking, and repair management demand distinct storage capabilities:

- Geospatial querying (PostGIS) for matching drivers with nearby mechanics/tow trucks.
- High-throughput key-value caching (Redis) for live streaming GPS coordinates.
- Relational integrity (PostgreSQL) for transactional service orders and user accounts.

To prevent distributed monoliths, **each service strictly owns its database schema**.

---

## Folder Map & Index

| Document                                                           | Description                                                                                       | Status              |
| ------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------- | ------------------- |
| **[`data-models.md`](./data-models.md)** ⭐                        | Database per service, ER diagrams, PostgreSQL/PostGIS schemas, and indexes.                       | **Completed**       |
| **[`data-dictionary.md`](./data-dictionary.md)** ⭐                | Detailed business data dictionary, attributes, types, and allowable status ENUMs.                 | **Ready for Draft** |
| **[`modeling-conventions.md`](./modeling-conventions.md)**         | Project-wide standards for identifiers (UUIDv4), timestamps, snake_case naming, and soft deletes. | **Ready for Draft** |
| **[`normalization-assessment.md`](./normalization-assessment.md)** | Trade-offs analysis: BCNF normalization vs intentional read-side denormalization for performance. | **Ready for Draft** |
| **[`migration-strategy.md`](./migration-strategy.md)**             | Zero-downtime Flyway migration rules, forward-only schema updates, and 2-phase field deprecation. | **Ready for Draft** |

---

## Data Ownership & Storage Engine Matrix

| Microservice         | DB Engine               | Primary Data Entities                         | Justification                                                      |
| -------------------- | ----------------------- | --------------------------------------------- | ------------------------------------------------------------------ |
| `auth-service`       | PostgreSQL 15           | `users`, `roles`, `credentials`               | ACID compliance, user auth, RBAC constraints.                      |
| `assistance-service` | PostgreSQL 15 + PostGIS | `assistance_requests`, `dispatch_assignments` | Native spatial indexing (`GEOGRAPHY`, `GIST`) for radius matching. |
| `workshop-service`   | PostgreSQL 15           | `workshops`, `service_orders`, `mechanics`    | Complex relational hierarchies, state transition integrity.        |
| `telemetry-service`  | Redis 7 + MongoDB 7     | Geo spatial streams, telemetry logs           | Low-latency in-memory GPS stream ingestion & time-series storage.  |

---

## Key Data Governance Rules

1. **No Cross-Database Joins:** Microservices never query another service's database directly. All cross-boundary data is obtained via REST contracts or domain events.
2. **Immutable Audit Trails:** Tables must implement soft deletes (`deleted_at`) alongside creation/update timestamps in UTC (`TIMESTAMPTZ`).
3. **Migration Integrity:** Direct DDL changes in staging/production are forbidden; all alterations run through versioned Flyway scripts (`V001__...sql`).

---

## Correlations with Other Sections

| If you modify this data section...  | You must also update...                                   |
| ----------------------------------- | --------------------------------------------------------- |
| Database Schemas (`data-models.md`) | DTOs & API Contracts (`07-api/`) and UML ERDs (`08-uml/`) |
| Entity Attributes                   | Domain Specifications (`02-domain/entities-and-rules.md`) |
| Service Persistence Configurations  | Microservice Infrastructure Guides (`09-microservices/`)  |
