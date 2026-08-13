# Changelog

Formato de versión: `YYYY.M.D` (fecha de publicación).

## 2026.8.13

Primera publicación pública. Incluye las mejoras derivadas de comparar el patrón
con [bm-skills](https://github.com/buildermethods/bm-skills) (skill-builder de Brian Casel):

- **Portabilidad como regla dura (#7):** el core es markdown puro; el skill corre
  en Codex, Cursor y Gemini, no solo en Claude Code. Nueva referencia `portabilidad.md`.
- **Paso 2 recomendar-y-confirmar:** en vez de un cuestionario en frío, lidera con
  la recomendación y el usuario confirma en una palabra.
- **Ejemplo real horneado:** decisión de flujo para hornear un input→output real
  (sanitizado de PII) que alinea el output.
- **Paso 8 — bucle de mejora:** MAA sobre el propio skill; la v1 es un borrador,
  se afina corriéndolo.
