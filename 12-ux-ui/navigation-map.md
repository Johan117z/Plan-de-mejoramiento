# Navigation Map — FixGo

> Define la estructura de pantallas del sistema, cómo se conectan entre sí y qué rutas existen. 
> Es la referencia de conexión entre el frontend y los endpoints backend del proyecto.

---

## Frontend route structure
---

## Screen map

| Screen | Route | Component | Minimum role | Backend service |
|--------|-------|-----------|--------------|----------------|
| Home | `/` | `HomePage` | Public | — |
| Login | `/auth/login` | `LoginPage` | Public | auth-service |
| Register | `/auth/register` | `RegisterPage` | Public | auth-service |
| Dashboard | `/dashboard` | `DashboardPage` | USER | assistance-service |
| Assistance Request | `/assistance/new` | `AssistanceFormPage` | USER | assistance-service |
| Live Tracking | `/assistance/:id` | `TrackingDetailPage` | USER | geolocation-service |
| Workshop Directory | `/workshops` | `WorkshopListPage` | USER | workshop-service |
| Admin Panel | `/admin` | `AdminDashboard` | ADMIN | auth-service |

---

## Main user flows

### Flow 1 — Solicitud de Asistencia Vehicular y Rastreo
**Related HUs:** HU-ASSIST-001, HU-ASSIST-002, HU-GEO-001

### Flow 2 — Autenticación
**Related HUs:** HU-AUTH-001, HU-AUTH-002

---

## Navigation rules

| Rule | Description |
|------|-------------|
| Authentication | Las rutas bajo `/dashboard`, `/assistance`, `/workshops`, `/admin` redirigen a `/auth/login` si no hay sesión activa |
| Authorization | Las rutas bajo `/admin` redirigen a `/dashboard` si el usuario no cuenta con el rol ADMIN |
| 404 | Rutas no definidas muestran la pantalla 404 con botón de retorno al dashboard |
| Confirmation | Acciones destructivas (cancelar asistencia, eliminar cuenta) requieren diálogo de confirmación previo |

---

## Correlations

- Design system → `11-ux-ui/design-system.md`
- Wireframes → `11-ux-ui/wireframes.md`
- Frontend API contracts → `07-api/contracts/openapi/`
- Roles and permissions → `00-governance/security-policy.md`