# Design Document

## Sistema de Gestión de Demanda Táctica de TI

---

## Overview

El sistema se construye sobre el ecosistema Microsoft 365 usando **Power Apps** como capa de interfaz y transacciones, **SharePoint Lists** como capa de datos, **Power Automate** para automatizaciones de flujo, y **Power BI** para analítica del Portal por Gerencia. La autenticación es transparente vía Azure AD / Entra ID heredada de Microsoft 365.

Se trata de una **Canvas App** principal (acceso de TI) y una **Canvas App** secundaria (acceso de gerencias), ambas conectadas a las mismas listas de SharePoint. El Portal por Gerencia puede implementarse como una tercera app o como un reporte de Power BI embebido.

---

## Architecture

```
┌─────────────────────────────────────────────────────────┐
│                   USUARIOS                              │
│  Gestor TI (Demanda)      Representante Gerencia        │
└──────────┬───────────────────────────┬──────────────────┘
           │                           │
           ▼                           ▼
┌─────────────────────┐   ┌─────────────────────────────┐
│  Canvas App TI      │   │  Canvas App Gerencias        │
│  - Pantalla Consul. │   │  - Portal Gerencia           │
│  - Registro Anx02   │   │  - Priorización              │
│  - Evaluac. Capac.  │   │  (+ Power BI embebido)       │
└──────────┬──────────┘   └──────────────┬───────────────┘
           │                             │
           └──────────┬──────────────────┘
                      │
                      ▼
         ┌────────────────────────┐
         │  SharePoint Lists      │
         │  (Dataverse opcional)  │
         │                        │
         │  · Iniciativas         │
         │  · Anexos_02           │
         │  · Backlog             │
         │  · Demanda_Tactica     │
         │  · Prioridades_Gerencia│
         │  · Historial_Estados   │
         │  · Auditoria           │
         └────────────┬───────────┘
                      │
                      ▼
         ┌────────────────────────┐
         │  Power Automate Flows  │
         │                        │
         │  · Firma → Backlog     │
         │  · ClearQuest → Eval.  │
         │  · Comité → Demanda    │
         │  · Notificaciones      │
         └────────────────────────┘
```

---

## Data Model

### Lista: Iniciativas

| Campo              | Tipo          | Obligatorio | Notas                              |
|--------------------|---------------|-------------|------------------------------------|
| ID                 | Auto-número   | Sí          | Formato: INI-AAAA-NNN              |
| Titulo             | Texto (150)   | Sí          | Nombre de la iniciativa            |
| Descripcion        | Texto (1000)  | No          |                                    |
| Gerencia           | Choice        | Sí          | Lista cerrada de gerencias         |
| Gestor             | Person        | No          | Usuario de AD                      |
| FechaRegistro      | Date          | Sí (auto)   | Asignada por el sistema            |
| Estado             | Choice        | Sí          | "Iniciativa" / "Con Anexo 02"      |
| AnexoAsociado      | Lookup        | No          | → Anexos_02                        |

### Lista: Anexos_02

| Campo              | Tipo          | Obligatorio | Notas                                     |
|--------------------|---------------|-------------|-------------------------------------------|
| ID                 | Auto-número   | Sí          | Formato: ANX-AAAA-NNN                     |
| NumeroAnexo        | Texto (50)    | Sí          | Número oficial del documento              |
| Gerencia           | Choice        | Sí          |                                           |
| Gestor             | Person        | Sí          |                                           |
| Descripcion        | Texto (2000)  | Sí          | Descripción del requerimiento             |
| FechaCreacion      | Date          | Sí (auto)   |                                           |
| Estado             | Choice        | Sí          | "Creado" / "Firmado"                      |
| IniciativaAsociada | Lookup        | No          | → Iniciativas                             |
| FechaFirma         | Date          | No          | Se completa al firmar                     |
| FirmanteGerencia   | Texto (100)   | No          | Nombre del firmante por la gerencia       |
| FirmanteTI         | Texto (100)   | No          | Nombre del firmante por TI                |

