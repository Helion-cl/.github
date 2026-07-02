# Ingeniería en Helion OS

> **Helion OS es un proyecto de código cerrado. No aceptamos Pull Requests de externos.** Este documento describe nuestros estándares operativos internos y funciona como referencia pública de nuestras prácticas de ingeniería.

---

## Configuración del Entorno Local

Helion OS corre sobre **Cloudflare Workers** con **D1** como base de datos distribuida. El entorno de desarrollo emula esta infraestructura mediante **Wrangler**.

### Requisitos previos

- Node.js 20+
- npm 10+
- Cuenta de Cloudflare con acceso al equipo Helion

### Puesta en marcha

```bash
# Clonar el repositorio (acceso restringido a miembros del equipo)
git clone https://github.com/Helion-cl/helion-os.git
cd helion-os

# Instalar dependencias
cd frontend && npm install && cd ..
cd backend && npm install && cd ..
```

### Emulación Edge con Wrangler

```bash
# Iniciar D1 local y el worker de API
cd backend
npx wrangler d1 execute helion-db --local --file=./prisma/migrations/init.sql
npx wrangler dev

# En otra terminal, iniciar el frontend SvelteKit
cd frontend
npm run dev
```

El frontend corre en `http://localhost:5173` y el backend en `http://localhost:8787`. Las variables de entorno se configuran en `backend/.dev.vars`.

---

## Convenciones de Commits

Seguimos el estándar **Conventional Commits**:

```
<tipo>(<scope> opcional): <descripción breve>

<cuerpo opcional>
```

| Tipo | Uso |
|------|-----|
| `feat` | Nueva funcionalidad |
| `fix` | Corrección de bug |
| `chore` | Mantenimiento, configuración, dependencias |
| `docs` | Cambios en documentación |
| `refactor` | Reestructuración sin cambio de comportamiento |
| `test` | Tests |
| `perf` | Mejora de rendimiento |
| `ci` | CI/CD |

**Ejemplos:**

```
feat(d1): agregar índice compuesto para búsquedas por tenant + fecha
fix(auth): corregir validación de token expirado en refresh
chore(deps): actualizar @sveltejs/kit a 2.8.0
```

---

## Gestión de Conocimiento

La documentación profunda, las investigaciones técnicas y la toma de decisiones arquitectónicas viven en nuestro **Obsidian Vault**. Este grafo de conocimiento es la fuente de verdad del proyecto y es orquestado mediante **MCP (Model Context Protocol)** por nuestros agentes de IA.

### Flujo de decisión

1. **Investigar** — Toda exploración técnica se documenta como nota en el Vault.
2. **Conectar** — Se establecen enlaces bidireccionales con módulos, APIs y decisiones relacionadas.
3. **Decidir** — Las conclusiones se formalizan como ADRs y se publican en `docs/architecture-decisions/`.
4. **Ejecutar** — Los agentes MCP extraen contexto del Vault para asistir en la implementación.

### Sincronización Vault ↔ Código

- Cada cambio significativo en el código debe reflejarse en la nota de módulo correspondiente del Vault.
- Los ADRs públicos en este repositorio son un subconjunto curado del conocimiento interno.
- El detalle de implementación y la propiedad intelectual permanecen en el Vault privado.

---

## Stack y Estándares

### TypeScript

- **`strict: true`** en `tsconfig.json`. Sin excepciones.
- Prohibido `any`. Usar `unknown` y type narrowing.
- Preferir `interface` sobre `type` para shapes de objeto.
- `import type` para imports de solo compilación.

### Svelte (Frontend)

- Runas de Svelte 5 (`$state`, `$derived`, `$effect`, `$props`). Prohibido `onMount`, `on:click`, `<slot>`.
- Estilos exclusivamente con Tailwind CSS.
- Componentes UI vía `npx shadcn-svelte@latest add`.

### Fastify (Backend)

- Handlers puros con repositories. Sin queries crudas en handlers.
- Validación de entrada con Zod en todas las rutas.
- Errores custom con `code` y `statusCode`.

### Formateo y Linting

```bash
npm run format        # Prettier
npm run lint          # ESLint
npm run lint:fix      # ESLint auto-fix
```

El CI rechaza cualquier cambio que no pase lint y format check.

---

## Flujo de Trabajo Interno

1. **Abrir un issue** usando las plantillas de este repositorio.
2. **Crear una rama** desde `main` con formato `tipo/descripcion`.
3. **Desarrollar** siguiendo las convenciones de este documento.
4. **Documentar en el Vault** los cambios que afecten arquitectura, APIs o decisiones de diseño.
5. **Abrir un Pull Request** usando la plantilla provista.
6. **Merge** mediante squash merge a `main` tras revisión de al menos un miembro del equipo.

---

> El rigor en la documentación es tan importante como el rigor en el código. Un sistema sin memoria está condenado a repetir sus errores.