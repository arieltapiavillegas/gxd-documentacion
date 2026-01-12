## B - Configuración del proceso de desempeño

### Configuración básica del proceso de desempeño

#### Referencias de implementación (GXX/Yii2)

* Vista principal: (GXX) `views/performance-process/edit-process.php`

* Fragmentos (parciales):

  * (GXX) `views/performance-process/edit-process/01.php`

  * (GXX) `views/performance-process/edit-process/02.php`

  * (GXX) `views/performance-process/edit-process/03.php`

  * (GXX) `views/performance-process/edit-process/04.php`

  * (GXX) `views/performance-process/edit-process/05.php`

  * (GXX) `views/performance-process/edit-process/06.php`

  * (GXX) `views/performance-process/edit-process/07.php`

  * (GXX) `views/performance-process/edit-process/08.php`

  * (GXX) `views/performance-process/edit-process/09.php` (módulo de seguimiento — desarrollo no culminado)

  * (GXX) `views/performance-process/edit-process/accordion-template.php`

  * (GXX) `views/performance-process/edit-process/commons.php`

* Controlador: (GXX) `controllers/PerformanceProcessController.php` -> `PerformanceProcessController->actionEdit`

* Modelo: (GXX) `models/PerformanceProcesses.php`

* Tabla: `ctv_gxd_performance_processes`

* Ubicación en UI (GXX/Yii2): **Desempeño → Administrar → Crear nuevo proceso**.

* El asistente de creación contiene **8 secciones** a completar:

  1. **Información general**

  2. **Ítems a medir**

  3. **Preguntas**

  4. **Alcance de la medición**

  5. **Cálculo de resultados**

  6. **Calibración**

  7. **Feedback**

  8. **Visualización de resultados**

#### 1) Información general

Campos:

* **Nombre del proceso**

* **Fecha de inicio** del proceso

* **Fecha de fin** del proceso

* **Fecha límite para responder** las evaluaciones

* **Antigüedad laboral mínima**

* **Presupuesto asignado**

* **Usuario firmante** (opcional): se usa para **firmar al pie** del correo electrónico enviado a los participantes, mostrando el nombre de este usuario.

#### 2) Ítems a medir

En esta sección se define qué dimensiones se medirán en el proceso:

* **Competencias**

* **Funciones**

* **Metas**

**Pesos (ponderación)**

* Para cada **módulo** (competencias/funciones/metas) se debe indicar el **peso** dentro del proceso de desempeño.

* La suma de los tres pesos debe ser **100%**.

* Está permitido asignar **0%** a cualquiera de los módulos.

* Si un módulo queda con **0%**, la nota obtenida en ese módulo se considera **referencial (sin efecto)**:

  * no se incorpora al cálculo del **promedio**,

  * no afecta la **nota final** del colaborador.

* Cuando una nota calculada tiene **peso 0%**, el sistema la marca con un **asterisco (*)** en las vistas, y en el **pie de página** se indica que la nota es **sin efecto**.

##### 2.1 Competencias (configuración)

Al activar la medición de competencias, se habilitan opciones.

**Escala de medición**

* Por defecto: queda seleccionada la **escala de la institución** a la que pertenece el proceso.

* Permite seleccionar medición por niveles con escala:

  * **1-2**, **1-3**, **1-4**, **1-5**, **1-6**, **1-7**, **1-8**, **1-9**, **1-10**

  * **Escala porcentual**

* Se puede cambiar, pero **no se recomienda** para evitar diferencias de redondeo respecto de la escala institucional.

**Opciones de visualización durante la evaluación (competencias)**

* **Mostrar nombre de competencias en la evaluación**: corresponde a la **categoría** de la competencia.

* **Mostrar descripción de la competencia**: explica en qué consiste la competencia medida.

* **Mostrar tipo de competencia en evaluación**: indica la **cualidad/tipo** de la competencia evaluada.

* **Listar reactivos en orden aleatorio**: aleatoriza el orden de los reactivos.

**Restricción**

* Si los **reactivos** se listan en **orden aleatorio**, no se permite mostrar la información asociada a:

  * nombre/categoría,

  * descripción,

  * tipo de competencia.

