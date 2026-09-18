# Traceability Matrix - FixGo Platform

> Traceability connects every line of code to its business justification.
> It allows answering: "Why does this function exist?" and "Which HU covers this part of the system?"
> It also identifies: unimplemented requirements and code without a requirement (possible technical debt).

---

## How to use this matrix

---

## FR → HU → Test → Service matrix

| FR ID  | FR Description                                        | HU(s)      | Tests that verify it       | Service                  | Status         |
| ------ | ----------------------------------------------------- | ---------- | -------------------------- | ------------------------ | -------------- |
| FR-001 | Broadcast emergency request within 10km radius        | HU-LOC-001 | `dispatch.service.spec.ts` | `fixgo-dispatch-service` | 🟡 In progress |
| FR-002 | Authenticate users and validate roles                 | HU-IAM-001 | `auth.jwt.spec.ts`         | `fixgo-iam-service`      | ✅ Done        |
| FR-003 | Stream real-time mechanic GPS location via WebSockets | HU-LOC-002 | `location.socket.spec.ts`  | `fixgo-location-service` | 🟡 In progress |
| FR-004 | Process service payment via gateway integration       | HU-PAY-001 | `payment.gateway.spec.ts`  | `fixgo-payment-service`  | 🔴 Pending     |

---

## NFR → Validation matrix

| NFR ID  | Description                               | How it is validated                  | Tool                 | Status         |
| ------- | ----------------------------------------- | ------------------------------------ | -------------------- | -------------- |
| NFR-001 | Geospatial query latency P95 < 250ms      | Automated load testing in staging    | k6                   | 🟡 In progress |
| NFR-002 | System availability 99.9% SLO             | Health probes & SLO dashboards       | Grafana + Prometheus | 🟡 Monitoring  |
| NFR-004 | JWT authentication with 1-hour expiration | Security contract & integration test | Postman + OWASP ZAP  | ✅ Validated   |

---

## Inverse traceability: HU → FR

| HU         | Title                                 | FR(s) it implements | Sprint   |
| ---------- | ------------------------------------- | ------------------- | -------- |
| HU-IAM-001 | User Authentication & Authorization   | FR-002              | Sprint 1 |
| HU-LOC-001 | Request Emergency Roadside Assistance | FR-001              | Sprint 1 |
| HU-LOC-002 | Real-time Mechanic GPS Tracking       | FR-003              | Sprint 2 |
| HU-PAY-001 | Process Service Payment               | FR-004              | Sprint 3 |

---

## Status legend

| Status         | Meaning                                 |
| -------------- | --------------------------------------- |
| ✅ Done        | Implemented, tested, and in production  |
| 🟡 In progress | Under development in the current sprint |
| 🔴 Pending     | In the backlog, not started             |
| ⏸ Blocked      | Has an external blocker                 |
| ❌ Cancelled   | Removed from scope                      |

---

## Identified gaps (requirements without coverage)

> This section is updated automatically or manually when reviewing the matrix.
> A gap is: an FR without an HU, or an HU without a test, or a test without implementation.

| Gap type        | Description                                         | Required action                         | Owner            | Date       |
| --------------- | --------------------------------------------------- | --------------------------------------- | ---------------- | ---------- |
| HU without test | HU-LOC-002 lacks WebSocket disconnect recovery test | Add socket reconnection test case       | Development Team | 2026-09-18 |
| Pending FR      | FR-004 has no assigned test cases yet               | Write integration tests before Sprint 3 | QA / Dev         | 2026-09-18 |

---

## How to maintain this matrix

1. When an HU is created: add the row in the FR → HU → Test → Service section
2. When a test is written: note the file in the "Tests that verify it" column
3. When an HU is completed: change the status to ✅
4. At each Sprint Planning: review gaps and assign actions

---

## Correlations

- User Stories → `04-requirements/user-stories.md`
- Non-Functional Requirements → `04-requirements/non-functional.md`
- Testing strategy → `11-quality/testing-strategy.md`
- DoD that determines when an HU is Done → `00-governance/definition-of-done.md`
