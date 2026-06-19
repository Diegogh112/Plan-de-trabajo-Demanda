# Requirements Document

## Sistema de Gestión de Demanda Táctica de TI

---

## Introduction

El área de Demanda Táctica de TI requiere un sistema de pantallas desarrollado en Power Apps, Power Automate y Power BI que gestione el ciclo de vida completo de los requerimientos de TI. El sistema abarca desde el registro inicial de iniciativas de las gerencias usuarias hasta la priorización como Demanda Táctica.

El flujo de vida de un requerimiento sigue estas etapas: Iniciativa → Anexo 02 (creación y firma) → Backlog (Registrado → Evaluado) → Comité de Proyectos de TI → Demanda Táctica (priorizada). El sistema comprende cinco pantallas principales: Pantalla de Consultas, Registro de Anexo 02, Portal por Gerencia, Priorización por Gerencia y Evaluación de Capacidad.

---

## Glossary

- **Sistema**: La solución completa de gestión de Demanda Táctica de TI implementada en Power Apps, Power Automate y Power BI.
- **Iniciativa**: Idea o necesidad registrada por una gerencia usuaria, de carácter informal, que da inicio al ciclo de vida de un requerimiento de TI.
- **Anexo_02**: Documento formal que TI crea en conjunto con la gerencia usuaria para formalizar un requerimiento. Tiene dos momentos: creación y firma.
- **Backlog**: Registro formal de todos los requerimientos de TI. Contiene requerimientos en estados "Registrado" o "Evaluado".
- **Requerimiento**: Unidad de trabajo de TI que atraviesa el ciclo de vida completo desde Iniciativa hasta Demanda Táctica.
- **Demanda_Tactica**: Registro de requerimientos priorizados para desarrollo, aprobados en el Comité de Proyectos de TI.
- **Comite_TI**: Reunión periódica donde se decide la priorización de requerimientos del Backlog hacia la Demanda Táctica.
- **Gerencia**: Unidad organizacional usuaria que origina iniciativas y hace seguimiento de sus requerimientos.
- **Gestor**: Responsable de TI asignado a gestionar uno o más requerimientos.
- **ClearQuest**: Herramienta externa de gestión de proyectos usada por el área de Construcción para crear trámites de requerimientos.
- **Pantalla_Consultas**: Pantalla del Sistema destinada al área de Demanda de TI para consultar el estado de todos los requerimientos.
- **Registro_Anexo02**: Pantalla del Sistema para registrar y firmar el Anexo 02.
- **Portal_Gerencia**: Pantalla del Sistema destinada a cada gerencia para ver el estado de sus propios requerimientos.
- **Pantalla_Priorizacion**: Pantalla del Sistema donde cada gerencia ordena sus requerimientos según prioridad de atención.
- **Pantalla_Capacidad**: Pantalla del Sistema para evaluar cruces de fechas al registrar nuevos requerimientos en la Demanda Táctica.
- **Estado_Backlog**: Valor que indica la situación de un requerimiento en el Backlog. Valores posibles: "Registrado", "Evaluado", "Priorizado", "Rechazado", "Paralizado".
- **Estado_Demanda**: Valor que indica la situación de un requerimiento dentro de la Demanda Táctica.
- **Estado_Ciclo**: Valor unificado que cubre todo el ciclo de vida del requerimiento, incluyendo fases pre-backlog. Valores posibles en orden: "Iniciativa Registrada" → "Solicitud en Proceso" → "Solicitud Visada" → "Registrado" → "Evaluado" → "Priorizado" → "Rechazado" / "Descartado".
- **Wireframe**: Representación visual referencial de una pantalla, sin diseño final, usada para validar estructura y contenido.

---

## Requirements

---

### Requisito 1: Ciclo de Vida del Requerimiento

**User Story:** Como gestor del área de Demanda de TI, quiero que el Sistema gestione el ciclo de vida completo de un requerimiento desde que ingresa como iniciativa hasta que se convierte en demanda táctica priorizada, de modo que pueda rastrear el avance de cada requerimiento en todo momento.

#### Criterios de Aceptación