### Lista: Backlog

| Campo              | Tipo          | Obligatorio | Notas                                          |
|--------------------|---------------|-------------|------------------------------------------------|
| ID                 | Auto-número   | Sí          | Formato: BKL-AAAA-NNN                          |
| Nombre             | Texto (150)   | Sí          |                                                |
| Gerencia           | Choice        | Sí          |                                                |
| Gestor             | Person        | Sí          |                                                |
| Estado_Ciclo       | Choice        | Sí          | Iniciativa Registrada / Solicitud en Proceso / Solicitud Visada / Registrado / Evaluado / Priorizado / Rechazado / Descartado |
| Estado_Backlog     | Choice        | Sí          | Registrado / Evaluado / Priorizado / Rechazado / Paralizado (solo aplica desde Registrado en adelante) |
| Fase               | Choice        | Sí          | "prebacklog" / "backlog" / "demanda"           |
| FechaIngreso       | Date          | Sí (auto)   |                                                |
| NumeroAnexo02      | Lookup        | Sí          | → Anexos_02 (debe estar Firmado)               |
| NumeroClearQuest   | Texto (50)    | No          | Al ingresarse, activa cambio a "Evaluado"      |
| FechaRechazo       | Date          | No          |                                                |
| MotivoRechazo      | Texto (500)   | No          |                                                |
| FechaParalizacion  | Date          | No          |                                                |
| MotivoParalizacion | Texto (500)   | No          |                                                |

### Lista: Demanda_Tactica

| Campo              | Tipo          | Obligatorio | Notas                                          |
|--------------------|---------------|-------------|------------------------------------------------|
| ID                 | Auto-número   | Sí          | Formato: DEM-AAAA-NNN                          |
| BacklogItem        | Lookup        | Sí          | → Backlog                                      |
| Nombre             | Texto (150)   | Sí          |                                                |
| Gerencia           | Choice        | Sí          |                                                |
| Gestor             | Person        | Sí          |                                                |
| Estado_Demanda     | Choice        | Sí          | En Evaluación / Aprobado / En Desarrollo / Pausado / Cancelado / Completado |
| Estado_TI          | Choice        | Sí          | Pendiente / En Gestión / Cerrado               |
| Estado_ClearQuest  | Choice        | Sí          | Abierto / En Proceso / Cerrado                 |
| FechaInicioP       | Date          | Sí          | Fecha inicio propuesta                         |
| FechaFinP          | Date          | Sí          | Fecha fin propuesta (debe ser > FechaInicioP)  |
| FechaPriorizacion  | Date          | Sí (auto)   |                                                |
| ConflictoCapacidad | Boolean       | No          | True si se guardó con cruces sin resolver      |
| DetalleConflicto   | Texto (2000)  | No          | JSON con lista de conflictos al momento de guardar |

### Lista: Prioridades_Gerencia

| Campo              | Tipo          | Obligatorio | Notas                                   |
|--------------------|---------------|-------------|-----------------------------------------|
| ID                 | Auto-número   | Sí          |                                         |
| Gerencia           | Choice        | Sí          |                                         |
| Requerimiento      | Lookup        | Sí          | → Backlog o Demanda_Tactica             |
| TipoRef            | Choice        | Sí          | "Backlog" / "Demanda"                   |
| PosicionPrioridad  | Number        | Sí          | Entero ≥ 1, único por gerencia          |
| FechaActualizacion | Date          | Sí (auto)   |                                         |
| UsuarioCambio      | Person        | Sí (auto)   |                                         |

### Lista: Historial_Estados

