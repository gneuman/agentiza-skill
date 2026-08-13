# Paso 2 — Diseñar el skill con el usuario (recomendar y confirmar)

No le sueltes al usuario un cuestionario en frío. En este punto ya leíste el SOP
(paso 1), así que **ya tienes una recomendación para casi todo.** El patrón es
**recomendar y confirmar**: lideras con tu mejor respuesta y una línea de por
qué, das 2-3 alternativas, y el usuario confirma en una palabra, elige otra, o te
corrige. Debe poder avanzar sin pensar, salvo que quiera meter mano fuerte.

Trabaja estos 5 puntos **en orden**, uno a la vez. No avances al paso 3 sin que
el usuario haya confirmado cada uno.

## 1. Triggers (qué frases lo invocan)

Del SOP saca las frases con las que el usuario realmente pediría esto. Recomienda:

> "Yo pondría estos triggers: **'cotizar', 'crear cotización', 'deal nuevo', '/cotizar'**.
> Los saqué de cómo describes el proceso. ¿Van bien, o falta/sobra alguno?"

Van al `description` como `Use when: "...", "...", "/skill"`.

Modo B (modernizar): **preserva los triggers existentes** del frontmatter y solo
confirma que sigan vigentes.

## 2. Herramientas externas

Infiere del SOP qué toca. Recomienda lo que detectaste:

> "Por lo que describes, esto pega a **MongoDB y Linear**, y manda un correo por
> Gmail. No veo que use Slack ni Stripe. ¿Correcto, o toca algo más?"

Define qué scripts van a `scripts/` y qué credenciales/endpoints van a
`references/`. Si detectaste una herramienta pero no estás seguro, dilo como
hueco, no como hecho.

## 3. Pausas obligatorias

Recomienda dónde el agente DEBE pausar antes de un efecto irreversible:

> "Este skill manda un correo al cliente, así que pondría una **pausa de
> confirmación antes de enviar** y `disable-model-invocation: true` para que solo
> corra si escribes el slash. ¿De acuerdo?"

Pausa típicamente requerida en: cobros, mensajes públicos (Slack, email, redes,
WhatsApp), escrituras destructivas (DROP, DELETE masivo), registros que el
cliente va a ver.

Cada pausa confirmada se traduce en **tres cosas**, no una:

**a) Un paso explícito de "Confirmar con el usuario"** antes del de ejecución.

**b) Una restricción en el frontmatter** (la prosa se ignora, el frontmatter no):

| Si el skill… | Va al frontmatter |
|---|---|
| Cobra, publica, manda correos o borra | `disable-model-invocation: true` |
| Solo lee y reporta (auditorías) | `allowed-tools: Read, Grep, Glob` |
| Escribe archivos, no toca servicios externos | Sin restricción, con la pausa basta |

**c) Cómo se prueba sin disparar el efecto** (el paso 7 lo necesita). Recomienda
uno: modo dry-run (imprime lo que haría), destinatario de prueba (un canal
propio), o corte antes del paso destructivo. Sin uno de los tres, el skill sale a
producción a ciegas.

Anótalo: el paso 5 lo usa al escribir el frontmatter. Detalle de cada campo en
[`../references/frontmatter.md`](../references/frontmatter.md).

## 4. Un ejemplo real horneado

Un ejemplo real input→output alinea el skill mucho mejor que cualquier
descripción. Recomienda hornear uno:

> "Metería como ejemplo dentro del skill **la cotización de Acme que ya hiciste**
> — el input (el brief) y el output final. Así el agente calca el formato en vez
> de improvisar. ¿Tienes uno a la mano, o lo saco del SOP?"

Siempre ofrece **"ninguno / no aplica"** como opción — algunos skills no se
benefician de un ejemplo horneado (los muy mecánicos, o los que cada corrida
produce algo distinto sin formato fijo). Si hay ejemplo, va a
`references/ejemplo-<caso>.md` y el paso que produce el artefacto lo referencia.

**Sanitiza antes de hornear:** si el ejemplo trae correos de terceros, nombres de
cliente reales, montos, o cualquier PII, reemplázalo por placeholders
(`cliente@ejemplo.com`, "Acme"). Un ejemplo horneado con PII real filtra datos a
todo el que instale el skill. Ver [`../references/anti-patrones.md`](../references/anti-patrones.md).

## 5. Confirmar el plan de pasos

Antes de escribir nada, muéstrale la secuencia completa que armaste en el paso 3
(o la que propones si vas en paralelo) para que la confirme:

> "El flujo me queda en 5 pasos: 1) leer el brief, 2) calcular precio, 3)
> redactar, 4) confirmar contigo, 5) crear el draft. ¿Así, o mueves algo?"

Incluye aquí **qué produce una corrida** (el artefacto y dónde cae) y **cómo
sabrá el skill que salió bien** — esa definición de "listo" se vuelve el
auto-check del skill en el paso 7.

## Espera confirmación de los cinco

No avances al paso 3 sin que el usuario haya confirmado cada punto. Si solo
respondió algunos, repite los que faltan. La gracia del recomendar-y-confirmar es
que la mayoría los va a aprobar de un tirón — pero aprobar no es lo mismo que
saltar.