1. THE Sistema SHALL registrar los cambios de estado de un requerimiento a lo largo del ciclo de vida: Iniciativa → Backlog (Registrado) → Backlog (Evaluado) → Backlog (Priorizado) → Demanda Táctica.
2. WHEN un Anexo_02 es firmado por la gerencia y por TI, THE Sistema SHALL mover el requerimiento asociado al Backlog con Estado_Backlog igual a "Registrado".
3. WHEN el área de Construcción aprueba y crea el trámite en ClearQuest, THE Sistema SHALL actualizar el Estado_Backlog del requerimiento a "Evaluado".
4. WHEN el Comite_TI selecciona un requerimiento para priorizar, THE Sistema SHALL actualizar el Estado_Backlog a "Priorizado" y registrar el requerimiento en la Demanda_Tactica.
5. WHEN el Comite_TI rechaza un requerimiento, THE Sistema SHALL actualizar el Estado_Backlog a "Rechazado" y registrar la fecha y motivo del rechazo.
6. WHEN el Comite_TI paraliza un requerimiento, THE Sistema SHALL actualizar el Estado_Backlog a "Paralizado" y registrar la fecha de paralización.
7. THE Sistema SHALL mantener el historial completo de cambios de estado de cada requerimiento, incluyendo fecha y usuario responsable del cambio.

---

### Requisito 2: Registro de Iniciativas

**User Story:** Como gestor del área de Demanda de TI, quiero registrar iniciativas de las gerencias usuarias en el Sistema, de modo que queden como antecedente previo al Anexo 02 y sean visibles para su seguimiento.

#### Criterios de Aceptación

1. THE Sistema SHALL permitir registrar una Iniciativa con los campos: nombre, descripción, gerencia originante, gestor asignado y fecha de registro.
2. WHEN se registra una Iniciativa, THE Sistema SHALL asignarle un identificador único correlativo de forma automática.
3. WHEN se registra una Iniciativa, THE Sistema SHALL establecer su estado como "Iniciativa" hasta que se genere el Anexo_02 correspondiente.
4. THE Sistema SHALL permitir asociar una Iniciativa existente al momento de crear un Anexo_02.
5. IF se intenta registrar una Iniciativa sin nombre o sin gerencia originante, THEN THE Sistema SHALL mostrar un mensaje de error indicando los campos obligatorios faltantes y no guardar el registro.

---

### Requisito 3: Registro y Firma del Anexo 02

**User Story:** Como gestor del área de Demanda de TI, quiero registrar el Anexo 02 en dos momentos separados (creación y firma), de modo que el flujo refleje fielmente el proceso real donde la firma puede ocurrir días o semanas después de la creación.

#### Criterios de Aceptación

1. THE Registro_Anexo02 SHALL permitir ingresar los datos del Anexo_02 en un primer momento (creación), registrando: número de anexo, gerencia, gestor, descripción del requerimiento, fecha de creación y datos de la iniciativa asociada (si existe).
2. WHEN se completa el formulario de creación del Anexo_02, THE Sistema SHALL guardar el registro con estado "Creado" y la fecha de creación.
3. THE Registro_Anexo02 SHALL permitir registrar la firma del Anexo_02 en un momento posterior a su creación, capturando: fecha de firma, nombre del firmante por la gerencia y nombre del firmante por TI.
4. WHEN se registra la firma del Anexo_02, THE Sistema SHALL actualizar el estado del Anexo_02 a "Firmado" y activar el traslado del requerimiento al Backlog.
5. WHILE un Anexo_02 tiene estado "Creado", THE Sistema SHALL mostrar el registro como pendiente de firma en la Pantalla_Consultas.
6. IF se intenta firmar un Anexo_02 que ya tiene estado "Firmado", THEN THE Sistema SHALL mostrar un mensaje de advertencia indicando que el anexo ya fue firmado e impedir la duplicación del registro.
7. IF se intenta guardar un Anexo_02 sin los campos obligatorios (número de anexo, gerencia, descripción), THEN THE Sistema SHALL mostrar los campos faltantes y no guardar el registro.
8. THE Registro_Anexo02 SHALL mostrar una vista previa de los datos ingresados antes de confirmar el guardado.

---

### Requisito 4: Gestión del Backlog

**User Story:** Como gestor del área de Demanda de TI, quiero mantener el Backlog actualizado con todos los requerimientos formalizados, de modo que el equipo tenga una fuente de verdad sobre el estado de cada requerimiento antes de su priorización.

#### Criterios de Aceptación