| Campo              | Tipo          | Obligatorio | Notas                                      |
|--------------------|---------------|-------------|--------------------------------------------|
| ID                 | Auto-número   | Sí          |                                            |
| EntidadTipo        | Choice        | Sí          | "Iniciativa" / "Anexo02" / "Backlog" / "Demanda" |
| EntidadID          | Number        | Sí          | ID del registro en la lista correspondiente|
| EstadoAnterior     | Texto (50)    | Sí          |                                            |
| EstadoNuevo        | Texto (50)    | Sí          |                                            |
| FechaHora          | DateTime      | Sí (auto)   |                                            |
| UsuarioResponsable | Person        | Sí (auto)   |                                            |
| Observacion        | Texto (500)   | No          |                                            |

### Lista: Auditoria

| Campo              | Tipo          | Notas                                              |
|--------------------|---------------|----------------------------------------------------|
| ID                 | Auto-número   |                                                    |
| Usuario            | Person        |                                                    |
| Accion             | Choice        | "Crear" / "Modificar" / "CambiarEstado"            |
| Entidad            | Texto (50)    | Nombre de la lista afectada                        |
| EntidadID          | Number        |                                                    |
| FechaHora          | DateTime      |                                                    |
| Detalle            | Texto (1000)  | Descripción del cambio                             |

---

## Components and Interfaces

### App 1: Canvas App TI — "Demanda Táctica TI"

Navegación lateral con 4 secciones principales:

```
┌─────────────────────────────────────────────────────────────┐
│  🏠  Demanda Táctica TI                    [Usuario]  [?]   │
├────────────┬────────────────────────────────────────────────┤
│            │                                                │
│ 📋 Consultas│          ÁREA DE CONTENIDO                    │
│            │                                                │
│ 📝 Anexo 02│                                                │
│            │                                                │
│ 📊 Backlog │                                                │
│            │                                                │
│ ⚡ Capacidad│                                                │
│            │                                                │
└────────────┴────────────────────────────────────────────────┘
```

#### Pantalla 1: Consultas (TI)

```
┌─────────────────────────────────────────────────────────────────┐
│  CONSULTA DE REQUERIMIENTOS                         [Exportar ↓]│
├─────────────────────────────────────────────────────────────────┤
│  FILTROS                                                        │
│  [Gerencia ▼] [Gestor ▼] [Estado Backlog ▼] [Estado Demanda ▼] │
│  [Desde: dd/mm/aaaa] [Hasta: dd/mm/aaaa]      [Limpiar filtros] │
│  Mostrando: 47 requerimientos                                   │
├──────┬─────────────────────┬──────────┬────────┬────────────────┤
│  ID  │  Nombre             │ Gerencia │ Gestor │ Est.Backlog    │
├──────┼─────────────────────┼──────────┼────────┼────────────────┤
│BKL-  │ Módulo Liquidación  │ Finanzas │ Pedro  │ 🟡 Evaluado   │
│001   │ de Haberes          │          │ García │                │
├──────┼─────────────────────┼──────────┼────────┼────────────────┤
│BKL-  │ Portal Autoservicio │ RRHH     │ Ana    │ 🟢 Priorizado │
│002   │ Empleados           │          │ López  │                │
├──────┼─────────────────────┼──────────┼────────┼────────────────┤
│BKL-  │ Dashboard Compras   │ Logística│ Luis   │ 🔵 Registrado │
│003   │ en Tiempo Real      │          │ Ruiz   │                │
├──────┴─────────────────────┴──────────┴────────┴────────────────┤
│  [◄] [1] [2] [3] ... [►]                     25 por página      │
└─────────────────────────────────────────────────────────────────┘

Panel lateral al seleccionar un requerimiento:
┌──────────────────────────────────┐
│ DETALLE: BKL-001                 │
│ Módulo Liquidación de Haberes    │
│ ─────────────────────────────── │
│ Gerencia:   Finanzas             │
│ Gestor:     Pedro García         │
│ Anexo 02:   ANX-2026-012         │
│ ClearQuest: CQ-2026-089          │
│ Est.Backlog: Evaluado            │
│ Est.Demanda: En Evaluación       │
│ ─────────────────────────────── │
│ HISTORIAL                        │
│ 12/06 Registrado → Evaluado      │
│       por Pedro García           │
│ 01/06 Iniciativa → Registrado    │
│       por Ana López              │
│                        [Cerrar ✕]│
└──────────────────────────────────┘
```

