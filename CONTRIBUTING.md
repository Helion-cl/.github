# Contribuyendo a Helion OS

Gracias por tu interés en contribuir a Helion OS. Este documento define el flujo de trabajo, estándares de calidad y convenciones que todo contribuidor debe seguir.

---

## Configuración Local

Helion OS corre sobre **Cloudflare Workers** con **D1** como base de datos. Para desarrollar localmente necesitás emular ese entorno.

### Requisitos previos

- Node.js 20+
- npm 10+
- Cuenta de Cloudflare (free tier suficiente)

### Instalación

```bash
# Clonar el repositorio
git clone https://github.com/Helion-cl/helion-os.git
cd helion-os

# Instalar dependencias del frontend
cd frontend && npm install && cd ..

# Instalar dependencias del backend
cd backend && npm install && cd ..
```

### Entorno local con Wrangler

```bash
# Iniciar D1 local y el worker de API
cd backend
npx wrangler d1 execute helion-db --local --file=./prisma/migrations/init.sql
npx wrangler dev

# En otra terminal, iniciar el frontend SvelteKit
cd frontend
npm run dev
```

El frontend corre en `http://localhost:5173` y el backend en `http://localhost:8787`. Las variables de entorno se configuran en `backend/.dev.vars` (ver `.env.example`).

---

## Convenciones de Commits

Este repositorio sigue el estándar **Conventional Commits**. Cada mensaje de commit debe tener la estructura:

```
<tipo>(<scope> opcional): <descripción breve>

<cuerpo opcional>
```

Tipos permitidos:

| Tipo | Uso |
|------|-----|
| `feat` | Nueva funcionalidad |
| `fix` | Corrección de bug |
| `chore` | Tareas de mantenimiento, configuración, dependencias |
| `docs` | Cambios en documentación |
| `refactor` | Reestructuración de código sin cambiar comportamiento |
| `test` | Añadir o modificar tests |
| `perf` | Mejora de rendimiento |
| `style` | Cambios de formato, linting (no de lógica) |
| `ci` | Cambios en CI/CD |
| `revert` | Reversión de un commit anterior |

**Ejemplos:**

```
feat(d1): agregar índice compuesto para búsquedas por tenant + fecha
fix(auth): corregir validación de token expirado en refresh
chore(deps): actualizar @sveltejs/kit a 2.8.0
```

---

## Estilo de Código

### TypeScript

- **`strict: true`** en `tsconfig.json`. Sin excepciones.
- Tipos explícitos en todas las firmas de funciones exportadas.
- Prohibido `any`. Usar `unknown` y type narrowing si el tipo es realmente desconocido.
- Preferir `interface` sobre `type` para shapes de objeto.
- `import type` para imports que solo se usan en tiempo de compilación.

### Formateo y Linting

El proyecto usa **Prettier** y **ESLint** con configuraciones compartidas en la raíz.

```bash
# Formatear todo el proyecto
npm run format

# Linting
npm run lint

# Linting con auto-fix
npm run lint:fix
```

El CI rechazará cualquier PR que no pase `npm run lint` y `npm run format:check`.

### Svelte

- Componentes con `<script lang="ts">`.
- Usar runas de Svelte 5 (`$state`, `$derived`, `$effect`, `$props`). Prohibido `onMount`, `on:click`, `<slot>`.
- Estilos exclusivamente con Tailwind CSS. No archivos `.css` nuevos.
- Componentes UI vía `npx shadcn-svelte@latest add <componente>`. No reinventar componentes estándar.

### Fastify / Backend

- Handlers puros: sin acceso directo a base de datos. Usar repositories.
- Validación de entrada con Zod en todas las rutas.
- Errores custom con `code` y `statusCode`.
- Schemas de ruta co-ubicados en archivos `schemas.ts`.

---

## Flujo de Trabajo

1. **Abrir un issue** describiendo el bug o feature antes de codificar.
2. **Crear una rama** desde `main` con formato `tipo/descripcion` (ej. `feat/d1-indexes`, `fix/auth-token-refresh`).
3. **Desarrollar** siguiendo las convenciones de este documento.
4. **Abrir un Pull Request** usando la plantilla provista. El PR debe pasar CI (lint, type-check, tests).
5. **Solicitar review** de al menos un maintainer.
6. **Merge** mediante squash merge a `main`.

---

## Tests

- Backend: tests de integración con `app.inject()` (Vitest). No se levanta servidor real.
- Frontend: tests unitarios con Vitest + Testing Library.
- Cobertura mínima: 80% en servicios y repositories del backend.

```bash
# Correr todos los tests
npm test

# Solo backend
cd backend && npx vitest run

# Solo frontend
cd frontend && npx vitest run
```

---

> ¿Dudas? Abrí un issue con label `question` o escribinos en el canal #eng de nuestro Slack.