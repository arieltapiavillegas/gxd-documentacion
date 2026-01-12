[Volver](README.md)

# G - Correos enviados

## 1) (GXX) Notificación a evaluadores al inicio del proceso

### Descripción

Se notifica a los evaluadores asignados cuando se da inicio al proceso de desempeño luego que las evaluaciones han sido asignadas.

### Ruta Web

/performance-process/status?id=

### Ubicación en Front

Menu Configuración -> Resumen.

Esta es la vista de iniciar proceso de desempeño luego de haber finalizado la configuración del mismo.\
Botón "Iniciar Proceso".

### Controlador

`PerformanceProcessController->actionStart`

Internamente llama a 

`PerformanceProcessController->sendEmails`

### Plantilla Email

/mail/send-test-reminder.php



## 2) (GXX) Notificación manual de evaluaciones pendientes

### Descripción

Se mandan los correos a discresción del administrador a través de una opción directa para ello.

### Ruta Web

/performance-process/view-all-tests?id=

### Ubicación en Front

Menu Evaluaciones -> Detalle

Acciones -> **“Notificar evaluadores pendientes”**

### Controlador

`PerformanceProcessController -> actionEnviarRecordatorioResponderTests`

Internamente llama a 

`PerformanceProcessController->sendEmails`

### Vista

/mail/send-test-reminder.php



## 2) (GXX) Notificación a evaluadores por colaborador agregado “post-inicio”

### Descripción

Se notifica a los evaluadores asignados cuando se incorpora un colaborador después de iniciado el proceso, porque se generan evaluaciones nuevas.

### Ruta Web

/performance-process/view-all-tests?id=

### Ubicación en Front

Menu Evaluaciones -> Detalle

Acciones -> **“Agregar colaboradores”**

### Controlador

`PerformanceProcessController -> actionAnadirColaboradorPostInicio`

Internamente llama a 

`PerformanceProcessController->sendEmails`

### Vista

/mail/send-test-reminder.php



## 3) (GXX) Recordatorio manual a evaluadores con evaluaciones pendientes

### Descripción

Recordatorio disparado manualmente, dirigido únicamente a evaluadores que mantienen al menos una evaluación en estado pendiente.

### Ruta Web

/performance-process/view-all-tests?id=

### Ubicación en Front

Menu Evaluaciones -> Detalle

Acciones -> **“Notificar evaluadores pendientes”**.

### Controlador

`PerformanceProcessController -> actionEnviarRecordatorioResponderTests`

Internamente llama a 

`PerformanceProcessController->sendEmails`

### Vista

/mail/send-test-reminder.php





## 4) (GXX) Al cambiar un evaluador de un test, correo al nuevo evaluador

### Descripción

Se notifica al nuevo evaluador asignado cuando se cambia el evaluador de un test.

### Ruta Web

/performance-process/view-all-tests?id=

### Ubicación en Front

Menu Evaluaciones -> Detalle

Acciones sobre el test -> **“Cambiar evaluador”**.

### Controlador

`PerformanceProcessController -> actionEditarEvaluatorTest`

Internamente llama a 

`PerformanceProcessController->_enviarCorreoAsignacionTest`

Que invoca directamente \$app->mailer sin reutilizar las funciones de notificacion que se describieron antes con PerformanceProcessController->sendEmails

### Vista

/mail/test-notificar-asignacion-evaluator.php



## 5) (GXX) Al eliminar un colaborador

### Descripción

Para cada colaborador eliminado, se revisa la lista de evaluadores afectados y se les notifica la lista de evaluaciones que aún tienen pendiente

### Ruta Web

/performance-process/filter-tests?id=

### Ubicación en Front

Menu Evaluaciones -> Detalle

Acciones -> **“Eliminar Colaboradores”**

### Controlador

`PerformanceProcessController -> actionEliminarParticipants` (individual)

`PerformanceProcessController -> actionEliminarParticipantsList` (listado)

Internamente llaman a 

`PerformanceProcessController->sendEmails`

### Vista

/mail/send-test-reminder.php



## 6) (GXX) Al recrear tests perdidos

### Descripción