#### Pantalla 2: Registro Anexo 02

Vista de lista + formulario en dos modos (Crear / Firmar):

```
┌─────────────────────────────────────────────────────────────────┐
│  ANEXOS 02                                    [+ Nuevo Anexo]   │
│  PENDIENTES DE FIRMA (5)  │  FIRMADOS (23)   │  TODOS (28)      │
├──────────────────────────────────────────────────────────────────┤
│  [Buscar por N° de Anexo o Gerencia...]                          │
├────────┬──────────────────┬──────────┬──────────┬───────────────┤
│ N°Anx  │ Nombre Req.      │ Gerencia │ F.Creac. │ Estado        │
├────────┼──────────────────┼──────────┼──────────┼───────────────┤
│ANX-012 │ Módulo Liquidad. │ Finanzas │ 01/06/26 │ ⏳ Creado     │
│        │                  │          │          │ [Firmar]      │
├────────┼──────────────────┼──────────┼──────────┼───────────────┤
│ANX-011 │ Portal Autoserv. │ RRHH     │ 28/05/26 │ ✅ Firmado    │
└────────┴──────────────────┴──────────┴──────────┴───────────────┘
```

Formulario de Creación:
```
┌─────────────────────────────────────────────────────────────────┐
│  NUEVO ANEXO 02                                      [✕ Cerrar] │
├─────────────────────────────────────────────────────────────────┤
│  Número de Anexo *    [________________________]                 │
│                                                                  │
│  Gerencia Solicitante *  [Seleccionar ▼]                         │
│                                                                  │
│  Gestor TI Responsable * [Seleccionar usuario]                   │
│                                                                  │
│  Iniciativa Asociada     [Buscar iniciativa...   ▼]  (opcional) │
│                                                                  │
│  Descripción del Requerimiento *                                 │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │                                                          │   │
│  │                                                          │   │
│  └──────────────────────────────────────────────────────────┘   │
│  (2000 caracteres máx.)                                          │
│                                                                  │
│              [Cancelar]          [Vista Previa →]                │
└─────────────────────────────────────────────────────────────────┘
```

Formulario de Firma (aparece al pulsar "Firmar"):
```
┌─────────────────────────────────────────────────────────────────┐
│  REGISTRAR FIRMA — ANX-012                           [✕ Cerrar] │
│  Módulo Liquidación de Haberes · Finanzas                        │
├─────────────────────────────────────────────────────────────────┤
│  Fecha de Firma *        [dd/mm/aaaa  📅]                        │
│                                                                  │
│  Firmante por la Gerencia *  [________________________]          │
│                                                                  │
│  Firmante por TI *           [________________________]          │
│                                                                  │
│  ⚠ Al confirmar, este requerimiento pasará al Backlog           │
│    con estado "Registrado".                                      │
│                                                                  │
│              [Cancelar]          [Confirmar Firma ✓]             │
└─────────────────────────────────────────────────────────────────┘
```

#### Pantalla 3: Evaluación de Capacidad