##### 2.2 Funciones de cargo (configuración)

**Escala de medición**

* Por defecto: queda seleccionada la **escala de la institución** a la que pertenece el proceso.

* Permite seleccionar medición por niveles con escala:

  * **1-2**, **1-3**, **1-4**, **1-5**, **1-6**, **1-7**, **1-8**, **1-9**, **1-10**

  * **Escala porcentual**

* Se puede cambiar, pero **no se recomienda** para evitar diferencias de redondeo respecto de la escala institucional.

**Opciones de visualización durante la evaluación (funciones)**

* **Mostrar nombre de funciones en la evaluación**.

* **Mostrar descripción de funciones en la evaluación**: explicación breve de qué es esa función.

* **Mostrar funciones en orden aleatorio**: aleatoriza el orden de funciones a evaluar.

* **Medir funciones sin tareas usando solo la descripción de la función**.

**Restricciones / comportamiento especial**

* Si las funciones se muestran en **orden aleatorio**, no se permite mostrar el **nombre de las funciones**.

* Si se activa **medir funciones sin tareas** (instituciones con funciones definidas pero sin tareas registradas):

  * el sistema crea automáticamente **una única tarea** asociada a cada función;

  * el **nombre de la tarea** corresponde a la **descripción de la función**.

##### 2.3 Metas (configuración)

* No tiene configuraciones adicionales.

* Las metas siempre se miden en **porcentaje**.

#### 3) Preguntas

En esta sección se configuran aspectos de las preguntas asociadas al proceso.

**3.1 Cantidad de reactivos por competencia (máximo)**

* Define el **máximo** de reactivos a incluir por cada competencia dentro del proceso.

* Si una competencia tiene **más** reactivos que el máximo configurado, se toman **solo los primeros N**.

* Si una competencia tiene **menos** reactivos que el máximo configurado, se toman **todos los disponibles**.

**3.2 Preguntas de texto libre**

* Permite habilitar/deshabilitar **preguntas de texto libre** (sí/no).

* Permite definir si las preguntas de texto libre son **obligatorias (requeridas)** (sí/no).

#### 4) Alcance de la medición

Define qué tipos de evaluaciones/relaciones se utilizarán en el proceso.

* **Autoevaluación**: asigna una evaluación para que el **mismo colaborador evaluado** se evalúe a sí mismo.

* **Descendente 90°**: asigna, por cada colaborador evaluado, una evaluación al **jefe de área** para que evalúe a sus colaboradores.

* **Ascendente 180°**: asigna a cada colaborador del área una evaluación para que evalúe a su **jefe directo**.

* **Cliente interno**: relación entre **cargos**.

  * El **cargo del evaluado** tiene definidos **cargos cliente** (es decir, cargos que “consumen” el trabajo/servicio del evaluado).

  * Al activar esta medición, se asigna una evaluación a las personas cuyo cargo esté dentro de esos **cargos cliente**, para que evalúen al colaborador en el aspecto de cliente interno.

* **Proveedor interno**: relación entre **cargos**.

  * El **cargo del evaluado** tiene definidos **cargos proveedor** (es decir, cargos que “proveen” trabajo/insumos al evaluado).

  * Al activar esta medición, se asigna una evaluación a las personas cuyo cargo esté dentro de esos **cargos proveedor**, para que evalúen al colaborador en el aspecto de proveedor interno.

**Pesos (ponderación) por alcance**

* Para cada tipo de alcance configurado se debe indicar el **peso porcentual** (0% a 100%).

* La suma de los pesos de los alcances debe ser **100%**.

* Está permitido asignar **0%** a un alcance.

* Si un alcance queda con **0%**, la nota obtenida en ese alcance se considera **referencial (sin efecto)**:

  * no se incorpora al cálculo del **promedio**,

  * no afecta la **nota final** del colaborador.

* Cuando una nota calculada tiene **peso 0%**, el sistema la marca con un **asterisco (*)** en las vistas, y en el **pie de página** se indica que la nota es **sin efecto**.

#### 5) Cálculo de resultados

