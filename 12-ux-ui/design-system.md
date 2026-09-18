# Design System — FixGo

> El sistema de diseño es el lenguaje visual compartido entre diseño y desarrollo.
> Previene inconsistencias, acelera el desarrollo y reduce el retrabajo.
> **Regla:** Antes de crear un nuevo componente, verifica aquí si ya existe.

---

## Design tokens

Los tokens son las variables del sistema de diseño. Cambiar un token actualiza todo el sistema.

### Colors

```css
/* Base palette — FixGo Identity */
--color-primary-50: #e6eefa; /* Azul claro fondo */
--color-primary-100: #b3cef5;
--color-primary-500: #0052cc; /* Azul Mecánico — Principal */
--color-primary-900: #001f4d; /* Azul Oscuro — Navbars */

--color-secondary-500: #ffab00; /* Amarillo Emergencia — Botones de auxilio/alertas */
--color-neutral-50: #f4f5f7; /* Gris Claro — Fondo de app */
--color-neutral-900: #172b4d; /* Gris Oscuro — Texto y encabezados */

/* Semantic colors */
--color-success: #36b37e; /* Verde — Servicio confirmado/Completado */
--color-warning: #ffab00; /* Amarillo — Grúa en camino/Pendiente */
--color-error: #ff5630; /* Rojo — Emergencia cancelada/Error */
--color-info: #0065ff; /* Azul — Información vial */

/* Text */
--color-text-primary: #172b4d;
--color-text-secondary: #5e6c84;
--color-text-disabled: #a5adba;

/* Backgrounds */
--color-bg-page: #f4f5f7;
--color-bg-card: #ffffff;
--color-bg-overlay: rgba(9, 30, 66, 0.54);
/* Families */
--font-family-sans: "Inter", system-ui, -apple-system, sans-serif;
--font-family-mono: "JetBrains Mono", monospace;

/* Sizes (modular scale 1.25) */
--font-size-xs: 0.75rem; /* 12px */
--font-size-sm: 0.875rem; /* 14px */
--font-size-base: 1rem; /* 16px */
--font-size-lg: 1.25rem; /* 20px */
--font-size-xl: 1.563rem; /* 25px */
--font-size-2xl: 1.953rem; /* 31px */
--font-size-3xl: 2.441rem; /* 39px */

/* Weights */
--font-weight-regular: 400;
--font-weight-medium: 500;
--font-weight-bold: 700;

/* Line height */
--line-height-tight: 1.2;
--line-height-normal: 1.5;
--line-height-loose: 1.8;
/* 4px system */
--space-1: 0.25rem; /* 4px */
--space-2: 0.5rem; /* 8px */
--space-3: 0.75rem; /* 12px */
--space-4: 1rem; /* 16px */
--space-6: 1.5rem; /* 24px */
--space-8: 2rem; /* 32px */
--space-12: 3rem; /* 48px */
--space-16: 4rem; /* 64px */
/* Border radius */
--radius-sm: 4px;
--radius-md: 8px;
--radius-lg: 16px;
--radius-full: 9999px; /* Pill */

/* Shadows */
--shadow-sm: 0 1px 2px rgba(9, 30, 66, 0.05);
--shadow-md: 0 4px 6px rgba(9, 30, 66, 0.1);
--shadow-lg: 0 10px 15px rgba(9, 30, 66, 0.15);
```
