# Documentación Gestión de Desempeño

> Nota: las ediciones se aplican siempre sobre la versión vigente del documento. Si el usuario edita manualmente el canvas, los cambios posteriores deben respetar ese estado.

## A - Contexto general

- **Servidor LMS**: Yii1
- **Servidor GXX**: Yii2

---

### Resumen

- Objetivo: Permitir la **medición de indicadores** para la **gestión de desempeño** de los colaboradores.
- Servidor/Framework: **GXX (Yii2)**
- Ubicación (alto nivel): (pendiente)

### Alcance funcional (visión general)

La medición se estructura en **tres áreas**:

1. **Competencias**: competencias asociadas al **cargo** que ejerce el colaborador.
2. **Funciones específicas del cargo**: evaluación según **funciones** propias del cargo.
3. **Metas**: medición según **metas** definidas.

### Conceptos y relaciones

#### Competencias

- Existe un **repositorio común** de competencias.
- Las competencias pueden ser **asignadas a distintos cargos** (compartibles entre cargos).
- Cada competencia se compone de **reactivos** (preguntas específicas) con los que se realiza su medición.
- En términos funcionales, las competencias representan **habilidades** requeridas para ejercer un cargo.

#### Funciones del cargo

- Las **funciones de cargo** son características/responsabilidades propias de un cargo.
- No son compartidas entre cargos (no existe repositorio común compartible como en competencias).
- Se miden a través de **tareas**, que son los ítems con los que se evalúa cada función.
- En términos funcionales, las funciones representan **responsabilidades/funciones** que se deben ejercer dentro del cargo.

#### Metas

- Se refieren a **objetivos medibles en el tiempo** que una persona debe cumplir.

#### Tareas

- Son los ítems mediante los cuales se miden funciones del cargo.
- Se crean **específicamente para cada persona**; pueden ser **todas distintas** entre colaboradores.

## B - Configuración del proceso de desempeño

### Configuración básica del proceso de desempeño

#### Referencias de implementación (GXX/Yii2)

- Vista principal: `views/performance-process/edit-process.php`

- Fragmentos (parciales):

  - `views/performance-process/edit-process/01.php`
  - `views/performance-process/edit-process/02.php`
  - `views/performance-process/edit-process/03.php`
  - `views/performance-process/edit-process/04.php`
  - `views/performance-process/edit-process/05.php`
  - `views/performance-process/edit-process/06.php`
  - `views/performance-process/edit-process/07.php`
  - `views/performance-process/edit-process/08.php`
  - `views/performance-process/edit-process/09.php` (módulo de seguimiento — desarrollo no culminado)
  - `views/performance-process/edit-process/accordion-template.php`
  - `views/performance-process/edit-process/commons.php`

- Controlador: `controllers/PerformanceProcessController.php` -> `PerformanceProcessController->actionEdit`

- Modelo: `models/PerformanceProcesses.php`

- Tabla: `ctv_gxd_performance_processes`



- Ubicación en UI (GXX/Yii2): **Desempeño -> Administrar -> Crear nuevo proceso**.

- El asistente de creación contiene **8 secciones** a completar:

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

- **Nombre del proceso**
- **Fecha de inicio** del proceso
- **Fecha de fin** del proceso
- **Fecha límite para responder** las evaluaciones
- **Antigüedad laboral mínima**
- **Presupuesto asignado**
- **Usuario firmante** (opcional): se usa para **firmar al pie** del correo electrónico enviado a los participantes, mostrando el nombre de este usuario.

#### 2) Ítems a medir

En esta sección se define qué dimensiones se medirán en el proceso:

- **Competencias**
- **Funciones**
- **Metas**

**Pesos (ponderación)**

- Para cada **módulo** (competencias/funciones/metas) se debe indicar el **peso** dentro del proceso de desempeño.
- La suma de los tres pesos debe ser **100%**.
- Está permitido asignar **0%** a cualquiera de los módulos.
- Si un módulo queda con **0%**, la nota obtenida en ese módulo se considera **referencial (sin efecto)**:
  - no se incorpora al cálculo del **promedio**,
  - no afecta la **nota final** del colaborador.
- Cuando una nota calculada tiene **peso 0%**, el sistema la marca con un **asterisco (*)** en las vistas, y en el **pie de página** se indica que la nota es **sin efecto**.

##### 2.1 Competencias (configuración)

Al activar la medición de competencias, se habilitan opciones.

**Escala de medición**

- Por defecto: queda seleccionada la **escala de la institución** a la que pertenece el proceso.
- Permite seleccionar medición por niveles con escala:
  - **1-2**, **1-3**, **1-4**, **1-5**, **1-6**, **1-7**, **1-8**, **1-9**, **1-10**
  - **Escala porcentual**
- Se puede cambiar, pero **no se recomienda** para evitar diferencias de redondeo respecto de la escala institucional.

**Opciones de visualización durante la evaluación (competencias)**

- **Mostrar nombre de competencias en la evaluación**: corresponde a la **categoría** de la competencia.
- **Mostrar descripción de la competencia**: explica en qué consiste la competencia medida.
- **Mostrar tipo de competencia en evaluación**: indica la **cualidad/tipo** de la competencia evaluada.
- **Listar reactivos en orden aleatorio**: aleatoriza el orden de los reactivos.

**Restricción**

- Si los **reactivos** se listan en **orden aleatorio**, no se permite mostrar la información asociada a:
  - nombre/categoría,
  - descripción,
  - tipo de competencia.

##### 2.2 Funciones de cargo (configuración)

**Escala de medición**

