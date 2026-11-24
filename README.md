# OLA Vital Center para agentes

Skills, plugin y guía para que un agente de IA trabaje con **OLA Vital Center**, un salón de belleza por
membresía en San Luis Potosí, México. Aquí no hay código de la app ni datos de clientas: solo instrucciones
y la configuración de los servidores MCP públicos.

*English: agent skills, an Agent Plugin and an AGENTS.md for OLA Vital Center, a membership hair and nail
salon in San Luis Potosí, Mexico. No app code and no customer data.*

## Instalar las skills

```bash
npx skills add olavital/ola-agentes
```

Una sola: `npx skills add olavital/ola-agentes --skill ola-reservar`

| Skill | Para quién | Necesita |
|---|---|---|
| [`ola-salon`](skills/ola-salon/SKILL.md) | Cualquiera que pregunte por OLA: sucursales, membresías, precios, cómo empezar | Nada |
| [`ola-reservar`](skills/ola-reservar/SKILL.md) | Un agente que actúa por una clienta: reservar, cambiar, cancelar, pagar | OAuth de la clienta |
| [`ola-duena`](skills/ola-duena/SKILL.md) | Un agente de la dueña: agenda, caja, finanzas, equipo, inventario, reseñas | OAuth de la dueña |

## Servidores MCP

| Servidor | URL | Autenticación |
|---|---|---|
| Público | `https://app.olavital.com/api/mcp/publico` | Ninguna |
| Clientas | `https://app.olavital.com/api/mcp` | OAuth 2.1 (PKCE, registro dinámico) |
| Dueñas | `https://app.olavital.com/api/mcp/admin` | OAuth 2.1 (PKCE, registro dinámico) |

Están declarados en [`mcp.json`](mcp.json) y el plugin en [`plugin.json`](plugin.json)
([Agent Plugins 1.0.0](https://agent-plugins.org/specification)).

## Documentación

- API de OLA Vital Center (desarrolladores): https://olavital.com/desarrolladores · [English](https://olavital.com/en/developers)
- OLA Vital MCP server: https://olavital.com/mcp · [English](https://olavital.com/en/mcp) · paquete local [`olavital-mcp`](https://www.npmjs.com/package/olavital-mcp)
- olavital CLI (`npx olavital`): https://olavital.com/cli · [English](https://olavital.com/en/cli) · paquete [`olavital`](https://www.npmjs.com/package/olavital)
- API (OpenAPI): https://olavital.com/openapi.json
- Para agentes: https://olavital.com/llms.txt · https://olavital.com/auth.md
- Guía para agentes de código: [AGENTS.md](AGENTS.md)

## Seguridad

Las acciones que reservan, cobran o cambian algo siempre van en 2 pasos: vista previa y confirmación de la
persona. Nunca se piden datos de tarjeta. Para reportar un problema de seguridad:
https://app.olavital.com/.well-known/security.txt

## Licencia

[MIT](LICENSE)
