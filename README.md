# Red Judicial — Extensión para Gemini CLI

> Plug-in legal chileno para Gemini CLI. Conecta tu terminal con el corpus oficial de jurisprudencia de la Corte Suprema, leyes vigentes (BCN) y doctrina académica chilena. Cada respuesta trae citas verificables a las fuentes.

[![Gemini CLI Extension](https://img.shields.io/badge/Gemini%20CLI-Extension-blue)](https://github.com/google-gemini/gemini-cli) [![npm](https://img.shields.io/npm/v/@redjudicial/mcp-server)](https://www.npmjs.com/package/@redjudicial/mcp-server) [![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)

---

## Qué hace

Esta extensión expone al Gemini CLI una sola herramienta: `redjudicial_search`. Con ella, Gemini puede:

- Buscar **sentencias por Rol exacto** (ej: `Rol 22069-2024`) y traer el texto completo con encabezado, considerandos (cuando los hay) y parte resolutiva.
- Buscar **temáticamente** ("requisitos para indemnización por accidente del trabajo", "principio pro reo en sentencias recientes").
- Consultar **leyes vigentes BCN** (Código Civil, Código del Trabajo, leyes especiales, etc.).
- Consultar **doctrina académica** chilena.
- Devolver **cita verificable** en `juris.pjud.cl` o `bcn.cl/leychile` por cada resultado.

## Para quién

- Abogados litigantes chilenos que trabajan desde terminal.
- Investigadores y académicos del derecho chileno.
- Desarrolladores de productos legaltech que integran flujos en Gemini CLI.

## Requisitos previos

1. **Gemini CLI instalado** ([instrucciones oficiales](https://github.com/google-gemini/gemini-cli)).
2. **Cuenta activa en Red Judicial** con un plan que incluya acceso al MCP (MCP Independiente, US$19,90/mes en adelante). Crea tu cuenta en [ia.redjudicial.cl](https://ia.redjudicial.cl).

La extensión se conecta **directamente** al servidor de Red Judicial vía OAuth — no instala paquetes locales ni requiere claves de API. La primera vez, Gemini CLI abre el navegador para que inicies sesión en Red Judicial y autorices el acceso.

## Instalación

```bash
gemini extensions install https://github.com/nicobarriga/gemini-cli-extension-redjudicial
```

O agrégalo manualmente al directorio `~/.gemini/extensions/`.

La primera vez que invoques una herramienta de `redjudicial`, Gemini CLI te pedirá autorizar el acceso vía OAuth a tu cuenta de Red Judicial.

## Uso

```bash
gemini

> Búscame la sentencia Rol 22069-2024 de la Corte Suprema y resume sus considerandos.

> ¿Qué dice el artículo 1545 del Código Civil sobre la fuerza obligatoria de los contratos?

> ¿Cuál es el plazo para demandar nulidad de despido en Chile? Cita ley y jurisprudencia reciente.
```

## Herramienta expuesta

### `redjudicial_search`

| Parámetro | Tipo | Descripción |
|---|---|---|
| `query` | string (requerido) | Rol exacto, número de norma o consulta temática en lenguaje natural. |
| `sources` | array opcional | Filtra qué fuentes consultar: `leyes`, `jurisprudencia`, `doctrina`. Por defecto consulta las tres. |
| `limit` | integer opcional | Cantidad máxima de fragmentos a devolver (default 10, máximo 30). |

### Respuesta estructurada

Cada resultado incluye:

- `cite_id` — identificador único de cita.
- `source_type` — `leyes` / `jurisprudencia` / `doctrina`.
- `tipo_resolucion` (solo jurisprudencia) — `confirmacion_simple` / `sentencia_con_considerandos` / `inadmisibilidad`.
- `tiene_considerandos_propios` (boolean).
- `appeals_court` — Si la CS confirma, indica la sentencia de instancia para derivar.
- `normas_citadas` — Cruce normativo (jerarquía, lex specialis, vigencia).
- `url` — Enlace verificable a `juris.pjud.cl` o `bcn.cl`.

## Anti-alucinación

La herramienta incluye reglas DURAS en el tool description que el modelo debe respetar:

- Nunca inventar considerandos.
- Si la sentencia es confirmación simple (no tiene considerandos propios), declararlo explícitamente y derivar al fallo de instancia.
- Citas textuales solo del campo `cita_textual`, no parafrasear como textual.
- Al final de cada respuesta, recordar al usuario verificar en `juris.pjud.cl`.

## Privacidad y datos

- Los datos del corpus (leyes, jurisprudencia, doctrina) están alojados en **AWS São Paulo** (sa-east-1, LATAM — no USA).
- Soberanía bajo Ley 21.719 (Chile, vigente diciembre 2026).
- Centro de confianza: [https://ia.redjudicial.cl/legal](https://ia.redjudicial.cl/legal).
- Política de privacidad: [https://ia.redjudicial.cl/legal/privacidad](https://ia.redjudicial.cl/legal/privacidad).

## Operador

Servicio operado por **Simpley SpA**, RUT 77.983.964-8, Chile, bajo la marca Red Judicial.

## Soporte

- Sitio: [https://ia.redjudicial.cl](https://ia.redjudicial.cl)
- Correo: contacto@redjudicial.cl
- Centro de confianza: [https://ia.redjudicial.cl/legal](https://ia.redjudicial.cl/legal)

## Licencia

MIT — ver [LICENSE](LICENSE).

---

**Topic recomendado para discovery automático en gallery oficial Gemini CLI**: `gemini-cli-extension`
