# Portabilidad — que el skill corra en cualquier harness

Un skill bien hecho no depende de Claude Code. La misma carpeta debe funcionar
en Codex, Cursor, Gemini CLI, o cualquier agente que soporte el estándar de
Agent Skills. Esto no es purismo: es lo que hace que un skill sea **regalable** y
sobreviva al harness con el que naciste.

## La línea que separa "core portable" de "específico del harness"

```
mi-skill/
  SKILL.md              ← core portable: markdown puro
  steps/<n>-<slug>.md   ← core portable: markdown puro
  references/<tema>.md  ← core portable: markdown puro
  scripts/<nombre>.*    ← específico: código ejecutable (el harness lo corre)
  assets/               ← específico: plantillas, esquemas, lo que el job pida
```

**Regla:** todo lo que el agente **lee para razonar** es `.md` puro. Todo lo que
el agente **ejecuta** vive en `scripts/` y puede ser lo que el trabajo necesite
(`.ts`, `.py`, `.sh`, un `.json` de esquema, un `.tpl`).

## Qué rompe la portabilidad (y cómo evitarlo)

| Anti-patrón | Por qué rompe | En su lugar |
|---|---|---|
| Instrucciones en `.tsx`, `.json` o cualquier formato que el harness deba interpretar | Otro agente no sabe leerlo como instrucción | Instrucción = `.md`. Punto. |
| Frontmatter con campos propietarios en el core (`allowed-tools`, `disable-model-invocation`) asumidos como universales | Otro harness los ignora o falla | `name` y `description` son los únicos universales. Los demás son mejoras específicas de Claude Code — úsalos, pero no construyas la lógica del skill **encima** de que existan |
| Rutas absolutas con nombre de usuario (`C:\Users\...`, `/home/gabriel/...`) | No existen en la máquina de quien lo instala | Rutas relativas al skill o a `$repo` |
| Un `script.ts` que asume `node_modules` de un repo específico | No corre fuera de ese repo | El script declara sus deps, o el paso dice qué instalar |
| Referencias a MCPs específicos como si fueran garantía | El que instala puede no tenerlos | El paso dice "si tienes el MCP X úsalo; si no, aquí está el fallback" |

## Los campos de frontmatter específicos SÍ se usan — pero como capa, no como base

`disable-model-invocation`, `allowed-tools`, `user-invocable` son de Claude Code
y valen la pena (ver [`frontmatter.md`](frontmatter.md)). La regla no es
"no los uses" — es: **el skill debe seguir teniendo sentido sin ellos.** Si le
quitas el frontmatter propietario a otro harness, el peor caso aceptable es que
pierda una salvaguarda (el modelo podría auto-invocarlo cuando en Claude Code no
lo haría), no que el skill deje de funcionar.

## Prueba rápida de portabilidad

Antes de dar por bueno un skill, pregúntate:

1. ¿Toda instrucción es `.md`? (grep de `.tsx`/`.json` fuera de `scripts/` y `assets/`)
2. ¿Hay rutas absolutas con nombre de usuario? (`grep -rE '/Users/|/home/|C:\\\\Users'`)
3. Si le borro el frontmatter a `name` + `description`, ¿el skill sigue teniendo lógica coherente?
4. ¿Un script asume un repo concreto sin decirlo?

Tres de cuatro en verde = portable. La #3 es la que más se falla.
