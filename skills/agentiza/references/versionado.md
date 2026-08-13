# Versionado y mantenimiento

Un skill no se termina: se mantiene. El SOP del que salió cambia, la API que
usaba se mueve, el precio sube. Un skill viejo no avisa que está viejo — sigue
corriendo y entregando lo de antes, que es peor que fallar.

## Qué se anota y dónde

El frontmatter no tiene campo de versión y no hace falta inventarlo: git ya
guarda el historial. Lo que sí conviene dejar escrito es **de dónde salió** y
**cuándo se revisó por última vez**, en un comentario al final del `SKILL.md`:

```markdown
<!--
Fuente: SOP de cobranza semanal (Loom del 2026-03-14)
Última revisión: 2026-07-26
Revisar cuando: cambie la tabla de precios o el proveedor de facturación
-->
```

Ese "revisar cuando" es lo que más rinde. Una fecha sola envejece sin decir
nada; un disparador concreto convierte el mantenimiento en algo que se nota.

## Cuándo un skill quedó viejo

| Señal | Qué pasó |
|---|---|
| El agente improvisa donde antes seguía el paso | Cambió algo que el skill da por hecho |
| Los `[FALTA: ...]` ya no aplican o faltan otros | El proceso cambió |
| Un paso menciona una herramienta que ya no usas | Migración a medias |
| Una `reference` tiene datos que ya no cuadran | La fuente cambió y la copia se quedó |
| El `SKILL.md` pasó de 100 líneas | Creció sin estructura → modo B de [`../steps/1-identificar-input.md`](../steps/1-identificar-input.md) |

## Cambiar un skill sin romperlo

Lo que se puede tocar libremente: el contenido de los pasos, las referencias,
los scripts. Lo que se toca con cuidado:

- **`name`** — es el slash que la gente teclea y lo que otros skills invocan.
  Cambiarlo es deprecar y crear otro (ver abajo), no renombrar.
- **`description`** — decide si el skill se usa. No se toca por estética; sí se
  reescribe si falla la prueba de descubrimiento del
  [paso 7](../steps/7-validar-e-instalar.md).
- **Quitar un paso** — si otro skill lo invoca o el usuario lo tiene en la
  cabeza como parte del flujo, avisar en vez de borrarlo en silencio.

Después de cualquier cambio de fondo, **volver a correr el paso 7**. Un skill
que dispara hoy puede dejar de disparar mañana si un skill vecino nuevo le come
el territorio.

## Deprecar

Los skills no se borran de golpe: alguien los tiene en la memoria muscular. El
patrón que funciona es dejar el nombre viejo apuntando al nuevo, con fecha.

En el `description` del skill **nuevo**:

```yaml
description: >
  ... Reemplazó a /cierre-dia el 2026-05-21.
  Use when: "cierro sesión", "/cierre-sesion".
```

En el skill **viejo**, si se conserva: una línea al inicio del cuerpo.

```markdown
> **Deprecado el 2026-05-21.** Usa `/cierre-sesion`. Este skill ya no se mantiene.
```

Casos reales de este ecosistema: `/loop-week` → `/cierre-semana` (el bloque MAA
se absorbió en el ritual de viernes) y `/cierre-dia` → `/cierre-sesion` (una
sesión no es un día).

**Cuándo borrarlo de verdad:** cuando lleve meses sin invocarse y el reemplazo
esté asentado. Antes no: un skill deprecado que redirige cuesta cuatro líneas;
uno borrado deja a alguien tecleando un slash que no existe.

## Qué pasa con el SOP original

Se guarda. Es la única forma de saber, dentro de seis meses, si el skill se
desvió o si el proceso cambió y nadie actualizó el skill.

Dónde ponerlo depende de qué trae: si nombra clientes, montos o personas
concretas, va a una ruta ignorada (`private/`, `scripts/local/`), nunca junto al
skill. Si está sanitizado, `references/sop-origen.md` sirve.

Al modernizar (modo B), la comparación skill-vs-SOP es el primer paso: casi
siempre revela pasos que se dejaron de hacer hace meses y nadie quitó del
documento.

## Ritmo

No hace falta un calendario. Dos momentos naturales:

1. **Cuando el skill falla o improvisa.** Es la señal más honesta.
2. **Cuando cambia lo que el skill da por hecho** — el disparador que anotaste
   en el comentario del `SKILL.md`.

Revisar por calendario skills que funcionan bien es trabajo inventado.