En esta sección se define la forma de cálculo/totalización y visualización de la nota final.

**5.1 Decimales durante el cálculo (redondeo interno)**

* Permite definir la cantidad de decimales a mantener durante los cálculos:

  * **0**, **1**, **2**, **3** o **4** decimales.

* En cada etapa interna de cálculo se aplica este redondeo.

* Este redondeo es **parte del cálculo interno** (no es solo un formato visual de la nota final).

**5.2 Redondeo de la nota final**

* Opciones:

  * **Tradicional**: si el siguiente dígito es **≥ 5**, redondea hacia arriba; si es **< 5**, redondea hacia abajo.

  * **Hacia arriba**: redondea siempre hacia arriba.

  * **Hacia abajo**: redondea siempre hacia abajo.

**5.3 Categorías de resultados (ayuda visual)**

* Opción para habilitar **categorías** que ayudan a interpretar la nota final mediante **rangos**.

* Por defecto existen los rangos:

  * **0–50%**: *Deficiente*

  * **50–100%**: *Esperado*

  * **≥ 100%**: *Sobresaliente*

* Los rangos/cortes pueden **modificarse** y se pueden **agregar** más puntos de corte.

#### 6) Calibración

Esta etapa permite **modificar la nota final** del colaborador evaluado.

* La nota calibrable corresponde al **promedio final** ya obtenido a partir de los módulos:

  * competencias,

  * funciones,

  * metas.

* No soporta calibrar cada módulo por separado; solo la **nota final**.

* La nota se expresa en **porcentaje y la calibración se aplica porcentualmente**.

* Solo puede ser realizada por el **administrador** de la institución correspondiente.

**Opciones de configuración**

* **Incluir calibración** (habilitar/deshabilitar).

* Visualización:

  1. Mostrar **solo** la **nota calibrada**.

  2. Mostrar **ambas** notas: **obtenida** y **calibrada**.

#### 7) Feedback

Esta etapa permite definir si se realizará un proceso de **feedback** posterior a la medición.

**Modalidad (al incluir feedback)**

* Opción **Incluir feedback** (sí/no).

* Si se incluye feedback, se define la modalidad:

  * **Feedback con el colaborador**: requiere una instancia de reunión con cada colaborador.

  * **Solo informe**: genera un informe con resultados **sin participación** del colaborador.

* Si se selecciona **solo informe**, no se habilitan todas las opciones de feedback.

**7.1 Rol del colaborador en el feedback (solo si es “Feedback con el colaborador”)**

* **El colaborador evaluado puede objetar resultados** (sí/no): permite que el colaborador objete los resultados luego de recibirlos.

* Si la objeción está habilitada, se puede habilitar adicionalmente:

  * **El colaborador evaluará la calidad del feedback realizado por su jefatura luego de su aceptación** (sí/no).

  * Esta evaluación consiste en una nota **1–5** hacia el evaluador (jefatura).

  * Solo se puede realizar si el colaborador **aceptó** el resultado de su proceso de feedback.

**7.2 Fecha límite del feedback**

* Permite indicar hasta qué fecha se espera realizar el feedback.

* Es **informativa**: el sistema no bloquea la realización del feedback aunque la fecha esté vencida.

**7.3 Instancias del feedback** Permite activar/desactivar componentes del feedback:

* **Planes de acción para brechas de desempeño detectadas** (sí/no).

* **Metas para el siguiente período** (sí/no).

* **Observaciones cualitativas del evaluador** (sí/no).

* **Observaciones cualitativas del evaluado** (sí/no):

  * disponible solo si la modalidad es **Feedback con el colaborador**;

  * permite que el colaborador evaluado redacte un texto que acompañe el feedback recibido.

**7.4 Tipos de planes de acción**

* Se habilita solo si en **7.3** se marca **Planes de acción para brechas de desempeño detectadas**.

* Los planes de acción están ligados a **brechas negativas en competencias**.

* Se debe generar un plan de acción **por cada competencia** con brecha negativa (no alcanzó el nivel esperado / nota mínima).

