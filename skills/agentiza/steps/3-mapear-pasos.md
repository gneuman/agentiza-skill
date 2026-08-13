# Paso 3 — Mapear pasos del SOP

Con el SOP / material fuente y las respuestas a las 3 preguntas, extrae los pasos reales del proceso.

## Reglas para mapear

1. **Un paso = una decisión o una acción atómica.** Si el SOP dice "buscar contacto y crear deal", son dos pasos.
2. **Numera secuencialmente.** `1-`, `2-`, `3-`. Si hay ramas (if/else), reflejalo en la decisión final del paso.
3. **Slug en kebab-case y en español.** `1-identificar-cliente.md`, no `1-IdentifyClient.md`.
4. **Si la pregunta 3 marcó pausa obligatoria**, ese es un paso propio: `<n>-confirmar-con-usuario.md`.

## Estructura interna de cada paso

Cada `steps/<n>-<slug>.md` sigue este molde (ver [`templates/step.md.tpl`](../templates/step.md.tpl)):

```markdown
# Paso N — <Título corto>

<1-2 oraciones: qué hace este paso y por qué importa>

## Inputs

- <qué espera del paso anterior o del usuario>

## Acciones

<las instrucciones detalladas, comandos, llamadas API>

## Output

- <qué produce para el siguiente paso>

## Decisión final

- Si <condición>, ir al paso <N+1>.
- Si <otra condición>, preguntar al usuario.
- Si <error>, ver `references/troubleshooting.md` (si aplica).
```

## Cuándo dividir más fino

Si un paso pasa de ~80 líneas o mezcla más de 3 herramientas externas distintas, considéralo dos pasos.

## Cuándo dividir menos fino

No hagas pasos triviales. "Verificar que .env existe" no merece archivo propio — va dentro del paso que necesita las credenciales.

## Output del paso

Lista numerada de pasos con título tentativo. Antes de generar archivos, **muestra esta lista al usuario** y pregunta:

> Estos son los pasos que detecté:
> 1. ...
> 2. ...
> 3. ...
>
> ¿Confirmas o ajustamos antes de generar?
