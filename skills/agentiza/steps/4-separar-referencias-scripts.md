# Paso 4 — Separar referencias y scripts

Con los pasos mapeados, decide qué contenido NO va dentro de los pasos.

## Qué va a `references/`

Material que el agente va a consultar **rara vez**, no en cada ejecución:

- Esquemas de datos (Mongo, SQL, tipos TypeScript)
- Voz de marca, reglas de tono
- Catálogos: lista de herramientas, precios, productos, plantillas
- Ejemplos de conversación end-to-end
- Tabla de errores comunes y cómo resolverlos
- Credenciales (NUNCA valores reales — solo nombres de variables y dónde están)

**Regla:** si lo abro una vez al mes, va a `references/`. Si lo necesito en cada corrida, va dentro del paso.

## Qué va a `scripts/`

Código ejecutable que pasaba de **20 líneas** dentro del SOP original:

- Scripts de Node/TypeScript con tipos vivos
- Shell scripts
- Templates de archivos (`*.tpl`, `*.template.*`)

Cada paso que ejecuta uno de estos scripts solo dice "corre `scripts/<nombre>.ts` con estos parámetros", no embebe el código.

## Qué NO se mueve

Bloques de código de <20 líneas que son **específicos del paso** (ejemplo: un comando de Mongo que solo se usa en el paso 3) se quedan dentro del paso. Mover todo a scripts hace que el agente tenga que abrir 8 archivos para entender 1 paso simple.

## Sanitizar antes de escribir nada

Los SOPs vienen de transcripts de Zoom, del CRM, de correos. Traen datos reales
pegados, y el skill se versiona en git — a veces en un repo que se comparte o
se vende. **Lo que entra aquí, entra para siempre**: borrarlo después no lo
saca del historial.

Nada de esto va a un archivo de skill:

| Qué | En vez de eso |
|---|---|
| Correos de terceros (`ana@cliente.com`) | `contacto@ejemplo.com`, o leerlo de la base en runtime |
| Nombres de personas reales, en ejemplos **o en las reglas** | El rol: "el cliente que más se atrasa" |
| Razón social de un cliente (`Acme Global S.A.`) | El rubro: "el cliente de logística". Una empresa identifica igual que una persona |
| Teléfonos, RFC, direcciones | Placeholders con el formato correcto |
| API keys, tokens, passwords, URIs con credenciales | `[FALTA: definir XXX_API_KEY en .env]` |
| Rutas absolutas con el usuario (`C:/Users/mnbon/...`) | Rutas relativas o `~/` |
| Montos de un deal concreto | Rangos, o la tabla de precios en una `reference` |
| IDs internos que apuntan a un registro real | El formato del ID, no un valor |

**Si el SOP fuente ya los trae pegados:** sanitizar al copiar. No arrastrar el
dato "porque venía así" — el SOP no estaba en git, el skill sí.

Cuando el proceso de verdad necesita datos reales para correr, el skill los
**lee**, no los guarda: de env vars, de la base, o de un archivo ignorado
(`config/*.local.yaml`). El skill dice dónde buscarlos, no cuáles son.

Y si el skill nombra a una persona concreta en algo más que un ejemplo, no va a
`~/.claude/skills/`: va a una ruta ignorada del proyecto.

## Lista a generar

Después de este paso, deberías tener una lista mental:

```
references/
  schema-<dominio>.md
  reglas-tono.md
  catalogo-<algo>.md
  ejemplo-conversacion.md

scripts/
  <accion-principal>.ts
  <utilidad>.sh
```

## Anti-patrones comunes

Ver [`references/anti-patrones.md`](../references/anti-patrones.md) para los errores típicos.
