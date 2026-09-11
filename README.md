# Bitácora de Turno — Control Operacional de Actividades

Aplicación web para registrar y controlar, día a día y por turno (A / C), si las
actividades operacionales de una planta se realizaron o no, con motivo obligatorio
cuando no se realizan.

**App en línea (no requiere instalar nada ni levantar un servidor):**
https://claude.ai/code/artifact/73d7bfd9-5647-427a-98b0-560114aafe26

La app corre publicada como un Artifact de Claude: el link de arriba es la aplicación
funcionando en la nube, con base de datos incluida. `app/index.html` en este repo es
el código fuente versionado — para actualizar la app en línea hay que republicarlo
desde una sesión de Claude Code (no basta con hacer push a este repo).

## Qué hace hoy (v1)

- **Registro diario**: se elige Fecha + Turno (A/C) y aparece el checklist de
  actividades agrupado por área/planta. Cada actividad se marca como
  *Realizado / No realizado / No aplica*; si es "No realizado" pide un comentario
  obligatorio con el motivo. Se guarda automáticamente (sin botón "Guardar"), en
  tiempo real y compartido con quien más esté mirando el mismo turno.
- **Novedades**: registro independiente del operacional, para fallas de equipos
  (Mecánico / Eléctrico / Instrumentación). Se reporta área, equipo, tipo de falla
  (catálogo editable por disciplina) y comentario. Queda "Pendiente" y sigue
  apareciendo turno tras turno hasta marcarla "OK" — no se resetea cada día. Se
  puede reabrir o eliminar si se cargó por error. El menú muestra un contador de
  novedades pendientes.
- **Dashboard**: % de cumplimiento del período, listado de todos los turnos
  registrados (con acceso directo a abrir cualquiera), cumplimiento por área,
  tendencia diaria, y una tabla con todas las actividades "no realizadas" y su
  motivo — la lista de pendientes de gestión. Exporta a CSV.
- **Catálogo**: pantalla para agregar/editar áreas, actividades y tipos de falla,
  y activar o desactivar sin perder el historial ya guardado.
- **Corrección de errores**: en Registro se puede limpiar una actividad mal marcada
  o eliminar el registro completo de un turno; en Novedades se puede reabrir o
  eliminar una novedad. Todas las eliminaciones piden confirmar dos veces.

El catálogo de áreas ya refleja la operación real (Molienda Convencional, Molienda
Unitario 1/2, Molienda SAG, Flotación Rougher/Scavenger, Columnas, Espesador
Concentrado) y se ha ido cargando con las actividades reales de control desde la
pestaña Catálogo.

## Datos y acceso

- Los datos quedan en la base de datos propia del Artifact (no en este repo, no en
  ningún servidor propio). Sobreviven recargas y republicaciones.
- Acceso restringido a personas autenticadas de la misma organización que el
  publicador del Artifact — nadie externo puede verlo ni editarlo.
- Cualquiera con acceso puede editar; el campo "Responsable" queda como registro de
  quién hizo el turno, pero no es un control de acceso por usuario.
- Límite técnico: 5000 documentos en total para esta app. Por eso cada turno
  (fecha + turno) se guarda como **un solo documento** con todas sus actividades
  adentro, en vez de un documento por actividad — a este ritmo alcanza para años de
  registro. Las novedades sí son un documento por evento reportado (no por turno),
  ya que se reportan con mucha menor frecuencia que las actividades diarias.

## Ideas para seguir agregando (no implementado aún)

Para que decidas qué priorizar:

- **Roles**: separar quién puede editar el Catálogo (supervisor) de quién solo
  registra el turno (operador).
- **Notificaciones**: aviso automático (o resumen diario) cuando una actividad
  crítica queda "No realizada".
- **Historial de cambios** por actividad (quién cambió qué y cuándo, no solo el
  último estado).
- **Actividades con frecuencia distinta a diaria** (semanal, mensual) o que no
  aplican a todos los turnos.
- **Fotos/evidencia** adjunta a un registro (requiere la capacidad de archivos del
  Artifact).
- **Versión desplegable en servidor propio** (Codelco intranet) si en algún
  momento se necesita salir del hosting de Claude — se puede portar el modelo de
  datos de este v1 a una base de datos tradicional (Postgres/SQLite) con un backend
  propio.

## Estructura del repo

```
app/
  index.html   # código fuente completo de la app (HTML + CSS + JS, un solo archivo)
README.md
```
