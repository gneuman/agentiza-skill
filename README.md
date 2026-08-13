# agentiza

**Convierte cualquier SOP, Loom o proceso en un skill de Claude Code modular y portable.**

Un skill gratis y de código abierto por [Gabriel Neuman](https://gabrielneuman.com) · GNB Labs.

> El skill opera **en español (México)**. La estructura que genera es portable a cualquier harness (Claude Code, Codex, Cursor, Gemini CLI). Si tu equipo trabaja en español y quiere convertir procesos en agentes, esto es para ti.
>
> *This skill operates in Spanish. The scaffolding it produces is portable across harnesses. See the English note at the bottom.*

---

## Qué hace

Le das un SOP, la transcripción de un Loom, tus notas, o simplemente le describes cómo haces algo a mano — y `agentiza` te lo devuelve como un **skill bien estructurado**:

```
mi-skill/
  SKILL.md              # índice, máximo 100 líneas
  steps/<n>-<slug>.md   # un archivo por paso
  references/<tema>.md  # lo que no cambia: esquemas, tono, catálogos
  scripts/<nombre>.*    # código ejecutable, fuera del markdown
```

En vez de un cuestionario en frío, **recomienda y confirma**: lee tu proceso, propone triggers/herramientas/pausas/ejemplo, y tú apruebas en una palabra. Detecta los huecos del SOP (`[FALTA: ...]`) en vez de inventarlos, valida que el skill **dispare solo** (no solo con el slash), y te enseña el bucle de mejora para que la v1 no se fosilice.

También **moderniza skills viejos** que crecieron sin estructura y pasaron las 200 líneas.

## Instalar (Claude Code, con auto-update)

La forma recomendada. Recibes las mejoras automáticamente cuando actualizo el repo:

```
/plugin marketplace add gneuman/agentiza-skill
/plugin install agentiza
```

Luego, en cualquier proyecto:

```
/agentiza
```

Y pega tu SOP, Loom, notas, o el `SKILL.md` monolítico que quieres modernizar.

## Instalar (manual / otros agentes)

El core es markdown puro, así que corre en cualquier harness que soporte Agent Skills:

```bash
git clone https://github.com/gneuman/agentiza-skill.git
cp -r agentiza-skill/skills/agentiza ~/.claude/skills/agentiza
# o ~/.agents/skills/agentiza para Codex/Cursor/Gemini
```

O simplemente dile a tu agente:

> "Instala el skill de github.com/gneuman/agentiza-skill en mi carpeta de skills."

## Cómo mejora con el tiempo

Este repo **es** el sistema de mejora continua. Yo afino `agentiza` con cada proyecto real; si lo instalaste vía `/plugin marketplace`, recibes esas mejoras sin re-descargar nada. Es el mismo patrón de MAA (Medir, Analizar, Actuar) que el skill te enseña a aplicar sobre tus propios skills.

## Qué trae de bueno (vs. escribir skills a mano)

- **7 reglas duras** del patrón modular, probadas en producción.
- **Portabilidad de fábrica:** instrucción = markdown; nada atado a un solo harness.
- **Recomendar y confirmar:** avanzas sin pensar, salvo que quieras meter mano.
- **Ejemplo real horneado** (sanitizado de PII) para que el output calque el formato.
- **Validación de auto-disparo:** el skill no está probado hasta que dispara con frases naturales, no solo con `/nombre`.
- **Bucle de mejora explícito:** la v1 es un borrador; el skill se afina corriéndolo.

## Licencia

MIT. Úsalo, fórkalo, adáptalo. Si te sirve, [cuéntame](https://gabrielneuman.com/contacto).

---

### English

`agentiza` turns any SOP, Loom transcript, or hand-run process into a modular, portable Claude Code skill. **It operates in Spanish (Mexico)** — that's intentional; it's built for Spanish-speaking teams. The scaffolding it produces (SKILL.md index + steps/ + references/) is harness-agnostic and works in Codex, Cursor, and Gemini CLI too. Install with `/plugin marketplace add gneuman/agentiza-skill` then `/plugin install agentiza`. MIT licensed.
