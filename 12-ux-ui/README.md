# 11 — UX/UI (User Experience & Interface) - FixGo

> **¿Qué es esto?** El diseño de la experiencia de usuario: cómo se ve el sistema, cómo se navega
> y cómo se comporta desde la perspectiva del conductor y del taller mecánico.

## Por qué el diseño precede al código

Cambiar un wireframe en Figma toma 5 minutos. Cambiar el código toma horas.
Cambiar el código en producción con usuarios reales varados en carretera cuesta días y reputación.

**Diseñar primero → Implementar después.**

---

## Estructura del Módulo

- **`navigation-map.md`**: El mapa de todas las pantallas/páginas, estructura de rutas por rol y matriz de acceso.
- **`design-system.md`**: Sistema de diseño de FixGo (tokens visuales, colores corporativos, tipografía, componentes y patrones de error).
- **`wireframes.md`**: Representación estructural de las pantallas críticas (Solicitud de Auxilio y Rastreo GPS).

---

## Matriz de Resumen de Pantallas y Roles

| Pantalla / Ruta   | Conductor | Taller / Mecánico | Admin | Servicio Backend    |
| ----------------- | --------- | ----------------- | ----- | ------------------- |
| `/` (Landing)     | ✅        | ✅                | ✅    | —                   |
| `/auth/login`     | ✅        | ✅                | ✅    | auth-service        |
| `/dashboard`      | ✅        | ✅                | ✅    | assistance-service  |
| `/assistance/new` | ✅        | ❌                | ❌    | assistance-service  |
| `/assistance/:id` | ✅        | ✅                | ✅    | geolocation-service |
| `/workshops`      | ✅        | ❌                | ✅    | workshop-service    |
| `/admin`          | ❌        | ❌                | ✅    | auth-service        |

---

## Preguntas Clave Resueltas

- **¿Cuántas pantallas tiene el sistema?** 8 pantallas principales divididas entre área pública, conductores, talleres y administración.
- **¿Cómo navega cada usuario?** A través de un flujo optimizado en tres pasos: Geolocalización $\rightarrow$ Selección de Servicio $\rightarrow$ Rastreo GPS en tiempo real.
- **¿Cuál es el lenguaje visual?** Colores de alto contraste (**Azul Mecánico `#0052CC`** para acciones clave y **Amarillo Emergencia `#FFAB00`** para alertas y auxilio).

---

## Correlaciones

| Esta sección se alimenta de...                         | Y alimenta a...                                 |
| ------------------------------------------------------ | ----------------------------------------------- |
| `04-requirements/user-stories.md` → Qué flujos existen | Desarrollo Frontend (React / Flutter)           |
| `02-domain/entities-and-rules.md` → Qué datos mostrar  | Campos en formularios y tarjetas de taller      |
| `07-api/` → Endpoints que consume la interfaz          | Integración de servicios y mapas en tiempo real |
