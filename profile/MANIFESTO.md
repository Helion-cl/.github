# The Helion Way: Nuestro Manifiesto de Ingeniería

Helion es un producto propietario construido con rigor de código abierto. Publicamos nuestros principios para dar transparencia a nuestros clientes y establecer el estándar para futuros miembros del equipo.

La industria gastronómica ha operado por décadas sobre sistemas legacy que colapsan en hora punta. Nosotros construimos para el restaurante del futuro: distribuido, resiliente y gobernado por decisiones de arquitectura documentadas y auditables.

---

### La Latencia es el Enemigo

Cada milisegundo cuenta cuando una cocina está en pleno servicio. Nuestra arquitectura se despliega en el **Edge** (Cloudflare Workers), con datos replicados globalmente mediante **Cloudflare D1** y estado compartido con **Durable Objects**. Las lecturas se resuelven desde el datacenter más cercano al restaurante, no desde un servidor central a miles de kilómetros.

No toleramos queries N+1, no cacheamos con suerte, no hacemos polling donde cabe un WebSocket. Cada decisión de infraestructura se mide contra el reloj del comensal que espera su plato.

---

### Fricción Cero en la Operación

Un mesero no debería pensar en la interfaz; debería pensar en el cliente. Un chef no debería luchar con la pantalla; debería cocinar.

Diseñamos cada pantalla con **SvelteKit** bajo principios de:

- **Toques mínimos** para completar las operaciones más frecuentes.
- **Navegación predictiva** que anticipa el siguiente paso lógico del flujo de trabajo.
- **Estados offline-first**: la operación no se detiene si la conexión fluctúa.
- **Feedback instantáneo** en cada acción crítica.

Probamos nuestras interfaces en condiciones reales: ruido ambiente, manos húmedas, prisas. Si una pantalla requiere más de tres toques para facturar una mesa, hay que rediseñarla.

---

### Cerebro Centralizado, Ejecución Distribuida

Toda decisión arquitectónica nace en nuestro **Obsidian Vault** — un grafo de conocimiento vivo que contiene el contexto, las investigaciones y los fundamentos de cada elección técnica. Desde allí, nuestros agentes de IA orquestan la ejecución mediante **MCP (Model Context Protocol)**, distribuyendo decisiones de diseño a la infraestructura de forma trazable y automatizada.

El Vault es la fuente de verdad. El código es la materialización de ese conocimiento. Nada se construye sin antes haber sido razonado, documentado y conectado en el grafo.

---

> *Este manifiesto es vinculante. Todo RFC y ADR se evalúa contra estos tres principios. Si una decisión de ingeniería no puede justificarse bajo al menos uno de ellos, no entra.*