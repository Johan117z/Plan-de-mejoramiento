# Product Vision — FixGo Platform

> The vision is the team's north star. All sprints, design decisions,
> and trade-offs are evaluated against this vision.
> It must be ambitious yet achievable, inspiring but specific.

---

## Vision statement

**For** stranded drivers and motorcyclists  
**who** face roadside mechanical emergencies without immediate assistance,  
**the** FixGo platform  
**is a** real-time geolocation roadside dispatching platform  
**that** instantly connects vehicle owners with nearby verified mechanics and tow operators with upfront transparent pricing,  
**unlike** traditional phone-based assistance call centers or fragmented contact lists,  
**our product** automates dispatch matching, real-time GPS tracking, and end-to-end transparent service routing.

---

## Team mission

To reduce driver stress and roadside waiting times by creating a seamless, reliable, and transparent digital ecosystem that instantly bridges stranded vehicle operators with qualified local service providers.

---

## Strategic pillars

| Pillar               | Description                                                           | Success metrics                       |
| -------------------- | --------------------------------------------------------------------- | ------------------------------------- |
| Speed & Matching     | Minimizing time-to-dispatch through efficient geolocation algorithms  | Mean Time to Dispatch (MTTD) < 3 mins |
| Reliability & Safety | Ensuring verified mechanics, accurate GPS tracking, and service trust | Service Completion Rate > 95%         |
| Transparency         | Providing clear, upfront service pricing and real-time status updates | User Satisfaction (CSAT) > 4.5/5      |

---

## High-level roadmap

| Horizon    | Period     | Objective                           | Epics / Features                                                                          | Uncertainty |
| ---------- | ---------- | ----------------------------------- | ----------------------------------------------------------------------------------------- | ----------- |
| H1 (Now)   | Q1-Q2 2026 | Validate core geolocation matching  | Real-time GPS request dispatch, basic driver/mechanic profiles, emergency alert creation  | Low         |
| H2 (Next)  | Q3 2026    | Streamline transaction flow & trust | In-app payment integration, user rating & review system, multi-vehicle profile management | Medium      |
| H3 (Later) | Q4 2026    | Enhance dispatch automation & scale | Predictive dispatching, offline SMS fallback routing, scheduled maintenance bookings      | High        |

---

## Product principles

1. **Safety and Speed First:** Every design choice must prioritize getting help to the driver as fast and safely as possible; UI interactions during emergencies must take fewer than 3 taps.
2. **Transparent Expectations:** No hidden fees or vague arrival times. Drivers must always see real-time mechanic location and upfront estimated costs.
3. **Offline Resilience:** Roadside emergencies happen in poor coverage zones; critical actions must gracefully degrade or queue via SMS/offline sync mechanisms.

---

## Product Definition of Done

**Objective:** Successfully deploy the FixGo MVP to validate rapid roadside dispatch and high driver satisfaction in urban and highway corridors.

| Key Result                                         | Baseline                    | Target               | Date       |
| -------------------------------------------------- | --------------------------- | -------------------- | ---------- |
| KR1: Average time from request to mechanic arrival | 90 minutes                  | < 30 minutes         | 2026-06-30 |
| KR2: Successful completed assistance dispatches    | 0                           | 1,000 requests/month | 2026-09-30 |
| KR3: Driver overall satisfaction score             | 2.5/5 (Phone call baseline) | > 4.5/5              | 2026-12-31 |

---

## Correlations

- Problem framing (the why) → `03-product/problem-framing.md`
- Backlog that implements the vision → `04-requirements/user-stories.md`
- KPIs in operations → `13-operations/README.md`