- Por defecto: queda seleccionada la **escala de la institución** a la que pertenece el proceso.
- Permite seleccionar medición por niveles con escala:
  - **1-2**, **1-3**, **1-4**, **1-5**, **1-6**, **1-7**, **1-8**, **1-9**, **1-10**
  - **Escala porcentual**
- Se puede cambiar, pero **no se recomienda** para evitar diferencias de redondeo respecto de la escala institucional.

**Opciones de visualización durante la evaluación (funciones)**

- **Mostrar nombre de funciones en la evaluación**.
- **Mostrar descripción de funciones en la evaluación**: explicación breve de qué es esa función.
- **Mostrar funciones en orden aleatorio**: aleatoriza el orden de funciones a evaluar.
- **Medir funciones sin tareas usando solo la descripción de la función**.

**Restricciones / comportamiento especial**

- Si las funciones se muestran en **orden aleatorio**, no se permite mostrar el **nombre de las funciones**.
- Si se activa **medir funciones sin tareas** (instituciones con funciones definidas pero sin tareas registradas):
  - el sistema crea automáticamente **una única tarea** asociada a cada función;
  - el **nombre de la tarea** corresponde a la **descripción de la función**.

##### 2.3 Metas (configuración)

- No tiene configuraciones adicionales.
- Las metas siempre se miden en **porcentaje**.

#### 3) Preguntas

En esta sección se configuran aspectos de las preguntas asociadas al proceso.

**3.1 Cantidad de reactivos por competencia (máximo)**

- Define el **máximo** de reactivos a incluir por cada competencia dentro del proceso.
- Si una competencia tiene **más** reactivos que el máximo configurado, se toman **solo los primeros N**.
- Si una competencia tiene **menos** reactivos que el máximo configurado, se toman **todos los disponibles**.

**3.2 Preguntas de texto libre**

- Permite habilitar/deshabilitar **preguntas de texto libre** (sí/no).
- Permite definir si las preguntas de texto libre son **obligatorias (requeridas)** (sí/no).

#### 4) Alcance de la medición

Define qué tipos de evaluaciones/relaciones se utilizarán en el proceso.

- **Autoevaluación**: asigna una evaluación para que el **mismo colaborador evaluado** se evalúe a sí mismo.
- **Descendente 90°**: asigna, por cada colaborador evaluado, una evaluación al **jefe de área** para que evalúe a sus colaboradores.
- **Ascendente 180°**: asigna a cada colaborador del área una evaluación para que evalúe a su **jefe directo**.
- **Cliente interno**: relación entre **cargos**.
  - El **cargo del evaluado** tiene definidos **cargos cliente** (es decir, cargos que “consumen” el trabajo/servicio del evaluado).
  - Al activar esta medición, se asigna una evaluación a las personas cuyo cargo esté dentro de esos **cargos cliente**, para que evalúen al colaborador en el aspecto de cliente interno.
- **Proveedor interno**: relación entre **cargos**.
  - El **cargo del evaluado** tiene definidos **cargos proveedor** (es decir, cargos que “proveen” trabajo/insumos al evaluado).
  - Al activar esta medición, se asigna una evaluación a las personas cuyo cargo esté dentro de esos **cargos proveedor**, para que evalúen al colaborador en el aspecto de proveedor interno.

**Pesos (ponderación) por alcance**

- Para cada tipo de alcance configurado se debe indicar el **peso porcentual** (0% a 100%).
- La suma de los pesos de los alcances debe ser **100%**.
- Está permitido asignar **0%** a un alcance.
- Si un alcance queda con **0%**, la nota obtenida en ese alcance se considera **referencial (sin efecto)**:
  - no se incorpora al cálculo del **promedio**,
  - no afecta la **nota final** del colaborador.
- Cuando una nota calculada tiene **peso 0%**, el sistema la marca con un **asterisco (*)** en las vistas, y en el **pie de página** se indica que la nota es **sin efecto**.

#### 5) Cálculo de resultados

En esta sección se define la forma de cálculo/totalización y visualización de la nota final.

**5.1 Decimales durante el cálculo (redondeo interno)**

- Permite definir la cantidad de decimales a mantener durante los cálculos:
  - **0**, **1**, **2**, **3** o **4** decimales.
- En cada etapa interna de cálculo se aplica este redondeo.
- Este redondeo es **parte del cálculo interno** (no es solo un formato visual de la nota final).

**5.2 Redondeo de la nota final**

- Opciones:
  - **Tradicional**: si el siguiente dígito es **≥ 5**, redondea hacia arriba; si es **< 5**, redondea hacia abajo.
  - **Hacia arriba**: redondea siempre hacia arriba.
  - **Hacia abajo**: redondea siempre hacia abajo.

**5.3 Categorías de resultados (ayuda visual)**

- Opción para habilitar **categorías** que ayudan a interpretar la nota final mediante **rangos**.
- Por defecto existen los rangos:
  - **0–50%**: *Deficiente*
  - **50–100%**: *Esperado*
  - **≥ 100%**: *Sobresaliente*
- Los rangos/cortes pueden **modificarse** y se pueden **agregar** más puntos de corte.

#### 6) Calibración

Esta etapa permite **modificar la nota final** del colaborador evaluado.

- La nota calibrable corresponde al **promedio final** ya obtenido a partir de los módulos:
  - competencias,
  - funciones,
  - metas.
- No soporta calibrar cada módulo por separado; solo la **nota final**.
- La nota se expresa en **porcentaje** y la calibración se aplica **porcentualmente**.
- Solo puede ser realizada por el **administrador** de la institución correspondiente.

