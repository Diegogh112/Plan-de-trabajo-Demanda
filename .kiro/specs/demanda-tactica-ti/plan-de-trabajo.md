# Plan de Trabajo — Sistema de Gestión de Demanda Táctica de TI

---

## Contexto

- **Equipo:** 5 personas con dedicación parcial (otras responsabilidades activas)
- **Dedicación estimada:** 4–8 horas/persona/semana para este proyecto
- **Modelo de avance:** entregas semanales demostrables con sesión de feedback
- **Stack:** Power Apps (Canvas App), SharePoint Lists, Power Automate, Power BI
- **Duración estimada total:** 10 semanas

---

## Roles del Equipo

| Rol                       | Cantidad | Responsabilidad principal                                      |
|---------------------------|----------|----------------------------------------------------------------|
| Líder / Coordinador       | 1        | Coordinación, revisión de avances, comunicación con gerencias  |
| Desarrollador Power Apps  | 2        | Construcción de pantallas y lógica en Canvas Apps             |
| Desarrollador Automate/SP | 1        | Listas de SharePoint, flujos de automatización, permisos       |
| Analista / QA             | 1        | Validación de requerimientos, pruebas, documentación           |

---

## Cronograma Semanal

### FASE 0 — Preparación (Semana 1)
**Objetivo:** Tener todo el entorno listo para desarrollar sin bloqueos técnicos.

| Tarea                                                         | Responsable          | Días est. |
|---------------------------------------------------------------|----------------------|-----------|
| Crear sitio SharePoint del proyecto                           | Dev Automate/SP      | 1         |
| Crear las 7 listas de SharePoint con todos sus campos         | Dev Automate/SP      | 2         |
| Configurar grupos de seguridad en Azure AD (2 roles)          | Dev Automate/SP      | 1         |
| Aplicar permisos por rol a cada lista                         | Dev Automate/SP      | 1         |
| Crear entorno de desarrollo en Power Apps                     | Dev Power Apps 1     | 0.5       |
| Cargar datos de prueba representativos en las listas          | Analista / QA        | 1.5       |
| Validar modelo de datos con el líder (revisar campos)         | Líder + Analista     | 1         |

**Entregable de la semana 1:** Entorno de desarrollo funcional con listas de SharePoint configuradas, permisos activos y datos de prueba cargados. Demo: navegar las listas en SharePoint y verificar que los permisos por rol funcionan correctamente.

---

### FASE 1 — Pantallas de Registro (Semanas 2–4)

#### Semana 2 — Registro de Anexo 02

| Tarea                                                         | Responsable          | Días est. |
|---------------------------------------------------------------|----------------------|-----------|
| Crear estructura base de Canvas App TI (navegación lateral)   | Dev Power Apps 1     | 1         |
| Formulario de creación de Anexo 02 (campos + validaciones)    | Dev Power Apps 1     | 2         |
| Vista lista de Anexos con pestañas Pendiente / Firmado / Todos| Dev Power Apps 2     | 1.5       |
| Vista previa antes de guardar                                 | Dev Power Apps 2     | 0.5       |
| Flow: Firma de Anexo → Alta automática en Backlog             | Dev Automate/SP      | 1.5       |
| Pruebas de creación y firma de Anexo 02                       | Analista / QA        | 1         |

**Entregable semana 2:** Se puede crear un Anexo 02, verlo en la lista como "Pendiente de Firma" y luego firmarlo. Al firmar, aparece automáticamente en el Backlog como "Registrado".

---

#### Semana 3 — Gestión del Backlog

| Tarea                                                         | Responsable          | Días est. |
|---------------------------------------------------------------|----------------------|-----------|
| Sección Backlog en Canvas App TI: lista con filtros           | Dev Power Apps 1     | 2         |
| Formulario de edición de ítem de Backlog (estados, ClearQuest)| Dev Power Apps 2     | 1.5       |
| Flow: Ingreso de N° ClearQuest → cambio automático a Evaluado | Dev Automate/SP      | 1         |
| Flow: Cambio de estado en Comité → Alta en Demanda Táctica    | Dev Automate/SP      | 1.5       |
| Registro automático en Historial_Estados en cada cambio       | Dev Automate/SP      | 1         |
| Pruebas del flujo Backlog (Registrado → Evaluado → Priorizado)| Analista / QA        | 1         |

**Entregable semana 3:** El flujo completo Anexo 02 → Backlog → cambios de estado funciona. Se puede registrar el número de ClearQuest y el sistema cambia el estado automáticamente.

---

#### Semana 4 — Registro de Iniciativas

