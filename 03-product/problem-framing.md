# Problem Framing — Problem Definition

> **Why this document exists:** Before designing solutions, the team must be
> aligned on the problem it solves. This document captures that alignment.
> A well-defined problem is already halfway to a solution.

---

## 1. The problem in one sentence

**Drivers and motorcyclists** who **experience mechanical failures on urban or highway routes** struggle with **securing immediate and reliable roadside assistance** because **current mechanisms depend on fragmented contacts and lack real-time geolocation**, resulting in **average wait times exceeding 90 minutes and uncertain service costs**.

---

## 2. Affected users

| Segment                          | Description                                           | Estimated size        | Priority |
| -------------------------------- | ----------------------------------------------------- | --------------------- | -------- |
| Stranded Drivers & Motorcyclists | Vehicle owners facing roadside mechanical emergencies | 50,000+ local drivers | High     |
| Mechanics & Tow Truck Operators  | Independent service providers seeking roadside jobs   | 200+ local workshops  | High     |

### Jobs-to-be-done (JTBD)

**When** my vehicle suffers an unexpected mechanical failure on the road,  
**I want** to request nearby verified mechanics or tow services using real-time location tracking,  
**so that** I can get back on the road safely and without excessive delays or unfair pricing.

---

## 3. Evidence of the problem

| Evidence type      | Source                                   | Date       | Key finding                                                        |
| ------------------ | ---------------------------------------- | ---------- | ------------------------------------------------------------------ |
| User interviews    | 25 interviews with drivers and mechanics | 2026-02-15 | 80% reported long delays and lack of cost transparency.            |
| Support data       | Local driver group surveys               | 2026-01-20 | Over 60% of manual calls fail to reach available mechanics nearby. |
| Benchmarking       | Traditional phone dispatch analysis      | 2026-02-01 | Response times range from 60 to 120 minutes using phone calls.     |
| Direct observation | Shadowing local tow dispatchers          | 2026-02-10 | Dispatching relies on manual phone calls without GPS map routing.  |

---

## 4. Current user solution (and its problems)

| Current solution              | Limitations                                                    | Cost/Friction             |
| ----------------------------- | -------------------------------------------------------------- | ------------------------- |
| Direct phone calls / WhatsApp | Slow response times, no real-time tracking, static phone lists | 1 to 2 hours of waiting   |
| Insurance Call Centers        | High subscription fees, complex approval steps, rigid coverage | High costs, slow dispatch |

---

## 5. Solution hypothesis

**We believe that** a geolocation-based matching application (FixGo)  
**for** drivers needing roadside assistance,  
**will achieve** faster connection times with nearby mechanics and transparent pricing.  
**We will know we succeeded when** average arrival times drop below 30 minutes.

---

## 6. Success metrics (North Star)

| Metric                         | Current baseline | 6-month target       | How to measure it          |
| ------------------------------ | ---------------- | -------------------- | -------------------------- |
| Mean Time to Assistance (MTTA) | 90 minutes       | < 30 minutes         | System timestamp telemetry |
| Completed Service Requests     | 0                | 1,000 requests/month | Database logs              |

**North Star Metric:** Average time elapsed from emergency request creation to mechanic arrival on-site (Target: < 30 minutes).

---

## 7. Hypothesis risks

| Risk                                 | Probability | Impact | Experiment to validate                           |
| ------------------------------------ | ----------- | ------ | ------------------------------------------------ |
| Low mechanic adoption of the app     | Medium      | High   | Pilot onboarding with 15 local workshops         |
| Poor GPS connectivity in rural areas | High        | High   | Offline request queueing and fallback SMS alerts |

---

## 8. Out of scope (we do not solve)

- Physical repair execution or spare part sales in-app: FixGo only manages matching, geolocation, and requests.
- Advanced vehicle telemetry (OBD-II hardware diagnostics): Excluded from the core initial scope.

---

## Correlations

- Product vision → `03-product/vision.md`
- HUs that implement this solution → `04-requirements/user-stories.md`
- Detailed KPIs → `13-operations/README.md`
