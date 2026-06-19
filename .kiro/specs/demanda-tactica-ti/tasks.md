# Implementation Plan: Sistema de Gestión de Demanda Táctica de TI

## Overview

El plan de implementación cubre 10 semanas de trabajo con un equipo de 5 personas a dedicación parcial (4–8 horas/persona/semana). Se organiza en 4 fases: Preparación, Registro, Consulta/Análisis y Capacidad/Integración. Cada semana termina con un entregable demostrable para sesión de feedback.

## Tasks

## FASE 0 — Preparación del Entorno

- [ ] 1. Configurar entorno base y modelo de datos en SharePoint
  - Crear sitio SharePoint dedicado al proyecto
  - Crear lista **Iniciativas** con todos los campos del modelo de datos
  - Crear lista **Anexos_02** con todos los campos del modelo de datos
  - Crear lista **Backlog** con todos los campos del modelo de datos
  - Crear lista **Demanda_Tactica** con todos los campos del modelo de datos
  - Crear lista **Prioridades_Gerencia** con todos los campos del modelo de datos
  - Crear lista **Historial_Estados** con todos los campos del modelo de datos
  - Crear lista **Auditoria** con todos los campos del modelo de datos
  - Configurar grupos de seguridad en Azure AD: Gestor_Demanda_TI y Representante_Gerencia
  - Aplicar permisos por rol a cada lista según la matriz de permisos del diseño
  - Cargar datos de prueba representativos (mínimo 10 requerimientos en distintos estados)
  - _Requerimientos: 1, 10_

---

## FASE 1 — Pantallas de Registro

- [ ] 2. Construir Canvas App TI — estructura base y Registro de Anexo 02
  - Crear Canvas App TI con navegación lateral (4 secciones)
  - Implementar formulario de creación de Anexo 02 con validaciones de campos obligatorios
  - Implementar vista de lista de Anexos con pestañas: Pendiente / Firmado / Todos
  - Implementar vista previa de datos antes de confirmar el guardado
  - Implementar formulario de firma con campos: fecha, firmante gerencia, firmante TI
  - Crear Flow Power Automate: Firma de Anexo → Alta automática en Backlog con estado "Registrado"
  - Registrar en Historial_Estados al crear Backlog desde firma
  - _Requerimientos: 3_

- [ ] 3. Implementar gestión del Backlog y sus transiciones de estado
  - Sección Backlog en Canvas App TI: lista con filtros (gerencia, gestor, estado, fechas)
  - Formulario de edición de ítem de Backlog (actualizar estados, ingresar N° ClearQuest)
  - Flow: Ingreso de N° ClearQuest → cambio automático a "Evaluado" + Historial_Estados
  - Flow: Cambio de estado en Comité (Priorizado/Rechazado/Paralizado) → actualizar Backlog + Historial_Estados
  - Flow: Estado "Priorizado" → crear ítem en Demanda_Tactica
  - Validación: impedir ingreso al Backlog sin Anexo_02 firmado asociado
  - _Requerimientos: 1, 4_

- [ ] 4. Implementar registro de Iniciativas y trazabilidad completa
  - Formulario de registro de Iniciativa con campos obligatorios y validaciones
  - Lista de Iniciativas con filtros en Canvas App TI
  - Asociación de Iniciativa existente al crear Anexo 02
  - Visualización de la cadena completa (Iniciativa → Anexo → Backlog → Demanda) en panel de detalle
  - Historial de estados visible en el panel de detalle de cualquier requerimiento
  - _Requerimientos: 2_

---

## FASE 2 — Pantallas de Consulta y Análisis

- [ ] 5. Construir Pantalla de Consultas para el área de Demanda TI
  - Tabla de requerimientos con todos los campos definidos en el requisito 5
  - Filtros múltiples simultáneos: gerencia, gestor, Estado_Backlog, Estado_Demanda, rango de fechas
  - Indicadores visuales de estado (colores diferenciados por cada valor de estado)
  - Panel lateral de detalle con historial de cambios al seleccionar un requerimiento
  - Función de exportación a Excel (con filtros aplicados)
  - Actualización de datos dentro de la sesión sin recarga manual (máx. 30 seg. de latencia)
  - _Requerimientos: 5_

