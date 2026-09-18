# User Stories — Backlog (FixGo Platform)

> **What to fill in here:** The product's User Story backlog.
> Each HU uses the standard format with Acceptance Criteria in Given/When/Then.
> Refined (Ready) HUs go to the sprint. Unrefined ones are epics or ideas.

---

## Backlog status

| Cut   | Sprint     | Total HUs | Refined | In progress | Completed |
| ----- | ---------- | --------- | ------- | ----------- | --------- |
| Cut 1 | Sprint 1-2 | 4         | 4       | 2           | 1         |
| Cut 2 | Sprint 3-4 | 3         | 1       | 0           | 0         |

---

## Epics

| ID     | Epic                            | Description                                                                                              |
| ------ | ------------------------------- | -------------------------------------------------------------------------------------------------------- |
| EP-IAM | Authentication & Identity       | User registration, login, JWT issuance, and role management for Drivers, Mechanics, and Workshop Admins. |
| EP-LOC | Emergency Assistance & Dispatch | Real-time GPS location tracking, spatial queries, tow truck dispatch, and active route updates.          |
| EP-PAY | Service Payments & Invoicing    | Secure transaction processing, payment gateway integration, and electronic service receipts.             |

---

## User Stories

### HU-IAM-001 — User Authentication & Role Assignment {#HU-IAM-001}

**Epic:** EP-IAM

> **As a** registered driver or mechanic  
> **I want to** authenticate with my credentials and receive a secure token  
> **So that** I can access role-specific roadside features securely

**Acceptance Criteria:**

```gherkin
Scenario 1: Successful user authentication
  Given a registered driver enters valid credentials (email and password)
  When they send a login request to the system
  Then the system responds with a HTTP 200 status and a valid JWT access token (1-hour expiration)
  And the token contains the user's role claim ("DRIVER")

Scenario 2: Invalid authentication credentials
  Given a user enters an unverified email or incorrect password
  When they submit the login form
  Then the system responds with HTTP 401 Unauthorized
  And displays the error message "Invalid credentials"
Gherkin
Scenario 1: Successful roadside assistance dispatch
  Given an authenticated driver with active device GPS
  When they select "Tow Service" and confirm their current coordinates
  Then the system records the geo-location point
  And broadcasts an emergency dispatch alert to all active tow operators within a 10km radius

Scenario 2: Request creation fails due to missing location
  Given a driver with disabled location services
  When they attempt to request emergency assistance
  Then the system blocks the submission
  And prompts the message "GPS precision required to locate your vehicle"
Gherkin
Scenario 1: Live movement update via WebSocket
  Given a mechanic has accepted a dispatch request and is en route
  When the mechanic moving vehicle updates its GPS coordinates
  Then the driver receives real-time position updates over WebSocket every 5 seconds
  And the map marker updates smoothly without full page refresh
---
```