**Opciones de configuración**

- **Incluir calibración** (habilitar/deshabilitar).
- Visualización:
  1. Mostrar **solo** la **nota calibrada**.
  2. Mostrar **ambas** notas: **obtenida** y **calibrada**.

#### 7) Feedback

Esta etapa permite definir si se realizará un proceso de **feedback** posterior a la medición.

**Modalidad (al incluir feedback)**

- Opción **Incluir feedback** (sí/no).
- Si se incluye feedback, se define la modalidad:
  - **Feedback con el colaborador**: requiere una instancia de reunión con cada colaborador.
  - **Solo informe**: genera un informe con resultados **sin participación** del colaborador.
- Si se selecciona **solo informe**, no se habilitan todas las opciones de feedback.

**7.1 Rol del colaborador en el feedback (solo si es “Feedback con el colaborador”)**

- **El colaborador evaluado puede objetar resultados** (sí/no): permite que el colaborador objete los resultados luego de recibirlos.
- Si la objeción está habilitada, se puede habilitar adicionalmente:
  - **El colaborador evaluará la calidad del feedback realizado por su jefatura luego de su aceptación** (sí/no).
  - Esta evaluación consiste en una nota **1–5** hacia el evaluador (jefatura).
  - Solo se puede realizar si el colaborador **aceptó** el resultado de su proceso de feedback.

**7.2 Fecha límite del feedback**

- Permite indicar hasta qué fecha se espera realizar el feedback.
- Es **informativa**: el sistema no bloquea la realización del feedback aunque la fecha esté vencida.

**7.3 Instancias del feedback** Permite activar/desactivar componentes del feedback:

- **Planes de acción para brechas de desempeño detectadas** (sí/no).
- **Metas para el siguiente período** (sí/no).
- **Observaciones cualitativas del evaluador** (sí/no).
- **Observaciones cualitativas del evaluado** (sí/no):
  - disponible solo si la modalidad es **Feedback con el colaborador**;
  - permite que el colaborador evaluado redacte un texto que acompañe el feedback recibido.

**7.4 Tipos de planes de acción**

- Se habilita solo si en **7.3** se marca **Planes de acción para brechas de desempeño detectadas**.
- Los planes de acción están ligados a **brechas negativas en competencias**.
- Se debe generar un plan de acción **por cada competencia** con brecha negativa (no alcanzó el nivel esperado / nota mínima).
- Tipos disponibles (se pueden habilitar uno o varios; luego se usan para tipificar el plan al momento del feedback):
  - **Acciones de la jefatura**
  - **Acciones personales**
  - **Capacitación**: obliga a seleccionar un **curso de la plataforma** asociado al plan de acción.
  - **Pasantía**
  - **Otra**
- Opción adicional: definir si es **obligatorio** crear un plan de acción cuando exista brecha negativa en una competencia.

**7.5 Metas (para el siguiente período)**

- Se habilita solo si en **7.3** se marca **Metas para el siguiente período**.

Configuración:

- **Cantidad mínima** y **cantidad máxima** de metas.
- **Ponderación por meta**:
  - define el **mínimo** y **máximo** porcentaje que puede asignarse como ponderación a una meta creada en el feedback.
- **Umbrales de cumplimiento** (informativo):
  - define a partir de qué punto una meta se considera **deficiente** o **sobrecumplida**;
  - no influye en cálculos de notas;
  - se utiliza para mostrar un **tooltip** al evaluar la meta en el siguiente proceso de desempeño.

#### 8) Visualización de resultados

**8.1 Disponibilidad de resultados** Define desde qué momento el colaborador evaluado puede acceder a los **resultados** (notas) obtenidos tras la totalización de evaluaciones.

Opciones:

1. **Nunca**: disponible solo en procesos **sin feedback** (si hay feedback, el colaborador siempre podrá ver su nota obtenida).
2. **Cuando la plataforma los calcule**: el colaborador puede ver su nota apenas quede calculada.
   - Si el proceso tiene **calibración**, el resultado se considera “calculado” cuando **finaliza la calibración**.
3. **Al iniciar el feedback**: la nota se publica al iniciar el proceso de feedback del colaborador.
4. **Al terminar el feedback**: la nota queda disponible cuando finaliza el proceso de feedback.
5. **Al cerrar todo el proceso**.

Nota:

- Cuando hay un proceso con **feedback**, la nota siempre está disponible para ser vista dentro del **módulo de feedback**.

**8.2 Escala de visualización de resultados**

- Permite definir en qué escala se mostrarán los resultados:
  - **Escala porcentual**, o
  - **Escala de la institución**.





---

### Configuracion de Áreas

**Referencias de implementación (GXX/Yii2)**

- Controlador: `controllers/PerformanceProcessController.php` -> `PerformanceProcessController->actionAreas`
- Vista: `views/performance-process/_areas.php` (presenta las opciones de **Agregar áreas** y **Reporte de áreas**)

#### Agregar áreas

**Procesamiento de inclusión de áreas**

- La inclusión de áreas se procesa en `controllers/PerformanceProcessController.php` -> `PerformanceProcessController->actionAddarea`.

- Para cada área seleccionada:

  - obtiene los usuarios del área,
  - valida **uno a uno** si pueden participar usando los criterios definidos en `controllers/PerformanceProcessController.php` -> `PerformanceProcessController->usuarioPuedeAnadirse`.

- Permite agregar **áreas** al proceso de desempeño.

- Se selecciona una **lista de áreas a incluir**.