1. THE Sistema SHALL registrar en el Backlog todos los requerimientos cuyo Anexo_02 haya sido firmado, con Estado_Backlog inicial igual a "Registrado".
2. THE Sistema SHALL almacenar por cada requerimiento en el Backlog los campos: identificador, nombre, gerencia, gestor, Estado_Backlog, fecha de ingreso al Backlog, número de Anexo_02 y número de trámite en ClearQuest (cuando aplique).
3. WHEN se registra el número de trámite ClearQuest de un requerimiento, THE Sistema SHALL actualizar el Estado_Backlog a "Evaluado" automáticamente.
4. THE Sistema SHALL permitir filtrar el Backlog por gerencia, gestor, Estado_Backlog y rango de fechas de ingreso.
5. THE Sistema SHALL mostrar el total de requerimientos en el Backlog agrupados por Estado_Backlog.
6. IF se intenta ingresar manualmente un requerimiento al Backlog sin Anexo_02 firmado asociado, THEN THE Sistema SHALL mostrar un mensaje de error indicando que el Anexo_02 debe estar firmado para ingresar al Backlog.

---

### Requisito 5: Pantalla de Consultas (Área de Demanda de TI)

**User Story:** Como gestor del área de Demanda de TI, quiero una pantalla centralizada que muestre todos los requerimientos con sus datos de demanda, backlog e iniciativas, de modo que pueda monitorear el estado del portafolio completo en un solo lugar.

#### Criterios de Aceptación

1. THE Pantalla_Consultas SHALL mostrar la lista de requerimientos con los campos: identificador, nombre, gerencia, gestor, Estado_Backlog, Estado_Demanda, número de Anexo_02, número de trámite ClearQuest y fecha de último cambio de estado.
2. THE Pantalla_Consultas SHALL permitir filtrar los requerimientos por gerencia, gestor, Estado_Backlog, Estado_Demanda y rango de fechas.
3. THE Pantalla_Consultas SHALL permitir aplicar múltiples filtros simultáneamente y mostrar el conteo de resultados filtrados.
4. WHEN el usuario limpia los filtros, THE Pantalla_Consultas SHALL mostrar la lista completa de requerimientos.
5. THE Pantalla_Consultas SHALL mostrar indicadores visuales diferenciados (por ejemplo, colores o íconos) según el Estado_Backlog y el Estado_Demanda de cada requerimiento.
6. THE Pantalla_Consultas SHALL permitir exportar la lista de requerimientos visible (con los filtros aplicados) a formato Excel.
7. WHEN el usuario selecciona un requerimiento de la lista, THE Pantalla_Consultas SHALL mostrar el detalle completo del requerimiento incluyendo su historial de cambios de estado.
8. THE Pantalla_Consultas SHALL actualizar los datos mostrados sin necesidad de recargar manualmente la pantalla al detectar cambios en los registros subyacentes dentro de la misma sesión de uso.

---

### Requisito 6: Portal por Gerencia

**User Story:** Como representante de una gerencia usuaria, quiero un portal que me muestre todos los requerimientos de mi gerencia (iniciativas, backlog y demanda táctica) con sus datos consolidados, de modo que pueda hacer seguimiento de mis solicitudes sin depender del área de TI para obtener información.

#### Criterios de Aceptación

1. THE Portal_Gerencia SHALL mostrar únicamente los requerimientos correspondientes a la gerencia del usuario autenticado.
2. THE Portal_Gerencia SHALL mostrar los requerimientos agrupados por etapa del ciclo de vida: Iniciativas, Backlog y Demanda Táctica.
3. THE Portal_Gerencia SHALL mostrar el conteo de requerimientos por cada Estado_Backlog y por cada Estado_Demanda.
4. THE Portal_Gerencia SHALL incluir al menos un gráfico de distribución de requerimientos por Estado_Demanda.
5. THE Portal_Gerencia SHALL mostrar por cada requerimiento los campos: nombre, Estado_Backlog, Estado_Demanda, gestor asignado, fecha de ingreso y fecha de último cambio de estado.
6. WHEN el usuario selecciona un requerimiento en el Portal_Gerencia, THE Sistema SHALL mostrar el detalle completo de ese requerimiento.
7. THE Portal_Gerencia SHALL permitir filtrar los requerimientos por etapa del ciclo de vida y por Estado_Backlog.
8. WHERE la gerencia tenga requerimientos en Demanda Táctica, THE Portal_Gerencia SHALL mostrar las fechas propuestas de inicio y fin de cada requerimiento.

