# E - Calibración

#### Referencias Técnicas

* **Controlador Vista**
  (GXX) `/performance-process-results/peoples?id=780` -> `PerformanceProcessResultsController->actionPeoples`

* **Vista**
  (GXX) `/views/performance-process-results/peoples.php`

* **Api para guardar calibración**
  (GXX) `/performance-process-results/save-calibration` -> `PerformanceProcessResultsController->actionSaveCalibration`

* **Publicacion de resultados calibrados** (GXX) `/performance-process-results/finalizar-calibracion?process_id=780` -> `PerformanceProcessResultsController->actionFinalizarCalibracion`

#### Funcionamiento del Proceso

Este proceso permite modificar la nota normalizada final, de los 3 modulos ya promediados, en la tabla del paso 4 del calculo de resultados: `ctv_gxd_performance_process_test_resume`.

Campos afectados:

* `result`: aumenta o disminuye su nota en ±100%

* `result_original`: guarda la nota original obtenida

* `calibration`: Guarda el valor porcentual del ajuste realizado

#### Publicacion de resultados calibrados

Esto no realiza cálculos, solo cambia el estatus del proceso.