```
┌─────────────────────────────────────────────────────────────────┐
│  EVALUACIÓN DE CAPACIDAD                                        │
│  Nuevo requerimiento: Portal Autoservicio Empleados             │
├─────────────────────────────────────────────────────────────────┤
│  Gestor: Ana López    Fecha Inicio: 01/07/26  Fin: 30/09/26     │
│  [Verificar cruces]                                             │
├─────────────────────────────────────────────────────────────────┤
│  LÍNEA DE TIEMPO — Ana López                                    │
│                                                                  │
│  JUN 2026       JUL 2026         AGO 2026        SEP 2026       │
│  ─────────────────────────────────────────────────────────      │
│  ████████████ Módulo Liquidación (BKL-001)                      │
│               ████████████████████████████ [NUEVO] Portal Auto  │
│                    ███████ Integrac.Contable(BKL-004)           │
│                                                                  │
│  ⚠ CRUCES DETECTADOS (2)                                        │
├─────────────────────────────────────────────────────────────────┤
│  Conflicto 1: BKL-001 Módulo Liquidación de Haberes             │
│  Inicio: 15/05/26  Fin: 15/08/26   Solapamiento: 45 días        │
│  [Modificar fechas de BKL-001]  Nueva Fin: [15/08/26 📅]        │
│                                                                  │
│  Conflicto 2: BKL-004 Integración Contable                      │
│  Inicio: 10/07/26  Fin: 10/08/26   Solapamiento: 31 días        │
│  [Modificar fechas de BKL-004]  Nueva Fin: [10/08/26 📅]        │
├─────────────────────────────────────────────────────────────────┤
│  O bien, modificar fechas del NUEVO requerimiento:              │
│  Inicio: [01/07/26 📅]    Fin: [30/09/26 📅]    [Recalcular]    │
├─────────────────────────────────────────────────────────────────┤
│  [Guardar con advertencia ⚠]        [Guardar sin conflictos ✓]  │
└─────────────────────────────────────────────────────────────────┘
```

---

### App 2: Canvas App Gerencias — "Mi Portal de Requerimientos"

#### Pantalla 4: Portal por Gerencia

```
┌─────────────────────────────────────────────────────────────────┐
│  GERENCIA DE FINANZAS — Mis Requerimientos     [Ana Flores]     │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐        │
│  │  TOTAL   │  │Iniciativas│  │ Backlog  │  │ Demanda  │        │
│  │    18    │  │    4      │  │    8     │  │    6     │        │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘        │
│                                                                  │
│  Estado de la Demanda (Donut)   Backlog por Estado (Barras)     │
│  ┌─────────────────────┐  ┌──────────────────────────────┐      │
│  │      🍩             │  │  Registrado   ████░░  3       │      │
│  │   En Dev: 2         │  │  Evaluado     ████░░  3       │      │
│  │   Aprobado: 2       │  │  Priorizado   ██░░░░  2       │      │
│  │   En Eval: 2        │  │                              │      │
│  └─────────────────────┘  └──────────────────────────────┘      │
│                                                                  │
├─────────────────────────────────────────────────────────────────┤
│  [Iniciativas (4)] [Backlog (8)] [Demanda (6)] [Todos (18)]     │
│  [Filtrar por estado ▼]                [Ver prioridades →]       │
├──────┬─────────────────────┬─────────────────┬──────────────────┤
│  #   │  Requerimiento      │  Estado         │  Fecha           │
├──────┼─────────────────────┼─────────────────┼──────────────────┤
│  1   │ Portal Autoservicio │ 🟢 En Desarrollo│ Ini: 01/07/26    │
│      │ Empleados           │                 │ Fin: 30/09/26    │
├──────┼─────────────────────┼─────────────────┼──────────────────┤
│  2   │ Dashboard Compras   │ 🟡 En Evaluación│ —                │
├──────┼─────────────────────┼─────────────────┼──────────────────┤
│  3   │ Módulo Liquidación  │ 🔵 Evaluado     │ —                │
└──────┴─────────────────────┴─────────────────┴──────────────────┘
```

#### Pantalla 5: Priorización por Gerencia

