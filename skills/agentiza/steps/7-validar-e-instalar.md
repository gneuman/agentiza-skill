# Paso 7 — Validar que el skill funcione de verdad

Generar los archivos no es terminar. Un skill puede estar perfecto por dentro y
no dispararse nunca: el harness lee el `description` y decide solo, así que un
skill que solo probaste con `/nombre` explícito **no está probado**.

## Inputs

- Los archivos generados en el paso 5.
- La lista de huecos del paso 6.

## Quién hace qué

Este paso tiene dos mitades y confundirlas hace que el skill se reporte como
probado sin haberse probado.

| Lo ejecuta el agente | Lo hace el humano |
|---|---|
| Escribir los archivos a disco | Reiniciar la sesión |
| Verificar links relativos | Probar que dispare con frases naturales |
| Revisar `name` vs. carpeta, YAML, colisiones | Correr el flujo con un caso real |

**El agente no puede reiniciar su propia sesión ni probar su propio
auto-disparo** — dentro de esta sesión el skill ya está cargado, así que la
prueba sería circular. Esa parte se le entrega al usuario como instrucción, y
el checklist queda **abierto** hasta que él responda. Reportarlo verde antes es
mentir.

## Acciones del agente

### 1. Escribir los archivos

Con la herramienta de escritura, a su ruta final — no como bloques para
copiar y pegar:

- Global: `~/.claude/skills/<nombre>/` si sirve en cualquier proyecto.
- Del proyecto: `<repo>/skills/<nombre>/` si depende de ese código o esos datos.

### 2. Verificaciones automáticas

Las cuatro que sí se pueden correr ahora:

```bash
# a) el name coincide con la carpeta
grep '^name:' <ruta>/SKILL.md          # debe ser igual al nombre del directorio

# b) el SKILL.md no pasa de 100 lineas
wc -l < <ruta>/SKILL.md

# c) no hay colision
ls ~/.claude/skills/ | grep -i <nombre>

# d) los links relativos resuelven
grep -ohE '\]\([^)#][^)]*\.(md|tpl|ts|sh)\)' <ruta>/SKILL.md <ruta>/steps/*.md \
  | sed -E 's/\]\((.*)\)/\1/' | sort -u
# cada resultado debe existir en disco
```

Si alguna falla, arreglar antes de entregar. Un link roto rompe el flujo a la
mitad y el agente improvisa en vez de avisar.

### 3. Entregar las pruebas que le tocan al humano

Redactar y pasarle **las tres frases concretas** para su skill (ver abajo cómo
elegirlas), no la instrucción genérica de "pruébalo".

## Pruebas que hace el humano

### Reiniciar y verificar que aparece

Los skills se leen al arrancar la sesión. En una nueva, escribir `/` y buscar el
nombre. Si no aparece, ver "Cuando no dispara".

### Probar el descubrimiento — la prueba que casi nadie hace

Escribe **tres frases que no son el trigger literal** pero que un usuario real
diría, y mira si el skill se activa solo.

Ejemplo para un skill de cotizar:

| Prueba | Qué mide |
|---|---|
| "necesito mandarle precios a este cliente" | Sinónimo natural, sin la palabra del trigger |
| "cuánto le cobro por esto" | Cómo lo diría alguien que no conoce el skill |
| "arma la propuesta de Acme" | Frase de trabajo real |

El matching es **semántico, no por palabra clave**. Poner `Use when: "cotizar"`
no garantiza que "mandarle precios" lo active: eso depende de que el
`description` describa bien el territorio.

- Las tres disparan → listo.
- Ninguna dispara → el `description` es muy estrecho. Ampliar el qué-hace, no
  agregar más frases al `Use when:`.
- Dispara cuando **no** debería → es muy amplio. Agregar la delimitación
  negativa (ver [`../references/frontmatter.md`](../references/frontmatter.md)).

### Correr el flujo completo una vez

Con un caso real del SOP original, de principio a fin. Verificar:

- Cada paso carga cuando toca (no todos de golpe al inicio).
- Las pausas de confirmación aparecen antes de lo irreversible.
- Los `[FALTA: ...]` salen a la superficie en vez de rellenarse con inventos.

**Si el skill hace algo irreversible, esta prueba NO se corre con un caso
real.** Probar un skill de cobranza "con un caso real" significa mandarle el
correo de verdad al cliente, y la pausa de confirmación no salva: quien está
probando dice que sí porque está probando.

