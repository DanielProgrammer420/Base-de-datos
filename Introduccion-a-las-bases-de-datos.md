¡Excelente iniciativa! Preparar un buen material de estudio es fundamental para el éxito en la materia. Como profesor de esta cátedra, me complace ayudarte a estructurar y comprender estos conceptos fundamentales.

A continuación, he preparado un resumen completo y didáctico del material que has compartido, diseñado para que te sirva como una guía de estudio profesional.

***

### **Título del Documento**

Introducción a las Bases de Datos (BASES DE DATOS I - FaCENA | UNNE)

### **Resumen General**

Este documento presenta los conceptos fundacionales de las bases de datos, comenzando por definir qué es una base de datos y un Sistema Gestor de Bases de Datos (SGBD), y destacando las ventajas cruciales que ofrecen sobre los sistemas de archivos tradicionales. Se exploran los componentes esenciales de un sistema de base de datos (Hardware, Software, Datos y Usuarios) y se detalla el **proceso de diseño** en seis etapas, desde el análisis de requisitos hasta la implementación física y de seguridad.

El núcleo del material se centra en dos modelos clave: el **Modelo Entidad-Relación (E-R)** para el diseño conceptual, que permite representar la realidad a través de entidades y relaciones; y el **Modelo Relacional**, que es la base de los SGBD modernos y organiza los datos en tablas. Se explica de manera práctica cómo "mapear" o traducir un diagrama E-R a un esquema relacional. Finalmente, se introduce el concepto de **normalización** como una técnica indispensable para refinar el diseño, eliminar la redundancia y garantizar la integridad de los datos, explicando las tres primeras formas normales (1FN, 2FN, 3FN) con ejemplos claros.

### **Resumen por Secciones o Temas**

#### **1. Fundamentos: ¿Qué es una Base de Datos y un SGBD?**

*   **Base de Datos (BD):** Es una colección de datos **estructurados**, **persistentes** y **relacionados** que describen las actividades de una organización. Sus propiedades clave son:
    *   **Estructurados e independientes:** La organización de los datos no depende de las aplicaciones que los usan ni del medio físico donde se guardan.
    *   **Mínima redundancia:** Se evita la duplicación innecesaria de información.
    *   **Compartidos:** Múltiples usuarios y aplicaciones pueden acceder a ellos de forma concurrente.
*   **Sistema Gestor de Base de Datos (SGBD):** Es el **software** que actúa como intermediario entre los usuarios/aplicaciones y la base de datos física. Su función es gestionar los datos de manera **robusta, eficiente y segura**. Ejemplos populares son MySQL, Oracle, SQL Server, PostgreSQL, SQLite.
*   **Ventajas de usar un SGBD sobre Sistemas de Archivos:** Los SGBD resuelven las dificultades inherentes a los sistemas de archivos, tales como:
    *   **Independencia de los datos:** Cambios en la estructura de los datos no requieren modificar las aplicaciones.
    *   **Acceso concurrente y recuperación de fallos:** Permite que varios usuarios operen al mismo tiempo sin corromper los datos y puede restaurar la base a un estado consistente tras un fallo.
    *   **Integridad y Seguridad:** Aplica reglas para que los datos sean correctos y controla quién puede acceder o modificar la información.
    *   **Reducción del tiempo de desarrollo:** Ofrece herramientas de alto nivel para gestionar los datos, simplificando la creación de aplicaciones.

#### **2. El Proceso de Diseño de una Base de Datos**

El diseño de una BD es un proceso metodológico que se divide en varias etapas:

1.  **Análisis de Requisitos:** Entender qué datos se deben almacenar y qué operaciones se realizarán sobre ellos.
2.  **Diseño Conceptual:** Crear un modelo de alto nivel que represente la realidad. La herramienta por excelencia aquí es el **Modelo Entidad-Relación (E-R)**.
3.  **Diseño Lógico:** Traducir el modelo conceptual a un modelo que el SGBD entienda. Para los SGBD relacionales, esto implica crear un **esquema relacional** (un conjunto de tablas).
4.  **Refinamiento del Esquema:** Analizar las tablas diseñadas para identificar y corregir problemas de redundancia e inconsistencia. La técnica clave es la **normalización**.
5.  **Diseño Físico:** Tomar decisiones sobre el almacenamiento (ej. creación de índices) para optimizar el rendimiento según la carga de trabajo esperada.
6.  **Diseño de Aplicaciones y Seguridad:** Definir cómo interactuarán las aplicaciones con la BD y establecer roles y permisos de usuario.