Cuando se utiliza la opción que está en cada test para recrear tests del colaborador, se notifica a los evaluadores cuando aparecer nuevos tests.

### Ruta Web

/performance-process/view-all-tests?id=

### Ubicación en Front

Menu Evaluaciones -> Detalle

Acciones sobre el test -> **“Recrear tests”**.

### Controlador

`PerformanceProcessController->actionFixTestsForUser`

Internamente llama a 

`PerformanceProcessController->sendEmails`

### Vista

/mail/send-test-reminder.php



## 7) (GXX) Totalizar notas -> no manda correo

### Descripción

Esta funcionalidad fue eliminada. Antes se mandaba un email a cada participante del proceso con la nota obtenida.



## 8) (GXX) Inicio de la etapa de Feedback (correos a jefes y, condicionalmente, a colaboradores)

### Descripción

Al avanzar a Feedback:

- Se envían correos a **jefes de área** con la lista de colaboradores a evaluar y link al LMS.
- Si el tipo de feedback es **“Feedback con colaborador”**, se envía correo al **colaborador** con sus notas/resultados.

### Ruta Web

/performance-process-results/index?id=

### Ubicación en Front

Menu "Resultados y Calibración" -> Borón **“Avanzar a Feedback”**

### Controlador

`PerformanceProcessResultsController->actionEndResultsStep`

Se llama directamente e \$mailer->compose

### Vista

- Colaboradores: Resultados personales\
  /mail/gxd/send-results-test-mail.php
- Jefes: Lista de personas a realizar feedback\
  /mail/send-results-boss-area.php



## 9) Citación por agendamiento de reunión de Feedback

### Descripción

Notificación al colaborador confirmando la citación de reunión de feedback (cuando la reunión queda agendada). Este evento ocurre desde GXX y desde LMS.

### GXX

- **Ruta Web**\
  performance-process-results/details?id=
- **Ubicación**\
  Menu Feedback -> Detalle -> botón calendario
- **Controlador**\
  `PerformanceProcessMeetingsController->actionDashboardAgendar` 
- Vista\
  /mail/send-scheduled-meeting-boss.php

### LMS

- **Ruta Web**\
  /student/boss/home?tab=performance
- **Ubicación**\
  Menu "Mi Equipo" -> Desempeño -> botón calendario
- **Controlador**\
  `/protected/modules/student/controllers/BossController->actionSaveMeetProgram()` 
- Vista\
  /protected/modules/student/views/boss/\_mailFeedback.php





## 10) (GXX) Notificación al cerrar la evaluación de Feedback

### Descripción

Al finalizar la evaluación de feedback se envía un correo al colaborador.  con el link necesario para el flujo de **aceptar/objetar** (ese link no está disponible en otra parte de la plataforma).

### GXX

- **Ruta Web**\
  /performance-process-meetings/dashboard-close?id=
- **Ubicación**\
  Dentro de la vista de realizar evaluación de feedback, el botón "Finalizar Feedback"
- **Controlador**\
  `PerformanceProcessMeetingsController->actionSendmailcolab` 
- Vista 1: cuando **SÍ** se permite objetar resultados\
  /mail/gxd/send-mail-meeting-colab.php
- Vista 2: cuando **NO** se permite objetar resultados\
  /mail/gxd/send-mail-meeting-colab-NO-REJECT.php

### LMS

- **Ruta Web**\
  /student/boss/cerrarFeedback?meet\_id=
- **Ubicación**\
  Dentro de la vista de realizar evaluación de feedback, el botón "Finalizar Feedback"
- **Controlador**\
  `/protected/modules/student/controllers/BossController->actionEnviarFeedback()` 
- Vista **única** para cuando se puede o no objetar resultados\
  /protected/modules/student/views/boss/\_send-mail-meeting-colab.php

NOTA: El correo correspondiente aun feedback donde el coladorador puede objetar los resultados contiene un link para realizar esta operación en el servidor GXX. Este es el único lugar en toda la plataforma en donde este link se presenta. Si el envío de este mail falla, el colaborador no podrá realizar la acción. (GXX) /performance-process-collaborator/accept-reject-results?parametros