- Al incluir áreas, el sistema incorpora automáticamente al proceso a **todas las personas** del/las área(s) seleccionada(s) que estén **habilitadas para participar** en un proceso de desempeño.

**Criterio de elegibilidad de personas (GXX/Yii2)** La validación de si un usuario puede añadirse al proceso se realiza en `controllers/PerformanceProcessController.php` -> `PerformanceProcessController->usuarioPuedeAnadirse`, y aplica estas validaciones (en orden lógico):

1. **Validación base del usuario** (`models/CtvUsers.php` -> `CtvUsers->is_valid`):
   - `ctv_users.status == 1`.
   - El usuario pertenece a la institución del proceso (`CtvUsers->getPertenece($institution_id)`).
2. El usuario **no** está **eliminado**.
3. El usuario ocupa un **cargo vigente**.
4. **Antigüedad mínima**:
   - Se valida con `controllers/PerformanceProcessController.php` -> `PerformanceProcessController->validarAntiguedadInstitucion`.
   - Se calcula la antigüedad efectiva (en **meses**) desde la **fecha de contratación** registrada en `ctv_user_belongs_institution` hasta la **fecha de término del proceso**.
5. El usuario **no** ha sido añadido a **otra área** dentro del **mismo proceso**.

#### Reporte de áreas

**Referencias de implementación (GXX/Yii2)**

- Controlador (AJAX / data):

  - `controllers/PerformanceProcessAreaController.php` -> `PerformanceProcessAreaController->actionReporteAreas` (trae por AJAX la información de la **lista de áreas**).
  - `controllers/PerformanceProcessAreaController.php` -> `PerformanceProcessAreaController->actionReporteColaboradores` (trae por AJAX la **lista de colaboradores** con su estado; reutiliza el mismo esquema de validaciones de elegibilidad documentado en **Agregar áreas**).

- Opción que permite ver, **área por área**:

  - cantidad total de colaboradores,
  - cuántos están **habilitados** para participar en el proceso,
  - cuántos **no** están habilitados para participar.

- El reporte lista **todas las áreas de la institución**, estén participando o no en el proceso.

**Detalle por área**

- Cada área incluye una opción **Ver detalles** que muestra, colaborador por colaborador:
  - estado dentro del proceso (si **ya está incluido**),
  - si **puede ser incluido** (aún no se ha incluido),
  - o si **no puede ser incluido**, indicando el **motivo** por el cual no puede participar.

#### Administrar área

- Permite **gestionar un área específica** dentro del proceso.

**Opciones**

- **Añadir puntualmente colaboradores al área**:
  - permite añadir colaboradores **del área** que **no** estén actualmente incluidos en el proceso;
  - si el área ya fue incorporada previamente (incluyendo a todos los elegibles), la lista puede quedar vacía;
  - si se elimina a un colaborador del proceso, puede volver a aparecer disponible para ser añadido.
- **Reporte de colaboradores**:
  - muestra la lista de personas del área **participen o no** en el proceso, con su **estado**;
  - reutiliza el mismo esquema de validación/estado usado en el detalle de colaboradores del **Reporte de áreas**.
- **Remover colaborador**:
  - endpoint: `controllers/PerformanceProcessAreaUserController.php` -> `PerformanceProcessAreaUserController->actionDelete`
  - internamente llama a: `controllers/PerformanceProcessController.php` -> `PerformanceProcessController->removeCollaborator`.
- **Eliminar área**:
  - endpoint: `controllers/PerformanceProcessAreaController.php` -> `PerformanceProcessAreaController->actionDelete`
  - elimina los colaboradores del área del proceso reutilizando `controllers/PerformanceProcessController.php` -> `PerformanceProcessController->removeCollaborator`.

---

### Configuracion de cargos

**Referencias de implementación (GXX/Yii2)**

- Vista: `views/performance-process/_cargos.php`

- Controlador (vista / modal): `controllers/PerformanceProcessController.php` -> `PerformanceProcessController->actionPositions`

- Endpoint agregar cargo: `controllers/PerformanceProcessController.php` -> `PerformanceProcessController->actionAddcargo`

- Endpoint eliminar cargo: `controllers/PerformanceProcessPositionsController.php` -> `PerformanceProcessPositionsController->actionErase`

- Presenta la lista de **cargos** ostentados por las personas que están participando en el proceso de desempeño.

#### Agregar cargos

- Si existen personas habilitadas para participar y sus cargos aún no han sido añadidos al proceso, se puede usar el botón **Agregar** para seleccionar esos cargos y añadirlos.
- Al añadir un cargo, ingresan al proceso todas las personas (de las **áreas que ya participan en el proceso**) que tengan el cargo seleccionado.
- No se incorporan personas de **otras áreas** que no estén participando en el proceso.

**Construcción del modal/formulario (Agregar cargos)**

- `controllers/PerformanceProcessController.php` -> `PerformanceProcessController->actionPositions`:
  - obtiene la lista de personas de **todas las áreas** que están participando en el proceso;
  - identifica cuáles **aún no** están participando en el proceso;
  - valida cuáles de esas personas **pueden participar** (según criterios de elegibilidad);
  - extrae los **cargos** de esas personas para construir el modal/formulario de **añadir cargo**.

#### Editar / personalizar cargo

**Referencias de implementación (GXX/Yii2)**

- Controlador: `controllers/PerformanceProcessPositionsController.php` -> `PerformanceProcessPositionsController->actionViews`

- Vista: `views/performance-process-positions/views.php`

- Permite **editar/personalizar** un cargo dentro del proceso.