Usar lo que se decidió en el [paso 2](2-preguntas-guiadas.md):

| Opción | Cómo se prueba |
|---|---|
| Dry-run | Correr con la bandera: imprime lo que haría, no lo hace |
| Destinatario de prueba | Un correo o canal propio en vez del del cliente |
| Corte antes del efecto | Se valida hasta la confirmación y ahí se detiene |

Un skill destructivo sin ninguna de las tres no se puede validar. Decirlo en el
reporte en vez de simular que se probó.

### Checklist de aceptación

Con dueño. Las del agente se marcan al terminar el paso 2 de arriba; las del
humano quedan **abiertas** hasta que responda — no se marcan por optimismo.

**Agente (automático):**
- [ ] `name` coincide con el nombre de la carpeta
- [ ] `SKILL.md` no pasa de 100 líneas
- [ ] Todos los links relativos resuelven
- [ ] Sin colisión de nombre
- [ ] Nada de PII, credenciales ni rutas con nombre de usuario
- [ ] Si es destructivo: `disable-model-invocation` puesto y modo de prueba definido

**Humano (en una sesión nueva):**
- [ ] Aparece en la lista de `/`
- [ ] Dispara con al menos 2 de las 3 frases naturales
- [ ] No dispara con temas vecinos que le tocan a otro skill
- [ ] El flujo completo corre de principio a fin
- [ ] Los huecos se reportan, no se inventan

Si una del agente falla, se arregla antes de entregar. Si una del humano falla,
vuelve al paso que corresponda — el skill no está listo.

## Cuando no dispara

En orden, de lo más común a lo más raro:

| Revisar | Cómo |
|---|---|
| ¿`name` coincide con la carpeta? | `skills/mi-skill/` → `name: mi-skill`. Si difieren, no carga. |
| ¿El YAML es válido? | Un `:` sin comillas dentro del `description` rompe el frontmatter entero. |
| ¿Reiniciaste la sesión? | Se leen al arrancar. |
| ¿Hay colisión de nombre? | `ls ~/.claude/skills/ \| grep <nombre>` y revisar también `<repo>/skills/`. |
| ¿El `description` dice cuándo usarlo? | Describir solo qué hace no basta: el harness necesita el cuándo. |
| ¿Compite con un skill hermano? | Dos descriptions parecidas se roban las invocaciones. Delimitar ambas. |

Aislar el problema: si `/nombre` explícito **sí** funciona pero las frases
naturales no, el contenido está bien y el problema es solo el `description`.

## Cuando dispara pero se pierde a la mitad

Más común que el no-dispara, y más difícil de ver porque arranca bien:

| Síntoma | Causa habitual |
|---|---|
| Se salta pasos o los hace en desorden | El `SKILL.md` no dice "lee cada paso solo cuando llegues a él" |
| Se detiene sin avisar en un paso | Link relativo roto: el agente no encuentra el archivo y sigue de memoria |
| Improvisa valores que el SOP no tenía | Faltan los `[FALTA: ...]`, o el paso no dice qué hacer si el dato no está |
| Hace dos pasos como si fueran uno | Un paso sin decisión de salida explícita |

Los cuatro se arreglan en el [paso 5](5-generar-archivos.md), no en el frontmatter.

## Output

- Skill escrito a disco y con las verificaciones automáticas en verde.
- Las tres frases de prueba, redactadas para este skill, entregadas al usuario.
- Checklist con lo del agente marcado y lo del humano pendiente.

## Decisión final

- Verificaciones del agente en verde → entregar **con las pruebas del humano
  pendientes**, diciendo cuáles son. No declarar el skill probado todavía.
- El usuario reporta que no dispara con ninguna frase → volver al
  [paso 2](2-preguntas-guiadas.md) y reescribir el `description`. Aquí sí se
  toca el frontmatter: el anti-patrón 6 prohíbe cambiarlo por estética, no por
  un fallo de descubrimiento.
- El usuario reporta que se rompe a la mitad → volver al
  [paso 5](5-generar-archivos.md); ver la tabla de arriba.
- Es destructivo y no hay modo de prueba → decirlo. Un skill que cobra o publica
  sin forma de ensayarse no se entrega como listo.
