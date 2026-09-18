# 04 — Requirements (FixGo Platform)

> **What is this?** The formal specification of what the FixGo system must do and how well it must perform.
> Functional: emergency roadside dispatch, geolocation, user authentication. Non-functional: sub-second latency, security, 99.9% availability.

## Why this section exists

Requirements are the contract between the engineering team and project stakeholders.
Without them:

- There is no way to verify whether the roadside dispatch engine is complete.
- Scope changes regarding location tracking have no baseline for comparison.
- Test suites in `11-quality/` have no success criteria.

---

## Types of requirements

### Functional (FR)

Describe **what the FixGo system does**: functions, real-time map updates, and payment processing.
_Example: "The system must allow a stranded driver to request a tow truck using active GPS coordinates."_

### Non-functional (NFR)

Describe **how it performs**: response times under heavy load, data encryption, map rendering speed.
_Example: "The dispatch microservice must process geo-proximity queries within less than 250ms for 95% of requests."_

---

## Structure and Files in this Folder

### `functional.md` ⭐

List of all functional requirements across microservices.

| ID     | Module / Microservice    | Description                                               | Source (HU) | Priority |
| ------ | ------------------------ | --------------------------------------------------------- | ----------- | -------- |
| FR-001 | `fixgo-dispatch-service` | Broadcast emergency request to drivers within 10km radius | HU-LOC-001  | High     |
| FR-002 | `fixgo-iam-service`      | Authenticate users via JWT and validate RBAC permissions  | HU-IAM-001  | High     |
| FR-003 | `fixgo-location-service` | Stream real-time mechanic location via WebSockets         | HU-LOC-002  | High     |

### `non-functional.md` ⭐

Quality, performance, and operational constraints for FixGo.

#### Performance

| ID      | Requirement            | Metric           | How to verify             |
| ------- | ---------------------- | ---------------- | ------------------------- |
| NFR-001 | Dispatch query latency | p95 < 250ms      | Load testing with k6      |
| NFR-002 | System throughput      | 1000 RPS minimum | Stress testing in staging |

#### Availability

| ID      | Requirement   | Metric            | How to verify                 |
| ------- | ------------- | ----------------- | ----------------------------- |
| NFR-010 | System Uptime | 99.9% monthly SLO | Prometheus & Grafana alerting |

#### Security

| ID      | Requirement  | Description                                                   |
| ------- | ------------ | ------------------------------------------------------------- |
| NFR-020 | API Security | Mandatory JWT in Bearer header; 1-hour expiration             |
| NFR-021 | Data Privacy | Encrypt PII and real-time location history at rest (Ley 1581) |

### `user-stories.md`

Formalized User Stories defining system interactions.

- `HU-LOC-001.md`: Request Emergency Roadside Assistance.

### `traceability-matrix.md` ⭐

Connects user stories, functional/non-functional requirements, and test cases.

| HU         | FR/NFR  | Description                  | Test Case   | Status    |
| ---------- | ------- | ---------------------------- | ----------- | --------- |
| HU-LOC-001 | FR-001  | Dispatch roadside assistance | TC-DISP-001 | ✅ Passed |
| HU-LOC-001 | NFR-001 | Response latency < 250ms     | TC-PERF-001 | ✅ Passed |

---

## Correlations with other sections

| This section feeds...            | Why                                                                                         |
| -------------------------------- | ------------------------------------------------------------------------------------------- |
| `05-architecture/`               | Performance/availability NFRs guide spatial indexing (PostGIS/Redis) and caching decisions. |
| `07-api/`                        | Integration FRs map directly to OpenAPI REST endpoints and WebSocket channels.              |
| `09-microservices/`              | FRs are domain-grouped into service boundaries (`dispatch`, `iam`, `location`).             |
| `11-quality/testing-strategy.md` | Every FR and NFR has corresponding integration/load test cases.                             |
| `15-project-control/risks.md`    | High NFR metrics (e.g., real-time WebSocket scaling) highlight technical risks.             |

---

## Questions answered by this section

- What actions can stranded drivers and mechanics perform in the system?
- What are the performance, security, and availability thresholds required for emergency roadside operations?
- How are user stories linked directly to verifiable code and automated test cases?