```
┌─────────────────────────────────────────────────────────────────┐
│  PRIORIDADES — GERENCIA DE FINANZAS                             │
│  Ordena tus requerimientos según la atención que necesitan      │
├─────────────────────────────────────────────────────────────────┤
│  Últimos cambios: 10/06/26 por Ana Flores · 03/06/26 por J.Ruiz │
├──────┬────────────────────────────────────┬────────────────────┤
│ Pos  │ Requerimiento                      │ Estado             │
├──────┼────────────────────────────────────┼────────────────────┤
│  ☰ 1 │ Portal Autoservicio Empleados      │ 🟢 En Desarrollo   │
├──────┼────────────────────────────────────┼────────────────────┤
│  ☰ 2 │ Dashboard Compras en Tiempo Real   │ 🟡 En Evaluación   │
├──────┼────────────────────────────────────┼────────────────────┤
│  ☰ 3 │ Módulo Liquidación de Haberes      │ 🔵 Evaluado        │
├──────┼────────────────────────────────────┼────────────────────┤
│  ☰ 4 │ Integración con Sistema Contable   │ 🔵 Evaluado        │
├──────┼────────────────────────────────────┼────────────────────┤
│  ☰ 5 │ Reportes de Ejecución Presupuestal │ ⚪ Registrado      │
└──────┴────────────────────────────────────┴────────────────────┘
│  (☰ = arrastra para reordenar)   [↑ Subir] [↓ Bajar]           │
│                                                                  │
│              [Cancelar]            [Guardar Prioridades ✓]       │
│                                                                  │
│  ⓘ Al guardar se notificará al área de TI del nuevo orden.      │
└─────────────────────────────────────────────────────────────────┘
```

---

## Power Automate Flows

### Flow 1: Firma de Anexo → Alta en Backlog
- **Trigger:** Cuando se actualiza un ítem en `Anexos_02` y `Estado` cambia a "Firmado"
- **Acciones:** Crear ítem en `Backlog` con Estado_Backlog = "Registrado" → Registrar en `Historial_Estados` → Notificar al gestor por email

### Flow 2: Número ClearQuest → Estado Evaluado
- **Trigger:** Cuando se actualiza un ítem en `Backlog` y `NumeroClearQuest` pasa de vacío a tener valor
- **Acciones:** Actualizar `Estado_Backlog` a "Evaluado" → Registrar en `Historial_Estados`

### Flow 3: Comité TI → Alta en Demanda Táctica
- **Trigger:** Cuando se actualiza un ítem en `Backlog` y `Estado_Backlog` cambia a "Priorizado"
- **Acciones:** Crear ítem en `Demanda_Tactica` → Actualizar `Iniciativas` si corresponde → Registrar en `Historial_Estados` → Notificar a gerencia

### Flow 4: Notificación de Priorización
- **Trigger:** Cuando se crea o actualiza un ítem en `Prioridades_Gerencia`
- **Acciones:** Notificar al gestor TI correspondiente con el nuevo orden

---

## Roles y Permisos en SharePoint

| Lista                | Gestor_Demanda_TI | Representante_Gerencia     |
|----------------------|-------------------|----------------------------|
| Iniciativas          | Lectura/Escritura | Solo su gerencia (lectura) |
| Anexos_02            | Lectura/Escritura | Solo su gerencia (lectura) |
| Backlog              | Lectura/Escritura | Solo su gerencia (lectura) |
| Demanda_Tactica      | Lectura/Escritura | Solo su gerencia (lectura) |
| Prioridades_Gerencia | Lectura/Escritura | Solo su gerencia (L/E)     |
| Historial_Estados    | Lectura           | Solo su gerencia (lectura) |
| Auditoria            | Lectura           | Sin acceso                 |

---

## Data Models

Ver sección **Data Model** arriba para el detalle completo de cada lista de SharePoint.

**Entidades principales:**
- `Iniciativas` → `Anexos_02` → `Backlog` → `Demanda_Tactica` (cadena de trazabilidad)
- `Prioridades_Gerencia` → referencias a `Backlog` o `Demanda_Tactica`
- `Historial_Estados` → referencia a cualquier entidad (polimórfico por tipo + ID)
- `Auditoria` → log transversal de todas las acciones

---

## Correctness Properties