- [ ] 6. Construir Portal por Gerencia (Canvas App Gerencias)
  - Crear Canvas App Gerencias con filtro automático por gerencia del usuario autenticado
  - Tarjetas resumen: total, por iniciativas, backlog y demanda táctica
  - Tabla de requerimientos agrupados con pestañas por etapa del ciclo de vida
  - Filtros por etapa y por Estado_Backlog dentro del portal
  - Panel de detalle al seleccionar un requerimiento
  - Reporte Power BI: gráfico de distribución por Estado_Demanda (donut)
  - Reporte Power BI: gráfico de barras por Estado_Backlog
  - Embeber reportes Power BI en Canvas App Gerencias
  - _Requerimientos: 6_

- [ ] 7. Implementar Priorización por Gerencia
  - Lista ordenable de requerimientos en Canvas App Gerencias (drag & drop o botones subir/bajar)
  - Guardar nuevo orden en lista Prioridades_Gerencia con registro de fecha y usuario
  - Recalcular posiciones de todos los ítems al insertar en posición ocupada
  - Mostrar historial de los últimos 3 cambios de orden con fecha y usuario
  - Confirmación explícita antes de guardar el nuevo orden
  - Flow: Notificar al gestor TI cuando la gerencia cambia sus prioridades
  - _Requerimientos: 7_

---

## FASE 3 — Evaluación de Capacidad e Integración

- [ ] 8. Construir Pantalla de Evaluación de Capacidad
  - Formulario de propuesta de fechas (inicio y fin) para nuevo requerimiento
  - Lógica de detección de cruces: inicio_A < fin_B AND inicio_B < fin_A por mismo gestor
  - Visualización tipo Gantt simplificado con línea de tiempo y resaltado de solapamientos
  - Lista de conflictos con detalle: nombre, gestor, fechas actuales, días de solapamiento
  - Formularios de modificación de fechas para requerimientos existentes en conflicto
  - Formulario de modificación de fechas del nuevo requerimiento
  - Recalculación automática de cruces al modificar cualquier fecha
  - Guardar con advertencia documentada (campo ConflictoCapacidad + DetalleConflicto en Demanda_Tactica)
  - _Requerimientos: 8_

- [ ] 9. Integración final, auditoría y preparación para producción
  - Navegación coherente entre Canvas App TI y Canvas App Gerencias
  - Implementar registro de auditoría en todos los flows de creación, modificación y cambio de estado
  - Pantalla de acceso denegado con mensaje explicativo para usuarios sin permiso
  - Pruebas de regresión completa de todos los flujos del ciclo de vida
  - Sesión de UAT con el área de Demanda TI
  - Sesión de UAT con representantes de gerencias piloto
  - Corrección de bugs de alta prioridad del UAT
  - Documentación básica de uso para usuarios finales
  - Configuración del entorno de producción y pase a producción
  - _Requerimientos: 9, 10, 11, 12_

## Task Dependency Graph

```json
{
  "waves": [
    {
      "wave": 1,
      "tasks": ["1"],
      "description": "Preparación: entorno SharePoint, permisos y datos de prueba"
    },
    {
      "wave": 2,
      "tasks": ["2"],
      "description": "Registro de Anexo 02 y Flow de alta en Backlog"
    },
    {
      "wave": 3,
      "tasks": ["3"],
      "description": "Gestión del Backlog y transiciones de estado"
    },
    {
      "wave": 4,
      "tasks": ["4"],
      "description": "Iniciativas y trazabilidad completa de extremo a extremo"
    },
    {
      "wave": 5,
      "tasks": ["5", "6", "8"],
      "description": "Pantallas de consulta, portal gerencias y evaluación de capacidad (paralelas)"
    },
    {
      "wave": 6,
      "tasks": ["7"],
      "description": "Priorización por gerencia (depende del portal gerencias)"
    },
    {
      "wave": 7,
      "tasks": ["9"],
      "description": "Integración final, auditoría, UAT y pase a producción"
    }
  ]
}
```

## Notes

- Las tareas 5, 6 y 8 pueden desarrollarse en paralelo una vez completada la tarea 4.
- La tarea 7 (Priorización) depende de la tarea 6 (Portal Gerencias) por compartir la Canvas App.
- Si el equipo enfrenta baja disponibilidad en alguna semana, las tareas de UX polish (indicadores de color, historial visible) son las primeras candidatas a mover a la semana siguiente sin bloquear el flujo funcional.
- La elección entre SharePoint Lists y Dataverse puede revisarse al final de la semana 1 según las licencias disponibles. SharePoint es la opción por defecto; Dataverse ofrece mejor rendimiento con volumen alto de datos.
