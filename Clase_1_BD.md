Claro, aquí tienes un resumen completo y estructurado de la "Clase #1: Base de datos", elaborado a partir de la transcripción proporcionada.

### **Resumen de la Clase #1: Introducción a las Bases de Datos y Diseño Conceptual**

---

#### **Tema Principal del Video**

El video es la primera clase del curso de Base de Datos. Su objetivo es introducir los conceptos fundamentales de las bases de datos, los sistemas gestores de bases de datos (SGBD) y, principalmente, iniciar el estudio del proceso de diseño de una base de datos, enfocándose en la **etapa de diseño conceptual** a través del **Modelo Entidad-Relación (MER)**.

---

#### **1. Cuestiones Administrativas y de Organización**

Antes de iniciar el contenido técnico, se abordan varios puntos organizativos:

*   **Horarios de Clases Teóricas:** Se discute la posibilidad de unificar las clases teóricas de lunes y miércoles en una sola sesión virtual los miércoles, para alinearla con otras materias virtuales y concentrar la actividad online en un solo día. La decisión final se comunicará próximamente.
*   **Grabación de Clases:** Se confirma que todos los encuentros virtuales serán grabados y estarán disponibles. No hay inconveniente en que los alumnos graben las clases por su cuenta.
*   **Herramientas de Comunicación:** Se menciona el uso de **Slack** como canal de comunicación. Se proporciona un enlace para guiar a los alumnos en el proceso de registro.
*   **Asistencia:** La asistencia a las clases virtuales se tomará descargando la lista de participantes de Zoom. Es **requisito indispensable** que los alumnos configuren su nombre de usuario en el formato **"Apellido, Nombre"** completo para ser registrados correctamente.
*   **Formación de Grupos:** Se anuncia que la formación de grupos para los proyectos comenzará la próxima semana. Los grupos serán de **cuatro integrantes** y se habilitará una sección en el aula virtual para que los alumnos se inscriban en uno de los 35-36 grupos disponibles.

---

#### **2. Contenido Principal de la Clase: Fundamentos y Diseño**

##### **2.1. Introducción a las Bases de Datos**

*   **Definición de Base de Datos (BD):** Se define como un conjunto de datos almacenados en un repositorio, que pueden o no tener una estructura.
*   **Base de Datos Relacional:** Es el foco de la asignatura. Se caracteriza por ser un conjunto de datos organizados en estructuras (similares a tablas) que, a su vez, se encuentran **relacionadas entre sí**.
*   **Objetivos Clave de una BD:**
    *   **Persistencia:** Los datos perduran en el tiempo.
    *   **Independencia:** Los datos son independientes de las aplicaciones que los usan y del hardware donde se almacenan.
    *   **Mínima Redundancia:** Evitar la repetición innecesaria de datos para mantener la consistencia y coherencia.
    *   **Uso Compartido:** Permite que múltiples usuarios y aplicaciones accedan a los datos simultáneamente.

##### **2.2. Sistema Gestor de Base de Datos (SGBD)**

*   **Definición:** Es el **software** encargado de administrar y servir las bases de datos. Actúa como intermediario entre los datos y las aplicaciones. También se le conoce como **manejador de base de datos**.
*   **Objetivo del SGBD:** Gestionar los datos de modo **robusto y eficiente**. La elección de un SGBD depende del contexto del problema (aplicaciones web, de escritorio, etc.).
*   **Ejemplos Mencionados:** MySQL, SQL Server, Oracle, MariaDB, y SQLite (mencionado como un repositorio más liviano, ideal para aplicaciones móviles).
*   **Ventajas sobre Sistemas de Archivos Tradicionales:** Los SGBD ofrecen gestión de acceso múltiple, seguridad avanzada, independencia de la plataforma y mayor eficiencia en el desarrollo.

##### **2.3. El Proceso de Diseño de una Base de Datos**

El diseño de una base de datos no es un proceso estrictamente lineal, sino **iterativo y de refinamiento**. Los pasos pueden variar dependiendo de si se parte de un sistema existente o de un requerimiento nuevo. Las etapas son:

1.  **Análisis de Requisitos:** Entender a fondo el problema y las necesidades de información. Es el paso más crucial.
2.  **Diseño Conceptual:** Se crea un modelo de alto nivel, abstracto e independiente de la tecnología. Aquí se utiliza el **Modelo Entidad-Relación (MER)** para representar los datos y sus relaciones.
3.  **Diseño Lógico:** El modelo conceptual se traduce a un modelo específico, como el **modelo relacional**. Aquí se empiezan a considerar las restricciones de un SGBD relacional (tablas, normalización).
4.  **Refinamiento del Esquema:** Se revisa y discute el diseño lógico con el equipo para asegurar que representa correctamente el problema.
5.  **Diseño Físico:** Se implementa el diseño en un motor de base de datos específico. Se definen tipos de datos, índices para optimizar búsquedas y otros ajustes de rendimiento.

