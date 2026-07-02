# 1. Uso de ADRs para documentar la arquitectura

**Fecha:** 2026-07-02

**Estado:** Aceptado

---

## Contexto

Helion OS es una plataforma en evolución activa con decisiones arquitectónicas de alto impacto — desde la elección de **Cloudflare D1** como base de datos distribuida hasta el uso de **SvelteKit** para el frontend y **Durable Objects** para estado compartido en el Edge. Estas decisiones no son triviales y sus fundamentos deben perdurar más allá de la memoria del equipo que las tomó.

Sin un registro formal, el razonamiento detrás de cada elección se diluye en hilos de chat, commits dispersos y conocimiento tribal. Esto genera:

- **Desalineamiento** cuando nuevos ingenieros cuestionan decisiones sin conocer el contexto original.
- **Riesgo de regresión** al proponer cambios que ya fueron evaluados y descartados.
- **Fricción en el onboarding**, ya que no hay un mapa claro de por qué la arquitectura tiene la forma que tiene.

Necesitamos un mecanismo sistemático, liviano y versionable para capturar el contexto, las opciones consideradas y las consecuencias de cada decisión arquitectónica significativa.

---

## Decisión

Adoptaremos el formato de **Architecture Decision Records (ADR)**, siguiendo la convención establecida por Michael Nygard y refinada por la comunidad de ThoughtWorks.

Cada ADR:

1. **Es un archivo Markdown** en `docs/architecture-decisions/`, numerado secuencialmente (`0001-`, `0002-`, ...).
2. **Tiene un título que describe la decisión**, no el problema.
3. **Contiene secciones obligatorias:** Título, Fecha, Estado, Contexto, Decisión, Consecuencias.
4. **Es inmutable una vez mergeado.** Si una decisión cambia, se crea un nuevo ADR que referencia y reemplaza al anterior (cambiando el estado del anterior a "Reemplazado").
5. **Se versiona junto con el código**, garantizando que el contexto arquitectónico viaje con el repositorio.

Estados posibles para un ADR:

| Estado | Significado |
|--------|-------------|
| Propuesto | En discusión, aún no aceptado |
| Aceptado | Aprobado y vinculante |
| Reemplazado | Sustituido por un ADR más reciente |
| Obsoleto | La decisión ya no aplica (ej. tecnología deprecada) |

---

## Consecuencias

### Positivas

- **Onboarding acelerado:** Un nuevo ingeniero puede leer los ADRs en secuencia y entender la evolución arquitectónica del proyecto en horas, no semanas.
- **Decisiones auditables:** Cada elección técnica tiene un documento que responde _qué_, _por qué_ y _qué alternativas se descartaron_.
- **Prevención de ciclos:** Cuando alguien propone "¿y si usamos PostgreSQL?", el ADR correspondiente documenta exactamente por qué se eligió D1 y bajo qué condiciones se reconsideraría.
- **Cultura de ingeniería:** Obliga a estructurar la investigación antes de codificar, elevando el nivel de rigor técnico del equipo.

### Negativas

- **Carga inicial:** Requiere disciplina para escribir ADRs en el momento de la decisión, no semanas después.
- **Mantenimiento:** Los ADRs obsoletos deben marcarse explícitamente; de lo contrario, generan confusión.
- **No reemplaza la documentación viva:** Un ADR documenta la decisión puntual, no el estado actual del sistema. Para eso existe el Vault (`Vault/KvernicolaOS/`).

---

## Referencias

- [Documenting Architecture Decisions — Michael Nygard](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions)
- [ADR GitHub Organization — Plantillas y ejemplos](https://adr.github.io/)