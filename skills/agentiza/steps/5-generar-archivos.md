# Paso 5 — Generar archivos

Genera todos los archivos del skill en este orden, usando los templates en [`../templates/`](../templates/).

## Orden de generación

1. **`SKILL.md`** — usar [`templates/SKILL.md.tpl`](../templates/SKILL.md.tpl). Frontmatter + descripción + lista de pasos con links + lista de referencias + lista de scripts.
2. **Cada `steps/<n>-<slug>.md`** — usar [`templates/step.md.tpl`](../templates/step.md.tpl).
3. **Cada `references/<tema>.md`** — usar [`templates/reference.md.tpl`](../templates/reference.md.tpl).
4. **Cada `scripts/<nombre>.ts`** o `.sh` — código limpio con comentario inicial sobre cómo se invoca.

## Reglas duras al generar

Aplican siempre. Ver detalle en [`references/reglas-duras.md`](../references/reglas-duras.md):

1. `SKILL.md` máximo 100 líneas. Si pasa, hay contenido que debe moverse a un step o reference.
2. Frontmatter completo: `name` + `description` con triggers explícitos.
3. Un paso = un archivo. No combinar.
4. Idioma: español de México (sin voseo). Ver [`references/idioma-y-tono.md`](../references/idioma-y-tono.md).
5. Cero código de >20 líneas embebido en `.md`. Va a `scripts/`.
6. Links relativos correctos (`steps/1-...md`, `../references/...md`, `../templates/...tpl`).

## Modo B (modernizar): preservar frontmatter

Si estás modernizando un skill existente, **copia el frontmatter del SKILL.md original sin modificarlo**. El `description` es lo que decide si el skill se invoca; tocarlo mientras reorganizas archivos mezcla dos cambios y si algo se rompe no sabes cuál fue.

Se reescribe en dos casos, no en otros: si el usuario lo pide, o si falla la prueba de descubrimiento del [paso 7](7-validar-e-instalar.md). Ver el anti-patrón 6 en [`../references/anti-patrones.md`](../references/anti-patrones.md).

## Dejar rastro de dónde salió

Al final del `SKILL.md`, un comentario con la procedencia. Dentro de seis meses
es la única forma de saber si el skill se desvió o si el proceso cambió y nadie
lo actualizó:

```markdown
<!--
Fuente: <de dónde salió el SOP — Loom, doc, conversación — con fecha>
Última revisión: <hoy>
Revisar cuando: <el disparador concreto: cambie el precio, la API, el proveedor>
-->
```

El "revisar cuando" es lo que más rinde: una fecha sola envejece sin avisar.
Detalle en [`../references/versionado.md`](../references/versionado.md).

## Salida: escribir a disco, no pegar en el chat

Escribe cada archivo a su ruta final con la herramienta de escritura. Un skill
que vive en el transcript no se puede verificar, no se puede probar y se pierde
al cerrar la sesión.

```
~/.claude/skills/<nombre>/SKILL.md          # o <repo>/skills/<nombre>/
~/.claude/skills/<nombre>/steps/1-....md
~/.claude/skills/<nombre>/references/....md
```

Al terminar, lista lo que escribiste con su ruta y su tamaño en líneas, para
que el usuario vea el resultado sin tener que abrir cada archivo:

```
✓ SKILL.md                          62 líneas
✓ steps/1-identificar-cliente.md    41 líneas
✓ references/tabla-precios.md       28 líneas
```

**Excepción:** si el usuario pidió explícitamente revisar antes de instalar,
muéstrale el `SKILL.md` en el chat y escribe el resto. El índice es lo que vale
la pena revisar a ojo; los pasos se leen mejor ya en disco.

El [paso 7](7-validar-e-instalar.md) corre las verificaciones sobre esos
archivos: si no existen, no hay nada que verificar.
