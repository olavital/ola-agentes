---
name: ola-duena
description: Use when the owner (dueña) of an OLA Vital Center salon wants an AI agent to run the back office — today's appointments and arrivals, weekly schedule, cash close, finances, memberships, team hours and payroll, inventory, leads, reviews, promotions and messages — through OLA's owner MCP server. Every change is a two-step preview the owner approves; high-risk actions also need a separate scope.
license: MIT
metadata:
  homepage: "https://olavital.com/desarrolladores"
  mcp: "https://app.olavital.com/api/mcp/admin"
---

# OLA Vital Center: operar como dueña

Actúas a nombre de la dueña de una sucursal, con su permiso. Los datos de clientas son personales:
no los copies fuera de la conversación ni los guardes.

## Conexión
- Servidor MCP: `https://app.olavital.com/api/mcp/admin` (streamable HTTP).
- OAuth 2.1 con PKCE S256 y registro dinámico. Metadatos del recurso:
  https://app.olavital.com/.well-known/oauth-protected-resource/api/mcp/admin
- Alcances:
  - `ola.admin`: consultas y borradores.
  - `ola.admin.acciones`: acciones de alto riesgo. La dueña lo acepta aparte.
- "Agente de confianza": la dueña puede dar una conexión de solo lectura (`ola.admin`) que se cierra tras 30 días sin uso
  y dura 90 como tope.

## Consultas y borradores (`ola.admin`)
- Día: `ola_citas_hoy`, `ola_llegadas_hoy`, `ola_resumen_del_dia`, `ola_horarios_libres`, `ola_agenda_semana`.
- Dinero: `ola_cierre_del_dia`, `ola_resumen_finanzas`, `ola_reporte_ejecutivo`, `ola_estado_resultados`, `ola_exportar_csv`.
- Clientas y ventas: `ola_buscar_clientas`, `ola_membresias`, `ola_prospectos`.
- `ola_alta_clienta` crea la ficha de una clienta en 2 pasos (vista previa y confirmar). No cobra ni abre membresía.
- Equipo: `ola_equipo`, `ola_horas_equipo`, `ola_nomina_comisiones`.
- Inventario: `ola_inventario_bajo`.
- Reputación y marketing: `ola_resenas`, `ola_promociones`, `ola_borrador_promocion`, `ola_borrador_mensaje`.

Los borradores no se publican ni se envían: la dueña los revisa en el panel.

## Acciones de alto riesgo (`ola.admin.acciones`, siempre en 2 pasos)
`ola_agendar_cita`, `ola_mover_cita`, `ola_estado_cita`, `ola_cobrar_venta`, `ola_archivar_clienta`,
`ola_baja_integrante`, `ola_publicar_promocion`, `ola_compra_inventario`, `ola_responder_resena`.

1. Llama sin `confirmar`: recibes una vista previa y un `token_confirmacion`.
2. Muéstrale la vista previa a la dueña. Solo si la aprueba, llama otra vez con los mismos datos,
   `confirmar: true` y ese token.

Cada acción confirmada queda en la bitácora del salón.

## Reglas
- Nunca confirmes una acción sin la aprobación explícita de la dueña en esa conversación.
- No hay herramientas para notas de salud, alergias ni datos fiscales: no los pidas.
- Dinero en MXN. Horas en 24 h, zona de la sucursal (America/Mexico_City).
- Si falta un alcance, pídeselo a la dueña; no busques otra ruta.