---

#### **3. El Diseño Conceptual y el Modelo Entidad-Relación (MER)**

Esta es la parte central de la clase, donde se realiza un ejercicio práctico.

*   **Propósito del MER:** Representar el comportamiento de los datos, cómo se agrupan (entidades) y cómo se asocian (relaciones) a un alto nivel, de forma que sea entendible incluso para alguien sin conocimientos técnicos (como el cliente o jefe).

##### **3.1. Componentes del Diagrama Entidad-Relación**

El profesor utiliza la herramienta online **ERDPlus** para ilustrar estos conceptos con un ejemplo práctico de **Empleados** y **Departamentos**.

*   **Entidad:** Representa un objeto del mundo real (tangible como "Empleado") o un concepto (abstracto como "Inscripción"). **Un punto clave es que una entidad cobra sentido por los atributos que contiene, no solo por su nombre.**
    *   *Ejemplo:* `Empleado`, `Departamento`.

*   **Atributos:** Son las características o propiedades que describen a una entidad.
    *   *Ejemplo:* Para la entidad `Empleado`, los atributos son `DNI`, `nombre`, `apellido`, `email`.

*   **Relación:** Es la asociación que existe entre dos o más entidades.
    *   *Ejemplo:* Un `Empleado` **"trabaja en"** un `Departamento`.

##### **3.2. Tipos de Atributos y Conceptos Avanzados**

Se explican diferentes comportamientos y tipos de atributos, representados con notaciones específicas en el diagrama:

*   **Atributo Identificador (Clave Primaria):** Atributo cuyo valor es único para cada instancia de la entidad y permite identificarla sin ambigüedad.
    *   *Ejemplo:* `DNI` o `Número de Legajo` para un `Empleado`.

*   **Atributo en una Relación:** A veces, un atributo no describe a una entidad por sí sola, sino a la **relación** entre ellas.
    *   *Ejemplo:* La `Fecha de Inicio` de un empleado en un departamento. No es un atributo del empleado (porque puede cambiar de departamento y tener varias fechas) ni del departamento, sino de la relación "trabaja en".

*   **Cardinalidad de la Relación:** Indica la frecuencia con la que se relacionan las instancias de las entidades (cuántos elementos de una entidad se pueden relacionar con cuántos de otra).
    *   *Ejemplo 1 (Uno a Muchos):* Un `Empleado` trabaja en **un** solo `Departamento`, pero un `Departamento` puede tener **muchos** `Empleados`.
    *   *Ejemplo 2 (Muchos a Muchos):* Un `Departamento` puede estar en **varios** `Edificios`, y un `Edificio` puede albergar **varios** `Departamentos`.
    *   **Aclaración importante:** En el modelo conceptual, las relaciones de muchos a muchos se representan directamente. Su transformación (por ejemplo, en una tabla intermedia) ocurre en el diseño lógico para bases de datos relacionales.

*   **Atributo Compuesto:** Un atributo que puede descomponerse en partes más pequeñas y atómicas.
    *   *Ejemplo:* `Nombre y Apellido` se descompone en `nombre` y `apellido` para tratarlos por separado.

*   **Atributo Derivado:** Un atributo que no se almacena directamente, sino que se calcula a partir de otro atributo ya existente. Se representa con una línea punteada.
    *   *Ejemplo:* La `Edad` de un empleado se deriva (calcula) a partir de su `Fecha de Nacimiento` y la fecha actual. La `Antigüedad` se calcula a partir de una fecha de ingreso.

*   **Atributo Multivaluado:** Un atributo que puede tener múltiples valores para una misma instancia de la entidad. Se representa con un óvalo de doble línea.
    *   *Ejemplo:* Un `Empleado` puede tener varios correos electrónicos (`email`).

*   **Atributo Opcional:** Un atributo que puede no tener un valor (ser nulo).
    *   *Ejemplo:* `Teléfono` puede ser opcional si no todos los empleados tienen uno.

---

#### **4. Tareas y Conclusiones**

*   **Conclusión Clave:** La etapa de diseño conceptual es fundamental para **entender y modelar la naturaleza y el comportamiento de los datos sin pensar aún en la implementación técnica**. El diseñador debe "quitarse el rol de programador" y enfocarse en los requerimientos del problema.
*   **Tarea Asignada:**
    1.  Leer el material teórico sobre el Modelo Entidad-Relación en el aula virtual.
    2.  Analizar el **"Caso de Estudio: Diagrama ER"** disponible en la sección "Práctica y Laboratorio".
    3.  Intentar realizar un primer bosquejo del modelo conceptual usando la herramienta **ERDPlus**, identificando entidades, atributos y relaciones.
    4.  Este ejercicio se trabajará y consultará en la clase del lunes.