#### **3. Modelo Conceptual: El Modelo Entidad-Relación (E-R)**

Este modelo permite visualizar la estructura de los datos antes de crear la base de datos. Sus elementos básicos son:

*   **Entidad:** Un objeto del mundo real que se puede distinguir de otros (ej. `Alumno`, `Profesor`). Se representa con un rectángulo.
*   **Atributo:** Una propiedad o característica de una entidad (ej. `nombre`, `DNI` para un `Alumno`). Se representa con un óvalo.
*   **Clave:** Un atributo (o conjunto de ellos) que **identifica de manera única** a una entidad. Se suele subrayar.
*   **Relación:** Una asociación entre dos o más entidades (ej. un `Profesor` **imparte** `Asignaturas`). Se representa con un rombo.
*   **Cardinalidad:** Indica el número de instancias de una entidad que pueden relacionarse con instancias de otra. Define la naturaleza de la relación:
    *   **Uno a uno (1:1):** Un Director dirige un único Instituto.
    *   **Uno a muchos (1:M):** Un Departamento tiene muchos Empleados, pero cada Empleado pertenece a un solo Departamento.
    *   **Muchos a muchos (N:M):** Un Alumno cursa varias Materias, y una Materia es cursada por varios Alumnos.

#### **4. El Modelo Relacional y sus Restricciones**

Es el modelo lógico en el que se basan la mayoría de los SGBD actuales. Su estructura es simple e intuitiva:

*   **Tabla (o Relación):** Representa una entidad. Es una estructura de dos dimensiones con filas y columnas.
*   **Columna (o Atributo):** Representa una propiedad de la entidad.
*   **Fila (o Tupla / Registro):** Representa una instancia específica de la entidad.
*   **Restricciones de Integridad:** Son reglas que garantizan la calidad y consistencia de los datos. Las más importantes son:
    *   **Restricción de Clave Principal (Primary Key - PK):** Una columna (o conjunto) que identifica unívocamente cada fila de la tabla. No puede contener valores nulos.
    *   **Restricción de Clave Externa (Foreign Key - FK):** Una columna en una tabla que hace referencia a la clave principal de otra tabla. Es el mecanismo que permite **relacionar las tablas** entre sí.

#### **5. Del Diseño Conceptual al Lógico (Mapeo E-R a Relacional)**

Este es un paso crucial y práctico en el diseño. Las reglas de transformación son:

1.  **Identificar Claves Primarias (PK):** Cada entidad se convierte en una tabla. El atributo clave del modelo E-R se convierte en la PK de la tabla.
2.  **Establecer Claves Foráneas (FK):** Las relaciones 1:1 y 1:M se implementan añadiendo una FK en una de las tablas.
3.  **Resolver Relaciones N:M:** Una relación muchos-a-muchos **no se puede representar directamente**. Se resuelve creando una **nueva tabla intermedia** (llamada tabla de unión o asociativa). Esta tabla contendrá como claves foráneas las claves primarias de las dos tablas que relaciona. La clave primaria de esta nueva tabla es, comúnmente, la combinación de ambas claves foráneas.

#### **6. Procesos de Normalización**

La normalización es un proceso formal para analizar y mejorar el diseño de las tablas con el fin de **minimizar la redundancia** y evitar anomalías de inserción, actualización y borrado.

*   **Primera Forma Normal (1FN):**
    *   **Regla:** Todos los atributos deben ser **atómicos**, es decir, no deben contener múltiples valores en una sola celda.
    *   **Ejemplo:** Una columna "Teléfonos" que almacena "660111222, 660222333" viola la 1FN.
    *   **Solución:** Crear una tabla separada `Teléfonos` con el ID del alumno y un solo número de teléfono por fila.

