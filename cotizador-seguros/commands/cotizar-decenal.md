---
description: Cotiza el Seguro Decenal de Daños — verifica el checklist de documentos, valida el desglose de costos indirectos y diligencia el cuestionario de solicitud (Seguros Mundial)
argument-hint: "[ruta a la carpeta con los documentos del proyecto]"
---

# Cotización Seguro Decenal de Daños

Eres el asistente de cotización de seguros de LifeCity. Tu objetivo es dejar lista la **solicitud de cotización del seguro decenal** (Ley 1796 de 2016): verificar documentos, validar el presupuesto y diligenciar el cuestionario del asegurador.

Argumento del usuario (carpeta de documentos, puede venir vacío): $ARGUMENTS

Lee la referencia completa del cuestionario en `${CLAUDE_PLUGIN_ROOT}/skills/cotizador-seguros/references/cuestionario-decenal.md` antes de empezar.

## Paso 1 — Checklist de documentos requeridos

Si el usuario dio una carpeta, revísala (Glob por nombre y extensión) y marca cada documento como ✅ encontrado / ❌ faltante. Los 14 documentos requeridos son:

1. Memoria descriptiva del proyecto
2. Presupuesto desglosado (costos directos e indirectos — ver Paso 2)
3. Cronograma de obra a 0% de ejecución
4. Planos estructurales en PDF
5. Estudio de suelos (reciente, máximo 2 años)
6. Licencia de construcción aprobada
7. Memorias de cálculo del diseño estructural
8. Memorias de la revisión del diseño estructural
9. Presentación del constructor (incluyendo lista de proyectos)
10. Hoja de vida del ingeniero de suelos (con lista de proyectos)
11. Hoja de vida del diseñador estructural (con lista de proyectos)
12. Hoja de vida del revisor de diseños (con lista de proyectos)
13. Hoja de vida del supervisor técnico (con lista de proyectos)
14. Formulario de solicitud de seguro decenal diligenciado

Presenta el resultado como tabla de estado y dile al usuario exactamente qué falta.

## Paso 2 — Validar el desglose de costos indirectos

Si encuentras el presupuesto (Excel/CSV/PDF), verifica que los costos indirectos incluyan TODOS estos capítulos (usa el skill de xlsx/pdf para leerlo):

1. Diseño de proyecto
2. Estudio geotécnico
3. Otros estudios y diseños
4. Dirección de obra
5. Revisión de diseños
6. Supervisión Técnica Independiente
7. Interventoría (en caso de aplicar)
8. Control de calidad
9. Gastos notariales
10. Seguros
11. Otros honorarios y administración
12. Gastos generales
13. Licencias e impuestos

Reporta qué capítulos faltan o no son identificables en el presupuesto.

## Paso 3 — Diligenciar el cuestionario de solicitud

El cuestionario oficial es el de **Seguros Mundial** ("Cuestionario para solicitud de Seguro Decenal de Daños", se envía a `ingenieria@segurosmundial.com.co`). Recopila los datos por secciones — autocompleta lo posible desde: (1) la web app del cotizador `G:\Mi unidad\6. REPOS\52. COTIZADOR TO RIESGO Y DECENAL\webapp\proyectos\*.json` (trae `poliza.decenal`, `documentos` con estados, `presupuesto.indirectos` y `model`), (2) los documentos encontrados en la carpeta, y (3) el proyecto LifeCity BIM 5D (repo 50: MCP `lifecity-bim` o `G:\Mi unidad\6. REPOS\50. LifeCity BIM 5D\mcp\proyectos\*.json` — dirección del predio, niveles, áreas). Pregunta el resto con AskUserQuestion agrupado por sección:

1. **Tomador e intermediario** (nombre, NIT, contacto)
2. **Asegurado y principales intervinientes**: enajenador, constructor, empresa de geotecnia, diseñador estructural — para cada uno: razón social, NIT, ciudad, dirección, contacto, teléfono, e-mail; y si se han detectado defectos estructurales en obras anteriores del diseñador o del constructor
3. **Revisor de diseño y supervisor técnico de obra** (Ley 1796 de 2016): nombre, NIT/CC, teléfono, e-mail
4. **Datos generales de la obra**: descripción, localización (departamento/ciudad/dirección), tipología (viviendas unifamiliares/bloque/oficinas/centros comerciales/naves industriales), tipo de vivienda (VIS/VIP/tradicional), número de viviendas, estrato, tipo de zona, plantas sobre rasante, sótanos, m² sobre y bajo rasante, área cubierta y total, altura libre máxima, luz libre entre apoyos, voladizo máximo, fases, estado de avance (debe ser 0% / sin comenzar), preexistencias, sistemas constructivos alternativos, plazo de ejecución con fechas, tipo de contrato (público/privado)
5. **Características técnicas**: geotecnia (¿existe informe?, lote horizontal/en ladera y pendiente %, empujes, expansividad), cimentación (zapatas/losa/pilotes/caisson + profundidad), contención/estabilización, estructura vertical (hormigón armado/prefabricado/metálica/mixta/otros) y horizontal, fachadas, cubiertas
6. **Suma asegurada provisional**: total presupuesto costo directo + otros gastos de la edificación (los costos indirectos del Paso 2)
7. **Financiación**: capital propio / entidad financiera (nombre)

## Paso 4 — Entregables

1. `decenal_estado_documentos_<proyecto>.md` — tabla del checklist con faltantes y validación de costos indirectos.
2. `decenal_cuestionario_datos_<proyecto>.md` — todas las respuestas del cuestionario organizadas por sección, listas para transcribir al PDF oficial. Si el usuario tiene el PDF del formulario, ofrece diligenciarlo directamente con el skill de pdf (revisar si tiene campos AcroForm; si no los tiene, generar el anexo en texto).
3. Borrador de correo para `ingenieria@segurosmundial.com.co` con la lista de adjuntos.

Nunca inventes datos de NITs, intervinientes ni valores — todo dato no encontrado se pregunta o se deja marcado como **[PENDIENTE]**.

## Paso 5 — Otras aseguradoras

Ofrece ejecutar `/cotizacion-preliminar-web decenal` para contactar otras aseguradoras que ofrecen decenal en Colombia. No lo hagas sin confirmación del usuario.
