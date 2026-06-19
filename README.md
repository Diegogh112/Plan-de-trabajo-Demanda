# Sistema de Gestión de Demanda Táctica TI
**Banco de la Nación — Oficina de Planificación y Presupuesto (OPP)**

Prototipo referencial de pantallas para el área de Demanda Táctica de TI.  
Stack previsto: Power Apps · SharePoint Lists · Power Automate · Power BI

---

## 🚀 Demo en línea
El prototipo se puede ver en: **[URL de Vercel — pendiente de deploy]**

---

## 📁 Estructura del repositorio

```
pantallas/
  app.html          ← Prototipo integrado (6 pantallas)
  index.html        ← Landing page
.kiro/specs/demanda-tactica-ti/
  requirements.md   ← 12 requerimientos funcionales
  design.md         ← Arquitectura, modelo de datos, wireframes
  tasks.md          ← 9 tareas de implementación
  plan-de-trabajo.md← Cronograma 10 semanas
vercel.json         ← Configuración de deploy
```

---

## 📋 Resumen del Plan de Trabajo

| Semana | Fase | Entregable |
|--------|------|-----------|
| 1 | Preparación | Entorno SharePoint + permisos + datos de prueba |
| 2 | Registro | Formulario Anexo 02 + Flow → Backlog |
| 3 | Registro | Backlog completo + Flows de estado (CQ, Comité) |
| 4 | Registro | Iniciativas + trazabilidad completa E2E |
| 5 | Consulta | Pantalla Consultas TI con filtros y exportación |
| 6 | Análisis | Portal por Gerencia con gráficos Power BI |
| 7 | Análisis | Priorización por Gerencia (drag & drop) |
| 8 | Capacidad | Evaluación de Capacidad — detección de cruces |
| 9 | Integración | Sistema integrado, auditoría, regresión |
| 10 | UAT/Prod | Validación con usuarios + pase a producción |

**Equipo:** 5 personas · dedicación parcial (4–8 h/persona/semana)  
**Duración total:** 10 semanas

---

## 🎯 Requerimientos clave

| # | Requerimiento | Pantalla |
|---|---------------|---------|
| 1 | Ciclo de vida del requerimiento (8 estados) | Transversal |
| 2 | Registro de Iniciativas | App TI |
| 3 | Registro y Firma del Anexo 02 | App TI |
| 4 | Gestión del Backlog | App TI |
| 5 | Pantalla de Consultas (TI) | App TI |
| 6 | Portal por Gerencia | App Gerencias |
| 7 | Priorización por Gerencia | App Gerencias |
| 8 | Evaluación de Capacidad | App TI |
| 9 | Gestión de Demanda Táctica | App TI |
| 10 | Autenticación y Control de Acceso | Transversal |

---

## 🔄 Ciclo de vida de un requerimiento

```
Iniciativa Registrada
  → Solicitud en Proceso   (Anexo 02 generado)
    → Solicitud Visada     (Anexo 02 firmado)
      → Registrado         (Trámite CQ creado — ingresa al Backlog)
        → Evaluado         (Aprobado por Construcción)
          → Priorizado     (Comité TI — ingresa a Demanda Táctica)
          → Rechazado      (Comité TI lo descarta)
          → Descartado     (Paralizado en cualquier etapa)
```

---

## 🏗️ Tareas de implementación

- **Tarea 1:** Configurar entorno base (SharePoint, permisos, datos de prueba)
- **Tarea 2:** Canvas App TI — Registro de Anexo 02 + Flow → Backlog
- **Tarea 3:** Backlog + transiciones de estado (Flows CQ y Comité)
- **Tarea 4:** Iniciativas + trazabilidad completa
- **Tarea 5:** Pantalla de Consultas TI (filtros, ficha navegable, exportación)
- **Tarea 6:** Portal por Gerencia (KPIs + gráficos Power BI)
- **Tarea 7:** Priorización por Gerencia (drag & drop + notificaciones)
- **Tarea 8:** Evaluación de Capacidad (Gantt + detección de cruces)
- **Tarea 9:** Integración final + auditoría + UAT + producción

---

## ⚙️ Deploy en Vercel

1. Conecta este repositorio en [vercel.com](https://vercel.com)
2. Framework: **Other** (sin framework)
3. Root directory: `.` (raíz del repo)
4. El archivo `vercel.json` ya está configurado para redirigir `/` → `/pantallas/app.html`

---

*Prototipo referencial — Junio 2026*
