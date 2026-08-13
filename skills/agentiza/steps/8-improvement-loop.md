# Paso 8 — Enseñar el bucle de mejora (MAA sobre el skill)

El paso 7 valida que el skill dispara y corre. Este paso entrega lo que hace que
el skill **mejore con el tiempo** en vez de fosilizarse en su primera versión.

La verdad incómoda: **la v1 de cualquier skill es un borrador del proceso, no su
forma final.** El skill se afina corriéndolo, no diseñándolo. Vas a predecir
secciones que nadie usa, y te van a faltar cosas que solo aparecen con el uso
real. Este es el paso donde le dejas al usuario el mecanismo para cerrar ese ciclo.

## Es MAA aplicado al propio skill

El [método MAA](../references/versionado.md) no es solo para código de producto —
aplica al skill mismo:

| MAA | En el skill |
|---|---|
| **Medir** | ¿Qué molestó en la última corrida? Una pregunta que sobró, una sección que **siempre** se edita a mano, un input que debió pedir y no pidió, un `[FALTA:]` que ya se puede llenar. |
| **Analizar** | ¿Es un caso raro o un patrón? Si pasó una vez, anótalo. Si pasó dos, es señal. La sección que corriges cada vez ya te está diciendo qué cambiar. |
| **Actuar** | Arréglalo en sitio: "actualiza el skill para que la próxima vez pida X". Mata lo que no se usó, promueve lo que siguió apareciendo. |

## Qué entregarle al usuario (breve, 3 líneas)

Al cerrar, dile literalmente:

1. **Cómo invocarlo**, con un ejemplo copiable.
2. **El bucle de mejora** — lo que más importa: *"Este es el borrador del
   proceso. Después de cada corrida real, fíjate qué chocó y arréglalo en el
   momento: 'actualiza el skill para que la próxima vez no me pregunte X' o
   'agrega Y al paso 3'. Los skills se vuelven buenos corriéndolos, no
   diseñándolos."*
3. **Cuándo re-modernizar** — si el `SKILL.md` vuelve a pasar de 100 líneas o un
   paso creció con material que no cambia, es hora de otra vuelta de `/agentiza`
   en modo B. El comentario de mantenimiento al pie del `SKILL.md` (paso 5) dice
   cuándo revisar.

## La señal más útil: lo que se edita a mano cada vez

Si el usuario corre el skill y **siempre** reescribe el mismo párrafo del output,
eso no es que el usuario sea especial — es que el skill tiene mal ese pedazo.
Esa edición repetida es la métrica más honesta de qué mejorar. Enséñale a
notarla: "si te encuentras arreglando lo mismo en cada corrida, ese es el próximo
cambio al skill, no un defecto tuyo".

## Output

- El usuario sabe cómo invocar el skill, cómo mejorarlo en sitio, y cuándo
  re-modernizarlo.
- El skill queda entregado **como v1 explícita**, no como producto terminado —
  con el ciclo de mejora abierto a propósito.
