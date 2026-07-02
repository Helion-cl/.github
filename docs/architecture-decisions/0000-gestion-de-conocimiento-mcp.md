# 0. Gestión de Decisiones mediante Obsidian y MCP

**Fecha:** 2026-07-02

**Estado:** Activo

---

## Contexto

Helion se construye sobre un stack tecnológico no trivial — **SvelteKit** como framework frontend, **Cloudflare Workers** y **D1** como plataforma Edge de ejecución y datos, **Durable Objects** para estado compartido, y **Turnstile** para protección anti-bot. Cada una de estas elecciones responde a un análisis profundo de trade-offs que debe perdurar más allá de la memoria del equipo.

Sin un registro formal, el razonamiento arquitectónico se diluye en:

- **Hilos de chat** que nadie re-lee.
- **Commits dispersos** sin contexto de por qué se tomó una dirección y no otra.
- **Conocimiento tribal** que se pierde cuando un miembro del equipo rota.

Al mismo tiempo, Helion es un producto **propietario y de código cerrado**. Exponer el detalle completo de nuestras decisiones de diseño en un repositorio público comprometería propiedad intelectual. Necesitamos un mecanismo que:

1. Mantenga el historial de diseño **visible al público** como vitrina de rigor ingenieril.
2. **Proteja la propiedad intelectual** manteniendo el detalle de implementación en un sistema privado.
3. Permita a nuestros **agentes de IA** acceder al contexto completo para asistir en el desarrollo.

---

## Decisión

Adoptaremos un modelo de **doble capa de conocimiento**:

### Capa 1: Obsidian Vault (Privado, Fuente de Verdad)

El **Obsidian Vault** (`Vault/`) es el cerebro del proyecto. Contiene:

- Notas de investigación técnica con el detalle completo de cada decisión.
- Grafos de dependencia entre módulos, APIs y modelos de datos.
- Bitácora de sesiones de diseño y decisiones de arquitectura.
- Contexto histórico de por qué se descartaron ciertas alternativas.

El Vault es privado y solo accesible para el equipo interno y los agentes de IA autorizados.

### Capa 2: ADRs Públicos (Vitrina Curada)

La carpeta `docs/architecture-decisions/` en este repositorio contiene un **subconjunto curado** de las decisiones del Vault, formateado como **Architecture Decision Records (ADRs)** siguiendo la convención de Michael Nygard.

Cada ADR público:

1. **Expone el qué y el por qué** de la decisión, sin revelar detalles de implementación propietaria.
2. **Referencia la nota del Vault** que contiene la investigación completa (visible solo para el equipo).
3. **Es inmutable una vez publicado.** Si una decisión cambia, se crea un nuevo ADR que referencia y reemplaza al anterior.

### Capa 3: Orquestación MCP

Nuestros agentes de IA utilizan **MCP (Model Context Protocol)** para:

- **Extraer** decisiones de diseño del Vault durante la implementación.
- **Publicar** ADRs curados en este repositorio cuando una decisión alcanza estado estable.
- **Conectar** issues, PRs y commits con las notas del Vault que los fundamentan.

Esto garantiza que cada línea de código tenga una traza hasta la decisión de diseño que la originó.

---

## Consecuencias

### Positivas

- **Transparencia sin exposición:** Clientes y futuros ingenieros ven el rigor de nuestro proceso sin acceder a propiedad intelectual.
- **Onboarding acelerado:** Un nuevo miembro del equipo lee los ADRs públicos para el panorama general y accede al Vault para el detalle profundo.
- **IA contextualizada:** Los agentes MCP siempre operan con el contexto completo del Vault, no solo con el código.
- **Auditabilidad:** Cada decisión técnica tiene un documento público que responde qué y por qué.

### Negativas

- **Disciplina de curación:** Publicar un ADR requiere destilar la investigación del Vault sin exponer secretos. Esto toma tiempo.
- **Doble mantenimiento:** El Vault y los ADRs deben mantenerse sincronizados. Si el equipo no actualiza ambos, divergen.
- **No apto para decisiones triviales:** Solo las decisiones con impacto arquitectónico o de producto justifican el ciclo Vault → MCP → ADR público.

---

## Referencias

- [Documenting Architecture Decisions — Michael Nygard](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions)
- [ADR GitHub Organization](https://adr.github.io/)
- [Model Context Protocol — Anthropic](https://modelcontextprotocol.io/)