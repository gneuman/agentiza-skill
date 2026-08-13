# Paso 1 — Identificar input y modo

## Gate: ¿esto amerita un skill?

Primero decide **si** hacer un skill, no cómo. Un skill es una carpeta que hay
que mantener; para un proceso de dos pasos que corres una vez, es peor que un
mensaje bien escrito.

| Si el proceso… | Va a |
|---|---|
| Se repite, tiene 3+ pasos, y decides lo mismo cada vez | **Skill** |
| Es una regla que aplica siempre, sin pasos | `CLAUDE.md` del proyecto |
| Es de una sola vez, aunque sea largo | Un prompt directo |
| Es determinista y sin criterio (renombrar, mover, convertir) | Un script |
| Son datos que se consultan (precios, catálogos, esquemas) | Una `reference` de un skill que ya existe |
| Ya lo hace otro skill con otro nombre | Editar ese, no crear uno nuevo |

Dos preguntas que resuelven la mayoría de los casos:

1. **¿Lo vas a correr más de tres veces?** Si no, es un prompt.
2. **¿Hay decisiones que tomar en el camino?** Si no las hay, es un script.

Si el gate dice que no es un skill, **decírselo al usuario** con la alternativa
concreta, y parar aquí:

> Esto son tres reglas que aplican siempre, no un proceso. Rinde más en el
> `CLAUDE.md` del proyecto: se cargan solas en cada sesión y no hay que
> invocarlas. ¿Las escribo ahí?

Si el usuario igual quiere el skill después de oír esto, se hace. Es su
llamada.

## Marcar el PII del material fuente — antes de tocarlo

Los SOPs vienen de transcripts, correos y del CRM: traen datos reales pegados.
Marcarlos **ahora**, no en el paso 4, porque el paso 3 te va a hacer mostrar los
pasos detectados al usuario y para entonces el dato ya salió a la conversación.

Recorre el material y anota qué hay que reemplazar:

- Correos de terceros, nombres de personas y de empresas (razón social incluida)
- Teléfonos, RFC, direcciones
- Montos de un deal concreto, IDs que apuntan a un registro real
- Credenciales, tokens, URIs con password

De aquí en adelante, **al citar el SOP usa el reemplazo, no el original** — en
los archivos y también en lo que le muestras al usuario. El cómo reemplazar
cada tipo está en la tabla de
[`4-separar-referencias-scripts.md`](4-separar-referencias-scripts.md#sanitizar-antes-de-escribir-nada).

Si el skill va a un repo, vale correr ahí el candado de pre-commit
(`scripts/install-pii-hook.sh`) como segunda capa. El hook global
`~/.claude/hooks/block-pii.sh` ya bloquea escrituras en cualquier repo.

## Identificar el input

Antes de hacer preguntas, identifica qué te pasaron:

## Modos

### Modo A: SOP nuevo

Input típico:
- Documento de Notion / Google Docs con un proceso paso a paso
- Transcripción de Loom donde alguien explica cómo hace algo
- Lista de bullets, notas crudas, o descripción verbal
- Ejemplo: "cuando llega un lead nuevo, busco en HubSpot, si existe..."

**Resultado:** vas a crear un skill desde cero.

### Modo B: Modernizar skill existente

Input típico:
- Un `SKILL.md` monolítico (>200 líneas)
- Path a una carpeta de skill que ya existe pero tiene todo en un solo archivo
- El usuario dice "modernizar este skill", "modulariza esto", "está muy largo"

**Resultado:** vas a partir el SKILL.md existente en pasos + referencias + scripts, **preservando el frontmatter** (`name`, `description`) intacto para no romper triggers.

## Preguntas previas (solo si el input es ambiguo)

Si no puedes determinar el modo:

> ¿Quieres crear un skill nuevo desde un SOP, o modernizar uno existente que ya tiene SKILL.md monolítico?

Si no puedes determinar el nombre del skill:

> ¿Cómo se va a llamar el skill? (ej: `cotizar`, `agentiza`, `weekly-report`). El nombre es el slash que lo invoca.

## Revisar colisiones antes de bautizar (solo modo A)

```bash
ls ~/.claude/skills/ | grep -i <nombre>     # globales
ls skills/ 2>/dev/null | grep -i <nombre>   # del proyecto
```

Eso atrapa el choque de nombre. El choque **semántico** —nombres distintos,
descripciones parecidas— no lo ve ningún `grep` y es el que de verdad rompe: los
dos skills se activan a medias y ninguno es confiable. Para detectarlo, leer el
`description` de los dos o tres skills del mismo dominio y preguntarse si un
usuario podría querer cualquiera con la misma frase. Si la respuesta es sí, hay
que delimitar ambos.

Por qué importa y cómo se delimita:
[`../references/frontmatter.md`](../references/frontmatter.md).

Si ya existe algo que hace lo mismo, **decirlo y proponer editar ese** en vez
de crear otro:

> Ya existe `/cotizar` y cubre esto. ¿Le agrego el caso nuevo o de verdad
> quieres un skill aparte?

El nombre debe decir qué hace, no ser una metáfora. `/sin-colision` se entiende;
`/carril` solo si ya sabes de qué va.

## Output del paso

Anota mentalmente:
- **Modo:** A (nuevo) o B (modernizar)
- **Nombre del skill:** kebab-case, en español si aplica, sin colisión verificada
- **Material fuente:** todo el contenido que vas a procesar
- **Destino:** global (`~/.claude/skills/`) o del proyecto (`skills/`)
