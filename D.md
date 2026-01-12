## D - Cálculo de Resultados

#### Referencias Técnicas

* **Controlador**: (GXX) `PerformanceProcessController->actionResume`

* **Vista**: (GXX) `/views/performance-process/resume.php`

* **Endpoint procesar cierre**: (GXX) `PerformanceProcessController->actionCierreValidarProceso`

  * Este endpoint actúa como el disparador del cierre del proceso. Internamente, invoca al procedimiento central para el cálculo de notas: el método (GXX) `PerformanceProcessController->calcular_notas_pendientes`. Este motor orquesta la ejecución de 4 pasos secuenciales donde la información se condensa y normaliza progresivamente.

Cada uno de los pasos explicados a continuación redondea la información con los parámetros indicados en la configuración básica del proceso de desempeño.

**Nota importante**: Este procedimiento no realiza cálculos de notas por áreas ni institucional.

#### Glosario de columnas comunes

Estas columnas están presentes en las 4 tablas resultantes de los pasos descritos a continuación:

* `process_id`: ID de la tabla `ctv_gxd_performance_processes`.

* `user_id`: ID del usuario evaluado (`ctv_users`).

* `competence_id`: ID de la competencia (referencia a `ctv_gxd_competences`).

* `goal_id`: ID de la meta (referencia a `ctv_gxd_goals`).

* `function_id`: ID de la función (referencia a `ctv_gxd_functions`).

* `result`: Resultado redondeado y en niveles de la institución.

* `result_expected`: Nivel esperado (según niveles de la institución).

* `result_normalized`: Resultado expresado en porcentaje (donde 100% es el nivel esperado).

* `result_gap`: Brecha, es la resta simple de la `$result` con `$result_expected`, indica la cantidad de nota que le faltó al colaborador para lograr el esperado. Si es negativo representa una deficiencia, si es positivo representa un sobrecumplimiento.

* `scale`: igual a 0 -> porcentual | mayor a 0 -> valores de 1 a `$scale`.

* `weighing`: peso relativo de la competencia funcion o meta respecto a las otras de su misma categoria.

#### Pasos del Procedimiento

**Paso 1: (GXX) `PerformanceProcessController->cierre_finalizarTests`**

* **Descripción**: Se encarga de marcar como no respondidos los test pendientes y de totalizar por competencia, funcion y meta cada test de forma individual.

* **Lógica de condensación**: En este paso desaparecen los reactivos y las tareas. El sistema promedia las notas de los reactivos para calcular la nota de la competencia, e igual en funciones con sus tareas.

* **Conversión de Escala (Única instancia)**: Este es el **único momento** en el proceso donde se realiza la conversión de escala de la "Escala del test" a la "Escala de la institución" para las competencias y funciones.

  * Las **metas** siempre se mantienen en formato de porcentaje.

* **Ponderación de reactivos y tareas**: Los reactivos de competencias y tareas de funciones son tomados todos con el mismo peso y se realiza un promedio simple.

* **Identificación**: Los resultados en esta tabla quedan identificados por 4 coordenadas:

  1. **Test**
  2. **Usuario evaluado**
  3. **Scope (Alcance)**
  4. **Competencia / Función / Meta**

* **Lógica interna**: Cada test es totalizado por la función (GXX) `PerformanceProcessController->totalizarTest`. MySQL calcula un promedio simple agrupando por Test, Usuario evaluado, Scope, Competencia / Función / Meta.

* **Tabla afectada**: `ctv_gxd_performance_process_test_resume_details`

* **Particularidad de los IDs**: Las columnas de identificación referencian al clon que existe dentro del proceso:

  * `competence_id`: apunta a `performance_process_competences`.
  * `goal_id`: apunta a `performance_process_user_goals`.
  * `function_id`: apunta a `performance_process_functions`.

**Paso 2: (GXX) `PerformanceProcessController->cierre_totalizarScopeIndvidual`**

* **Descripción**: Tomamos las notas caracterizadas por usuario, dimension, scope, test. Para cada scope debemos quedarnos con una nota única consolidando la puntuación obtenida por cada uno de los tests de un mismo scope.

* **Lógica de condensación**: Todos los tests que fueron respondidos dentro de un mismo scope para un colaborador determinado son tomados con el mismo peso y se promedian entre ellos para asignar una puntuación en competencias/funciones/metas a ese scope. En esta etapa **desaparece el Test ID** porque ya es totalizado junto a los demás.