| Tarea                                                         | Responsable          | Días est. |
|---------------------------------------------------------------|----------------------|-----------|
| Sección Iniciativas en Canvas App TI: formulario + lista       | Dev Power Apps 1     | 2         |
| Asociación de Iniciativa al crear un Anexo 02                 | Dev Power Apps 2     | 1         |
| Visualización de Iniciativa en el detalle del Backlog/Demanda | Dev Power Apps 2     | 1         |
| Integración del historial de estados en el panel de detalle   | Dev Power Apps 1     | 1.5       |
| Pruebas integradas: Iniciativa → Anexo → Backlog → Demanda    | Analista / QA        | 1.5       |

**Entregable semana 4:** Flujo completo desde Iniciativa hasta Demanda Táctica funciona de punta a punta. El historial de estados se registra y visualiza correctamente en cada etapa.

---

### FASE 2 — Pantallas de Consulta y Análisis (Semanas 5–7)

#### Semana 5 — Pantalla de Consultas TI

| Tarea                                                         | Responsable          | Días est. |
|---------------------------------------------------------------|----------------------|-----------|
| Tabla de requerimientos con todos los campos requeridos       | Dev Power Apps 1     | 2         |
| Filtros múltiples simultáneos (gerencia, gestor, estados, fechas) | Dev Power Apps 2  | 2         |
| Indicadores visuales de estado por colores                    | Dev Power Apps 2     | 0.5       |
| Panel lateral de detalle con historial                        | Dev Power Apps 1     | 1.5       |
| Función de exportación a Excel                                | Dev Automate/SP      | 1         |
| Pruebas de filtros y exportación                              | Analista / QA        | 1         |

**Entregable semana 5:** La pantalla de consultas muestra todos los requerimientos filtrados, con indicadores de color, detalle completo con historial y exportación funcional.

---

#### Semana 6 — Portal por Gerencia

| Tarea                                                         | Responsable          | Días est. |
|---------------------------------------------------------------|----------------------|-----------|
| Crear Canvas App de Gerencias (estructura base)               | Dev Power Apps 1     | 1         |
| Tarjetas resumen (totales por etapa)                          | Dev Power Apps 1     | 1         |
| Tabla de requerimientos agrupados por etapa con pestañas      | Dev Power Apps 2     | 2         |
| Reporte Power BI: gráfico donut por Estado_Demanda            | Dev Power Apps 2     | 1.5       |
| Reporte Power BI: barras por Estado_Backlog                   | Dev Power Apps 1     | 1         |
| Embeber Power BI en Canvas App de Gerencias                   | Dev Automate/SP      | 0.5       |
| Pruebas de visibilidad (solo ve su gerencia)                  | Analista / QA        | 1         |

**Entregable semana 6:** Cada gerencia puede acceder a su portal, ver sus requerimientos agrupados, los conteos por estado y los gráficos de distribución. El filtro por rol funciona correctamente.

---

#### Semana 7 — Priorización por Gerencia

| Tarea                                                         | Responsable          | Días est. |
|---------------------------------------------------------------|----------------------|-----------|
| Lista ordenable en Canvas App de Gerencias (drag & drop o botones subir/bajar) | Dev Power Apps 1 | 2 |
| Guardar nuevo orden en lista Prioridades_Gerencia             | Dev Power Apps 2     | 1.5       |
| Historial de los últimos 3 cambios de orden                   | Dev Power Apps 1     | 1         |
| Confirmación explícita antes de guardar                       | Dev Power Apps 2     | 0.5       |
| Flow: Notificar al gestor TI al cambiar prioridades           | Dev Automate/SP      | 1         |
| Pruebas de reordenamiento y persistencia                      | Analista / QA        | 1         |

**Entregable semana 7:** Las gerencias pueden ver y reordenar sus requerimientos. Los cambios se guardan correctamente, se notifica a TI y se visualiza el historial de los últimos cambios.

---

### FASE 3 — Evaluación de Capacidad e Integración (Semanas 8–9)

#### Semana 8 — Evaluación de Capacidad

| Tarea                                                         | Responsable          | Días est. |
|---------------------------------------------------------------|----------------------|-----------|
| Lógica de detección de cruces de fechas por gestor            | Dev Power Apps 1     | 2         |
| Visualización de línea de tiempo (Gantt simplificado)         | Dev Power Apps 1     | 2         |
| Lista de conflictos con detalle (días de solapamiento)        | Dev Power Apps 2     | 1         |
| Formularios de modificación de fechas (existentes + nuevo)    | Dev Power Apps 2     | 1.5       |
| Recalculación automática al modificar fechas                  | Dev Power Apps 1     | 1         |
| Guardar con advertencia documentada en Demanda_Tactica        | Dev Power Apps 2     | 0.5       |
| Pruebas de escenarios de cruce y resolución                   | Analista / QA        | 1.5       |

