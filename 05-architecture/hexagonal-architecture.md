# Hexagonal Architecture (Ports & Adapters) — FixGo Platform

> Hexagonal architecture organizes each microservice in the FixGo platform so that the **business domain (Dispatch, Geolocation, IAM, Payments) is completely independent** of external frameworks, databases, and third-party APIs (e.g., Mapbox, FCM, PostgreSQL).

---

## The Problem It Solves for FixGo

---

## Service Directory Structure (FixGo Pattern)

---

## FixGo Ports & Adapters Examples

### 1. Driving Port (Input Port)

Defines how external controllers initiate roadside assistance:

```typescript
// src/domain/dispatch/ports/in/RequestAssistancePort.ts
import { RequestAssistanceCommand } from '@application/dispatch/dtos/RequestAssistanceCommand';
import { AssistanceResponseDto } from '@application/dispatch/dtos/AssistanceResponseDto';

export interface RequestAssistancePort {
  execute(command: RequestAssistanceCommand): Promise<AssistanceResponseDto>;
}
// src/domain/dispatch/ports/out/AssistanceRepositoryPort.ts
import { AssistanceRequest } from '@domain/dispatch/AssistanceRequest';
import { AssistanceRequestId } from '@domain/dispatch/AssistanceRequestId';
import { GeoCoordinates } from '@domain/shared/value-objects/GeoCoordinates';

export interface AssistanceRepositoryPort {
  save(request: AssistanceRequest): Promise<void>;
  findById(id: AssistanceRequestId): Promise<AssistanceRequest null |>;
  findNearbyAvailableTows(location: GeoCoordinates, radiusKm: number): Promise<string[]>;
}
// src/infrastructure/adapters/in/http/DispatchController.ts
import { RequestAssistancePort } from '@domain/dispatch/ports/in/RequestAssistancePort';

@Controller('/api/v1/dispatch')
export class DispatchController {
  constructor(
    private readonly requestAssistancePort: RequestAssistancePort,
  ) {}

  @Post('/request')
  async handleRequest(@Body() dto: RequestAssistanceHttpRequest): Promise<AssistanceResponseDto> {
    const command = new RequestAssistanceCommand(
      dto.driverId,
      dto.latitude,
      dto.longitude,
      dto.serviceType,
    );
    return await this.requestAssistancePort.execute(command);
  }
}
// src/infrastructure/adapters/out/persistence/PostGISAssistanceRepository.ts
import { AssistanceRepositoryPort } from '@domain/dispatch/ports/out/AssistanceRepositoryPort';
import { AssistanceRequest } from '@domain/dispatch/AssistanceRequest';
import { GeoCoordinates } from '@domain/shared/value-objects/GeoCoordinates';

export class PostGISAssistanceRepository implements AssistanceRepositoryPort {
  constructor(private readonly dbConnection: DatabaseClient) {}

  async save(request: AssistanceRequest): Promise<void> {
    const query = `
      INSERT INTO assistance_requests (id, driver_id, location, status, created_at)
      VALUES ($1, $2, ST_SetSRID(ST_MakePoint($3, $4), 4326), $5, $6)
    `;
    await this.dbConnection.query(query, [
      request.id.value,
      request.driverId,
      request.location.longitude,
      request.location.latitude,
      request.status,
      request.createdAt,
    ]);
  }

  async findNearbyAvailableTows(location: GeoCoordinates, radiusKm: number): Promise<string[]> {
    const query = `
      SELECT operator_id FROM active_tows
      WHERE ST_DWithin(
        current_location,
        ST_SetSRID(ST_MakePoint($1, $2), 4326)::geography,
        $3 * 1000
      ) AND is_available = true
    `;
    const result = await this.dbConnection.query(query, [location.longitude, location.latitude, radiusKm]);
    return result.rows.map(row => row.operator_id);
  }
}
// src/application/dispatch/RequestAssistanceUseCase.ts
import { RequestAssistancePort } from '@domain/dispatch/ports/in/RequestAssistancePort';
import { AssistanceRepositoryPort } from '@domain/dispatch/ports/out/AssistanceRepositoryPort';
import { NotificationPort } from '@domain/dispatch/ports/out/NotificationPort';

export class RequestAssistanceUseCase implements RequestAssistancePort {
  constructor(
    private readonly assistanceRepo: AssistanceRepositoryPort,
    private readonly notificationPort: NotificationPort,
  ) {}

  async execute(command: RequestAssistanceCommand): Promise<AssistanceResponseDto> {
    // 1. Create domain aggregate (Invariants validated inside domain)
    const request = AssistanceRequest.create(
      command.driverId,
      command.latitude,
      command.longitude,
      command.serviceType
    );

    // 2. Query nearby operators via driven port
    const nearbyOperatorIds = await this.assistanceRepo.findNearbyAvailableTows(
      request.location,
      10 // 10 km radius
    );

    // 3. Persist entity
    await this.assistanceRepo.save(request);

    // 4. Dispatch external alerts
    await this.notificationPort.notifyOperators(nearbyOperatorIds, request.id.value);

    return new AssistanceResponseDto(request.id.value, request.status, nearbyOperatorIds.length);
  }
}
```
