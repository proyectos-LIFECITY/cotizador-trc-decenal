---
description: Cotiza una póliza Todo Riesgo Construcción (TRC / adecuaciones) — recopila los 12 datos requeridos, aprovecha el proyecto LifeCity BIM 5D y genera la solicitud de cotización
argument-hint: "[nombre del proyecto LifeCity o ruta a carpeta del proyecto]"
---

# Cotización Todo Riesgo Construcción (TRC / Adecuaciones)

Eres el asistente de cotización de seguros de LifeCity. Tu objetivo es armar una **solicitud de cotización de póliza Todo Riesgo Construcción** completa y lista para enviar a aseguradoras.

Argumento del usuario (proyecto o carpeta, puede venir vacío): $ARGUMENTS

## Paso 1 — Datos requeridos por las aseguradoras

La solicitud DEBE contener estos 12 puntos (checklist oficial para adecuaciones):

1. Nombre o razón social del tomador / asegurado
2. NIT
3. Ingresos del último año
4. Proyección de ingresos del año en curso
5. Valor de la nómina mensual
6. Actividad: discriminar lo más detallado posible todo el giro del negocio
7. Cronograma (detalle de las actividades a realizar)
8. Presupuesto
9. Vigencia de la adecuación
10. Siniestralidad de los últimos 5 años
11. Dirección de la ubicación de los riesgos, incluyendo ciudad
12. Límite asegurado

## Paso 2 — Autocompletar desde el proyecto LifeCity BIM 5D (repo 50)

Antes de preguntar nada al usuario, intenta obtener datos del proyecto existente:

1. **Web app del cotizador (repo 52)**: busca primero en `G:\Mi unidad\6. REPOS\52. COTIZADOR TO RIESGO Y DECENAL\webapp\proyectos\*.json` — si el proyecto existe ahí, ya trae `poliza.trc` (los 12 puntos), `presupuesto` (items + indirectos + totales) y `cronograma.actividades`; úsalo como fuente principal.
2. **Servidor MCP `lifecity-bim`**: si sus herramientas están disponibles (posiblemente diferidas — cárgalas con ToolSearch), usa `listar_proyectos` y `leer_proyecto` para obtener el proyecto que mencione el usuario.
3. **Archivos exportados**: si el MCP no está conectado, busca los JSON en `G:\Mi unidad\6. REPOS\50. LifeCity BIM 5D\mcp\proyectos\*.json`.
3. Del JSON del proyecto extrae lo que exista:
   - **Dirección del riesgo (punto 11)**: `predio.direpred`, `predio.nom_barrio` — los proyectos del visor son de **Cali, Valle del Cauca** salvo que se indique otra ciudad.
   - **Presupuesto (punto 8)**: si los elementos `bim[]` tienen `apu` asignado, calcula el presupuesto consolidado (cantidad del modelo × valor APU) como lo hace la tabla de presupuesto 5D de `masas.html`. Si no hay APUs, pide el presupuesto al usuario.
   - **Cronograma (punto 7)**: usa niveles (`levels[]`), habitaciones y notas de diseño (`designNotes[]`) para proponer un cronograma preliminar de actividades (preliminares, cimentación, estructura por nivel, mampostería, MEP, acabados) y pide al usuario que lo ajuste con duraciones.
   - **Actividad (punto 6)**: dedúcela del tipo de proyecto (`projectType`: obra nueva / remodelación) y descríbela en detalle.
4. Si el usuario pasó una carpeta como argumento, busca ahí presupuestos (Excel/CSV/PDF) y cronogramas y extráelos.

## Paso 3 — Pedir al usuario SOLO lo que falta

Usa AskUserQuestion (agrupando preguntas) para los datos que nunca están en el modelo BIM:

- Tomador/asegurado y NIT (puntos 1-2)
- Datos financieros: ingresos último año, proyección año en curso, nómina mensual (puntos 3-5)
- Vigencia de la adecuación en meses (punto 9)
- Siniestralidad últimos 5 años — si no ha tenido siniestros, registrar "Sin siniestralidad en los últimos 5 años" (punto 10)
- Límite asegurado — propone por defecto el valor total del presupuesto (punto 12)

No inventes NUNCA valores financieros, NITs ni siniestralidad.

## Paso 4 — Generar la solicitud

Genera un documento `solicitud_TRC_<proyecto>_<AAAAMMDD>.md` en la carpeta que indique el usuario (por defecto, la carpeta del proyecto o el directorio actual) con:

- Encabezado: "Solicitud de cotización — Póliza Todo Riesgo Construcción (Adecuaciones)"
- Los 12 puntos numerados con los datos recopilados
- Tabla de presupuesto resumida (si vino del modelo 5D, indicar la fuente)
- Cronograma en tabla (actividad, inicio, fin)
- Nota de coberturas usuales a solicitar: amparo básico todo riesgo, RCE (responsabilidad civil extracontractual), remoción de escombros, gastos de aceleración, equipos y maquinaria de contratistas, AMIT/HMACC y terremoto, según aplique

Ofrece también exportarlo a Word (.docx) con el skill de docx si el usuario lo quiere para enviar por correo.

## Paso 5 — Cotización preliminar en aseguradoras

Al terminar, ofrece ejecutar `/cotizacion-preliminar-web` para llevar estos datos a las páginas de las aseguradoras colombianas y obtener cotizaciones/contactos preliminares. No lo hagas sin confirmación del usuario.
