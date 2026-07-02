---
name: Reporte de Anomalía Operativa
about: Reportar un error o degradación en producción
title: '[ANOMALÍA] '
labels: ['bug']
assignees: []
---

## Impacto en Producción

<!-- Marcá el nivel de impacto operativo actual -->

- [ ] 🔴 **Alto** — Servicio interrumpido, clientes afectados, pérdida de transacciones
- [ ] 🟡 **Medio** — Funcionalidad degradada pero con workaround disponible
- [ ] 🟢 **Bajo** — Defecto cosmético o en funcionalidad no crítica

## Descripción de la Anomalía

<!-- Describí el comportamiento observado y cómo difiere del esperado. -->

## Pasos de Reproducción

1.
2.
3.
4.

## Logs de Cloudflare

<!-- Si la anomalía involucra Workers, D1, Durable Objects o Turnstile, adjuntá los logs relevantes de Cloudflare Dashboard o Wrangler tail. -->

```
<!-- Pegar logs aquí -->
```

## Entorno

- **Rama/versión desplegada:** [ej. main @ a1b2c3d, v1.4.2]
- **Navegador (si aplica):** [ej. Chrome 131, POS Android]
- **Tenant(s) afectado(s):** [ej. restaurante-demo, todos]
- **Zona horaria del incidente:** [ej. 2026-07-02 14:30 CLT]

## Evidencia Adicional

<!-- Screenshots, grabaciones de pantalla, respuestas de API, trazas de error. -->