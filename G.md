[Volver](README.md)

# G - Pendientes y Errores

### 1) Revisar

* En `views/performance-process-positions/views.php`, el endpoint de **eliminar preguntas de cargos** apunta al endpoint de **eliminar preguntas de área** (`OpenQuestionsController->actionDeletequestionArea`).

* **Configuración de sobrecumplimiento en Metas**: Al crear metas en el proceso de desempeño se permite editar la configuración de sobrecumplimiento. Este mismo modal es el que se utiliza para asignar metas desde el módulo de gestión de personas, solo que allá tiene un IF para ocultar la configuración de sobrecumplimiento (fue pedido así desde área de productos). Este IF debería eliminarse ya que se pierde la capacidad de editar todos los campos de una meta. Luego del proceso no sería posible ajustar cambios a estos valores de sobrecumplimiento.

### 2) Validaciones/seguridad en endpoints

* No validan acceso ni congruencia de datos; ejecutan la acción “a ciegas”:

  * `controllers/PerformanceProcessController.php` -> `PerformanceProcessController->actionPositions`

  * `controllers/PerformanceProcessController.php` -> `PerformanceProcessController->actionAddcargo`

  * `controllers/PerformanceProcessPositionsController.php` -> `PerformanceProcessPositionsController->actionErase`

  * `controllers/PerformanceProcessAreaController.php` -> `PerformanceProcessAreaController->actionDelete`

  * `controllers/OpenQuestionsController.php` -> `OpenQuestionsController->actionSavequestion`

  * `controllers/OpenQuestionsController.php` -> `OpenQuestionsController->actionDeletequestion`

  * `controllers/OpenQuestionsController.php` -> `OpenQuestionsController->actionSavequestionPersona`

  * `controllers/OpenQuestionsController.php` -> `OpenQuestionsController->actionDeletequestionPersona`

  * `controllers/OpenQuestionsController.php` -> `OpenQuestionsController->actionSavequestionArea`

  * `controllers/OpenQuestionsController.php` -> `OpenQuestionsController->actionDeletequestionArea`

  * `controllers/PerformanceProcessAreaUserController.php` -> `PerformanceProcessAreaUserController->actionAddcompetence`

  * `controllers/PerformanceProcessAreaUserController.php` -> `PerformanceProcessAreaUserController->actionDeletecompetenceuser`

  * `controllers/PerformanceProcessCompetenceController.php` -> `PerformanceProcessCompetenceController->actionCreate`

  * `controllers/PerformanceProcessCompetenceController.php` -> `PerformanceProcessCompetenceController->actionDelete`

  * `controllers/PerformanceProcessPositionsController.php` -> `PerformanceProcessPositionsController->actionAddcompetence`

  * `controllers/PerformanceProcessCompetenceController.php` -> `PerformanceProcessCompetenceController->actionDeletecompetencepos`

### 3) Errores

* **Si el evaluador o el evaluado no cuentan con un registro en la tabla `ctv_user_profiles`, la opción de Descargar detalles en la vista de evaluaciones fallará.**

  * Controlador: `ExportController->actionViewExcelDetails`.

* Cuando se eliminan colaboradores de un proceso de desempeño, el área a la que pertenecen es eliminada automáticamente porque al parecer existe un trigger en BD o evento en Yii2 que realiza eso. Si luego se quiere añadir al proceso algún colaborador del area eliminada, no sería posible desde el front.

* Al añadir una competencia desde la vista de **personas** no funciona correctamente:

  * UI muestra: “competencias agregadas exitosamente”.

  * Endpoint retorna excepción: `yii\base\InvalidArgumentException`: `app\models\PositionCompetences has no relation named \"area\"`.

* Al eliminar competencias desde la vista de **cargos**, los porcentajes de peso no se redistribuyen para volver a sumar **100%**.

* Al añadir una competencia desde la vista de **cargos**, la competencia se carga **sin reactivos** y muchas veces finaliza con error.

* Al añadir una competencia desde la vista de **cargos**, la competencia se carga con un peso fijo de **1%**, provocando que el peso global en el proceso sea erróneo.

* Al iniciar un proceso de desempeño, el mensaje “El usuario ($user_id) no encontrado” ocurre cuando la persona (evaluadora) no tiene registro en la tabla `ctv_user_profiles`. Validación en: `PerformanceProcessController->usuarioPuedeSerEvaluador`.

* Cuando una persona es eliminada de la plataforma pero su registro persiste en `ctv_gxd_performance_process_position_clients` o `suppliers`, el error seguirá ocurriendo hasta eliminar el registro manualmente de esas tablas.

* **Recrear tests en lista de evaluaciones**: Esta opción puede volver a asignar tests a un usuario, incluyendo evaluaciones que estén en estado **soft-deleted** (como una evaluación 90° reasignada). Esto genera duplicados, dejando al evaluado con dos evaluaciones del mismo tipo activas.