* Tipos disponibles (se pueden habilitar uno o varios; luego se usan para tipificar el plan al momento del feedback):

  * **Acciones de la jefatura**

  * **Acciones personales**

  * **Capacitación**: obliga a seleccionar un **curso de la plataforma** asociado al plan de acción.

  * **Pasantía**

  * **Otra**

* Opción adicional: definir si es **obligatorio** crear un plan de acción cuando exista brecha negativa en una competencia.

**7.5 Metas (para el siguiente período)**

* Se habilita solo si en **7.3** se marca **Metas para el siguiente período**.

Configuración:

* **Cantidad mínima** y **cantidad máxima** de metas.

* **Ponderación por meta**:

  * define el **mínimo** y **máximo** porcentaje que puede asignarse como ponderación a una meta creada en el feedback.

* **Umbrales de cumplimiento** (informativo):

  * define a partir de qué punto una meta se considera **deficiente** o **sobrecumplida**;

  * no influye en cálculos de notas;

  * se utiliza para mostrar un **tooltip** al evaluar la meta en el siguiente proceso de desempeño.

#### 8) Visualización de resultados

**8.1 Disponibilidad de resultados** Define desde qué momento el colaborador evaluado puede acceder a los **resultados** (notas) obtenidos tras la totalización de evaluaciones.

Opciones:

1. **Nunca**: disponible solo en procesos **sin feedback** (si hay feedback, el colaborador siempre podrá ver su nota obtenida).

2. **Cuando la plataforma los calcule**: el colaborador puede ver su nota apenas quede calculada.

   * Si el proceso tiene **calibración**, el resultado se considera “calculado” cuando **finaliza la calibración**.

3. **Al iniciar el feedback**: la nota se publica al iniciar el proceso de feedback del colaborador.

4. **Al terminar el feedback**: la nota queda disponible cuando finaliza el proceso de feedback.

5. **Al cerrar todo el proceso**.

Nota:

* Cuando hay un proceso con **feedback**, la nota siempre está disponible para ser vista dentro del **módulo de feedback**.

**8.2 Escala de visualización de resultados**

* Permite definir en qué escala se mostrarán los resultados:

  * **Escala porcentual**, o

  * **Escala de la institución**.

### Configuracion de Áreas

**Referencias de implementación (GXX/Yii2)**

* Controlador: (GXX) `controllers/PerformanceProcessController.php` -> `PerformanceProcessController->actionAreas`

* Vista: (GXX) `views/performance-process/_areas.php` (presenta las opciones de **Agregar áreas** y **Reporte de áreas**)

#### Agregar áreas

**Procesamiento de inclusión de áreas**

* La inclusión de áreas se procesa en (GXX) `controllers/PerformanceProcessController.php` -> `PerformanceProcessController->actionAddarea`.

* Para cada área seleccionada:

  * obtiene los usuarios del área,

  * valida **uno a uno** si pueden participar usando los criterios definidos en (GXX) `controllers/PerformanceProcessController.php` -> `PerformanceProcessController->usuarioPuedeAnadirse`.

* Permite agregar **áreas** al proceso de desempeño.

* Se selecciona una **lista de áreas a incluir**.

* Al incluir áreas, el sistema incorpora automáticamente al proceso a **todas las personas** del/las área(s) seleccionada(s) que estén **habilitadas para participar** en un proceso de desempeño.

**Criterio de elegibilidad de personas (GXX/Yii2)** La validación de si un usuario puede añadirse al proceso se realiza en (GXX) `controllers/PerformanceProcessController.php` -> `PerformanceProcessController->usuarioPuedeAnadirse`, y aplica estas validaciones (en orden lógico):

1. **Validación base del usuario** ((GXX) `models/CtvUsers.php` -> `CtvUsers->is_valid`):

   * `ctv_users.status == 1`.

   * El usuario pertenece a la institución del proceso (`CtvUsers->getPertenece($institution_id)`).

2. El usuario **no** está **eliminado**.

3. El usuario ocupa un **cargo vigente**.

