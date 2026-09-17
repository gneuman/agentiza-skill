---
name: oferta
description: >
  Audita una propuesta comercial contra las cinco palancas que hacen que una
  oferta se compre sola (promesa, bonos, garantia, forma de pago,
  urgencia/escasez) y devuelve semaforo por elemento con los cambios concretos.
  Sirve para revisar antes de enviar y para diagnosticar una propuesta que no
  cerro, traduciendo la respuesta del cliente al elemento que falta.
  Use when: "revisa esta propuesta", "por que no cierra", "mejora la oferta",
  "audita la cotizacion", "la oferta esta mal", "/oferta", "no me compraron",
  "el cliente dice que esta caro", "quedo en que lo iba a ver",
  "me dijo que lo iba a pensar".
---

# /oferta — Por que no cierra tu propuesta

Casi siempre el problema no es el producto: es el **empaque**. Producto es lo que
entregas; oferta es como lo presentas. Un buen producto con mala oferta pierde
contra un producto mediocre bien ofertado, y esa derrota se lee como "esta caro".

**El modelo:** el cliente dice que si cuando percibe **mas recompensa que
riesgo**. Cinco palancas mueven ese balance — dos suben la recompensa, dos bajan
el riesgo, una empuja a decidir hoy.

```
      RECOMPENSA                RIESGO
   ┌──────────────┐        ┌──────────────┐
   │ 1. PROMESA   │        │ 3. GARANTIA  │   ← peso alto
   │ 2. BONOS     │        │ 4. PAGO      │
   └──────────────┘        └──────────────┘
              ╲                  ╱
               ╲ 5. URGENCIA    ╱            ← el fulcro: por que HOY
                ╲  y ESCASEZ   ╱
```

## Como se usa

1. **Lee la propuesta completa.** Archivo, URL, o pegada en el chat. Si todavia
   no existe, este skill sirve igual para escribirla.
2. **Si ya se envio y no cerro, pide la respuesta del cliente.** Es el dato mas
   valioso que existe y casi siempre nombra el elemento que falta — ver
   `references/traducir-objeciones.md`. Sin ese texto estas adivinando.
3. **Califica las cinco** con `references/las-cinco-palancas.md`. Semaforo por
   elemento.
4. **Entrega el semaforo primero, la reescritura despues.** Que el humano decida
   si aplica todo o parte.
5. **Reescribe solo lo rojo y lo amarillo.** Lo verde no se toca.

## Las reglas duras

<important>
1. **La promesa se juzga por una prueba: ¿la puede REPETIR el cliente en una junta
   a la que tu no entras?** Si necesita el documento enfrente, no hay promesa. En
   B2B esa junta siempre existe. Casi siempre el problema esta aqui.
2. **Un problema bien dicho NO es una promesa.** Describir lo que esta roto es el
   *antes*; la promesa es el *despues*.
3. **Promete la CAPACIDAD, no el caso de uso.** El dolor que el cliente nombro es
   por donde se empieza, no lo que se vende. Si tu promesa nombra un solo proceso
   y el cliente dice "esta caro", **tiene razon** — tu promesa esta compitiendo
   contra tu precio.
4. **La garantia cubre solo lo que TU controlas.** La prueba: *"¿puedo cumplir
   esto yo solo, sin que nadie del lado del cliente mueva un dedo?"*. Si la
   respuesta es no, estas garantizando el desempeno ajeno.
5. **Ante "esta caro", la respuesta es la garantia, no el descuento.** El
   descuento cuesta siempre y ensena que tu numero cede cuando alguien empuja.
6. **La escasez tiene que ser real.** Un cupo inventado se nota, y cuando se nota
   te lleva por delante la credibilidad de todo lo demas.
7. **Nunca inventes numeros para llenar un hueco.** Si no hay baseline, se dice
   "no se mide hoy" y medirlo se vuelve el primer entregable. Un hueco declarado
   es mas fuerte que un numero que el cliente sabe que inventaste.
8. **Al corregir una palanca, barre el documento entero.** El error se filtra: una
   promesa encogida contamina la garantia y el "que estas comprando".
</important>

## Referencias

| Archivo | Cuando abrirlo |
|---|---|
| `references/las-cinco-palancas.md` | Siempre. La rubrica de calificacion con ejemplos |
| `references/traducir-objeciones.md` | Cuando hay respuesta del cliente que descifrar |
| `references/formas-de-pago.md` | Cuando la forma de pago salio roja o amarilla |
| `references/caso-real.md` | Caso completo: de 4 rojos a propuesta reescrita, con los 3 errores del camino |

## El entregable

Siempre en este orden:

```
## Semaforo
| Palanca | Score | Por que |
(una linea por palanca, con la cita textual de la propuesta que lo evidencia)

## El diagnostico en una frase
(cual de las cinco esta costando el cierre — normalmente es una, no cinco)

## Los cambios
(concretos y copiables, no "deberias considerar")
```

## Si la propuesta tiene las cinco en verde y aun asi no cierra

El problema no es la oferta — es precio, momento o interlocutor, y eso se ataca
distinto. No sigas reescribiendo el documento.

---

**Origen.** Las cinco palancas vienen del trabajo de Taki Moore sobre offer
upgrades. Lo que agrega este skill es el metodo de auditoria, la tabla para
traducir objeciones y un caso real documentado — construido aplicandolo a
propuestas B2B de servicio.

Por [Gabriel Neuman](https://gabrielneuman.com) · GNB Labs.
El desarrollo completo del metodo:
[gabrielneuman.com/blog/por-que-no-cierran-tus-propuestas](https://gabrielneuman.com/blog/por-que-no-cierran-tus-propuestas)