**Entregable semana 8:** Al proponer fechas para un nuevo requerimiento, el sistema detecta cruces, los muestra en la línea de tiempo, permite resolverlos y guarda correctamente (con o sin advertencia).

---

#### Semana 9 — Integración, Auditoría y Pulido

| Tarea                                                         | Responsable          | Días est. |
|---------------------------------------------------------------|----------------------|-----------|
| Menú de navegación unificado entre las dos Apps               | Dev Power Apps 1     | 1         |
| Implementar registro de auditoría en todos los flujos         | Dev Automate/SP      | 2         |
| Pantalla de acceso denegado para rol incorrecto               | Dev Power Apps 2     | 0.5       |
| Revisión de experiencia de usuario en ambas apps (UX polish)  | Dev Power Apps 1 + 2 | 2         |
| Pruebas de regresión completa (todos los flujos)              | Analista / QA        | 2         |
| Corrección de bugs encontrados en pruebas                     | Dev Power Apps 1 + 2 | 1.5       |

**Entregable semana 9:** Sistema integrado completo, con auditoría, navegación coherente y sin errores funcionales conocidos. Listo para UAT.

---

### FASE 4 — Pruebas con Usuarios y Ajustes (Semana 10)

| Tarea                                                         | Responsable          | Días est. |
|---------------------------------------------------------------|----------------------|-----------|
| Sesión de UAT con el área de Demanda TI (App TI)              | Líder + Analista     | 1.5       |
| Sesión de UAT con representantes de 2–3 gerencias (App Ger.)  | Líder + Analista     | 1.5       |
| Implementar ajustes de alta prioridad del UAT                 | Dev Power Apps 1 + 2 | 2         |
| Documentación básica de uso (guías para usuarios)             | Analista             | 1.5       |
| Configuración del entorno de producción y pase a prod         | Dev Automate/SP      | 1         |

**Entregable semana 10:** Sistema en producción. Usuarios finales validaron las funcionalidades clave. Guías de uso disponibles.

---

## Resumen del Cronograma

```
Semana  │ Fase        │ Entregable Principal
────────┼─────────────┼────────────────────────────────────────────────
  1     │ Preparación │ Entorno y datos de prueba listos
  2     │ Registro    │ Anexo 02: crear y firmar (+ Flow Backlog)
  3     │ Registro    │ Backlog completo + Flows de estado
  4     │ Registro    │ Iniciativas + historial integrado de extremo a extremo
  5     │ Consulta    │ Pantalla de Consultas TI con filtros y exportación
  6     │ Análisis    │ Portal por Gerencia con gráficos Power BI
  7     │ Análisis    │ Priorización por Gerencia operativa
  8     │ Capacidad   │ Evaluación de Capacidad con detección de cruces
  9     │ Integración │ Sistema integrado, auditoría, pruebas de regresión
 10     │ UAT/Prod    │ Validación con usuarios y pase a producción
```

---

## Riesgos y Mitigaciones

| Riesgo                                          | Impacto | Probabilidad | Mitigación                                               |
|-------------------------------------------------|---------|--------------|----------------------------------------------------------|
| Semanas con baja disponibilidad del equipo      | Alto    | Alta         | Tareas priorizadas A/B; las B se mueven a la semana siguiente |
| Complejidad del Gantt en Power Apps             | Medio   | Media        | Usar HTML control o tabla en lugar de gráfico nativo     |
| Modelo de datos requiere ajustes tras el UAT    | Medio   | Media        | Diseño flexible en SharePoint; semana 10 incluye buffer  |
| Acceso a licencias Power BI Pro para gerencias  | Alto    | Baja         | Evaluar alternativa con gráficos nativos de Canvas App   |
| Resistencia de las gerencias al nuevo proceso   | Medio   | Media        | Involucrar a 1–2 gerencias como pilotos desde la semana 1|

---

## Sesiones de Feedback Semanal

Cada viernes, sesión de 30–45 minutos con el líder del área:
1. Demo del entregable de la semana (pantalla o flujo funcional)
2. Validación contra los criterios de aceptación del requerimiento correspondiente
3. Registro de ajustes solicitados (backlog del sprint siguiente)
4. Confirmación de disponibilidad para la semana siguiente

El feedback de cada sesión define si la semana se cierra o si se necesita un día adicional de corrección antes de avanzar.