4. **Antigüedad mínima**:

   * Se valida con (GXX) `controllers/PerformanceProcessController.php` -> `PerformanceProcessController->validarAntiguedadInstitucion`.

   * Se calcula la antigüedad efectiva (en **meses**) desde la **fecha de contratación** registrada en `ctv_user_belongs_institution` hasta la **fecha de término del proceso**.

5. El usuario **no** ha sido añadido a **otra área** dentro del **mismo proceso**.

#### Reporte de áreas

**Referencias de implementación (GXX/Yii2)**

* Controlador (AJAX / data):

  * (GXX) `controllers/PerformanceProcessAreaController.php` -> `PerformanceProcessAreaController->actionReporteAreas` (trae por AJAX la información de la **lista de áreas**).

  * (GXX) `controllers/PerformanceProcessAreaController.php" -> `PerformanceProcessAreaController->actionReporteColaboradores` (trae por AJAX la **lista de colaboradores** con su estado; reutiliza el mismo esquema de validaciones de elegibilidad documentado en **Agregar áreas**).

* Opción que permite ver, **área por área**:

  * cantidad total de colaboradores,

  * cuántos están **habilitados** para participar en el proceso,

  * cuántos **no** están habilitados para participar.

* El reporte lista **todas las áreas de la institución**, estén participando o no en el proceso.

**Detalle por área**

* Cada área incluye una opción **Ver detalles** que muestra, colaborador por colaborador:

  * estado dentro del proceso (si **ya está incluido**),

  * si **puede ser incluido** (aún no se ha incluido),

  * o si **no puede ser incluido**, indicando el **motivo** por el cual no puede participar.

#### Administrar área

* Permite **gestionar un área específica** dentro del proceso.

**Opciones**

* **Añadir puntualmente colaboradores al área**:

  * permite añadir colaboradores **del área** que **no** estén actualmente incluidos en el proceso;

  * si el área ya fue incorporada previamente (incluyendo a todos los elegibles), la lista puede quedar vacía;

  * si se elimina a un colaborador del proceso, puede volver a aparecer disponible para ser añadido.

* **Reporte de colaboradores**:

  * muestra la lista de personas del área **participen o no** en el proceso, con su **estado**;

  * reutiliza el mismo esquema de validación/estado usado en el detalle de colaboradores del **Reporte de áreas**.

* **Remover colaborador**:

  * endpoint: (GXX) `controllers/PerformanceProcessAreaUserController.php` -> `PerformanceProcessAreaUserController->actionDelete`

  * internamente llama a: (GXX) `controllers/PerformanceProcessController.php" -> `PerformanceProcessController->removeCollaborator`.

* **Eliminar área**:

  * endpoint: (GXX) `controllers/PerformanceProcessAreaController.php" -> `PerformanceProcessAreaController->actionDelete`

  * elimina los colaboradores del área del proceso reutilizando (GXX) `controllers/PerformanceProcessController.php" -> `PerformanceProcessController->removeCollaborator`.

### Configuracion de cargos

**Referencias de implementación (GXX/Yii2)**

* Vista: (GXX) `views/performance-process/_cargos.php`

* Controlador (vista / modal): (GXX) `controllers/PerformanceProcessController.php` -> `PerformanceProcessController->actionPositions`

* Endpoint agregar cargo: (GXX) `controllers/PerformanceProcessController.php" -> `PerformanceProcessController->actionAddcargo`

* Endpoint eliminar cargo: (GXX) `controllers/PerformanceProcessPositionsController.php" -> `PerformanceProcessPositionsController->actionErase`

* Presenta la lista de **cargos** ostentados por las personas que están participando en el proceso de desempeño.

#### Agregar cargos

* Si existen personas habilitadas para participar y sus cargos aún no han sido añadidos al proceso, se puede usar el botón **Agregar** para seleccionar esos cargos y añadirlos.

* Al añadir un cargo, ingresan al proceso todas las personas (de las **áreas que ya participan en el proceso**) que tengan el cargo seleccionado.

* No se incorporan personas de **otras áreas** que no estén participando en el proceso.

**Construcción del modal/formulario (Agregar cargos)**

