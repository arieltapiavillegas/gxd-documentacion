[Volver](README.md)

## C - Evaluaciones

### Detalle de evaluaciones

* La lista se construye desde la tabla `ctv_gxd_performance_tests`.

* Lista todas las evaluaciones agendadas para el proceso de desempeño.

* Incluye filtros por:

  * área,

  * cargo,

  * evaluador,

  * usuario evaluado,

  * alcance,

  * estado.

### Responder masivo

* Opción no presente en el frontend que permite responder evaluaciones automáticamente con datos aleatorios.

* URL:

  * (GXX) `pruebas-fabrica/gxd?id=ID-PROCESO-DESEMPEÑO&limit=100`

* Controlador:

  * (GXX) `pruebas-fabrica/gxd` -> `PruebasFabricaController->actionGxd`

* Parámetros:

  * `id`: ID del proceso de desempeño.

  * `limit`: opcional, por defecto **100**. Responde evaluaciones en lotes de 100 por cada solicitud del URL.

**Acciones disponibles**

* **Agregar colaborador**

  * Solo se pueden añadir colaboradores que no estén participando en el proceso sobre las áreas que sí están participando. Las áreas que no participan en el proceso no pueden añadir colaboradores a través de esta opción.

  * API para popular el modal:

    * (GXX) `GET performance-process/anadir-colaborador-post-inicio` -> `PerformanceProcessController->actionAnadirColaboradorPostInicio`

  * API para procesar la acción:

    * (GXX) `POST performance-process/anadir-colaborador-post-inicio` -> `PerformanceProcessController->actionAnadirColaboradorPostInicio`

  * Al procesar la acción se notifica por email a los evaluadores asignados como evaluaciones pendientes.

* **Descargar detalles**

* **Notificar evaluadores pendientes**

  * Esta opción envía un recordatorio de que tienen evaluaciones pendientes solamente a aquellos evaluadores que tienen por lo menos una evaluación en estatus de pendiente.

  * Controlador: (GXX) `PerformanceProcessController->actionEnviarRecordatorioResponderTests`.

* **Eliminar colaboradores**

  * Controlador Vista listado: (GXX) `performance-process/filter-tests` -> `PerformanceProcessController->actionFilterTests`

  * Vista: (GXX) `/views/performance-process/filter_tests.php`

  * Controlador API Procesar eliminacion: (GXX) `performance-process/eliminar-participants` -> `PerformanceProcessController->actionEliminarParticipants`

* **Responder evaluación como administrador**:

  * abre la plantilla para responder la evaluación asignada al evaluador, pero desde el punto de vista del **administrador**.

  * Controlador: (GXX) `performance-process/poll` -> `PerformanceProcessController->actionPoll`

  * Vista: (GXX) `/views/performance-process/poll.php`

  * Guardar respuestas: (GXX) `performance-process/savepoll` -> `PerformanceProcessController->actionSavepoll`

* **Cambiar evaluador**:

  * permite seleccionar un nuevo evaluador desde la lista de todos los usuarios activos de la institución. **API para popular el modal (lista de evaluadores)**

  * (GXX) `performance-process/editar-evaluator-test` -> `PerformanceProcessController->actionEditarEvaluatorTest` *API para procesar el cambio de evaluador*

  * (GXX) `performance-process/editar-evaluator-test` -> `PerformanceProcessController->actionEditarEvaluatorTest`

  * Nota: requiere que el usuario tenga una entrada en `ctv_user_profiles`; si no existe, se produce error.

  * Comportamiento:

    * el test asignado al evaluador actual queda en estatus **soft-deleted**,

    * se crea un **clon** del test con las respuestas en blanco para el nuevo evaluador.

* **Cambiar estatus**:

  * la evaluación solo puede ser llevada a un siguiente estatus, acorde a la lista de transiciones permitidas. **API para popular el modal con opciones**

  * (GXX) `performance-process/editar-status-test` -> `PerformanceProcessController->actionEditarStatusTest` **API para procesar cambio de estatus**

  * (GXX) `performance-process/editar-status-test` -> `PerformanceProcessController->actionEditarStatusTest`

  * Transiciones permitidas referidas en `PerformanceTests::STATUS_FLUJO`:

    ```
    const STATUS_FLUJO = [
        self::STATUS_ASIGNADA => [self::STATUS_NO_CONTESTADA, self::STATUS_CANCELADA],
        self::STATUS_NO_CONTESTADA => [self::STATUS_ASIGNADA, self::STATUS_CANCELADA],
        self::STATUS_CANCELADA => [self::STATUS_ASIGNADA, self::STATUS_NO_CONTESTADA],
        self::STATUS_FINALIZADA => [self::STATUS_ASIGNADA, self::STATUS_NO_CONTESTADA, self::STATUS_CANCELADA],
    ];
    ```

* **Recrear evaluaciones faltantes**:

  * permite asignar tests al colaborador debido a movimientos en la estructura organizacional de la institución. Regenera todos los tests que debería recibir el colaborador evaluado con los actores actuales del organigrama. Si detecta que una evaluación ya fue asignada, la omite.

  * **UI**: La opción aparece junto a cada evaluación; si el colaborador tiene muchas evaluaciones, el **mismo link** puede aparecer varias veces, ya que la acción se ejecuta sobre el **evaluado** (usuario) y no sobre una evaluación específica.

  * Controlador: (GXX) `performance-process/fix-tests-for-user` -> `PerformanceProcessController->actionFixTestsForUser`

  * Ejemplo URL: (GXX) `performance-process/fix-tests-for-user?user_id=142262&process_id=780`