---

### Requisito 7: Priorización por Gerencia

**User Story:** Como representante de una gerencia usuaria, quiero poder ordenar mis requerimientos según el orden de atención que mi gerencia prefiere, incluyendo la posibilidad de colocar un requerimiento nuevo por encima de otros ya en demanda, de modo que TI entienda cuál es nuestra prioridad real.

#### Criterios de Aceptación

1. THE Pantalla_Priorizacion SHALL mostrar todos los requerimientos activos de la gerencia del usuario autenticado, ordenados por el orden de prioridad vigente.
2. THE Pantalla_Priorizacion SHALL permitir al usuario reordenar los requerimientos mediante una interfaz de arrastrar y soltar o mediante controles de subir/bajar posición.
3. WHEN el usuario confirma el nuevo orden de prioridad, THE Sistema SHALL guardar el orden actualizado y registrar la fecha y el usuario que realizó el cambio.
4. THE Pantalla_Priorizacion SHALL permitir insertar un requerimiento nuevo en cualquier posición del orden de prioridad, incluso por encima de requerimientos ya incluidos en la Demanda_Tactica.
5. THE Pantalla_Priorizacion SHALL mostrar para cada requerimiento: nombre, Estado_Backlog, Estado_Demanda y posición de prioridad actual.
6. WHEN un requerimiento es insertado en una posición ya ocupada, THE Sistema SHALL recalcular y actualizar la posición de todos los requerimientos afectados de forma automática.
7. THE Pantalla_Priorizacion SHALL mostrar el historial de los últimos tres cambios de orden de prioridad realizados por la gerencia, incluyendo fecha y usuario.
8. IF el usuario intenta guardar el orden de prioridad sin confirmar la acción, THEN THE Sistema SHALL solicitar confirmación explícita antes de persistir los cambios.

---

### Requisito 8: Evaluación de Capacidad

**User Story:** Como gestor del área de Demanda de TI, quiero que el Sistema detecte automáticamente cruces de fechas al registrar un nuevo requerimiento en la Demanda Táctica y me permita resolver esos cruces, de modo que el equipo no quede sobrecomprometido con fechas solapadas.

#### Criterios de Aceptación

1. WHEN se proponen fechas de inicio y fin para un nuevo requerimiento en la Demanda_Tactica, THE Pantalla_Capacidad SHALL detectar automáticamente todos los requerimientos existentes cuyas fechas se solapan con las fechas propuestas para el mismo gestor o equipo.
2. THE Pantalla_Capacidad SHALL mostrar una visualización tipo línea de tiempo (Gantt simplificado) de los requerimientos del gestor o equipo afectado, resaltando visualmente los solapamientos detectados.
3. THE Pantalla_Capacidad SHALL mostrar para cada conflicto de fechas: nombre del requerimiento en conflicto, gestor asignado, fecha de inicio y fin actuales, y el rango de solapamiento en días.
4. THE Pantalla_Capacidad SHALL permitir al gestor modificar las fechas de inicio y/o fin de los requerimientos existentes para resolver el cruce.
5. THE Pantalla_Capacidad SHALL permitir al gestor modificar las fechas propuestas del nuevo requerimiento como alternativa para resolver el cruce.
6. WHEN el gestor modifica fechas y las guarda, THE Sistema SHALL recalcular automáticamente la detección de cruces y actualizar la visualización.
7. WHEN no existen cruces de fechas para el nuevo requerimiento propuesto, THE Pantalla_Capacidad SHALL mostrar un mensaje de confirmación indicando que no hay conflictos de capacidad.
8. IF el gestor intenta registrar un nuevo requerimiento en la Demanda_Tactica sin resolver todos los cruces de fechas detectados, THEN THE Sistema SHALL mostrar una advertencia indicando los conflictos pendientes y permitir igualmente guardar el registro con la advertencia documentada.

---

### Requisito 9: Gestión de la Demanda Táctica

**User Story:** Como gestor del área de Demanda de TI, quiero mantener el registro de la Demanda Táctica actualizado con todos los estados relevantes (demanda, TI, ClearQuest), de modo que el portafolio de desarrollo refleje la situación real en todo momento.

