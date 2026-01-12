# F - Feedback

#### Avanzar a Feedback

* **Endpoint para avanzar a feedback**
  (GXX) `/performance-process-results/end-results-step?process_id=780` -> `PerformanceProcessResultsController->actionEndResultsStep`

Este controlador llama a `PerformanceProcessResultsController->crear_feedback_y_correos`. En este paso se envían emails a los involucrados en el proceso de feedback:

* A los **jefes de área** para realizar las evaluaciones de feedback de sus colaboradores. El correo incluye la lista de los colaboradores a evaluar y un link hacía el servidor LMS para realizar las evaluaciones.

* A los **colaboradores** solamente si el tipo de feedback es **FEEDBACK CON COLABORADOR**. El correo incluye las notas que el colaborador obtuvo en competencias, funciones y metas.

Se crea por cada colaborador una entrada única en la tabla `ctv_gxd_performance_process_meetings`, con `status=0` (Pendiente).

#### 1. Gestión de Reuniones de Feedback

**Referencias Técnicas**

* **Controlador**: (GXX) `PerformanceProcessResultsController`

* **Vista de lista de reuniones**: (GXX) `/views/performance-process-results/feedback_list.php`

* **Vista de ejecución de feedback**: (GXX) `/views/performance-process-results/feedback_reunion.php`

* **Tabla**: `ctv_gxd_performance_process_meetings`

* **Agendar reuniones de feedback**:

  * **Endpoint para popular el modal de agendar reunión**:
    (GXX) `performance-process-meetings/dashboard-agendar` -> `PerformanceProcessMeetingsController->actionDashboardAgendar`

  * **Endpoint para procesar agendar la reunión de feedback**:
    (GXX) `performance-process-meetings/dashboard-agendar` -> `PerformanceProcessMeetingsController->actionDashboardAgendar`

    * Al agendar la reunión se manda un email al colaborador notificando la citación.

* **Cambiar el evaluador del feedback**:

  * **Endpoint para popular el modal de cambio de evaluador**:
    (GXX) `/performance-process-results/cambiar-evaluador` -> `PerformanceProcessResultsController->actionCambiarEvaluador`

  * **Endpoint para procesar el cambio**:
    (GXX) `/performance-process-results/cambiar-evaluador` -> `PerformanceProcessResultsController->actionCambiarEvaluador`

* **Guardar evaluación de feedback**:

  * **Endpoint para guardar y enviar notificación**:
    (GXX) `/performance-process-meetings/sendmailcolab` -> `PerformanceProcessMeetingsController->actionSendmailcolab`

**Estados de la reunión (`status`)**

* **0 (Pendiente)**: Reunión creada, aún no iniciada por el evaluador.

* **1 (En proceso)**: El evaluador ha guardado cambios parciales pero no ha finalizado.

* **2 (Finalizada/Enviada)**: El evaluador cerró la reunión. Si el proceso permite objeción, queda a la espera de la acción del colaborador.

* **3 (Aceptada)**: El colaborador aceptó el feedback recibido.

* **4 (Objetada)**: El colaborador rechazó los resultados.

#### 2. Componentes del Feedback

Dependiendo de la configuración del proceso, se habilitan los siguientes módulos en la vista de la reunión:

**2.1 Planes de Acción**

* **Controlador**: (GXX) `PerformanceActionPlansController`

* **Agregar plan de acción**:

  * **Endpoint para popular modal de formulario**:
    (GXX) `/performance-action-plans/agregar` -> `PerformanceActionPlansController->actionAgregar`

  * **Endpoint para procesar guardado**:
    (GXX) `/performance-action-plans/agregar` -> `PerformanceActionPlansController->actionAgregar`

* **Tabla**: `ctv_gxd_performance_process_action_plans`

* **Lógica**: si se tienen competencias en la tabla `ctv_gxd_performance_process_test_resume_compilated` donde con brecha negativa `result_gap < 0` se puede asociar uno o varios planes de acción a cada una de estas competencias DE FORMA OPCIONAL, puede quedar en blanco. Si el proceso tiene marcado que se deben crear planes de acción a las competencias con brechas negativas, al momento de finalizar el feedback se validará que cada competencia con brecha negativa tenga al menos 1 plan de acción.

* **Tipos**: Definidos en la configuración (Capacitación, Pasantía, Acciones personales, etc.).

* **Vinculación**: Si el tipo es **Capacitación**, se debe asociar un curso desde el catálogo del LMS.

**2.2 Metas para el siguiente período**

* **Tabla**: `ctv_gxd_goals`

* **Lógica**: Permite al evaluador proponer los objetivos que se medirán en el próximo ciclo de desempeño. El listado que se presenta aquí son las metas no evaluadas del colaborador. Las metas no quedan asociadas al proceso de desempeño, por lo cual si se crear más metas fuera de este módulo, también saldrán listadas aquí.

**2.3 Observaciones cualitativas**

* Comentarios del evaluador: Guardados en `ctv_gxd_performance_process_meetings.observations`.

* Comentarios del evaluado: Guardados en `ctv_gxd_performance_process_meetings.observations_collab`.

* Desde la vista de LMS el evaluador no puede llenar los comentarios del evaluado, solo se podría esta excepción desde el administrador en el servidor GXX.

#### 3. Flujo de Objeción y Evaluación de Calidad

**Referencias Técnicas**

* **Controlador Vista**: (GXX) `performance-process-collaborator/accept-reject-results` -> `PerformanceProcessCollaboratorController->actionAcceptRejectResults`

* **Vista**: (GXX) `views/performance-process-collaborator/accept-reject-results.php`

* **Endpoint procesar objeción/aceptación/calidad-feedback**: (GXX) `performance-process-collaborator/accept-reject-results` -> `PerformanceProcessCollaboratorController->actionAcceptRejectResults`

**Lógica del Flujo**

* El flujo de objeción de resultados solo puede iniciarse desde el correo electrónico que manda la plataforma cuando se cierra la evaluación de feedback. Este link no está presente en ningún otro lugar de la plataforma, solo en el email.

* **Objeción**: Si el colaborador marca el feedback como objetado (`status=4`), debe registrar un motivo en la tabla `ctv_gxd_performance_process_meetings` (campo `objection_reason`).

* **Calidad del Feedback**: Si está habilitado, tras aceptar (`status=3`), el colaborador califica la gestión de su jefe.

  * **Campo**: `ctv_gxd_performance_process_meetings.feedback_quality_score` (escala 1-5).