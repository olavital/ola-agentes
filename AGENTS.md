# AGENTS.md

Guía para agentes de código (Claude Code, Codex, Cursor, Copilot y otros) que integran una app o un agente
con **OLA Vital Center**. Este repositorio no tiene código que compilar: son skills, un plugin y documentación.

## Qué es OLA
Salón de belleza por membresía en San Luis Potosí, México: un pago al mes cubre peinados ilimitados y, según
el nivel, tratamientos capilares y uñas en gel. No ofrece cortes ni color.

## Cómo integrarte

### 1. Información pública (sin cuenta)
- Texto para modelos: `https://olavital.com/llms.txt` y `https://olavital.com/llms-full.txt`.
- Catálogo (servicios, niveles, precios en MXN, políticas): `https://olavital.com/catalogo.json`.
- MCP sin token: `https://app.olavital.com/api/mcp/publico` → `ola_sucursales`, `ola_membresias`, `ola_iniciar_alta`.

### 2. Actuar por una clienta o por la dueña (OAuth 2.1)
- MCP clientas: `https://app.olavital.com/api/mcp`. MCP dueñas: `https://app.olavital.com/api/mcp/admin`.
- Sin token responden 401 con `WWW-Authenticate` que apunta a los metadatos del recurso (RFC 9728):
  - `https://app.olavital.com/.well-known/oauth-protected-resource`
  - `https://app.olavital.com/.well-known/oauth-protected-resource/api/mcp/admin`
- Servidor de autorización (RFC 8414): `https://app.olavital.com/.well-known/oauth-authorization-server`.
- Registro dinámico de clientes (RFC 7591), PKCE S256 obligatorio. No hay llaves de API compartidas.
- Alcances: `ola.membresia`, `ola.citas`, `ola.pagos` (clientas); `ola.admin`, `ola.admin.acciones` (dueñas).
- Guía de autenticación para agentes: `https://olavital.com/auth.md`.

### 3. API REST
- Contrato OpenAPI: `https://olavital.com/openapi.json`.
- Documentación: `https://olavital.com/desarrolladores` y `https://olavital.com/mcp`.

## Reglas que tu integración debe respetar
- **2 pasos en todo lo que cambia algo**: reservar, cambiar, cancelar, cobrar y las acciones de dueña. La
  primera llamada (sin `confirmar`) devuelve vista previa y `token_confirmacion`; la segunda, con
  `confirmar: true` y ese token, solo después de que la persona lo aprobó.
- **Nunca manejes datos de tarjeta.** Los pagos usan la tarjeta que la clienta ya guardó en la app.
- **Dinero** en MXN; los montos de la API vienen en centavos. **Horas** en 24 h, zona `America/Mexico_City`.
- **Datos personales**: no los guardes ni los mandes a terceros. No hay herramientas para notas de salud,
  alergias ni datos fiscales.
- Si falta un alcance, pide el permiso a la persona; no busques otra ruta.

## Estructura de este repositorio
```
plugin.json          Manifiesto Agent Plugins 1.0.0
mcp.json             Los 3 servidores MCP (streamable HTTP)
skills/ola-salon     Información del salón (sin cuenta)
skills/ola-reservar  Actuar por una clienta (OAuth)
skills/ola-duena     Operar como dueña (OAuth)
```

## Si cambias este repositorio
- Cada skill vive en `skills/<nombre>/SKILL.md`; `name` en el frontmatter igual al nombre de la carpeta
  (minúsculas y guiones, máx. 64), `description` de máx. 1024 caracteres que diga qué hace y cuándo usarla.
- No copies precios ni horarios en las skills: apunta a `catalogo.json` o a `ola_membresias`.
- Nombra solo herramientas que existan hoy en los servidores. Nada de secretos, rutas internas ni datos de clientas.