- Muestra la lista de personas del proceso que ostentan ese cargo.

- Desde esta vista se puede:

  - **Eliminar** a un colaborador.
  - Acceder a la **vista personal del colaborador** dentro del proceso (editar colaborador).

**Relaciones entre cargos (cliente / proveedor)**

- Presenta la lista de **cargos clientes** y la lista de **cargos proveedores** según las relaciones entre cargos.
- Permite:
  - **eliminar** una relación existente para efectos del proceso de desempeño,
  - o **volver a añadir** un cargo eliminado en estas listas.

Endpoints:

- Eliminar cliente: `controllers/PerformanceProcessPositionClientsController.php` -> `PerformanceProcessPositionClientsController->actionEliminar`
- Añadir cliente: `controllers/PerformanceProcessPositionsController.php` -> `PerformanceProcessPositionsController->actionAddcliente`
- Eliminar proveedor: `controllers/PerformanceProcessPositionSuppliersController.php` -> `PerformanceProcessPositionSuppliersController->actionEliminar`
- Añadir proveedor: `controllers/PerformanceProcessPositionsController.php` -> `PerformanceProcessPositionsController->actionAddProveedor`

#### Eliminar cargos

- Se realiza mediante `controllers/PerformanceProcessPositionsController.php` -> `PerformanceProcessPositionsController->actionErase`.
- Comportamiento:
  - busca todas las personas que tienen el **cargo** indicado;
  - las elimina del proceso reutilizando `controllers/PerformanceProcessController.php` -> `PerformanceProcessController->removeCollaborator`.
- `controllers/PerformanceProcessController.php` -> `PerformanceProcessController->removeCollaborator`:
  - remueve al colaborador de todas las tablas correspondientes del proceso;
  - elimina información adicional asociada al proceso para esa persona.

---

### Configurar personas

- Presenta la lista completa de personas que participan en el proceso de desempeño.
- Para cada persona:
  - opción de **eliminar colaborador**.
  - enlace a la **configuración personal del colaborador** dentro del proceso.
- El enlace a la página personal del colaborador es el **mismo** que se utiliza en **Configuracion de cargos** (lista de ocupantes de un cargo).

---

### Configuración de preguntas abiertas

1. **Preguntas abiertas globales**

   - Preguntas que se asignan a **cada uno** de los colaboradores que están participando en el proceso de desempeño.
   - Ubicación en UI: tiene su **propia sección** dentro de la configuración del proceso.

   **Referencias de implementación (GXX/Yii2)**

   - Controlador: `controllers/PerformanceProcessController.php` -> `PerformanceProcessController->actionOpenQuestions`
   - Vista: `views/performance-process/_openquestions.php`

   **Endpoints (crear/eliminar)**

   - Guardar pregunta global: `controllers/OpenQuestionsController.php` -> `OpenQuestionsController->actionSavequestion`
   - Eliminar pregunta global: `controllers/OpenQuestionsController.php` -> `OpenQuestionsController->actionDeletequestion`

2. **Preguntas abiertas para un área**

   - Preguntas que se asignan a **todas** las personas que componen un **área**.
   - Ubicación en UI: se configura dentro de la **configuración personal de un área**.

   **Endpoints (crear/eliminar)**

   - Guardar pregunta por área: `controllers/OpenQuestionsController.php" -> "OpenQuestionsController->actionSavequestionArea`
   - Eliminar pregunta por área: `controllers/OpenQuestionsController.php" -> "OpenQuestionsController->actionDeletequestionArea`

3. **Preguntas abiertas para un cargo**

   - Preguntas que se asignan a **todas** las personas que tienen el mismo **cargo**.
   - Ubicación en UI: se configura dentro de la **configuración personal de cada cargo**.

   **Endpoints (crear/eliminar)**

   - Guardar pregunta por cargo: `controllers/OpenQuestionsController.php` -> `OpenQuestionsController->actionSavequestion`
   - Eliminar pregunta por cargo: `controllers/OpenQuestionsController.php" -> "OpenQuestionsController->actionDeletequestionArea`

4. **Preguntas abiertas para una persona**

   - Preguntas asignadas particularmente a un **colaborador específico**.
   - Ubicación en UI: se configura dentro de la **vista de configuración personal del colaborador**.

   **Endpoints (crear/eliminar)**

   - Guardar pregunta por persona: `controllers/OpenQuestionsController.php" -> "OpenQuestionsController->actionSavequestionPersona`
   - Eliminar pregunta por persona: `controllers/OpenQuestionsController.php" -> "OpenQuestionsController->actionDeletequestionPersona`

---

### Configuración de competencias

- En cada opción se presenta la lista de **competencias** que están participando en el proceso tras una recopilación de todas las competencias que tienen asignadas las personas del proceso.
- Estas competencias se consolidan y se muestran en la vista.
- En cada competencia se dispone de:
  - opción de **eliminar**,
  - enlace para **editar** (en una vista aparte) los reactivos que conforman la competencia.

**Nota**

- La lógica de manejo de **reactivos** no está centralizada. Cada controlador mantiene su propia copia del código (frontend y backend) para gestionar reactivos.

1. **Configuración de competencias globales**

**Referencias de implementación (GXX/Yii2)**

- Controlador: `controllers/PerformanceProcessCompetenceController.php` -> `PerformanceProcessCompetenceController->actionView`
- Vista: `views/performance-process-competence/view.php`
- Endpoint eliminar competencia: `controllers/PerformanceProcessCompetenceController.php` -> `PerformanceProcessCompetenceController->actionDelete`

