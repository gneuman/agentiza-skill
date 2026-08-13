# Paso 6 — Detectar huecos y reportar

Antes de cerrar, entrega dos artefactos al usuario.

## Artefacto 1 — Tabla de mapeo

Tabla que muestra de dónde salió cada archivo:

| Sección del SOP original | Archivo generado |
|---------------------------|------------------|
| Intro / contexto | `SKILL.md` (descripción) |
| "Paso 1: buscar contacto" | `steps/1-identificar-contacto.md` |
| "Paso 2: validar deal" | `steps/2-validar-deal.md` |
| Tabla de productos al final | `references/catalogo-productos.md` |
| Bloque de código de 80 líneas en sección 4 | `scripts/crear-deal.ts` |
| (no estaba en el SOP) | `references/idioma-y-tono.md` (default del template) |

Esto sirve para que el usuario verifique que nada se perdió.

## Artefacto 2 — Lista de huecos `[FALTA: ...]`

Cualquier valor que el SOP no especificó pero el skill necesita debe ir marcado como `[FALTA: ...]` dentro del archivo correspondiente, **y** repetido al final del reporte:

```
Huecos detectados:
- steps/2-validar-deal.md → [FALTA: cuál es el endpoint de validación]
- references/catalogo-productos.md → [FALTA: precio del producto Premium]
- scripts/crear-deal.ts → [FALTA: nombre exacto de la colección Mongo]
```

## Reglas para huecos

1. **No inventes valores.** Si el SOP no dice cuánto cuesta algo o cuál es el endpoint, márcalo como `[FALTA: ...]`.
2. **Sé específico en el hueco.** No basta `[FALTA: precio]`. Mejor: `[FALTA: precio mensual del plan Pro en MXN — el SOP solo decía "más caro"]`.
3. **Credenciales SIEMPRE son huecos.** Nunca pongas valores reales de API keys ni passwords en archivos de skill. Marca como `[FALTA: definir variable .env XXX_API_KEY]`.

4. **PII y datos de terceros también son huecos.** Correos, nombres de clientes,
   teléfonos, montos de un deal concreto: nada de eso va a un archivo de skill.
   Ver la sección de sanitizar en
   [`4-separar-referencias-scripts.md`](4-separar-referencias-scripts.md).

## Cierre

Termina el reporte con:

```
✓ Skill /<nombre> generado.

Para instalarlo:
  1. Crea ~/.claude/skills/<nombre>/
  2. Pega cada archivo en su path correspondiente
  3. Resuelve los <N> huecos marcados como [FALTA: ...]
  4. Reinicia Claude Code
```

## Decisión final

Generar no es terminar: un skill puede estar impecable por dentro y no
dispararse nunca. Sigue al [Paso 7](7-validar-e-instalar.md) para instalarlo y
probar que se activa **solo**, con frases naturales y no únicamente escribiendo
el slash.
