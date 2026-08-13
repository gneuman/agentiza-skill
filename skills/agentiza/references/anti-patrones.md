# Anti-patrones al estructurar un skill

Errores típicos que hay que evitar.

## 1. SKILL.md como muro de texto

❌ Meter todo el contenido del SOP en `SKILL.md`.
✅ `SKILL.md` lista los pasos y apunta a archivos. El contenido vive en `steps/`.

## 2. Pasos que mezclan acciones

❌ "Paso 3: buscar contacto, validar deal y generar reporte"
✅ Tres pasos separados: 3-buscar-contacto, 4-validar-deal, 5-generar-reporte.

## 3. Código embebido como bloques markdown gigantes

❌ Un bloque ```typescript de 80 líneas dentro de `steps/4-...md`.
✅ Mover a `scripts/<accion>.ts` y desde el paso decir "ejecuta `scripts/<accion>.ts`".

## 4. Referencias dentro de pasos

❌ Repetir el esquema completo de Mongo en cada paso que lo usa.
✅ `references/schema-<dominio>.md` y los pasos linkean.

## 5. Inventar valores que el SOP no tiene

❌ "Usa la API key XYZ" cuando el SOP nunca la mencionó.
✅ Marcar como `[FALTA: API key — definir variable .env XXX_API_KEY]`.

## 6. Tocar el frontmatter por estética

❌ Reescribir `description` y triggers "para que se vea más limpio" o "más ordenado" mientras modernizas.
✅ Preservar el frontmatter intacto. Un skill que ya dispara bien no se toca: cualquier cambio puede romper la invocación, y el beneficio estético es cero.

**La excepción, y es la única:** si la prueba de descubrimiento del [paso 7](../steps/7-validar-e-instalar.md) falla —el skill no se activa con frases naturales— el `description` **está mal y hay que reescribirlo**. Ahí no es estética, es la causa del fallo.

Regla corta: no lo toques porque te parece feo; sí tócalo porque falló la prueba.

## 7. Pasos sin decisión explícita

❌ Un paso que termina sin decir qué viene después.
✅ Cada paso cierra con: "Si X → ir al paso N+1. Si Y → preguntar al usuario. Si error → ver references/troubleshooting.md."

## 8. Mover snippets cortos a scripts

❌ Mover un comando de 5 líneas de Mongo a `scripts/buscar.ts`.
✅ Si es <20 líneas y específico del paso, déjalo embebido. Mover todo a scripts hace que el agente abra 8 archivos para entender 1 paso.

## 9. Nombres en inglés cuando el contexto es español

❌ `steps/1-IdentifyClient.md`, `references/PricingTable.md`
✅ `steps/1-identificar-cliente.md`, `references/tabla-precios.md`

## 10. Sin pausa de confirmación antes de acciones irreversibles

❌ El skill envía un email al cliente automáticamente.
✅ Paso explícito de "Confirmar con el usuario" antes de cualquier acción irreversible (cobros, envíos públicos, escrituras destructivas).
