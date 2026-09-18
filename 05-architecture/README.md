# 05 — Architecture Specifications & System Design — FixGo Platform

> **What is this?** This directory contains the complete technical snapshot and architectural design decisions for the **FixGo** platform. It details how the system is organized, the design patterns applied across microservices, deployment models, and the Architectural Decision Records (ADRs) that justify technology choices.

---

## Executive Overview

## FixGo's architecture is built as a **Microservices & Event-Driven Platform** tailored for low-latency emergency dispatch, real-time vehicle telemetry tracking, and scalable service order management. It strictly enforces **Hexagonal Architecture (Ports & Adapters)** within each microservice boundary to guarantee technology independence.

## Directory Index & Navigation Guide

### 1. `overview.md` ⭐ (Start Here)

Provides the overall system architecture, C4 Level 1 (Context) and Level 2 (Container) diagrams, the complete service catalog (IAM, Dispatch, Location, Workshop, Notification), and core architectural principles (API-First, Database-per-Service).

### 2. `hexagonal-architecture.md`

Specifies how the **Ports and Adapters** pattern is structured across FixGo's codebase (`domain/`, `application/`, and `infrastructure/` boundaries) to isolate domain business logic from PostGIS, WebSockets, or third-party SDKs like Mapbox and FCM.

### 3. `pattern-guide.md`

Catalog of software design patterns (GoF) and microservices architectural patterns (API Gateway, Outbox Pattern, Choreographed Saga, Circuit Breakers) adopted throughout FixGo, specifying when and how to implement them.

### 4. `decisions/` ⭐⭐ — Architecture Decision Records (ADRs)

Formal log recording critical design decisions, alternatives evaluated, and project consequences:

- [`ADR-001`](decisions/records/0001-postgis-geospatial.md): Selection of PostgreSQL + PostGIS for spatial queries.
- [`ADR-002`](decisions/records/0002-jwt-authentication.md): Stateless JWT strategy for driver, mechanic, and workshop access.

---

## Core Architectural Principles for FixGo

1. **Database per Service:** No microservice directly queries another service's database. Data sharing occurs solely via REST APIs or domain events.
2. **Geospatial Efficiency:** All location telemetry and radius searches utilize standard Spatial Reference Identifiers (`SRID 4326 - WGS 84`) indexed with GIST.
3. **Resiliency by Design:** External failures (e.g., Mapbox routing downtime) fallback gracefully via Circuit Breakers without blocking active dispatch requests.
4. **Contract-Driven Development:** OpenAPI specifications dictate frontend and backend integrations prior to feature implementation.

---

## Correlations with Other Sections

| Architectural Source / Artifact     | Feeds Into / Configures                             |
| ----------------------------------- | --------------------------------------------------- |
| `02-domain/bounded-contexts.md`     | Bounded context isolation for `09-microservices/`   |
| `04-requirements/non-functional.md` | Scalability, security, and response time thresholds |
| `05-architecture/decisions/`        | Enforces standards in `09-microservices/_template/` |
| `07-api/contracts/`                 | Implements API Gateway edge routes and REST specs   |