* (GXX) `controllers/PerformanceProcessController.php" -> `PerformanceProcessController->actionPositions`:

  * obtiene la lista de personas de **todas las áreas** que están participando en el proceso;

  * identifica cuáles **aún no** están participando en el proceso;

  * valida cuáles de esas personas **pueden participar** (según criterios de elegibilidad);

  * extrae los **cargos** de esas personas para construir el modal/formulario de **añadir cargo**.

#### Editar / personalizar cargo

**Referencias de implementación (GXX/Yii2)**

* Controlador: (GXX) `controllers/PerformanceProcessPositionsController.php` -> `PerformanceProcessPositionsController->actionViews`

* Vista: (GXX) `views/performance-process-positions/views.php`

* Permite **editar/personalizar** un cargo dentro del proceso.

* Muestra la lista de personas del proceso que ostentan ese cargo.

* Desde esta vista se puede:

  * **Eliminar** a un colaborador.

  * Acceder a la **vista personal del colaborador** dentro del proceso (editar colaborador).

**Relaciones entre cargos (cliente / proveedor)**

* Presenta la lista de **cargos clientes** y la lista de **cargos proveedores** según las relaciones entre cargos.

* Permite:

  * **eliminar** una relación existente para efectos del proceso de desempeño,

  * o **volver a añadir** un cargo eliminado en estas listas.

Endpoints:

* Eliminar cliente: (GXX) `controllers/PerformanceProcessPositionClientsController.php` -> `PerformanceProcessPositionClientsController->actionEliminar`

* Añadir cliente: (GXX) `controllers/PerformanceProcessPositionsController.php" -> `PerformanceProcessPositionsController->actionAddcliente`

* Eliminar proveedor: (GXX) `controllers/PerformanceProcessPositionSuppliersController.php` -> `PerformanceProcessPositionSuppliersController->actionEliminar`

* Añadir proveedor: (GXX) `controllers/PerformanceProcessPositionsController.php" -> `PerformanceProcessPositionsController->actionAddProveedor`

#### Eliminar cargos

* Se realiza mediante (GXX) `controllers/PerformanceProcessPositionsController.php" -> `PerformanceProcessPositionsController->actionErase`.

* Comportamiento:

  * busca todas las personas que tienen el **cargo** indicado;

  * las elimina del proceso reutilizando (GXX) `controllers/PerformanceProcessController.php" -> `PerformanceProcessController->removeCollaborator`.

* (GXX) `controllers/PerformanceProcessController.php" -> `PerformanceProcessController->removeCollaborator`:

  * remueve al colaborador de todas las tablas correspondientes del proceso;

  * elimina información adicional asociada al proceso para esa persona.

### Configurar personas

* Presenta la lista completa de personas que participan en el proceso de desempeño.

* Para cada persona:

  * opción de **eliminar colaborador**.

  * enlace a la **configuración personal del colaborador** dentro del proceso.

* El enlace a la página personal del colaborador es el **mismo** que se utiliza en **Configuracion de cargos** (lista de ocupantes de un cargo).

### Configuración de preguntas abiertas

1. **Preguntas abiertas globales**

   * Preguntas que se asignan a **cada uno** de los colaboradores que están participando en el proceso de desempeño.

   * Ubicación en UI: tiene su **propia sección** dentro de la configuración del proceso.

   **Referencias de implementación (GXX/Yii2)**

   * Controlador: (GXX) `controllers/PerformanceProcessController.php` -> `PerformanceProcessController->actionOpenQuestions`

   * Vista: (GXX) `views/performance-process/_openquestions.php`

   **Endpoints (crear/eliminar)**

   * Guardar pregunta global: (GXX) `controllers/OpenQuestionsController.php` -> `OpenQuestionsController->actionSavequestion`

   * Eliminar pregunta global: (GXX) `controllers/OpenQuestionsController.php` -> `OpenQuestionsController->actionDeletequestion`

2. **Preguntas abiertas para un área**

   * Preguntas que se asignan a **todas** las personas que componen un **área**.

   * Ubicación en UI: se configura dentro de la **configuración personal de un área**.

   **Endpoints (crear/eliminar)**

   * Guardar pregunta por área: (GXX) `controllers/OpenQuestionsController.php" -> `OpenQuestionsController->actionSavequestionArea`

   * Eliminar pregunta por área: (GXX) `controllers/OpenQuestionsController.php" -> `OpenQuestionsController->actionDeletequestionArea`

