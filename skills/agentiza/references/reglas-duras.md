# Reglas duras del patrón

Las 7 reglas no negociables al estructurar un skill. Vienen de la práctica con `naira`, `assessment` y otros skills propios.

## 1. SKILL.md máximo 100 líneas

`SKILL.md` es índice, no contenido. Si pasa de 100 líneas, hay material que debe moverse a `steps/` o `references/`.

Estructura del SKILL.md:

```
---
name: <nombre>
description: <una oración + Use when: "...", "...", "/skill">
---

# /<nombre> — <título corto>

<2-3 oraciones de qué hace>

## Flujo

1. [Paso 1](steps/1-...md) — <título>
2. [Paso 2](steps/2-...md) — <título>
...

## Referencias

- [`references/...md`](references/...md) — <para qué sirve>
...

## Scripts (si aplica)

- [`scripts/...ts`](scripts/...ts) — <para qué sirve>
```

## 2. El `description` decide si el skill se usa

Es lo único que el harness lee siempre; el cuerpo se carga cuando el skill ya se activó. Debe decir **qué hace** (tercera persona, verbo primero) y **cuándo**, y terminar con `Use when: "...", "...", "/skill"`.

El matching es **semántico, no por palabra clave**: las frases del `Use when:` anclan, pero lo que hace que dispare con las palabras reales del usuario es que el qué-hace describa bien el territorio. Un `description` que solo lista triggers falla con cualquier sinónimo.

`name` y `description` son los dos obligatorios, no el frontmatter completo. `allowed-tools`, `disable-model-invocation`, `user-invocable` y `extends` existen y a veces son la herramienta correcta — ver [`frontmatter.md`](frontmatter.md).

## 3. Un paso = un archivo

No combinar dos pasos "porque son cortos". El agente lee cada archivo on-demand; mezclar pasos rompe ese beneficio.

Tampoco abusar de pasos triviales. "Verificar .env" no merece archivo propio — va dentro del paso que necesita las credenciales.

## 4. Lo que no cambia va a `references/`

- Esquemas de datos
- Voz de marca, reglas de tono
- Catálogos de productos / herramientas / precios
- Ejemplos de conversación end-to-end
- Plantillas y formatos

Si el agente lo abre raro vez, no merece estar en el flujo principal.

## 5. Cero código de >20 líneas embebido en `.md`

Bloques largos de código TypeScript / Bash / Python se mueven a `scripts/<nombre>.ts` (o `.sh`, `.py`). El paso solo dice "ejecuta `scripts/<nombre>.ts` con estos parámetros".

Excepción: snippets cortos (<20 líneas) específicos del paso pueden quedarse embebidos.

## 6. Idioma y tono

- Español de México siempre. Sin voseo, sin argentinismos.
- Tono directo. Sin "vamos a empoderar", sin "sinergia", sin relleno.
- Ver detalle en [`idioma-y-tono.md`](idioma-y-tono.md).

## 7. El core es portable — instrucción = markdown

Todo lo que el agente **lee para razonar** (`SKILL.md`, `steps/`, `references/`)
es `.md` puro, sin formatos que un harness deba interpretar. Lo que el agente
**ejecuta** vive en `scripts/` y puede ser lo que el trabajo pida. El skill debe
seguir teniendo lógica coherente aunque le quites todo el frontmatter menos
`name` y `description` — así corre en Codex, Cursor o Gemini, no solo en Claude
Code. Un skill regalable es un skill portable. Ver [`portabilidad.md`](portabilidad.md).