**Editar competencias (reactivos por alcance)**

- Se edita la configuración de **reactivos** por cada alcance del proceso:
  - autoevaluación,
  - 90° (descendente),
  - 180° (ascendente),
  - cliente interno,
  - proveedor interno.
- Guardar configuración de reactivos: `controllers/PerformanceProcessCompetenceController.php` -> `PerformanceProcessCompetenceController->actionEditcompetence`

**Editar ponderación de competencias (global)**

- En la vista de competencias globales existe una opción para modificar la **ponderación** de las competencias.
- Endpoint que trae datos para construir el modal: `controllers/PerformanceProcessController.php` -> `PerformanceProcessController->actionAjax_search_competences`
- Endpoint que guarda las ponderaciones: `controllers/PerformanceProcessController.php` -> `PerformanceProcessController->actionSavePonderacion`

**Añadir competencia (global)**

- Se realiza en una vista dedicada (no solo un modal).
- Presenta un `<select>` con las competencias disponibles para añadir al proceso.
- Se añaden **de uno en uno**.
- Si no hay reactivos disponibles, no se puede añadir la competencia.
- En esta vista es obligatorio indicar qué reactivos se van a añadir por cada alcance del proceso de desempeño:
  - autoevaluación,
  - 90° (descendente),
  - 180° (ascendente),
  - cliente interno,
  - proveedor interno.

**Referencias (vista/endpoint de creación)**

- Controlador: `controllers/PerformanceProcessCompetenceController.php` -> `PerformanceProcessCompetenceController->actionCreate` (GET vista / POST creación)
- Vista: `views/performance-process-competence/create.php`

2. **Configuración de competencias para cargos**

- El listado de competencias por cargo se presenta dentro de la vista personal de **editar cargo** (descrita en la sección de cargos).

**Editar competencias (reactivos por alcance)**

- Se edita la configuración de **reactivos** por cada alcance del proceso:
  - autoevaluación,
  - 90° (descendente),
  - 180° (ascendente),
  - cliente interno,
  - proveedor interno.
- Controlador: `controllers/PerformanceProcessPositionsController.php` -> `PerformanceProcessPositionsController->actionReagents`
- Vista: `views/performance-process-positions/reagents.php`
- Guardar configuración de reactivos: `controllers/PerformanceProcessPositionsController.php` -> `PerformanceProcessPositionsController->actionEditcompetence`

Endpoints:

- Añadir competencia por cargo: `controllers/PerformanceProcessPositionsController.php` -> `PerformanceProcessPositionsController->actionAddcompetence`
- Eliminar competencia por cargo: `controllers/PerformanceProcessCompetenceController.php" -> "PerformanceProcessCompetenceController->actionDeletecompetencepos`

3. **Configuración de competencias para personas**

- El listado de competencias se presenta dentro de la **vista personal de editar persona** (sección **Configurar personas**).

**Editar competencias (reactivos por alcance)**

- Se edita la configuración de **reactivos** por cada alcance del proceso:
  - autoevaluación,
  - 90° (descendente),
  - 180° (ascendente),
  - cliente interno,
  - proveedor interno.
- Controlador: `controllers/PerformanceProcessAreaUserController.php` -> `PerformanceProcessAreaUserController->actionReagents`
- Vista: `views/performance-process-area-user/reagents.php`
- Guardar configuración de reactivos: `controllers/PerformanceProcessAreaUserController.php` -> `PerformanceProcessAreaUserController->actionEditcompetence`

**Endpoints**

- Añadir competencia (persona): `controllers/PerformanceProcessAreaUserController.php` -> `PerformanceProcessAreaUserController->actionAddcompetence`
- Eliminar competencia (persona): `controllers/PerformanceProcessAreaUserController.php" -> "PerformanceProcessAreaUserController->actionDeletecompetenceuser`

---

### Configuración de funciones de cargo

1. **Funciones de cargo globales**

   Esta configuración se gestiona desde la **vista personal de editar cargo**.

2) **Funciones de cargo para personas**

   Esta configuración se gestiona desde la **vista personal de editar colaborador**.

3. **Funcionalidades comunes**

En estas vistas se muestra la tabla con las funciones del cargo y permite:

- **agregar** función,
- **editar** función,
- **eliminar** función.

**Edición / detalle de una función (modal)**

Al editar una función se presenta un **modal** que permite visualizar la configuración de evaluación:

- en la **vertical**: la lista de **tareas del cargo** asociadas a la función,
- en la **horizontal**: cada alcance donde será evaluada (autoevaluación, 90°, 180°, cliente interno, proveedor interno).

**Endpoints (modal / selección de tareas por alcance)**

- Traer datos que se muestran en el modal: `controllers/PerformanceProcessFunctionsController.php` -> `PerformanceProcessFunctionsController->actionTasksScope`
- Procesar selecciones del modal: `controllers/PerformanceProcessFunctionsController.php` -> `PerformanceProcessFunctionsController->actionChangeTasksScope`

**Añadir función de cargo (modal)**

- Endpoint para llenar el modal de añadir función: `controllers/PerformanceProcessFunctionsController.php` -> `PerformanceProcessFunctionsController->actionListEmptyFunctions`
- Solo lista funciones de cargo que no tienen tareas participando en el proceso.
- Luego de elegir la función a añadir, se reutiliza el mismo modal de edición para seleccionar tareas por alcance.

---

### Iniciar proceso de desempeño

- Endpoint para dar inicio al proceso de desempeño:
  - `controllers/PerformanceProcessController.php` -> `PerformanceProcessController->actionStart`