3. **Preguntas abiertas para un cargo**

   * Preguntas que se asignan a **todas** las personas que tienen el mismo **cargo**.

   * Ubicación en UI: se configura dentro de la **configuración personal de cada cargo**.

   **Endpoints (crear/eliminar)**

   * Guardar pregunta por cargo: (GXX) `controllers/OpenQuestionsController.php" -> `OpenQuestionsController->actionSavequestion`

   * Eliminar pregunta por cargo: (GXX) `controllers/OpenQuestionsController.php" -> `OpenQuestionsController->actionDeletequestionArea`

4. **Preguntas abiertas para una persona**

   * Preguntas asignadas particularmente a un **colaborador específico**.

   * Ubicación en UI: se configura dentro de la **vista de configuración personal del colaborador**.

   **Endpoints (crear/eliminar)**

   * Guardar pregunta por persona: (GXX) `controllers/OpenQuestionsController.php" -> `OpenQuestionsController->actionSavequestionPersona`

   * Eliminar pregunta por persona: (GXX) `controllers/OpenQuestionsController.php" -> `OpenQuestionsController->actionDeletequestionPersona`

### Configuración de competencias

* En cada opción se presenta la lista de **competencias** que están participando en el proceso tras una recopilación de todas las competencias que tienen asignadas las personas del proceso.

* Estas competencias se consolidan y se muestran en la vista.

* En cada competencia se dispone de:

  * opción de **eliminar**,

  * enlace para **editar** (en una vista aparte) los reactivos que conforman la competencia.

**Nota**

* La lógica de manejo de **reactivos** no está centralizada. Cada controlador mantiene su propia copia del código (frontend y backend) para gestionar reactivos.

1. **Configuración de competencias globales**

**Referencias de implementación (GXX/Yii2)**

* Controlador: (GXX) `controllers/PerformanceProcessCompetenceController.php` -> `PerformanceProcessCompetenceController->actionView`

* Vista: (GXX) `views/performance-process-competence/view.php`

* Endpoint eliminar competencia: (GXX) `controllers/PerformanceProcessCompetenceController.php" -> `PerformanceProcessCompetenceController->actionDelete`

**Editar competencias (reactivos por alcance)**

* Se edita la configuración de **reactivos** por cada alcance del proceso:

  * autoevaluación,

  * 90° (descendente),

  * 180° (ascendente),

  * cliente interno,

  * proveedor interno.

* Guardar configuración de reactivos: (GXX) `controllers/PerformanceProcessCompetenceController.php" -> `PerformanceProcessCompetenceController->actionEditcompetence`

**Editar ponderación de competencias (global)**

* En la vista de competencias globales existe una opción para modificar la **ponderación** de las competencias.

* Endpoint que trae datos para construir el modal: (GXX) `controllers/PerformanceProcessController.php" -> `PerformanceProcessController->actionAjax_search_competences`

* Endpoint que guarda las ponderaciones: (GXX) `controllers/PerformanceProcessController.php" -> `PerformanceProcessController->actionSavePonderacion`

**Añadir competencia (global)**

* Se realiza en una vista dedicada (no solo un modal).

* Presenta un `<select>` con las competencias disponibles para añadir al proceso.

* Se añaden **de uno en uno**.

* Si no hay reactivos disponibles, no se puede añadir la competencia.

* En esta vista es obligatorio indicar qué reactivos se van a añadir por cada alcance del proceso de desempeño:

  * autoevaluación,

  * 90° (descendente),

  * 180° (ascendente),

  * cliente interno,

  * proveedor interno.

**Referencias (vista/endpoint de creación)**