* **Identificación**: Los resultados quedan identificados unívocamente por la combinación de:

  * **Usuario evaluado**: El colaborador que recibe la nota.
  * **Competencia / Función / Meta**: La dimensión medida.
  * **Scope (Alcance)**: Tipo de relación (Autoevaluación, 90°, etc.).

* **Tabla afectada**: `ctv_gxd_performance_process_test_resume_by_scope`

**Paso 3: (GXX) `PerformanceProcessController->cierre_totalizarScopePonderados`**

* **Descripción**: Consolida las puntuaciones obtenidas en los diversos alcances (scope) para obtener una nota única por cada elemento medido.

* **Lógica de condensación**: Se toman las notas del Paso 2 (caracterizadas por usuario, dimensión, scope) y se totalizan. Para cada colaborador, se calcula la nota de cada competencia/funcion/meta como promedio ponderado con las notas que obtuvo en cada uno de los alcances. Aquí se utiliza por primera vez la ponderación de los alcances que se definieron en la configuración básica del proceso de desempeño.

* **Condensación**: En esta etapa **desaparece el SCOPE**, ya que cada scope es totalizado junto a los demás.

* **Identificación**: Los resultados quedan identificados unívocamente por:

  * **Usuario evaluado**: El colaborador que recibe la nota.
  * **Competencia / Función / Meta**: La dimensión medida.

* **Tabla afectada**: `ctv_gxd_performance_process_test_resume_compilated`

**Paso 4: (GXX) `PerformanceProcessController->cierre_totalizarCuadroFinal`**

* **Descripción**: Calculamos la nota final obtenida en cada dimensión (competencia, función, metas), calcula la puntuación final obtenida promediando de forma ponderada la nota final que se obtuvo en cada dimensión.

* **Cálculo de promedio final**: El promedio final se calcula utilizando el valor normalizado de cada dimensión, no el valor en escala de la institución, porque cada valor puede tener un nivel esperado distinto, entonces se optó por promediar la nota normalizada porcentual.

* **Tabla afectada**: `ctv_gxd_performance_process_test_resume`

* Al terminar de calcularse los resultados, el ID de la tabla `ctv_gxd_performance_process_test_resume` le es copiado a las otras 3 tablas a todas las filas que alimentaron el cálculo de ese resultado único en:

  * `ctv_gxd_performance_process_test_resume_details.parent_result_id`
  * `ctv_gxd_performance_process_test_resume_by_scope.parent_result_id`
  * `ctv_gxd_performance_process_test_resume_compilated.parent_result_id`

Y se marca en cada una de ellas:

* `ctv_gxd_performance_process_test_resume_details.active=1`
* `ctv_gxd_performance_process_test_resume_by_scope.active=1`
* `ctv_gxd_performance_process_test_resume_compilated.active=1`
* `ctv_gxd_performance_process_test_resume.active = 1` Indicando que este batch corresponde a la nota vigente del colaborador dentro de este proceso.

Asimismo, para indicar que el colaborador fue totalizado, se marca su registro de usuario del proceso `ctv_gxd_performance_process_area_users.results_ok=1`

### Recálculo de notas

Cuando es necesario un recálculo de notas a un colaborador, los datos vigentes de las tablas 1, 2, 3, 4 son marcados como `active=0` y el registro `ctv_gxd_performance_process_area_users.results_ok=0`. Con esto se indica que el colaborador aún no tiene un resultado válido y necesita ser recalculado nuevamente.

Existe un método que es usado luego de esto cuando se necesita invalidar la nota de un usuario porque ha cambiado algo:
(GXX) `PerformanceProcessController::unset_user_results($process_id, $user_id = null)`

Este recibe el ID del proceso y el id de usuario (o un array de IDs de usuarios).

#### Nota sobre pesos de 0%

* Cuando una competencia/función/meta/scope tiene un peso de **0%**, la nota calculada ahí queda sin efecto.

* En estos casos, el registro se marca con la columna `score_without_effects = 1` en las tablas:

  * `ctv_gxd_performance_process_test_resume_by_scope`
  * `ctv_gxd_performance_process_test_resume_compilated`
  * `ctv_gxd_performance_process_test_resume`

* **Visualización en Frontend**: Cuando esto ocurre, en la interfaz de usuario se muestra un asterisco **(\*)** acompañando a la nota calculada y se incluye una nota al pie de página con el texto: **"(\*) nota sin efecto"**.