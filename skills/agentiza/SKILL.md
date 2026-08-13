---
name: agentiza
description: >
  Convierte un SOP, documento de proceso, transcripción de Loom, o descripción
  de cómo haces algo a mano en un Claude Code skill modular y bien estructurado
  (SKILL.md como índice + steps/ + references/ + scripts/). Hace 3 preguntas
  guiadas, detecta huecos en el SOP, y entrega los archivos listos para
  pegar en ~/.claude/skills/<nombre>/. También sirve para MODERNIZAR skills
  monolíticos existentes que ya pasaron las 200 líneas. Use when:
  "agentiza", "agentizar", "convertir SOP en agente", "modernizar skill",
  "crear skill", "modularizar skill", "skill nuevo", "/agentiza".
---

# /agentiza — De SOP a Agente Prompt

Skill para convertir cualquier proceso documentado (SOP, Loom, notas, descripción verbal) en un **Claude Code skill modular** siguiendo la estructura:

```
mi-skill/
  SKILL.md              # índice máximo 100 líneas
  steps/<n>-<slug>.md   # un archivo por paso
  references/<tema>.md  # lo que no cambia: esquemas, tono, catálogos
  scripts/<nombre>.ts   # código ejecutable (no embebido en markdown)
```

También sirve para **modernizar skills viejos** que crecieron sin estructura.

---

## Flujo

Lee cada paso solo cuando llegues a él:

1. [Identificar input y modo](steps/1-identificar-input.md) — gate de "¿amerita un skill?", colisión de nombres, SOP nuevo vs modernizar
2. [Diseñar el skill con el usuario](steps/2-preguntas-guiadas.md) — recomendar y confirmar 5 decisiones (triggers, herramientas, pausas, ejemplo, plan)
3. [Mapear pasos del SOP](steps/3-mapear-pasos.md) — extraer y numerar los pasos reales
4. [Separar referencias y scripts](steps/4-separar-referencias-scripts.md) — qué va a `references/`, qué a `scripts/`, y qué se sanitiza
5. [Generar archivos](steps/5-generar-archivos.md) — escribir todos los `.md` y `.ts`
6. [Detectar huecos y reportar](steps/6-detectar-huecos.md) — `[FALTA: ...]` y mapeo SOP → archivos
7. [Validar e instalar](steps/7-validar-e-instalar.md) — probar que **dispare solo**, no solo con el slash
8. [Enseñar el bucle de mejora](steps/8-improvement-loop.md) — la v1 es un borrador; MAA sobre el propio skill

## Referencias

- [`references/reglas-duras.md`](references/reglas-duras.md) — las 7 reglas no negociables del patrón
- [`references/frontmatter.md`](references/frontmatter.md) — los campos que existen y cuál decide si el skill se usa
- [`references/portabilidad.md`](references/portabilidad.md) — que el skill corra en cualquier harness (Codex, Cursor, Gemini)
- [`references/composicion.md`](references/composicion.md) — skills que llaman a otros skills
- [`references/versionado.md`](references/versionado.md) — cómo se mantiene y se depreca un skill
- [`references/idioma-y-tono.md`](references/idioma-y-tono.md) — español de México, sin voseo, sin relleno
- [`references/anti-patrones.md`](references/anti-patrones.md) — qué NO hacer al estructurar un skill

## Plantillas

- [`templates/SKILL.md.tpl`](templates/SKILL.md.tpl) — template del índice
- [`templates/step.md.tpl`](templates/step.md.tpl) — template de un paso
- [`templates/reference.md.tpl`](templates/reference.md.tpl) — template de un archivo de referencia

---

## Cómo se invoca

```
/agentiza
```

Luego pega el SOP, Loom transcript, notas, o el SKILL.md monolítico que quieres modernizar. El skill arranca con las 3 preguntas guiadas y entrega los archivos.

## Salida esperada

Al final del flujo, el usuario recibe:

1. `SKILL.md` completo (índice).
2. Cada `steps/<n>-<slug>.md` como archivo separado.
3. Cada `references/<tema>.md` como archivo separado.
4. Cada `scripts/<nombre>.ts` como archivo separado (si aplica).
5. Tabla "sección del SOP original → archivo en el skill".
6. Lista de huecos detectados (`[FALTA: ...]`) que el usuario debe completar.

<!--
Fuente: patrón de skills modulares de GNB Labs, formalizado el 2026-05.
Última revisión: 2026-08-13 — se agregaron el paso 8 (bucle de mejora, MAA sobre
el skill), la referencia de portabilidad y la regla dura 7; el paso 2 pasó a
recomendar-y-confirmar con ejemplo horneado. Cambios inspirados en bm-skills
(skill-builder de Brian Casel). Antes (2026-07-26): paso 7 y refs de
frontmatter/composición/versionado.
Revisar cuando: cambien los campos de frontmatter que soporta el harness, o
cuando el límite de 100 líneas del SKILL.md deje de ser el criterio.
-->
