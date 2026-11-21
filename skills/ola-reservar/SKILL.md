---
name: ola-reservar
description: Use when acting for a member (clienta) of OLA Vital Center, the membership salon in San Luis Potosí, Mexico, to see her membership, find open times, book, reschedule or cancel an appointment, or pay what she owes. Connects to OLA's member MCP server with her OAuth 2.1 permission; every booking, cancellation and payment is shown to her first and only confirmed after she approves.
license: MIT
metadata:
  homepage: "https://olavital.com/desarrolladores"
  mcp: "https://app.olavital.com/api/mcp"
---

# OLA Vital Center: reservar como clienta

Actúas a nombre de una clienta, con su permiso. Puedes exactamente lo que ella puede en la app, nada más.

## Conexión
- Servidor MCP: `https://app.olavital.com/api/mcp` (streamable HTTP).
- OAuth 2.1 con PKCE S256 y registro dinámico de clientes (RFC 7591). Sin token responde 401 con
  `WWW-Authenticate` que apunta a los metadatos.
- Metadatos del recurso: https://app.olavital.com/.well-known/oauth-protected-resource
- Servidor de autorización: https://app.olavital.com/.well-known/oauth-authorization-server
- Alcances: `ola.membresia` (ver su membresía y perfil), `ola.citas` (reservar, cambiar, cancelar).
  Pagar pide además `ola.pagos`; pídelo solo si ella quiere pagar.

## Herramientas
| Herramienta | Para qué |
|---|---|
| `ola_mi_membresia` | Nivel, estado, hasta cuándo está pagada y lo usado del mes |
| `ola_mi_perfil` | Nombre, correo y sucursal (sin teléfono ni datos de salud o fiscales) |
| `ola_opciones_reserva` | Servicios y estilistas que puede reservar |
| `ola_horarios_disponibles` | Horarios libres del mes |
| `ola_reservar_cita` | Reserva (2 pasos) |
| `ola_cambiar_cita` | Cambia día, hora o estilista (2 pasos). No reserves y canceles por separado |
| `ola_cancelar_cita` | Cancela (2 pasos) |
| `ola_pagar` | Paga su adeudo con la tarjeta que ya guardó (2 pasos) |
| `ola_mis_referidos` | Su enlace para invitar amigas y cuántas se activaron (aquí no se mandan mensajes) |

## Flujo para reservar
1. `ola_opciones_reserva` → elige servicio y, si ella quiere, estilista.
2. `ola_horarios_disponibles` para el mes.
3. `ola_reservar_cita` **sin** `confirmar`: devuelve la vista previa y un `token_confirmacion`.
4. Muéstrale día, hora, servicio y sucursal. Solo si lo aprueba, llama otra vez con los mismos datos,
   `confirmar: true` y ese `token_confirmacion`.

Cancelar y cambiar siguen el mismo patrón de 2 pasos.

## Pagos
- `ola_pagar` sin `confirmar` muestra concepto, monto y referencia; no cobra.
- Enséñale el monto. Solo si lo aprueba, llama con `confirmar: true` y la `referencia`.
- El monto lo fija OLA; repetir con la misma referencia no cobra dos veces.
- Nunca pidas, recibas ni escribas datos de tarjeta.

## Reglas
- Nunca confirmes sin la aprobación explícita de la clienta en esa conversación.
- Horas en 24 h, zona America/Mexico_City. Cancelación sin costo con la anticipación que marcan las políticas
  (https://olavital.com/catalogo.json, campo `politicas`).
- Si un error dice que falta un alcance, pide el permiso; no intentes otra ruta.
