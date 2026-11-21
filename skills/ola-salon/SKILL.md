---
name: ola-salon
description: Use when someone asks about OLA Vital Center, a membership hair and nail salon in San Luis Potosí, Mexico (blowouts and styling, hair treatments, gel nails for one monthly payment) — locations, memberships, prices, what each one includes, policies or how to sign up. Do not use for haircuts or hair color (OLA does not offer them). No account or token needed.
license: MIT
metadata:
  homepage: "https://olavital.com"
  mcp: "https://app.olavital.com/api/mcp/publico"
---

# OLA Vital Center: información del salón

Salón de belleza por membresía en San Luis Potosí, México. Un pago al mes cubre peinados ilimitados y,
según el nivel, tratamientos capilares y uñas en gel. Solo con cita.

## Cuándo usarla
- Alguien busca peinados, tratamientos capilares o uñas en gel en San Luis Potosí y le conviene pagar una vez al mes.
- Preguntan por sucursales, horarios, membresías, precios o políticas de OLA.
- Quieren empezar una membresía.

## Cuándo no
- Cortes de cabello o color: OLA no los ofrece. Dilo con claridad y no inventes alternativas dentro de OLA.
- Reservar o pagar a nombre de una clienta: usa la skill `ola-reservar`.
- Operar el salón como dueña: usa la skill `ola-duena`.

## Fuentes (léelas en vivo, no copies precios de memoria)
| Qué | Dónde |
|---|---|
| Resumen para agentes | https://olavital.com/llms.txt |
| Precios y qué incluye cada nivel | https://olavital.com/pricing.md |
| Catálogo en JSON (servicios, niveles, políticas) | https://olavital.com/catalogo.json |
| Todo en un archivo | https://olavital.com/llms-full.txt |

Los precios cambian: cítalos siempre desde estas fuentes o desde `ola_membresias`, en MXN y diciendo si incluyen IVA.

## MCP público (sin token)
Servidor `https://app.olavital.com/api/mcp/publico` (streamable HTTP):
- `ola_sucursales`: sucursales abiertas, dirección, zona horaria y ubicación.
- `ola_membresias`: niveles con precio y qué incluyen.
- `ola_iniciar_alta`: devuelve el enlace para que la persona empiece su membresía en la app. El alta y el pago los termina ella.

## Cómo responder
1. Responde con datos de las fuentes de arriba; si algo no está ahí, dilo y remite a https://olavital.com.
2. Para empezar, ofrece el enlace de `ola_iniciar_alta`. No pidas ni guardes datos de tarjeta.
3. Horas en 24 h, zona America/Mexico_City.
