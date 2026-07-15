---
name: cotizador-seguros
description: Cotización de seguros de construcción en Colombia — póliza Todo Riesgo Construcción (TRC/adecuaciones) y Seguro Decenal de Daños (Ley 1796 de 2016). Usar cuando el usuario quiera cotizar, solicitar o preparar documentos para una póliza TRC, decenal, seguro de obra o seguro de adecuaciones, o cuando pregunte por requisitos de aseguradoras colombianas para proyectos de construcción.
---

# Cotizador de seguros de construcción (LifeCity)

Este plugin prepara solicitudes de cotización de dos pólizas para proyectos de construcción en Colombia, integrándose con los proyectos del editor **LifeCity BIM 5D** (repo 50, `G:\Mi unidad\6. REPOS\50. LifeCity BIM 5D`).

## Flujos disponibles

| Necesidad | Comando |
|---|---|
| Póliza Todo Riesgo Construcción / adecuaciones | `/cotizador-seguros:cotizar-todo-riesgo` |
| Seguro Decenal de Daños | `/cotizador-seguros:cotizar-decenal` |
| Cotización preliminar en webs de aseguradoras | `/cotizador-seguros:cotizacion-preliminar-web` |

Si el usuario pide "cotizar el seguro de la obra" sin especificar, pregunta cuál de las dos pólizas necesita (o ambas). La TRC cubre daños durante la construcción; la decenal cubre daños estructurales por 10 años después de entregada (obligatoria para enajenadores de vivienda nueva según Ley 1796 de 2016) y solo se cotiza con la obra a 0% de ejecución.

## Datos del proyecto LifeCity BIM 5D

Fuentes para autocompletar, en orden de preferencia:

1. **Web app del cotizador** (repo 52): `G:\Mi unidad\6. REPOS\52. COTIZADOR TO RIESGO Y DECENAL\webapp\proyectos\*.json` — proyectos guardados desde la app independiente (`webapp/Cotizador.bat`, puerto 8124). Su schema: `{model, poliza:{trc,decenal}, presupuesto:{items,indirectos}, cronograma, documentos, aseguradoras}` — ya trae los datos de póliza estructurados.
2. **MCP `lifecity-bim`** (si está conectado): `listar_proyectos`, `leer_proyecto`.
3. **JSON exportados**: `G:\Mi unidad\6. REPOS\50. LifeCity BIM 5D\mcp\proyectos\*.json`.

Estructura útil del JSON: `predio` (npn, direpred, nom_barrio — proyectos de Cali), `projectType` (obra nueva / remodelación), `levels[]` (niveles → número de plantas), `rooms[]` (habitaciones con uso y dimensiones → áreas), `bim[]` (elementos con `apu` opcional → presupuesto 5D), `designNotes[]`.

## Referencias

- `references/cuestionario-decenal.md` — estructura completa del cuestionario de Seguros Mundial y checklist de documentos/costos indirectos.
- `references/aseguradoras.md` — aseguradoras colombianas con TRC y decenal, URLs y canales de contacto.

## Reglas

- Nunca inventar NITs, valores financieros, siniestralidad ni datos de intervinientes: se extraen de documentos, se preguntan, o quedan **[PENDIENTE]**.
- Nunca enviar formularios web ni correos a aseguradoras sin confirmación explícita del usuario.
- Los entregables se generan en español, listos para enviar al intermediario o a la aseguradora.