*   **Segunda Forma Normal (2FN):**
    *   **Regla:** La tabla debe estar en 1FN y, además, todos los atributos que no son clave deben depender funcionalmente de la **clave principal completa**. (Esto es relevante para tablas con claves primarias compuestas).
    *   **Concepto:** Se busca eliminar **dependencias parciales**.
    *   **Ejemplo:** Si una tabla de matrículas tuviera la clave `(ID_Alumno, ID_Curso)` y un atributo `Nombre_Tutor`, y el tutor dependiera solo del curso y no del alumno, habría una dependencia parcial.
    *   **Solución:** Crear una tabla `Cursos` con `ID_Curso` y `Nombre_Tutor`.

*   **Tercera Forma Normal (3FN):**
    *   **Regla:** La tabla debe estar en 2FN y no deben existir **dependencias transitivas**.
    *   **Concepto:** Una dependencia transitiva ocurre cuando un atributo no clave depende de otro atributo no clave, en lugar de depender directamente de la PK. `PK -> Atributo1 -> Atributo2`.
    *   **Ejemplo:** En una tabla de alumnos con `ID` (PK), `Localidad` y `Provincia`, la provincia depende de la localidad, y la localidad depende del ID. Esto es una dependencia transitiva. Si cambia la provincia de una localidad, habría que actualizar múltiples registros de alumnos.
    *   **Solución:** Crear una tabla `Localidades` con `Localidad` y `Provincia`, y en la tabla de alumnos dejar solo la referencia a `Localidad`.

***

### **Conceptos Clave**

*   Una base de datos organiza la información de forma estructurada para minimizar la redundancia y ser compartida.
*   Un SGBD es el software que gestiona la base de datos, proveyendo seguridad, eficiencia y acceso concurrente.
*   El diseño de bases de datos es un proceso formal que va de lo abstracto (conceptual) a lo concreto (físico).
*   El Modelo E-R es una herramienta visual para el diseño conceptual (entidades, relaciones, atributos).
*   El Modelo Relacional es el pilar de las bases de datos modernas, organizando los datos en tablas relacionadas mediante claves.
*   Las relaciones N:M siempre se resuelven creando una tabla intermedia.
*   La normalización (1FN, 2FN, 3FN) es un proceso esencial para crear un diseño de tablas robusto, eficiente y libre de anomalías.

### **Términos Técnicos Definidos**

*   **SGBD (Sistema Gestor de Base de Datos):** Software para crear, mantener y controlar el acceso a una base de datos.
*   **Atomicidad:** Propiedad de un atributo que indica que su valor es indivisible (no se puede descomponer en partes más pequeñas con significado propio).
*   **Redundancia:** Duplicación innecesaria de datos.
*   **Entidad:** Objeto o concepto del mundo real sobre el cual se almacena información.
*   **Clave Primaria (PK):** Columna o conjunto de columnas que identifica de forma única cada fila de una tabla.
*   **Clave Foránea (FK):** Columna o conjunto de columnas en una tabla que establece un vínculo con la clave primaria de otra tabla, haciendo cumplir la integridad referencial.
*   **Cardinalidad:** Define el tipo de relación numérica entre dos entidades (1:1, 1:M, N:M).
*   **Dependencia Funcional:** Un atributo B depende funcionalmente de un atributo A si a cada valor de A le corresponde un único valor de B. Es la base de la normalización.
*   **Dependencia Transitiva:** Condición en la que un atributo no clave depende de otro atributo no clave. Se elimina para alcanzar la 3FN.

### **Preguntas para Autoevaluación o Repaso**

1.  ¿Cuáles son las tres ventajas principales de utilizar un SGBD en lugar de un sistema de archivos tradicional para gestionar los datos de una empresa?
2.  Describe brevemente las etapas del diseño de una base de datos. ¿En qué etapa se utiliza el Modelo E-R y en cuál la normalización?
3.  ¿Qué es una relación de "muchos a muchos" (N:M) y por qué requiere un tratamiento especial al pasar al modelo relacional? ¿Cómo se soluciona?
4.  Explica con tus propias palabras qué significa que un atributo sea "atómico". ¿Qué Forma Normal se encarga de asegurar esta propiedad?
5.  Considera una tabla `Factura` con los siguientes campos: `NroFactura` (PK), `Fecha`, `ID_Cliente`, `NombreCliente`, `DireccionCliente`. ¿Esta tabla cumple con la Tercera Forma Normal (3FN)? Justifica tu respuesta. Si no la cumple, ¿cómo la rediseñarías?
