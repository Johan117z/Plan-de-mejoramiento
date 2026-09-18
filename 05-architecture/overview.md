# System Architecture Overview — FixGo Platform

> **What to fill in here:** The technical snapshot of the FixGo system architecture.
> It includes the C4 system and container diagrams, microservices catalog, and core principles.

---

## 1. Adopted Architectural Style

**Style:** Microservices + Event-Driven Architecture (EDA) + Hexagonal Architecture per Service.

**Justification:** FixGo requires real-time geospatial location processing, low-latency tow truck dispatching, and high scalability for concurrent roadside emergencies. Microservices allow independent scaling of telemetry streams while keeping user identity and payment domains isolated.

**Reference ADR:** [`ADR-001-postgis-geospatial.md`](decisions/records/0001-postgis-geospatial.md)

---

## 2. C4 Diagram — System Level (Context)

```mermaid
graph TB
    subgraph "External Users"
        D[Stranded Driver]
        M[Mechanic / Tow Operator]
        A[Workshop Administrator]
    end

    subgraph "FixGo Ecosystem"
        FG[FixGo Platform]
    end

    subgraph "External Services"
        MAP[Mapbox Routing / GIS API]
        FCM[Firebase Cloud Messaging]
        PAY[Payment Gateway Engine]
    end

    D -->|Requests assistance & tracks mechanic| FG
    M -->|Receives dispatch alerts & streams location| FG
    A -->|Manages workshop orders & appointments| FG

    FG -->|Geocoding & ETAs| MAP
    FG -->|Push notifications| FCM
    FG -->|Processes transactions| PAY
graph TB
    subgraph "FixGo Platform"
        GW[API Gateway / Ingress<br/>:8080]

        IAM[fixgo-iam-service<br/>:3001]
        DISP[fixgo-dispatch-service<br/>:3002]
        LOC[fixgo-location-service<br/>:3003]
        WORK[fixgo-workshop-service<br/>:3004]
        NOTIF[fixgo-notification-service<br/>:3005]

        BUS[(Message Broker<br/>RabbitMQ / Redis PubSub)]

        DB_IAM[(IAM DB<br/>PostgreSQL)]
        DB_DISP[(Dispatch DB<br/>PostgreSQL + PostGIS)]
        DB_WORK[(Workshop DB<br/>PostgreSQL)]
        REDIS[(Telemetry Cache<br/>Redis Spatial)]
    end

    MOB[Mobile App Driver/Mechanic] --> GW
    WEB[Workshop Web Portal] --> GW

    GW -->|/api/v1/auth| IAM
    GW -->|/api/v1/dispatch| DISP
    GW -->|/ws/v1/telemetry| LOC
    GW -->|/api/v1/workshops| WORK

    IAM --> DB_IAM
    DISP --> DB_DISP
    WORK --> DB_WORK
    LOC --> REDIS

    DISP -->|AssistanceRequestedEvent| BUS
    BUS -->|Consume event| NOTIF
    BUS -->|Consume event| LOC
---
```
