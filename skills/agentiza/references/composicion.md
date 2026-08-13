# Skills que llaman a otros skills

Un skill no tiene que hacer todo. Casi siempre es un eslabón: recibe lo que
dejó otro y entrega lo que el siguiente necesita. Escribir esa relación es lo
que convierte skills sueltos en un sistema.

## Las tres formas

### 1. Heredar — `extends`

Para reglas transversales que no quieres repetir:

```yaml
extends: [voz-gnb]
```

El skill hereda las reglas de tono, idioma y anti-slop sin copiarlas. Cuando la
voz cambia, cambia en un lugar.

**Cuándo:** todo skill que produce texto para publicar.
**Cuándo no:** si solo necesitas una tabla de datos, es una `reference`, no una
herencia.

### 2. Invocar — desde un paso

Un paso puede pedir que se corra otro skill:

```markdown
## Acciones

Antes de escribir, corre `/voz-gnb` para cargar la voz canónica.

Si el usuario no tiene el PRD listo, corre `/prd` y vuelve aquí con el
resultado.
```

**Cuándo:** el otro skill hace un trabajo completo que este necesita entero.
**Cuándo no:** si solo necesitas dos reglas de él, cópialas a una `reference`.
Invocar un skill de 400 líneas para usar una tabla es caro.

### 3. Encadenar — declarar el pipeline

Cuando varios skills forman una secuencia, cada uno dice de dónde viene y a
dónde va:

```markdown
## Salida esperada

El PRD queda en `docs/prd/<slug>.md`. Siguiente paso: `/cotizar` lo lee para
ponerle precio.
```

El eslabón se escribe en el paso final, en la sección de salida. Así el usuario
—y el agente— saben qué sigue sin adivinar.

## Ejemplos reales de este ecosistema

```
Pre-venta:   /prd → /elias → /cotizar → /arquitectura-proyecto
Contenido:   /voz-gnb (extends) → /blog-gnb → /distribuye
Cierre:      /cierre-sesion → (viernes) → /cierre-semana
Tránsito:    cualquiera → /handoff → sesión nueva
```

Ninguno de estos duplica lo del anterior. `/cotizar` no vuelve a definir el
alcance: lee el PRD. `/cierre-semana` no vuelve a preguntar qué hiciste: lee
los cierres de sesión.

## Cómo declararlo

En el frontmatter, para quien mantiene el skill:

```yaml
extends: [voz-gnb]
related: [prd, elias, cotizar]
```

En el `description`, para el harness — solo si hay riesgo de confusión:

> No confundir con `/handoff`, que mueve contexto entre sesiones.

En el paso donde ocurre, con la instrucción concreta:

> Si no hay PRD, corre `/prd` primero y vuelve con el archivo.

## Errores comunes

**Cadenas circulares.** A invoca B, B invoca A. Se cuelga. Un pipeline es una
línea, no un ciclo.

**Invocar por una tabla.** Cargar 400 líneas para usar diez es desperdicio.
Copia esas diez a una `reference` y anota de dónde salieron.

**Encadenar sin contrato.** "Después corre `/cotizar`" no sirve si no dices qué
le dejas y dónde. Nombra el archivo y la ruta.

**Duplicar en vez de heredar.** Copiar las reglas de voz a cada skill de
contenido significa que la próxima corrección se aplica a uno solo y los demás
se quedan viejos. Para eso está `extends`.