### Property 1: Ciclo de vida secuencial
Un requerimiento no puede saltar estados en el ciclo de vida. El orden obligatorio es: Iniciativa Registrada → Solicitud en Proceso → Solicitud Visada → Registrado → Evaluado → Priorizado. Los estados Rechazado y Descartado pueden alcanzarse desde cualquier punto del ciclo antes de Priorizado.
**Validates: Requirements 1.1, 1.2, 1.4**

### Property 2: Irreversibilidad del Anexo firmado
Un Anexo_02 en estado "Firmado" no puede volver a estado "Creado". El sistema debe bloquear cualquier intento de revertir este estado.
**Validates: Requirements 3.4, 3.6**

### Property 3: Integridad del Backlog
Todo ítem en el Backlog debe tener un Anexo_02 asociado con estado "Firmado". No pueden existir ítems de Backlog sin este vínculo.
**Validates: Requirements 4.1, 4.6**

### Property 4: Coherencia de fechas
Las fechas en Demanda_Tactica deben cumplir que FechaFinP sea estrictamente mayor que FechaInicioP. Ningún requerimiento puede tener fecha de fin anterior o igual a la de inicio.
**Validates: Requirements 8.4, 8.5, 9.1**

### Property 5: Unicidad de posición de prioridad
La posición en Prioridades_Gerencia debe ser única por gerencia. No pueden existir dos ítems con la misma posición para la misma gerencia en un momento dado.
**Validates: Requirements 7.3, 7.6**

### Property 6: Completitud del historial de estados
El Historial_Estados debe registrar un evento por cada cambio de estado, sin excepción, incluyendo estado anterior, estado nuevo, fecha/hora y usuario responsable del cambio.
**Validates: Requirements 1.7, 5.7, 9.3**

---

## Error Handling

| Escenario                                        | Comportamiento esperado                                          |
|--------------------------------------------------|------------------------------------------------------------------|
| Campo obligatorio vacío al guardar               | Resaltar campo en rojo, mensaje descriptivo, no guardar          |
| Intentar firmar un Anexo ya firmado              | Advertencia visual, bloquear la acción                          |
| Intentar ingresar al Backlog sin Anexo firmado   | Error, explicar el requisito previo                             |
| Guardar Demanda con cruces sin resolver          | Permitir guardar pero documentar advertencia en el registro      |
| Usuario sin rol accede a pantalla restringida    | Redirigir a pantalla de acceso denegado con mensaje              |
| Flow de Power Automate falla                     | Email de notificación al administrador, registro en log de errores|

---

## Testing Strategy

- **Pruebas unitarias:** Validar cada formulario de forma aislada (campos, validaciones, mensajes de error).
- **Pruebas de integración:** Recorrer el flujo completo Iniciativa → Anexo → Backlog → Demanda con Power Automate activo.
- **Pruebas de permisos:** Verificar que cada rol solo accede a lo que le corresponde (con usuarios de prueba de cada tipo).
- **Pruebas de capacidad:** Verificar detección de cruces con al menos 3 escenarios: sin cruce, 1 cruce, múltiples cruces.
- **UAT:** Sesiones con el área de Demanda TI y con representantes de gerencias piloto (semana 10).



| Decisión                 | Elección                   | Razón                                                       |
|--------------------------|----------------------------|-------------------------------------------------------------|
| Capa de datos            | SharePoint Lists           | Sin costo adicional, integración nativa con Power Apps      |
| Capa de datos (alternativa) | Dataverse               | Mayor capacidad y relaciones, requiere licencia P2          |
| Interfaz principal       | Canvas App                 | Control total de UI, flexibilidad para la interfaz de TI    |
| Portal Gerencias         | Canvas App + Power BI embed| Gráficos ricos con Power BI, interacción con Canvas App     |
| Automatizaciones         | Power Automate             | Nativo de M365, sin infraestructura adicional               |
| Gantt de capacidad       | Canvas App custom control  | HTML control o librería de componentes PCF                  |
| Autenticación            | Azure AD heredado           | Transparente, sin desarrollo adicional                      |

 