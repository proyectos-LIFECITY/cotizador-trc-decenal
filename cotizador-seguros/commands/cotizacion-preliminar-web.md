---
description: Cotización preliminar en las páginas web de aseguradoras colombianas (TRC o decenal) usando los datos ya recopilados
argument-hint: "[trc | decenal] [aseguradoras específicas]"
---

# Cotización preliminar web — Aseguradoras colombianas

Con los datos recopilados por `/cotizar-todo-riesgo` o `/cotizar-decenal` (búscalos en la conversación o en los archivos `solicitud_TRC_*.md` / `decenal_cuestionario_datos_*.md`), visita las páginas de las aseguradoras colombianas para obtener cotizaciones preliminares o iniciar el contacto comercial.

Argumento: $ARGUMENTS (tipo de póliza y/o aseguradoras a consultar; si está vacío, pregunta cuál póliza y ofrece la lista completa)

Lee la lista de aseguradoras y URLs en `${CLAUDE_PLUGIN_ROOT}/skills/cotizador-seguros/references/aseguradoras.md`.

## Herramientas

Usa el navegador vía MCP `claude-in-chrome`. Si las herramientas están diferidas, cárgalas en UNA sola llamada a ToolSearch:
`select:mcp__claude-in-chrome__tabs_context_mcp,mcp__claude-in-chrome__navigate,mcp__claude-in-chrome__computer,mcp__claude-in-chrome__read_page,mcp__claude-in-chrome__tabs_create_mcp,mcp__claude-in-chrome__form_input,mcp__claude-in-chrome__get_page_text,mcp__claude-in-chrome__find`

Si el navegador no está conectado, dile al usuario que abra Chrome con la extensión de Claude e intenta de nuevo; NO uses computer-use como reemplazo.

## Procedimiento por aseguradora

1. Navega a la página del producto (TRC / ingeniería / decenal según el caso).
2. Busca: formulario de cotización en línea, botón "cotizar", canal para empresas/corporativo, o datos de contacto comercial (correo, teléfono, formulario de contacto).
3. Si hay formulario de cotización o contacto:
   - Diligéncialo con los datos recopilados (tomador, NIT, ciudad, actividad, presupuesto/valor asegurado, contacto: baronajuandavid@gmail.com salvo que el usuario indique otro).
   - **REGLA DURA: nunca envíes (submit) un formulario sin mostrar antes al usuario un resumen de lo que se va a enviar y recibir su confirmación explícita.** Esto aplica a cada aseguradora por separado.
4. Si no hay cotización en línea (lo usual en TRC/decenal, que son productos suscritos por ingeniería), captura: nombre del producto, requisitos publicados, correo/teléfono del área comercial o de ingeniería, y cualquier simulador disponible.
5. Registra los resultados a medida que avanzas.

## Entregable

Genera `cotizaciones_preliminares_<AAAAMMDD>.md` con una tabla comparativa:

| Aseguradora | Producto | ¿Cotización en línea? | Estado (enviada / contacto obtenido / no ofrece) | Contacto | Notas / requisitos |

Y un resumen final con los siguientes pasos recomendados (a quién llamar/escribir, qué documentos adjuntar según la póliza).

## Límites

- No crees cuentas ni aceptes términos legales en nombre del usuario sin confirmación.
- No ingreses datos financieros sensibles (estados financieros, siniestralidad detallada) en formularios web genéricos; eso va por correo directo al área de ingeniería.
- Si una página exige login o CAPTCHA, repórtalo y pasa a la siguiente.