#### Criterios de Aceptación

1. THE Sistema SHALL registrar en la Demanda_Tactica los requerimientos priorizados por el Comite_TI, con los campos: identificador, nombre, gerencia, gestor, Estado_Demanda, estado de TI, estado de ClearQuest, fechas propuestas de inicio y fin, y fecha de priorización.
2. THE Sistema SHALL permitir actualizar de forma independiente el Estado_Demanda, el estado de TI y el estado de ClearQuest de cada requerimiento en la Demanda_Tactica.
3. THE Sistema SHALL registrar en el historial del requerimiento cada cambio de estado en la Demanda_Tactica, con fecha y usuario responsable.
4. THE Sistema SHALL mostrar el total de requerimientos en la Demanda_Tactica agrupados por Estado_Demanda.
5. WHEN un requerimiento en la Demanda_Tactica es cancelado o paralizado, THE Sistema SHALL conservar el registro histórico y no eliminarlo.

---

### Requisito 10: Autenticación y Control de Acceso

**User Story:** Como administrador del Sistema, quiero que cada usuario vea y modifique únicamente la información correspondiente a su rol, de modo que la información sensible del portafolio esté protegida y cada actor vea solo lo que le corresponde.

#### Criterios de Aceptación

1. THE Sistema SHALL autenticar a los usuarios mediante las credenciales corporativas de Microsoft 365 (Azure Active Directory / Entra ID).
2. THE Sistema SHALL diferenciar al menos dos roles de usuario: Gestor_Demanda_TI (acceso a todas las pantallas y registros) y Representante_Gerencia (acceso solo al Portal_Gerencia, Pantalla_Priorizacion y al detalle de sus propios requerimientos).
3. WHILE un usuario tiene el rol Representante_Gerencia, THE Sistema SHALL mostrar únicamente los requerimientos de su propia gerencia en todas las pantallas a las que tiene acceso.
4. IF un usuario intenta acceder a una pantalla para la que no tiene permiso, THEN THE Sistema SHALL redirigirlo a una pantalla de acceso denegado con un mensaje explicativo.
5. THE Sistema SHALL registrar en un log de auditoría las acciones de creación, modificación y cambio de estado realizadas por cada usuario, con fecha, hora e identificador del usuario.

---

### Requisito 11: Wireframes y Representación Visual Referencial

**User Story:** Como miembro del equipo de proyecto, quiero contar con wireframes o mockups referenciales de cada pantalla, de modo que el equipo tenga una idea visual común de la estructura y contenido esperado antes de iniciar el desarrollo.

#### Criterios de Aceptación

1. THE Sistema SHALL ser acompañado por wireframes referenciales de las cinco pantallas: Pantalla_Consultas, Registro_Anexo02, Portal_Gerencia, Pantalla_Priorizacion y Pantalla_Capacidad.
2. THE Sistema SHALL incorporar en los wireframes los campos, filtros y acciones principales definidos en los requisitos de cada pantalla.
3. WHERE el equipo valide los wireframes con las gerencias usuarias y solicite ajustes, THE Sistema SHALL reflejar los ajustes aprobados antes de iniciar el desarrollo de la pantalla correspondiente.

---

### Requisito 12: Plan de Trabajo y Gestión del Proyecto

**User Story:** Como líder del equipo de proyecto, quiero un plan de trabajo detallado con avances semanales que considere que el equipo de cinco personas tiene responsabilidades paralelas, de modo que el proyecto sea ejecutable de forma realista y permita dar feedback periódico.

#### Criterios de Aceptación

1. THE Sistema SHALL ser desarrollado conforme a un plan de trabajo que divida el alcance en entregas semanales verificables.
2. THE Sistema SHALL ser planeado considerando una dedicación parcial del equipo (no dedicación exclusiva) para las cinco personas involucradas.
3. WHEN se completa cada semana de trabajo, THE Sistema SHALL contar con un entregable parcial demostrable que permita al líder dar feedback antes de continuar con la siguiente semana.
4. THE Sistema SHALL ser planeado en fases que respeten el orden de dependencias del ciclo de vida: primero las estructuras de datos y autenticación, luego las pantallas de registro, y finalmente las pantallas de consulta y analítica.
