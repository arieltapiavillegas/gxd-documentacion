
## A - Contexto general

* **Servidor LMS**: Yii1

* **Servidor GXX**: Yii2

---

### Resumen

* Objetivo: Permitir la **medición de indicadores** para la **gestión de desempeño** de los colaboradores.

* Servidor/Framework: **GXX (Yii2)**

* Ubicación (alto nivel): (pendiente)

### Alcance funcional (visión general)

La medición se estructura en **tres áreas**:

1. **Competencias**: competencias asociadas al **cargo** que ejerce el colaborador.

2. **Funciones específicas del cargo**: evaluación según **funciones** propias del cargo.

3. **Metas**: medición según **metas** definidas.

### Conceptos y relaciones

#### Competencias

* Existe un **repositorio común** de competencias.

* Las competencias pueden ser **asignadas a distintos cargos** (compartibles entre cargos).

* Cada competencia se compone de **reactivos** (preguntas específicas) con los que se realiza su medición.

* En términos funcionales, las competencias representan **habilidades** requeridas para ejercer un cargo.

#### Funciones del cargo

* Las **funciones de cargo** son características/responsabilidades propias de un cargo.

* No son compartidas entre cargos (no existe repositorio común compartible como en competencias).

* Se miden a través de **tareas**, que son los ítems con los que se evalúa cada función.

* En términos funcionales, las funciones representan **responsabilidades/funciones** que se deben ejercer dentro del cargo.

#### Metas

* Se refieren a **objetivos medibles en el tiempo** que una persona debe cumplir.

#### Tareas

* Son los ítems mediante los cuales se miden funciones del cargo.

* Se crean **específicamente para cada persona**; pueden ser **todas distintas** entre colaboradores.

