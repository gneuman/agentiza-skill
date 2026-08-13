# Frontmatter: los campos que existen

El frontmatter es lo único que el harness lee **siempre**. El cuerpo del
`SKILL.md` se carga solo cuando el skill ya se activó. Por eso un skill con
buen contenido y mal `description` no corre nunca.

## Los dos obligatorios

### `name`

Kebab-case, `[a-z0-9-]`, máximo 64 caracteres. **Debe coincidir con el nombre de
la carpeta** — si difieren, el skill no carga y no hay mensaje de error.

Es el slash que se teclea: `name: cotizar` → `/cotizar`. Sin tildes ni
mayúsculas (`/cotizacion-rápida` es imposible de escribir bien).

### `description`

Máximo ~1024 caracteres. Es el campo que decide si el skill se usa. Tres partes:

```yaml
description: >
  <QUÉ HACE — en tercera persona, verbo primero>
  <CUÁNDO NO — el límite, si hay un skill vecino>
  Use when: "<frase 1>", "<frase 2>", "/<nombre>".
```

**Tercera persona.** "Convierte un SOP en un skill modular", no "Yo convierto"
ni "Te ayudo a convertir". El harness lee una ficha de capacidad, no un saludo.

**El qué hace importa más que el `Use when:`.** El matching es semántico: el
modelo compara la intención del usuario contra la descripción completa. Frases
literales ayudan a anclar, pero un `description` que solo lista triggers no
dispara con las palabras que el usuario realmente usa.

**Delimitación negativa.** Si hay un skill vecino, decirlo:

> No confundir con `/handoff`, que mueve contexto entre sesiones.

Sin eso, dos skills parecidos se roban invocaciones y ninguno es confiable.

## Los opcionales que sí valen

### `allowed-tools`

Restringe qué puede usar el skill. Es la forma correcta de garantizar que un
skill de análisis no escriba nada:

```yaml
allowed-tools: Read, Grep, Glob
```

Úsalo cuando el skill solo lee (auditorías, reportes, diagnósticos). Es más
fuerte que pedirlo en prosa: las instrucciones se pueden ignorar, la
restricción de herramientas no.

### `disable-model-invocation`

```yaml
disable-model-invocation: true
```

El skill **solo** corre si el usuario escribe `/nombre`. El modelo nunca lo
activa solo. Para lo que no debe pasar por accidente: publicar, cobrar, borrar,
mandar correos.

### `user-invocable`

```yaml
user-invocable: false
```

Lo contrario: no aparece como slash command, solo lo invoca otro skill o el
modelo. Para skills que son librería de otros, no comandos.

### `extends`

```yaml
extends: [voz-gnb]
```

Hereda otro skill. Todo skill de contenido de este ecosistema extiende
`voz-gnb` en vez de repetir las reglas de tono. Ver
[`composicion.md`](composicion.md).

### Campos del template de proyecto

`triggers` (3-5 frases, además del slash) y `related` (skills vecinos que
aparecen en el mismo pipeline). No los lee el harness: son documentación para
quien mantiene el skill. Útiles, opcionales.

## Colisiones

El choque de nombre lo atrapa un `ls | grep` (ver
[`../steps/1-identificar-input.md`](../steps/1-identificar-input.md)): gana uno
y el otro desaparece sin aviso.

El que importa aquí es el **semántico**: dos skills con nombres distintos pero
descriptions que cubren el mismo territorio. Ambos se activan a medias y el
comportamiento es impredecible. Se arregla delimitando los dos, no renombrando:

```yaml
# en /handoff
description: > ... Mueve contexto a una sesión nueva. No confundir con
  /cierre-sesion, que es el log retrospectivo del día.

# en /cierre-sesion
description: > ... Log retrospectivo con pendientes y horas. No confundir con
  /handoff, que prepara el contexto para otra sesión.
```

Cada uno nombra al otro. Sin eso, una frase ambigua activa cualquiera de los dos.

Tampoco usar "claude" ni "anthropic" en el nombre.

## Dónde vive el skill

| Ubicación | Cuándo |
|---|---|
| `~/.claude/skills/<n>/` | Sirve en cualquier proyecto |
| `<repo>/skills/<n>/` | Depende del código o los datos de ese repo |
| `<repo>/.claude/skills/<n>/` | Igual, pero no se comparte con el equipo |
| Plugin | Se distribuye o se vende |

Ante la duda, del proyecto: subirlo a global después es copiar una carpeta;
bajarlo es descubrir que dependía de cosas que no existen en otro lado.

## Ejemplo completo

```yaml
---
name: cotizar
description: >
  Convierte una llamada o una descripción de alcance en una cotización con
  precio, entregables y plazo, usando la tabla de precios del proyecto. No
  confundir con /prd, que define qué se construye antes de ponerle precio.
  Use when: "cotiza esto", "cuánto le cobro", "arma la propuesta",
  "mándale precios al cliente", "/cotizar".
allowed-tools: Read, Grep, Glob, Write
---
```