---

## C - Evaluaciones

### Detalle de evaluaciones

- La lista se construye desde la tabla `ctv_gxd_performance_tests`.
- Lista todas las evaluaciones agendadas para el proceso de desempeño.
- Incluye filtros por:
  - área,
  - cargo,
  - evaluador,
  - usuario evaluado,
  - alcance,
  - estado.

### Responder masivo

- Opción no presente en el frontend que permite responder evaluaciones automáticamente con datos aleatorios.
- URL:
  - `pruebas-fabrica/gxd?id=ID-PROCESO-DESEMPEÑO&limit=100`
- Controlador:
  - `pruebas-fabrica/gxd` -> `PruebasFabricaController->actionGxd`
- Parámetros:
  - `id`: ID del proceso de desempeño.
  - `limit`: opcional, por defecto **100**. Responde evaluaciones en lotes de 100 por cada solicitud del URL.

**Acciones disponibles**

- **Agregar colaborador**
  - Solo se pueden añadir colaboradores que no estén participando en el proceso sobre las áreas que sí están participando. Las áreas que no participan en el proceso no pueden añadir colaboradores a través de esta opción.
  - API para popular el modal:
    - `GET performance-process/anadir-colaborador-post-inicio` -> `PerformanceProcessController->actionAnadirColaboradorPostInicio`
  - API para procesar la acción:
    - `POST performance-process/anadir-colaborador-post-inicio` -> `PerformanceProcessController->actionAnadirColaboradorPostInicio`
  - Al procesar la acción se notifica por email a los evaluadores asignados como evaluaciones pendientes.
- **Descargar detalles**
- **Notificar evaluadores pendientes**
  - Esta opción envía un recordatorio de que tienen evaluaciones pendientes solamente a aquellos evaluadores que tienen por lo menos una evaluación en estatus de pendiente.
  - Controlador: `PerformanceProcessController->actionEnviarRecordatorioResponderTests`.
- **Eliminar colaboradores**
  - Controlador Vista listado: `PerformanceProcessController->actionFilterTests`.
  - Vista: `/views/performance-process/filter_tests.php`.
  - Controlador API Procesar eliminacion: `PerformanceProcessController->actionEliminarParticipants`.
- **Responder evaluación como administrador**:
  - abre la plantilla para responder la evaluación asignada al evaluador, pero desde el punto de vista del **administrador**.
  - Controlador: `controllers/PerformanceProcessController.php` -> `PerformanceProcessController->actionPoll` (ruta: `performance-process/poll`)
  - Vista: `views/performance-process/poll.php`
  - Guardar respuestas: `performance-process/savepoll` -> `PerformanceProcessController->actionSavepoll`
- **Cambiar evaluador**:
  - permite seleccionar un nuevo evaluador desde la lista de todos los usuarios activos de la institución. **API para popular el modal (lista de evaluadores)**
  - `performance-process/editar-evaluator-test` -> `PerformanceProcessController->actionEditarEvaluatorTest` *API para procesar el cambio de evaluador*
  - `performance-process/editar-evaluator-test` -> `PerformanceProcessController->actionEditarEvaluatorTest`
  - Nota: requiere que el usuario tenga una entrada en `ctv_user_profiles`; si no existe, se produce error.
  - Comportamiento:
    - el test asignado al evaluador actual queda en estatus **soft-deleted**,
    - se crea un **clon** del test con las respuestas en blanco para el nuevo evaluador.
- **Cambiar estatus**:
  - la evaluación solo puede ser llevada a un siguiente estatus, acorde a la lista de transiciones permitidas. **API para popular el modal con opciones**

  - `performance-process/editar-status-test` -> `PerformanceProcessController->actionEditarStatusTest` **API para procesar cambio de estatus**

  - `performance-process/editar-status-test` -> `PerformanceProcessController->actionEditarStatusTest`

  - Transiciones permitidas referidas en el modelo `PerformanceTests` en la constante `STATUS_FLUJO`:

    ```php
    // Indica a qué estatus puede ser llevada la evaluación desde este modal,
    // tomando como key el estatus actual de la evaluación.
    const STATUS_FLUJO = [
        self::STATUS_ASIGNADA => [self::STATUS_NO_CONTESTADA, self::STATUS_CANCELADA],
        self::STATUS_NO_CONTESTADA => [self::STATUS_ASIGNADA, self::STATUS_CANCELADA],
        self::STATUS_CANCELADA => [self::STATUS_ASIGNADA, self::STATUS_NO_CONTESTADA],
        self::STATUS_FINALIZADA => [self::STATUS_ASIGNADA, self::STATUS_NO_CONTESTADA, self::STATUS_CANCELADA],
    ];
    ```
- **Recrear evaluaciones faltantes**:
  - permite asignar tests al colaborador debido a movimientos en la estructura organizacional de la institución, donde pueden aparecer nuevos actores que no fueron considerados al inicio del proceso.
  - Esta revisión se realiza regenerando todos los tests que debería recibir el colaborador evaluado con los actores actuales del organigrama. Si detecta que una evaluación ya fue asignada, la omite.
  - Controlador:
    - `performance-process/fix-tests-for-user` -> `PerformanceProcessController->actionFixTestsForUser`
  - Ejemplo URL:
    - `performance-process/fix-tests-for-user?user_id=142262&process_id=780`
  - UI:
    - La opción aparece junto a cada evaluación; si el colaborador tiene muchas evaluaciones, el **mismo link** puede aparecer varias veces, ya que la acción se ejecuta sobre el **evaluado** (usuario) y no sobre una evaluación específica.