* Controlador: (GXX) `controllers/PerformanceProcessCompetenceController.php" -> `PerformanceProcessCompetenceController->actionCreate` (GET vista / POST creación)

* Vista: (GXX) `views/performance-process-competence/create.php`

20. **Configuración de competencias para cargos**

* El listado de competencias por cargo se presenta dentro de la vista personal de **editar cargo** (descrita en la sección de cargos).

**Editar competencias (reactivos por alcance)**

* Se edita la configuración de **reactivos** por cada alcance del proceso:

  * autoevaluación,

  * 90° (descendente),

  * 180° (ascendente),

  * cliente interno,

  * proveedor interno.

* Controlador: (GXX) `controllers/PerformanceProcessPositionsController.php" -> `PerformanceProcessPositionsController->actionReagents`

* Vista: (GXX) `views/performance-process-positions/reagents.php`

* Guardar configuración de reactivos: (GXX) `controllers/PerformanceProcessPositionsController.php" -> `PerformanceProcessPositionsController->actionEditcompetence`

Endpoints:

* Añadir competencia por cargo: (GXX) `controllers/PerformanceProcessPositionsController.php" -> `PerformanceProcessPositionsController->actionAddcompetence`

* Eliminar competencia por cargo: (GXX) `controllers/PerformanceProcessCompetenceController.php" -> `PerformanceProcessCompetenceController->actionDeletecompetencepos`

30. **Configuración de competencias para personas**

* El listado de competencias se presenta dentro de la **vista personal de editar persona** (sección **Configurar personas**).

**Editar competencias (reactivos por alcance)**

* Se edita la configuración de **reactivos** por cada alcance del proceso:

  * autoevaluación,

  * 90° (descendente),

  * 180° (ascendente),

  * cliente interno,

  * proveedor interno.

* Controlador: (GXX) `controllers/PerformanceProcessAreaUserController.php" -> `PerformanceProcessAreaUserController->actionReagents`

* Vista: (GXX) `views/performance-process-area-user/reagents.php`

* Guardar configuración de reactivos: (GXX) `controllers/PerformanceProcessAreaUserController.php" -> `PerformanceProcessAreaUserController->actionEditcompetence`

**Endpoints**

* Añadir competencia (persona): (GXX) `controllers/PerformanceProcessAreaUserController.php" -> `PerformanceProcessAreaUserController->actionAddcompetence`

* Eliminar competencia (persona): (GXX) `controllers/PerformanceProcessAreaUserController.php" -> `PerformanceProcessAreaUserController->actionDeletecompetenceuser`

### Configuración de funciones de cargo

1. **Funciones de cargo globales**

   Esta configuración se gestiona desde la **vista personal de editar cargo**.

2. **Funciones de cargo para personas**

   Esta configuración se gestiona desde la **vista personal de editar colaborador**.

3. **Funcionalidades comunes**

En estas vistas se muestra la tabla con las funciones del cargo y permite:

* **agregar** función,

* **editar** función,

* **eliminar** función.

**Edición / detalle de una función (modal)**

Al editar una función se presenta un **modal** que permite visualizar la configuración de evaluación:

* en la **vertical**: la lista de **tareas del cargo** asociadas a la función,

* en la **horizontal**: cada alcance donde será evaluada (autoevaluación, 90°, 180°, cliente interno, proveedor interno).

**Endpoints (modal / selección de tareas por alcance)**

* Traer datos que se muestran en el modal: (GXX) `controllers/PerformanceProcessFunctionsController.php" -> `PerformanceProcessFunctionsController->actionTasksScope`

* Procesar selecciones del modal: (GXX) `controllers/PerformanceProcessFunctionsController.php" -> `PerformanceProcessFunctionsController->actionChangeTasksScope`

**Añadir función de cargo (modal)**

* Endpoint para llenar el modal de añadir función: (GXX) `controllers/PerformanceProcessFunctionsController.php" -> `PerformanceProcessFunctionsController->actionListEmptyFunctions`

* Solo lista funciones de cargo que no tienen tareas participando en el proceso.

* Luego de elegir la función a añadir, se reutiliza el mismo modal de edición para seleccionar tareas por alcance.

### Iniciar proceso de desempeño

* Endpoint para dar inicio al proceso de desempeño:

  * (GXX) `controllers/PerformanceProcessController.php" -> `PerformanceProcessController->actionStart`