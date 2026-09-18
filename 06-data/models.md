# Data Models per Service — FixGo

> **What to fill in here:** The data schema for each microservice in the FixGo platform.
> Each service has its own section and database.
> Schema changes are always executed via versioned migrations (Flyway).

---

## Data modeling principles

### 1. Database per Service (Mandatory)

No service directly accesses another service's database. Communication between services is strictly done via REST APIs or asynchronous domain events (RabbitMQ).

### 2. Standard audit fields

All relational tables include the following mandatory columns:

```sql
id          UUID        PRIMARY KEY DEFAULT gen_random_uuid(),
created_at  TIMESTAMPTZ NOT NULL DEFAULT NOW(),
updated_at  TIMESTAMPTZ NOT NULL DEFAULT NOW(),
deleted_at  TIMESTAMPTZ -- NULL = active (soft delete)
erDiagram
    ASSISTANCE_REQUESTS ||--o{ DISPATCH_ASSIGNMENTS : generates
    ASSISTANCE_REQUESTS {
        uuid id PK
        uuid driver_id
        uuid vehicle_id
        varchar service_type
        geography location_point
        varchar address_text
        varchar status
        timestamptz created_at
    }
    DISPATCH_ASSIGNMENTS {
        uuid id PK
        uuid request_id FK
        uuid tow_operator_id
        varchar status
        integer eta_minutes
        timestamptz dispatched_at
    }
CREATE EXTENSION IF NOT EXISTS "postgis";

CREATE TABLE assistance_requests (
    id             UUID         PRIMARY KEY DEFAULT gen_random_uuid(),
    driver_id      UUID         NOT NULL,
    vehicle_id     UUID         NOT NULL,
    service_type   VARCHAR(50)  NOT NULL
                   CHECK (service_type IN ('TOW_TRUCK', 'BATTERY_JUMP', 'TIRE_CHANGE', 'FUEL_DELIVERY')),
    location_point GEOGRAPHY(POINT, 4326) NOT NULL,
    address_text   TEXT         NOT NULL,
    status         VARCHAR(50)  NOT NULL DEFAULT 'PENDING'
                   CHECK (status IN ('PENDING', 'DISPATCHED', 'IN_PROGRESS', 'COMPLETED', 'CANCELLED')),

    -- Audit
    created_at     TIMESTAMPTZ  NOT NULL DEFAULT NOW(),
    updated_at     TIMESTAMPTZ  NOT NULL DEFAULT NOW(),
    deleted_at     TIMESTAMPTZ
);

-- Spatial and Audit Indexes
CREATE INDEX idx_assistance_requests_location ON assistance_requests USING GIST (location_point);
CREATE INDEX idx_assistance_requests_driver ON assistance_requests (driver_id);
CREATE INDEX idx_assistance_requests_status ON assistance_requests (status) WHERE deleted_at IS NULL;
CREATE TABLE service_orders (
    id             UUID           PRIMARY KEY DEFAULT gen_random_uuid(),
    workshop_id    UUID           NOT NULL,
    vehicle_id     UUID           NOT NULL,
    mechanic_id    UUID,
    diagnosis_notes TEXT,
    total_cost     NUMERIC(12, 2) DEFAULT 0.00,
    status         VARCHAR(50)    NOT NULL DEFAULT 'RECEIVED'
                   CHECK (status IN ('RECEIVED', 'DIAGNOSING', 'IN_REPAIR', 'READY_FOR_PICKUP', 'COMPLETED')),

    -- Audit
    created_at     TIMESTAMPTZ    NOT NULL DEFAULT NOW(),
    updated_at     TIMESTAMPTZ    NOT NULL DEFAULT NOW(),
    deleted_at     TIMESTAMPTZ
);

CREATE INDEX idx_service_orders_workshop ON service_orders (workshop_id);
CREATE INDEX idx_service_orders_status ON service_orders (status) WHERE deleted_at IS NULL;
V{version_number}__{snake_case_description}.sql

Examples:
  V001__create_assistance_requests_table.sql
  V002__add_spatial_index_to_assistance.sql
  V003__create_service_orders_table.sql
-- Safe migration example: Adding nullable field or column with default
ALTER TABLE service_orders ADD COLUMN priority VARCHAR(20) NOT NULL DEFAULT 'REGULAR';
```
