# HU-LOC-001: Request Immediate Roadside Assistance

> **ID convention:** `HU-[SERVICE_ABBREVIATION]-[NNN]`
> Examples: HU-IAM-001, HU-SCHED-023, HU-REF-005

---

## Story

**As a** registered driver experiencing a vehicle breakdown  
**I want to** submit a roadside assistance request with my real-time GPS location  
**So that** nearby mechanics or tow truck drivers can accept the service and come to my location

---

## Acceptance criteria

> Format: "Given [context/initial state], when [user action], then [expected and verifiable result]"

- [ ] **AC1:** Given that the driver is authenticated and GPS is enabled, when they select a service type (e.g., tow truck, battery jump) and submit the request, then the system records the geo-coordinates and broadcasts the dispatch event to nearby mechanics within a 10km radius.
- [ ] **AC2:** Given that a request is successfully created, when the system assigns a mechanic, then the driver receives a real-time notification with the mechanic's estimated time of arrival (ETA) and profile details.
- [ ] **AC3:** Given that the driver's device GPS is disabled or inaccurate, when they attempt to request assistance, then the system displays a clear error message prompting them to enable high-accuracy location services or manually select a pin on the map.

---

## Technical notes

> Implementation constraints, performance considerations, required integrations

**Responsible service(s):** Location & Dispatch Service (`fixgo-dispatch-service`)  
**Endpoint(s) implemented:** `POST /api/v1/assistance-requests`  
**Events generated:** `RoadsideAssistanceRequestedEvent`, `DispatchBroadcastSentEvent`  
**Required permissions:** `assistance:request` (Role: `DRIVER`)

---

## Definition of Done (DoD)

> This HU can only be closed when it meets the team's full DoD.
> See: [`00-governance/definition-of-done.md`](../../00-governance/definition-of-done.md)

**Additional checks specific to this HU:**

- [ ] Geospatial index (PostGIS / MongoDB 2dsphere) created and verified for performance under 100ms.
- [ ] WebSocket connection tested for real-time dispatch updates.

---

## Estimation and priority

| Field         | Value                              |
| ------------- | ---------------------------------- |
| Story Points  | 5                                  |
| Priority      | High                               |
| Target sprint | Sprint 1                           |
| Dependencies  | HU-IAM-001 (Driver Authentication) |