## D - Calibración y Resultados

> Sección pendiente de completar.

## E - Feedback

> Sección pendiente de completar.

## F - Pendientes

### 1) Revisar

- En `views/performance-process-positions/views.php`, el endpoint de **eliminar preguntas de cargos** apunta al endpoint de **eliminar preguntas de área** (`OpenQuestionsController->actionDeletequestionArea`).

### 2) Validaciones/seguridad en endpoints

- No validan acceso ni congruencia de datos; ejecutan la acción “a ciegas”:
  - `controllers/PerformanceProcessController.php` -> `PerformanceProcessController->actionPositions`
  - `controllers/PerformanceProcessController.php` -> `PerformanceProcessController->actionAddcargo`
  - `controllers/PerformanceProcessPositionsController.php` -> `PerformanceProcessPositionsController->actionErase`
  - `controllers/PerformanceProcessAreaController.php" -> "PerformanceProcessAreaController->actionDelete`
  - `controllers/OpenQuestionsController.php` -> `OpenQuestionsController->actionSavequestion`
  - `controllers/OpenQuestionsController.php` -> `OpenQuestionsController->actionDeletequestion`
  - `controllers/OpenQuestionsController.php" -> "OpenQuestionsController->actionSavequestionPersona`
  - `controllers/OpenQuestionsController.php" -> "OpenQuestionsController->actionDeletequestionPersona`
  - `controllers/OpenQuestionsController.php" -> "OpenQuestionsController->actionSavequestionArea`
  - `controllers/OpenQuestionsController.php" -> "OpenQuestionsController->actionDeletequestionArea`
  - `controllers/PerformanceProcessAreaUserController.php` -> `PerformanceProcessAreaUserController->actionAddcompetence`
  - `controllers/PerformanceProcessAreaUserController.php` -> `PerformanceProcessAreaUserController->actionDeletecompetenceuser`
  - `controllers/PerformanceProcessCompetenceController.php` -> `PerformanceProcessCompetenceController->actionCreate`
  - `controllers/PerformanceProcessCompetenceController.php` -> `PerformanceProcessCompetenceController->actionDelete`
  - `controllers/PerformanceProcessPositionsController.php` -> `PerformanceProcessPositionsController->actionAddcompetence`
  - `controllers/PerformanceProcessCompetenceController.php" -> "PerformanceProcessCompetenceController->actionDeletecompetencepos`

### 3) Errores

- Si el evaluador o el evaluado no tienen profile, la opción de descargar datos en la vista de evaluaciónes fallará.
  - Controlador: `ExportController->actionViewExcelDetails`.
- Cuando se eliminan colaboradores de un proceso de desempeño, el área a la que pertenecen es eliminada automáticamente porque al parecer existe un trigger en BD o evento en Yii2 que realiza eso. Si luego se quiere añadir al proceso algún colaborador del area eliminada, no sería posible desde el front.
- Al añadir una competencia desde la vista de **personas** no funciona correctamente:
  - UI muestra: “competencias agregadas exitosamente”.
  - Endpoint retorna excepción:
    - `yii\base\InvalidArgumentException`: `app\models\PositionCompetences has no relation named \"area\"`.
    - Trace relevante:
      - `#7 /var/www/gxx/controllers/PerformanceProcessAreaUserController.php(434): yii\db\ActiveQuery->one()`
- Al eliminar competencias desde la vista de **cargos**, los porcentajes de peso no se redistribuyen para volver a sumar **100%** (se evidencia al eliminar la misma competencia de todos los cargos que la tienen).
- Al añadir una competencia desde la vista de **cargos**, la competencia se carga **sin reactivos** (debería cargar reactivos por defecto según el máximo configurado en el proceso). Muchas veces finaliza con error.
- Al añadir una competencia desde la vista de **cargos**, la competencia se carga con un peso fijo de **1%**, provocando que el peso global de la competencia en el proceso sea **1%** (visible en la vista de **competencias globales**, donde se indican los pesos).
- Al iniciar un proceso de desempeño, el mensaje “El usuario (\$user\_id) no encontrado” está relacionado a que la persona no tiene registro en la tabla `ctv_user_profiles`. Esto ocurre específicamente cuando la persona es añadida como **evaluadora** al momento de asignarse una evaluación que debe responder. La validación/uso se realiza en `controllers/PerformanceProcessController.php` -> `PerformanceProcessController->usuarioPuedeSerEvaluador`.
- Cuando una persona es eliminada de la plataforma y el problema persiste (“El usuario (\$user\_id) no encontrado”), si su registro aún existe en:
  - `ctv_gxd_performance_process_position_clients.client_user_id`, o
  - `ctv_gxd_performance_process_position_suppliers.supplier_user_id`, el error seguirá ocurriendo porque el proceso ya tiene cargada la relación de cargos previa. En ese caso, se debe eliminar manualmente el registro de la persona que genera el problema desde la tabla correspondiente.
- **Recrear tests en lista de evaluaciones**: esta opción puede volver a asignar tests a un usuario, incluyendo evaluaciones que hayan sido eliminadas o estén en estado **soft-deleted**. Esto puede generar duplicados. Ejemplo:
  - si una evaluación 90° fue reasignada a otro evaluador, la evaluación original del jefe actual queda en **soft-delete**;
  - si luego se ejecuta la opción de recrear evaluaciones para ese evaluado, el sistema puede asignar nuevamente la evaluación del jefe actual, dejando al evaluado con **dos** evaluaciones 90° (la reasignada y la re-creada).