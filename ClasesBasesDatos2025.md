
### **Resumen de la Clase Práctica BDI (07/08/2025) (Primera Clase Virtual)**

---

#### **Tema Principal del Video**

El video corresponde al primer encuentro práctico del curso de Base de Datos. Su objetivo es introducir a los estudiantes en los conceptos fundamentales de las bases de datos y, de manera central, iniciar el proceso de **diseño conceptual**. La clase se enfoca en enseñar cómo modelar un problema utilizando el **Diagrama Entidad-Relación (DER)**, explicando sus componentes y reglas a través de un ejemplo práctico construido en vivo.

---

#### **1. Cuestiones Administrativas y de Organización**

Al inicio de la clase, se abordan varios puntos organizativos para el curso:

*   **Horarios de Clases Teóricas:** Se está evaluando la posibilidad de unificar las clases teóricas de lunes y miércoles en una única sesión virtual los miércoles, para mayor comodidad de los estudiantes y para concentrar las actividades online. La decisión final se comunicará próximamente.
*   **Grabación de Clases:** Se confirma que todos los encuentros virtuales serán grabados y puestos a disposición de los alumnos. No existe ningún inconveniente en que los estudiantes graben las clases por su cuenta.
*   **Herramientas de Comunicación:** Se menciona el uso de **Slack** como canal de comunicación oficial. Se compartirá una guía para facilitar el registro de los alumnos.
*   **Asistencia:** La asistencia a las clases virtuales se registrará descargando la lista de participantes de Zoom. Es **requisito indispensable** que los alumnos configuren su nombre de usuario en el formato **"Apellido, Nombre"** completo para ser registrados correctamente.
*   **Formación de Grupos:** Se anuncia que la formación de grupos para los proyectos comenzará la próxima semana. Los grupos serán de **cuatro integrantes**, y se habilitará una sección en el aula virtual para que los alumnos se inscriban en uno de los 35 grupos disponibles.

---

#### **2. Contenido Principal: Fundamentos y Diseño Conceptual**

##### **2.1. Introducción a las Bases de Datos**

*   **Definición de Base de Datos (BD):** Se define de manera general como un conjunto de datos almacenados en un repositorio, que pueden o no tener una estructura.
*   **Base de Datos Relacional:** Es el foco de la asignatura. Se caracteriza por ser un conjunto de datos organizados en estructuras (que luego serán tablas) que se encuentran **relacionadas entre sí**.
*   **Objetivos Clave de una BD:**
    *   **Persistencia:** Los datos perduran en el tiempo.
    *   **Independencia:** Los datos son independientes de las aplicaciones que los consumen y del hardware donde se almacenan.
    *   **Mínima Redundancia:** El objetivo es evitar la repetición innecesaria de datos para mantener la consistencia y coherencia.
    *   **Uso Compartido:** Permite que múltiples usuarios y aplicaciones accedan a los datos de forma concurrente.

##### **2.2. Sistema Gestor de Base de Datos (SGBD)**

*   **Definición:** Es el **software** encargado de administrar y servir las bases de datos. Actúa como intermediario entre los datos y los usuarios/aplicaciones. También se le conoce como **manejador de base de datos**.
*   **Objetivo del SGBD:** Gestionar los datos de modo **robusto y eficiente**. La elección de un SGBD depende del contexto del problema (aplicaciones web, de escritorio, móviles, etc.).
*   **Ejemplos Mencionados:** MySQL, SQL Server, Oracle, MariaDB, y SQLite (mencionado como un repositorio más liviano, ideal para aplicaciones móviles).

##### **2.3. El Proceso de Diseño de una Base de Datos**

El diseño de una base de datos es un proceso **iterativo y de refinamiento**, no estrictamente lineal. Las etapas principales son:

1.  **Análisis de Requisitos:** Entender a fondo el problema y las necesidades de información. Es el paso más crucial.
2.  **Diseño Conceptual:** Se crea un modelo de alto nivel, abstracto e independiente de la tecnología. Aquí se utiliza el **Modelo Entidad-Relación (MER)**.
3.  **Diseño Lógico:** El modelo conceptual se traduce a un modelo específico, como el **modelo relacional** (tablas, columnas, etc.).
4.  **Refinamiento del Esquema:** Se revisa y ajusta el diseño lógico.
5.  **Diseño Físico:** Se implementa el diseño en un motor de base de datos específico (ej. MySQL), definiendo tipos de datos, índices para optimizar búsquedas y otros ajustes de rendimiento.

---

#### **3. El Diseño Conceptual y el Modelo Entidad-Relación (MER) en la Práctica**

Esta es la parte central de la clase, donde se realiza un ejercicio práctico utilizando la herramienta online **ERDPlus**.

*   **Propósito del MER:** Representar el comportamiento de los datos, cómo se agrupan (entidades) y cómo se asocian (relaciones) a un alto nivel. Es una herramienta de comunicación que debe ser entendible incluso por alguien sin conocimientos técnicos (como el cliente o el jefe).

##### **3.1. Componentes del Diagrama Entidad-Relación**

El profesor construye un ejemplo práctico sobre la marcha para ilustrar los componentes.

*   **Entidad:** Representa un objeto del mundo real (tangible como "Empleado") o un concepto (abstracto como "Inscripción").
    *   **Punto Clave:** Una entidad cobra sentido y se define por los **atributos que contiene**, no solo por su nombre. Un conjunto de atributos como DNI, nombre y email define a una `Persona` o `Empleado`, no a un `Auto`.
    *   *Ejemplo en clase:* `Empleado`, `Departamento`.

*   **Atributos:** Son las características o propiedades que describen a una entidad.
    *   *Ejemplo:* Para la entidad `Empleado`, los atributos son `DNI`, `número de legajo`, `nombre`, `email`.

*   **Relación:** Es la asociación que existe entre dos o más entidades.
    *   *Ejemplo:* Un `Empleado` **"trabaja en"** un `Departamento`.

##### **3.2. Tipos de Atributos y Conceptos Avanzados del MER**

Se explican diferentes comportamientos de los atributos, cada uno con una notación gráfica específica en el diagrama:

*   **Atributo Identificador (Clave Primaria):** Atributo cuyo valor es **único** para cada instancia de la entidad y permite identificarla sin ambigüedad.
    *   *Ejemplo:* `DNI` o `Número de Legajo` para un `Empleado`.

*   **Atributo en una Relación:** A veces, un atributo no describe a una entidad por sí sola, sino a la **relación** entre ellas.
    *   *Ejemplo clave discutido:* La `Fecha de Inicio` de un empleado en un departamento. Si un empleado puede cambiar de departamento a lo largo de su historia laboral, esta fecha no es un atributo del empleado (porque tendría varias), sino de cada instancia de la relación "trabaja en".

*   **Cardinalidad de la Relación:** Indica la frecuencia con la que se relacionan las instancias de las entidades (cuántos elementos de una entidad se pueden relacionar con cuántos de otra).
    *   *Ejemplo (Uno a Muchos):* Un `Empleado` trabaja en **un** solo `Departamento` a la vez, pero un `Departamento` puede tener **muchos** `Empleados`.

*   **Participación (Opcionalidad):** Indica si la existencia de una entidad requiere que participe en una relación.
    *   *Ejemplo:* Un `Empleado` **siempre** debe tener un departamento asignado (participación obligatoria), pero un `Departamento` puede existir **sin** tener empleados (participación opcional).

*   **Atributo Compuesto:** Un atributo que puede descomponerse en partes más pequeñas y atómicas.
    *   *Ejemplo:* `Nombre y Apellido` se puede descomponer en los atributos `nombre` y `apellido` para tratarlos por separado, aunque conceptualmente se presenten juntos.

*   **Atributo Derivado:** Un atributo que no se almacena directamente, sino que se **calcula** a partir de otro atributo ya existente. Se representa con una línea punteada.
    *   *Ejemplo:* La `Edad` de un empleado se deriva (calcula) a partir de su `Fecha de Nacimiento` y la fecha actual. Se modela para reflejar un requerimiento del negocio, aunque no se guarde como un dato estático.

*   **Atributo Multivaluado:** Un atributo que puede tener **múltiples valores** para una misma instancia de la entidad. Se representa con un óvalo de doble línea.
    *   *Ejemplo:* Un `Empleado` puede tener varios correos electrónicos (`email`).

*   **Atributo Opcional:** Un atributo que puede no tener un valor (ser nulo).
    *   *Ejemplo:* `Teléfono` puede ser opcional si no todos los empleados están obligados a proporcionar uno.

---

#### **4. Tareas y Conclusiones**

*   **Conclusión Clave:** La etapa de diseño conceptual es fundamental para **entender y modelar la naturaleza y el comportamiento de los datos sin pensar aún en la implementación técnica**. El diseñador debe "quitarse el rol de programador" y enfocarse en los requerimientos del problema y en cómo los datos representan ese problema.
*   **Tarea Asignada:**
    1.  Leer el material teórico sobre el Modelo Entidad-Relación disponible en el aula virtual.
    2.  Analizar el **"Caso de Estudio: Diagrama ER"** que ya está habilitado.
    3.  Realizar un primer bosquejo del modelo conceptual usando la herramienta **ERDPlus**, identificando entidades, atributos y relaciones según lo aprendido en clase.
    4.  Este ejercicio se trabajará y se resolverán dudas sobre él en la clase del lunes.


---


### **Resumen Completo de la Clase de Base de Datos (13/08/2025) Segunda Clase Virtual**

---

#### **Tema Principal del Video**

El tema central de la clase es una introducción teórica al **proceso de diseño de bases de datos**, enfocándose en las distintas etapas que lo componen, desde el análisis inicial hasta la implementación física. La clase pone un énfasis especial en la **etapa de diseño conceptual** y el uso del **Diagrama Entidad-Relación (DER)**, profundizando en conceptos críticos como la cardinalidad y las reglas de negocio.

---

#### **I. Puntos Clave Explicados (Organización y Contexto Inicial)**

Antes de iniciar la teoría, se abordaron varios puntos administrativos y de organización del curso:

*   **Consultas sobre Cardinalidad:** Un alumno pregunta si se pueden "asumir" cardinalidades. El profesor indica que este será un tema central de la clase y que no se deben asumir, sino que deben surgir del análisis.
*   **Conformación de Grupos:** Se reitera que los grupos deben ser de cuatro integrantes. Los grupos de dos personas serán disueltos y sus miembros reasignados para completar grupos de tres.
*   **Metodología de Clases Prácticas:** En las clases prácticas (laboratorios), los equipos resolverán ejercicios y serán llamados a exponer su solución. Es fundamental que **todos los miembros del equipo participen activamente** en la presentación.
*   **Alumnos Recursantes (2024):** Aquellos que aprobaron el trabajo de campo en 2024 no necesitan volver a realizarlo, a menos que lo deseen. Se les recomienda no unirse a equipos que sí deben hacerlo para no afectar la distribución de la carga de trabajo.

---

#### **II. Etapas en el Diseño de una Base de Datos**

El profesor describe el diseño de una base de datos como un proceso metodológico dividido en varias etapas secuenciales. El diseño puede partir de dos escenarios:
1.  **Desde Cero:** A partir de un nuevo requerimiento de un cliente.
2.  **Sobre un Sistema Existente:** Mejorando o modificando una base de datos que ya está en producción (lo cual es más complejo por la existencia de datos reales).

Las etapas del diseño son:

1.  **Análisis de Requisitos:**
    *   Es la primera y fundamental etapa. Se utilizan técnicas de Ingeniería del Software para entender el problema.
    *   El objetivo es identificar los objetos del problema, los datos a representar y cómo se asocian entre sí. Se debe documentar todo, incluyendo tablas, atributos, relaciones y restricciones.

2.  **Diseño Conceptual:**
    *   Es un **modelo de alto nivel**, abstracto y semántico (se enfoca en el significado y comportamiento de los datos).
    *   Es independiente de la tecnología o el motor de base de datos que se usará.
    *   La herramienta principal es el **Diagrama Entidad-Relación (DER)**.
    *   **Importante:** No se debe confundir "Modelo Entidad-Relación" (conceptual) con "Modelo Relacional" (lógico).

3.  **Diseño Lógico:**
    *   Se realiza una **reducción del modelo conceptual a un modelo específico**, en este curso, el **Modelo Relacional**.
    *   Se consideran las restricciones propias del modelo relacional (tablas, columnas, claves primarias, claves foráneas).
    *   Se aplican **técnicas de normalización** (formas normales) para minimizar la redundancia de datos y maximizar la consistencia e integridad.

4.  **Refinamiento del Esquema:**
    *   Es un proceso iterativo para ajustar y mejorar el modelo lógico. Se revisa si el grado de normalización es el adecuado, evitando la "sobreestructura" que puede generar costos de mantenimiento elevados.

5.  **Diseño Físico:**
    *   Se decide el **motor de base de datos específico** (Oracle, SQL Server, MySQL, PostgreSQL, etc.).
    *   Se consideran las características propias del motor: cómo maneja los archivos, las transacciones, los índices, etc.

6.  **Diseño de Aplicaciones y Seguridad:**
    *   Esta etapa final se enfoca en cómo se accederá y utilizará la base de datos.
    *   Se definen aspectos de **rendimiento, seguridad, roles de usuario** y control de acceso a los datos.

---

#### **III. Conceptos Importantes del Diseño Conceptual**

*   **Entidad:** Representa un objeto del mundo real sobre el cual se desea almacenar información (ej: `Empleado`, `Sala`). Una entidad solo tiene sentido si posee un conjunto de atributos que la describen.
*   **Atributo:** Una propiedad o característica de una entidad (ej: `DNI`, `Nombre` para la entidad `Empleado`).
*   **Instancia:** Un registro o conjunto de valores específicos que representan a un objeto de una entidad (ej: el empleado "Juan Pérez" con DNI "12345678").
*   **Clave:** Un atributo (o conjunto de ellos) que identifica de forma única a cada instancia de una entidad.
*   **Relación:** Representa una asociación o vínculo entre dos o más entidades. Describe cómo se comportan los datos de diferentes entidades entre sí (ej: un `Empleado` **reserva** una `Sala`).

---

#### **IV. Punto Central: Cardinalidad y Reglas de Negocio**

Este fue el tema más detallado de la clase.

*   **¿Qué es la Cardinalidad?**
    *   Define el número de instancias de una entidad que pueden estar asociadas con una instancia de otra entidad. Especifica la **frecuencia** (uno a uno, uno a muchos, muchos a muchos) y la **existencia** (obligatoria u opcional) de una relación.

*   **¿De dónde surge?**
    *   **CRÍTICO:** La cardinalidad **NO se inventa ni se asume**. Surge directamente de las **reglas de negocio** establecidas en el análisis de requisitos. Si una regla no está clara en la documentación, se debe preguntar al cliente o analista.

*   **Importancia de una Cardinalidad Correcta:**
    *   Una cardinalidad mal definida en el diseño conceptual se convierte en un grave problema en el diseño lógico y en la implementación.
    *   Puede causar inconsistencia de datos, permitir registros no deseados o bloquear la inserción de datos válidos, generando problemas operativos en el sistema.

*   **Ejemplo de Análisis de Cardinalidad:**
    *   **Regla de Negocio:** "Cada sala tiene asignado un equipo multimedia de uso exclusivo".
        *   **Análisis:**
            *   Una `Sala` tiene **un** `Equipo`.
            *   Como es de "uso exclusivo", un `Equipo` pertenece a **una sola** `Sala`.
            *   **Cardinalidad de Frecuencia:** Uno a Uno (1:1).
    *   **Análisis de Existencia (Participación):**
        *   ¿Todas las salas **deben** tener un equipo? La regla dice "cada sala tiene", lo que sugiere que la participación es **obligatoria** para `Sala`.
        *   ¿Todo equipo **debe** estar en una sala? El requerimiento no lo especifica. Podría haber equipos en inventario sin asignar. Esto define si la participación es **opcional** para `Equipo`. Esta duda debe ser resuelta en el análisis de requisitos.

---

#### **V. Análisis de Casos Prácticos (Ejemplos del Video)**

El profesor utilizó un caso de estudio sobre la reserva de salas para ilustrar los conceptos.

1.  **Relación Muchos a Muchos (N:M) - `Sala` y `Equipo`**
    *   **Escenario modificado:** "Una sala puede tener asignados varios equipos" y "un equipo puede estar asignado a varias salas".
    *   **Modelo Conceptual:** Se representa con una relación N:M directa entre `Sala` y `Equipo`.
    *   **Alternativa (Promoción a Entidad):** Si la relación N:M tiene atributos propios (ej: `fecha_asignacion`), es una buena práctica convertir la relación en una entidad intermedia (ej: `Asignacion`).
        *   El modelo `Sala --(N:M)-- Equipo` se transforma en:
            *   `Sala --(1:N)-- Asignacion --(N:1)-- Equipo`.
        *   El profesor enfatiza que si se elige este camino, las nuevas cardinalidades (1:N) y las reglas de existencia deben ser definidas correctamente.

2.  **Modificación del Modelo y Entidades Débiles - `Reserva` y `Participantes`**
    *   **Escenario Inicial:** La relación `Reserva` entre `Empleado` y `Sala` es de muchos a muchos (N:M) y tiene atributos (fecha, hora).
    *   **Nueva Regla (Variante):** Se necesita guardar un listado de los **participantes** en cada reserva. Además, cada reserva tiene un código único.
    *   **Solución y Modificación del Diseño:**
        1.  La relación `Reserva` se **convierte en una entidad** `Reserva`, ya que ahora tiene un identificador único y se relaciona con una nueva entidad.
        2.  Se crea una nueva entidad llamada `Detalle_Participante` (o similar) para almacenar los datos de quienes asisten.
        3.  `Detalle_Participante` se modela como una **entidad débil**.

*   **Concepto de Entidad Débil:**
    *   Es una entidad que **no puede ser identificada de forma única por sus propios atributos**. Su existencia depende completamente de otra entidad (la entidad "propietaria").
    *   En el ejemplo, un participante (con su DNI) puede asistir a múltiples reservas. Por lo tanto, el DNI por sí solo no es una clave única en la tabla de `Detalle_Participante`. La entidad débil necesita la clave de la entidad `Reserva` para poder identificar unívocamente cada registro.
    *   En el DER, se representa con un doble recuadro.

---

### **Resumen Completo de la Clase de Proyecto/Laboratorio (13/08/2025) Tercera Clase Virtual**

---

#### **Tema Principal del Video**

El tema central de la clase es la **presentación detallada del Proyecto de Estudio (o trabajo integrador) de la materia**. Los profesores explican su propósito, estructura, fases de entrega, metodología de trabajo, herramientas obligatorias y, fundamentalmente, los criterios de evaluación. Además, se dedica una parte importante de la clase a resolver dudas sobre la conformación y organización de los grupos de trabajo.

---

#### **I. Introducción y Propósito del Proyecto de Estudio**

El proyecto es un **trabajo integrador** que los estudiantes desarrollarán a lo largo del cuatrimestre. Su objetivo principal es aplicar de manera práctica los conocimientos teóricos adquiridos en la materia a un caso real.

Los objetivos de aprendizaje que se evaluarán son:
*   **Colaboración y Trabajo en Equipo:** Demostrar la capacidad de trabajar de forma coordinada.
*   **Planificación y Organización:** Gestionar el desarrollo del proyecto de manera estructurada.
*   **Comunicación Clara:** Saber explicar y "vender" el trabajo realizado.
*   **Creatividad e Innovación:** Aportar soluciones propias al problema planteado.
*   **Uso de Herramientas:** Manejar adecuadamente el software requerido para el diseño y la gestión del proyecto (ERDPlus, Git, etc.).

---

#### **II. Estructura y Componentes del Proyecto**

El proyecto se divide en dos grandes componentes: un **caso de estudio práctico** y una **investigación sobre temas técnicos avanzados**.

**A. El Caso de Estudio (La base del proyecto):**
1.  **Elección del Tema:** Cada grupo debe elegir un problema del mundo real para modelar (ej: un sistema de gestión de una biblioteca, una tienda online, una clínica, etc.). Pueden utilizar el mismo caso de estudio que estén desarrollando en otras materias, como Taller de Programación.
2.  **Modelado de la Base de Datos:** Deberán aplicar los conocimientos de la materia para:
    *   Realizar el **Diseño Conceptual** (Diagrama Entidad-Relación).
    *   Pasar al **Diseño Lógico** (Modelo Relacional).
    *   Finalmente, implementar el **Diseño Físico**.
3.  **Implementación en un Motor de BD:**
    *   Crear los **scripts de SQL** para generar todas las tablas, atributos y restricciones del modelo.
    *   Generar otro script para realizar una **carga de datos inicial** que sea representativa y permita probar las funcionalidades.
    *   Se recomienda usar **SQL Server**, ya que es el motor utilizado en las clases prácticas, pero se permite el uso de otros motores si el grupo lo prefiere.

**B. La Investigación Técnica (El componente avanzado):**
Sobre el caso de estudio implementado, todos los grupos deberán investigar y aplicar los siguientes temas:

*   **Tres Temas Comunes y Obligatorios para todos:**
    1.  **Manejo de permisos a nivel de usuario y de base de datos.**
    2.  **Procedimientos y Funciones Almacenadas.**
    3.  **Optimización de consultas a través del uso de Índices.**

*   **Un Cuarto Tema Adicional:**
    *   La cátedra asignará un cuarto tema de investigación diferente para cada grupo (aunque algunos temas podrían repetirse entre grupos).
    *   Si un grupo tiene un interés particular en investigar un tema, puede proponerlo a los docentes.

**Importante:** Estos temas de investigación **no forman parte del contenido regular de la materia**, por lo que requieren un trabajo de investigación autónomo por parte de los estudiantes. Los profesores ofrecerán clases de consulta para aclarar dudas sobre estos temas.

---

#### **III. Fases de Entrega del Proyecto**

El proyecto se entregará en dos etapas:

1.  **Primera Etapa:**
    *   **Contenido:** Se enfoca en la descripción del caso de estudio y la implementación del modelo de datos. Incluye el documento del proyecto con el diseño conceptual y los scripts SQL de creación de la base de datos y carga de datos.
    *   **Objetivo:** Demostrar que el grupo ha definido y construido la base de su proyecto. Aún no se incluye la investigación técnica.

2.  **Segunda Etapa (Entrega Final):**
    *   **Contenido:** Sobre el modelo ya creado, se debe implementar la aplicación práctica de **todos los temas de investigación asignados**. Esto incluye la creación de procedimientos, funciones, índices, gestión de permisos, etc., con sus respectivos scripts.
    *   **Objetivo:** Completar el proyecto integrando la parte práctica con la investigación. Esta entrega es la que se utilizará para la evaluación final.

---

#### **IV. Herramientas y Metodología de Trabajo**

*   **Repositorio Obligatorio (Git):**
    *   Cada grupo debe crear un repositorio en **GitHub o GitLab**.
    *   Todos los integrantes deben realizar "commits" para que sus aportes individuales sean visibles.
    *   Se debe dar acceso a los profesores para el seguimiento continuo del trabajo. **No es aceptable que una sola persona suba todo al final**.
*   **Documentación:**
    *   Se debe seguir una guía de elaboración disponible en el aula virtual para confeccionar un documento formal del proyecto. El documento debe ser **claro, concreto y sintético**.
*   **Diagramas:**
    *   Los diagramas de entidad-relación deben realizarse con la herramienta **ERDPlus**.
*   **Trabajo Colaborativo (Clave):**
    *   Aunque los grupos pueden dividirse las tareas de investigación inicialmente, es **requisito fundamental que todos los integrantes del grupo conozcan y entiendan todos los temas investigados**. En la defensa final, se realizarán preguntas de cualquier tema a cualquier integrante. No será válida la excusa "ese tema lo investigó mi compañero".

---

#### **V. Criterios de Evaluación**

*   **Nota de Aprobación:** Se necesita un **6 (seis)** para aprobar el proyecto y regularizar la materia, y un **7 (siete) o más** para promocionar.
*   **Evaluación Grupal e Individual:** Aunque el trabajo es grupal, la **evaluación final es individual**. Esto significa que las notas pueden variar entre los integrantes de un mismo grupo, e incluso algunos podrían aprobar y otros no.
*   **Defensa Final:**
    *   Una vez aceptada la entrega final, el grupo deberá realizar una exposición de no más de **15 minutos**.
    *   Durante la exposición, los profesores harán **1-2 preguntas a cada integrante** sobre cualquiera de los temas del proyecto para evaluar su conocimiento individual.
*   **Proceso de Retroalimentación:** El proyecto no tiene "recuperatorio", pero sí un proceso de ajuste. Si la entrega final no cumple con los requisitos, los profesores harán una devolución con los puntos a corregir, y el grupo tendrá una nueva oportunidad para presentarlo.

---

#### **VI. Organización de los Grupos (Punto Clave de Discusión)**

Este fue un tema central en la clase debido a casos particulares de alumnos recursantes.

*   **Regla General:** Los grupos deben ser de **4 integrantes**. No se aceptan grupos de 2, y los de 3 solo en casos excepcionales y con la misma carga de trabajo.
*   **Alumnos con Proyecto Aprobado (de 2024):**
    *   **Opción 1 (Recomendada):** Formar grupos de 4 entre quienes ya tienen el proyecto aprobado. Estos grupos solo realizarán las actividades prácticas de la materia y no el proyecto.
    *   **Opción 2:** Volver a hacer el proyecto si así lo desean, permaneciendo en sus grupos actuales.
    *   **Opción 3 (Consensuada):** Pueden permanecer en un grupo "mixto" (con compañeros que sí deben hacer el proyecto). En este caso, **colaboran y aportan su experiencia**, pero solo se evalúa a los que necesitan la nota. La cátedra advierte que la carga de trabajo sigue siendo la misma para todos.

---

#### **VII. Conclusiones y Recomendaciones Clave**

*   **Acción Inmediata:** La principal recomendación para los estudiantes es que **comiencen a definir el tema de su proyecto lo antes posible**.
*   **Trabajo Paralelo:** A medida que avanzan con los ejercicios prácticos de la materia, deben ir aplicando esos conocimientos para modelar su propio proyecto.
*   **Organización:** Es urgente que los grupos de 2 y 3 integrantes se reorganicen para cumplir con el requisito de 4 miembros. Se publicará un listado para facilitar este proceso.
#### **VI. Conclusiones y Próximos Pasos**

*   El diseño conceptual es una etapa crucial que requiere un entendimiento profundo del problema y sus reglas de negocio.
*   No hay que apresurarse a pensar en "tablas intermedias" durante el diseño conceptual; el foco debe estar en representar fielmente el comportamiento de las entidades y sus relaciones.
*   La próxima semana, la clase se centrará en el **Diseño Lógico**, específicamente en cómo mapear o reducir un Diagrama Entidad-Relación a un **Modelo Relacional** (tablas, filas, columnas, claves primarias y foráneas).
---

### **Resumen Completo de la Clase Práctica de Base de Datos (14/08/2025) Cuarta Clase Virtual**

---

#### **Tema Principal del Video**

El tema central de la clase es una **sesión práctica de modelado de bases de datos**, donde diferentes grupos de alumnos presentan y defienden sus soluciones a una serie de ejercicios. El objetivo es aplicar los conceptos teóricos del diseño conceptual, específicamente la creación de **Diagramas Entidad-Relación (DER)**, y fomentar una discusión colaborativa para analizar, corregir y mejorar los modelos propuestos.

---

#### **I. Metodología de la Clase y Objetivos Adicionales**

Antes de comenzar con los ejercicios, el profesor destaca la importancia de estas sesiones virtuales para desarrollar habilidades blandas ("soft skills") cruciales en el ámbito profesional:

*   **Práctica de Exposición:** Aprender a presentar ideas, proyectos y avances de manera clara y concisa en un tiempo limitado.
*   **Trabajo en Equipo:** Coordinar la presentación entre varios miembros.
*   **Desenvolvimiento Profesional:** Mejorar la forma de comunicarse en reuniones virtuales, una dinámica muy común en el entorno laboral actual.

La dinámica de la clase consiste en llamar a grupos al azar para que expongan su solución a un ejercicio en un tiempo máximo de **15 minutos**. Durante y después de cada presentación, tanto los profesores como los demás alumnos pueden hacer preguntas, ofrecer observaciones y proponer soluciones alternativas, generando un debate enriquecedor.

---

#### **II. Revisión y Discusión de Ejercicios de Modelado**

##### **Ejercicio 1: Empresa de Venta de Productos**

*   **Planteamiento:** Modelar la base de datos para una empresa que vende productos a clientes, los cuales son suministrados por proveedores.
*   **Solución (Grupo 25):**
    *   **Entidades:** `Clientes`, `Productos`, `Proveedores`.
    *   **Relaciones:**
        *   `Clientes` y `Productos`: Una relación **muchos a muchos (N:M)** llamada `compra`. Un cliente puede comprar muchos productos y un producto puede ser comprado por muchos clientes.
        *   `Proveedores` y `Productos`: Una relación **uno a muchos (1:N)**. Un proveedor puede suministrar muchos productos, pero un producto es suministrado por un solo proveedor.
*   **Discusión y Puntos de Aprendizaje Clave:**
    *   **Claves Primarias:** Se señaló la importancia de marcar los atributos que funcionan como identificadores únicos (claves), como el `código` en `Producto`.
    *   **Participación (Existencia) Opcional vs. Obligatoria:**
        *   En la relación 1:N (`Proveedor` suministra `Producto`), se concluyó que la participación es **obligatoria**: un producto debe tener un proveedor para existir en el sistema, y un proveedor debe suministrar al menos un producto para ser relevante.
        *   En la relación N:M (`Cliente` compra `Producto`), se aclara que la regla de participación no es tan "directa" entre las entidades principales, ya que la relación `compra` es la que determina la existencia del vínculo. Un producto puede existir sin haber sido comprado aún por un cliente (participación opcional).
    *   **Atributos Compuestos:** Se consultó si atributos como `Dirección` deben descomponerse (en calle, número, etc.). El profesor explicó que el **nivel de atomicidad depende del contexto y los requisitos del problema**. Si el sistema necesita operar con las partes de la dirección por separado, se desglosa; si no, se puede dejar como un solo atributo.

##### **Ejercicio 2: Concesionaria de Automóviles**

*   **Planteamiento:** Diseñar la base de datos para una concesionaria que vende coches y realiza revisiones. Un coche solo puede ser comprado por un cliente, pero un cliente puede comprar varios coches. Un coche puede tener múltiples revisiones.
*   **Solución (Grupo 12):**
    *   **Entidades:** `Coches`, `Clientes`, `Revisiones`.
    *   **Relaciones:**
        *   `Clientes` y `Coches`: Relación **1:N**.
        *   `Coches` y `Revisiones`: Relación **1:N**.
*   **Discusión y Puntos de Aprendizaje Clave:**
    *   **Modelado de Atributos Complejos (el punto central del debate):** El enunciado indicaba que en una revisión se desea saber si se hizo "cambio de filtro, cambio de aceite, cambio de frenos u otros".
        1.  **Propuesta 1 (Grupo 12):** Un solo atributo de texto llamado `detalle`.
        2.  **Propuesta 2 (Grupo 32):** Un **atributo multivaluado** llamado `cambio`, ya que en una misma revisión se pueden realizar múltiples servicios.
        3.  **Evolución del Modelo (propuesto por el profesor):** ¿Qué pasaría si cada servicio (cambio de aceite, etc.) tuviera un costo asociado? Esto hace que las soluciones anteriores sean insuficientes. La mejor solución sería crear una nueva entidad llamada `Servicio` (con atributos como `descripcion_servicio`, `precio`) y relacionarla con `Revisión` a través de una relación **muchos a muchos (N:M)**. Esto demuestra cómo los requisitos adicionales pueden transformar el modelo.
    *   **Lógica de Negocio vs. Implementación:** Se debatió si un cliente debe tener obligatoriamente un coche asociado. Un alumno argumentó que podría ser opcional, ya que un cliente puede registrarse en la base de datos antes de realizar una compra. El profesor aclaró que, si bien la lógica de los eventos en el tiempo es válida, en el modelo conceptual se debe reflejar la regla de negocio final: un "cliente" de la concesionaria es alguien que ha comprado al menos un coche.

##### **Ejercicio 3: Empresa de Transporte**

*   **Planteamiento:** Gestionar camioneros, camiones, paquetes y provincias. Un camionero puede conducir diferentes camiones en diferentes fechas, y un camión puede ser conducido por varios camioneros.
*   **Solución (Grupo 40):**
    *   **Entidades:** `Camionero`, `Camión`, `Paquete`, `Provincia`.
    *   **Relación Clave:** Una relación **muchos a muchos (N:M)** entre `Camionero` y `Camión`.
*   **Discusión y Puntos de Aprendizaje Clave:**
    *   **Atributos en las Relaciones:** El Grupo 9 propuso añadir un atributo `fecha` directamente en la relación N:M `conduce`. El profesor confirmó que **esto es correcto y una buena práctica en el diseño conceptual** para reflejar datos que pertenecen a la interacción entre dos entidades.
    *   **Evitar el "Pensamiento Relacional" en la Etapa Conceptual:** El Grupo 9 había incluido un atributo `ID_Destinatario` en la entidad `Paquete`. El profesor corrigió esto, explicando que **no se deben incluir claves foráneas en un DER**. El foco debe estar en modelar las entidades y sus relaciones naturales. Si el destinatario tiene sus propios atributos (nombre, dirección), lo correcto es crear una nueva entidad `Destinatario` y relacionarla con `Paquete`.
    *   **Importancia de la Participación:** Se recordó que es fundamental definir si la participación en las relaciones es obligatoria u opcional (ej: ¿un camión puede existir sin tener un camionero asignado?).

##### **Ejercicio 4: Gestión Académica de un Instituto**

*   **Planteamiento:** Modelar alumnos, módulos (materias), profesores y cursos.
*   **Solución (Grupo 3):**
    *   **Entidades:** `Alumno`, `Módulo`, `Profesor`, `Curso`.
    *   **Relaciones:** N:M entre `Alumno` y `Módulo` (matriculación), y una relación entre `Curso` y `Alumno` para representar la pertenencia y el rol de "delegado".
*   **Discusión y Punto de Aprendizaje Clave:**
    *   **Ambigüedad en los Requisitos:** Este ejercicio generó la mayor discusión debido a que el enunciado era ambiguo. La principal duda era: **¿cuál es la diferencia entre "Módulo" y "Curso"?**
        *   Algunos grupos lo interpretaron como entidades separadas (ej: "Curso" como el año/división y "Módulo" como la materia).
        *   Otros grupos fusionaron los conceptos o trataron a "Curso" no como una entidad, sino como una **relación recursiva** entre alumnos para agruparlos.
    *   **La Lección Principal:** Cuando los requisitos no son claros, surgen múltiples interpretaciones y modelos diferentes. La conclusión fue que en un escenario real, es **indispensable hacer preguntas al cliente** para aclarar estas ambigüedades antes de continuar con el diseño. Debido a este debate, el profesor creó un foro para que los grupos subieran y discutieran sus diferentes soluciones a este ejercicio.

---

#### **III. Conclusiones y Recomendaciones Finales**

*   **Foco en el Modelo Conceptual:** En esta etapa, el objetivo es representar la realidad del negocio, no la implementación técnica. Evita pensar en tablas, claves foráneas o detalles de bajo nivel.
*   **Las Reglas de Negocio lo son Todo:** La cardinalidad y, especialmente, la participación (opcional/obligatoria) deben derivarse directamente de las reglas de negocio establecidas en los requisitos. No se deben suponer.
*   **El Diseño es Iterativo:** Un modelo puede (y debe) evolucionar a medida que se comprenden mejor los requisitos o se añaden nuevas funcionalidades, como se vio en el Ejercicio 2.
*   **Tarea:** Los alumnos deben subir sus soluciones y preguntas sobre el Ejercicio 4 al foro del aula virtual para continuar la discusión.
*   **Próximos Pasos:** La siguiente clase será presencial y se continuará con la misma metodología de exposición y debate sobre una nueva serie de ejercicios.

---

### **Resumen Completo de la Clase de Base de Datos (20/08/2025) Quinta Clase Virtual**

---

#### **Tema Principal del Video**

El tema central de la clase es la **transición del Diseño Conceptual (Diagrama Entidad-Relación) al Diseño Lógico (Esquema Relacional)**. El profesor explica los conceptos fundamentales del modelo relacional y detalla, paso a paso, las reglas de mapeo o "reducción" para convertir entidades, atributos y relaciones en tablas, columnas y claves.

---

#### **I. Corrección Clave del Diseño Conceptual: Relaciones Muchos a Muchos (N:M)**

Antes de avanzar al nuevo tema, el profesor realiza una corrección fundamental sobre cómo interpretar la **restricción de participación (obligatoria u opcional)** en las relaciones de cardinalidad **muchos a muchos (N:M)**.

*   **El Problema:** En clases anteriores se había generado confusión al analizar la participación desde la perspectiva de las entidades (ej: "¿puede existir un cliente sin un producto?").
*   **La Regla Corregida (CRUCIAL):** En una relación N:M, la participación se debe analizar **desde la perspectiva de la relación misma**.
*   **Análisis Correcto:**
    *   **Ejemplo `Cliente compra Producto`:** La pregunta no es si un cliente *puede* existir sin un producto, sino si una **`compra`** puede existir sin un cliente o sin un producto. La respuesta es no.
    *   **Ejemplo `Autor escribe Libro`:** La pregunta no es si un autor *puede* existir sin haber escrito un libro, sino si la acción de **`escribir`** (el vínculo entre un autor y un libro específico) puede existir sin un autor o sin un libro. La respuesta es no.
*   **Conclusión Final:** Por definición, para que una instancia de una relación N:M exista, debe involucrar obligatoriamente a una instancia de cada una de las entidades que conecta. Por lo tanto, en el diseño conceptual, la participación de las entidades en una relación de cardinalidad N:M **siempre es obligatoria**.

*   **Representaciones Equivalentes:** Se aclara que un modelo con una relación N:M (con un rombo) es conceptualmente equivalente a un modelo donde esa relación se convierte en una entidad intermedia conectada con relaciones 1:N a las entidades originales. Ambas formas son válidas en el diseño conceptual.

---

#### **II. Introducción al Modelo Relacional: Conceptos Fundamentales**

El modelo relacional es la base para la mayoría de los motores de bases de datos comerciales (SQL Server, Oracle, MySQL, etc.). Introduce una nueva terminología:

*   **Tabla (o Relación):** Es la estructura principal que almacena datos. Corresponde a una **entidad** en el modelo conceptual.
*   **Columna (o Campo):** Representa una propiedad o característica. Corresponde a un **atributo**.
*   **Fila (Registro o Tupla):** Es una instancia única de datos dentro de una tabla. Corresponde a una **instancia** en el modelo conceptual.
*   **Dominio:** Es el conjunto de valores permitidos para una columna. Define el **tipo de dato** (entero, texto, fecha, etc.) y otras restricciones (rango, formato). Es un concepto clave para la **consistencia de los datos**.
*   **Restricciones de Integridad:** Son reglas que garantizan la calidad, consistencia y coherencia de los datos. La calidad de una base de datos depende de la calidad de la información que contiene.

---

#### **III. El Proceso de Mapeo: Del Modelo Conceptual al Modelo Relacional (Paso a Paso)**

Esta es la parte central de la clase. El profesor detalla las reglas para transformar cada elemento de un Diagrama Entidad-Relación (DER) a un Esquema Relacional.

1.  **Regla para Entidades Fuertes:**
    *   Cada entidad se convierte en una **tabla** en el modelo relacional.

2.  **Regla para Atributos Simples:**
    *   Cada atributo se convierte en una **columna** de la tabla correspondiente.
    *   Se debe definir el **dominio** de cada columna (ej: `INTEGER`, `VARCHAR(100)`, `DATE`).

3.  **Regla para Atributos Compuestos (ej: `nombre_completo` con `nombre` y `apellido`):**
    *   El atributo compuesto como tal **no se representa**.
    *   Sus **componentes simples** (`nombre`, `apellido`) se convierten en columnas separadas en la tabla.

4.  **Regla para Atributos Multivaluados (ej: un empleado con varios `teléfonos`):**
    *   **CRÍTICO:** Este tipo de atributo viola la regla de atomicidad del modelo relacional (una celda, un valor).
    *   El atributo multivaluado **se elimina** de la tabla original.
    *   Se crea una **nueva tabla separada** exclusivamente para ese atributo (ej: tabla `Telefono`).
    *   Esta nueva tabla contendrá, como mínimo, dos columnas:
        *   La clave primaria de la tabla original (que actuará como **clave foránea**).
        *   La columna para el valor del atributo multivaluado (ej: `numero_telefono`).
    *   La clave primaria de esta nueva tabla es, por lo general, una **clave primaria compuesta** por ambas columnas.

5.  **Regla para Atributos Derivados (ej: `edad` calculada a partir de `fecha_nacimiento`):**
    *   Los atributos derivados **NO se incluyen** como columnas en el esquema relacional.
    *   Su valor se calcula en tiempo de ejecución a través de consultas, vistas o funciones cuando sea necesario.

6.  **Regla para Claves Primarias (PK):**
    *   Cada tabla debe tener una clave primaria definida, que debe ser **única y no nula**.
    *   Se elige entre las "claves candidatas" (atributos únicos y no opcionales) de la entidad original.

7.  **Regla para Relaciones 1:N (Uno a Muchos):**
    *   La clave primaria de la tabla del lado **'1'** de la relación se añade como una **clave foránea (FK)** en la tabla del lado **'N'**.
    *   Regla mnemotécnica: *"El de muchos es el que arrastra la clave foránea"*.

8.  **Regla para Relaciones N:M (Muchos a Muchos):**
    *   Una relación N:M directa **no existe** en el modelo relacional.
    *   La relación se convierte en una **nueva tabla intermedia** (tabla de unión o "junction table").
    *   La clave primaria de esta nueva tabla es una **clave primaria compuesta** formada por las claves foráneas de las dos tablas que conecta.

---

#### **IV. Ejemplo Práctico de Mapeo (Caso `Empleado`-`Departamento`)**

El profesor aplica estas reglas en vivo usando un caso de estudio:

*   **Entidades `Empleado` y `Departamento`:** Se convierten en tablas `empleado` y `departamento`.
*   **Atributo compuesto `apellido_y_nombre`:** Se mapea a dos columnas: `nombre` y `apellido` en la tabla `empleado`.
*   **Atributo multivaluado `telefono`:** Se crea una nueva tabla `telefono` con las columnas `dni_empleado` (FK) y `numero_telefono`. La PK de esta tabla es la combinación de ambas.
*   **Atributo derivado `edad`:** No se incluye en la tabla `empleado`.
*   **Relación 1:N `Departamento tiene Empleados`:** La PK de `departamento` (`codigo_departamento`) se añade como una FK a la tabla `empleado`.

El profesor también explica cómo manejar la **integridad referencial**: si se intenta modificar un `dni_empleado` en la tabla `empleado` que ya tiene teléfonos asociados en la tabla `telefono`, el motor de la base de datos (si está bien configurado) lo impedirá o propagará el cambio en cascada para mantener la consistencia.

---

#### **V. Conclusiones y Próximos Pasos**

*   El esquema relacional obtenido tras el mapeo es el punto de partida para la siguiente fase del diseño: la **normalización**.
*   Un buen diseño conceptual facilita enormemente la creación de un esquema relacional sólido y coherente.
*   **Tarea para los Alumnos:** Aplicar estas reglas de mapeo para pasar los ejercicios de la Serie 1 del modelo conceptual al relacional y comenzar a trabajar en la Serie 2.
---

### **Resumen Completo de la Clase de Proyecto/Laboratorio (20/08/2025) Sexta Clase Virtual**

---

#### **Tema Principal del Video**

El tema central de la clase es profundizar en el **Modelo Relacional** y consolidar el proceso de **mapeo desde el Diseño Conceptual**. La sesión funciona como una clase de consulta y refuerzo, abordando conceptos clave como las restricciones de integridad, el manejo de valores nulos, la atomicidad de los datos y, de manera muy detallada, las reglas para transformar diferentes tipos de entidades y relaciones (fuertes, débiles, 1:N, N:M) a su equivalente en el esquema relacional.

---

#### **I. Repaso y Profundización de Conceptos del Modelo Relacional**

La clase comienza reforzando los conceptos introducidos en la sesión teórica anterior.

*   **Estructura del Modelo Relacional:**
    *   **Relación (Tabla):** Estructura bidimensional de columnas y filas.
    *   **Atributo (Columna):** Característica con un nombre único dentro de la tabla y un **dominio** definido (tipo de dato). El orden de las columnas no es relevante.
    *   **Tupla (Fila/Registro):** Conjunto de datos que representa una instancia única.

*   **Restricciones de Integridad:**
    *   Son reglas para proteger la base de datos de estados **inconsistentes** (datos que no reflejan la realidad o son contradictorios).
    *   **Calidad de los Datos:** Se reitera el principio fundamental: *"Una base de datos solo es tan buena como la información almacenada en ella"*. Si se inserta "basura", se extrae "basura".
    *   El **Sistema Gestor de Base de Datos (SGBD)** es el encargado de hacer cumplir estas restricciones, una vez que han sido definidas en el modelo.

---

#### **II. El Concepto de NULO y su Importancia**

Este fue un punto clave y detallado de la clase.

*   **¿Qué es un Valor NULO?**
    *   Representa la **ausencia de un dato** en la intersección de una fila y una columna.
    *   **Importante:** `NULL` no es lo mismo que un cero (`0`), un espacio en blanco (`' '`) o un guion (`'-'`). Estos últimos son valores concretos, mientras que `NULL` es la inexistencia de un valor.
    *   Usar valores como `0` para representar la ausencia de un dato es un **mal diseño** que lleva a inconsistencias.

*   **Relación con el Diseño Conceptual:**
    *   Un atributo **opcional** en el modelo conceptual se traduce en una columna que **permite valores nulos** en el modelo relacional.
    *   Un atributo **obligatorio** se traduce en una columna que **NO permite valores nulos** (`NOT NULL`).

*   **Nulos en Claves:**
    *   **Clave Primaria (PK):** **Nunca** puede contener valores nulos. Su función es identificar unívocamente cada fila, por lo que siempre debe existir un valor.
    *   **Clave Foránea (FK):** **Puede** aceptar valores nulos. Esto ocurre cuando la participación en la relación es opcional. Por ejemplo, si un `Empleado` *puede* o no pertenecer a un `Departamento`, la columna `id_departamento` (FK) en la tabla `empleado` debe permitir nulos.

---

#### **III. Atomicidad y Redundancia de Datos**

*   **Atomicidad:**
    *   Se refiere al grado en que un dato es **indivisible**. Un dato es atómico cuando está en su mínima expresión y no puede descomponerse más.
    *   **Ejemplo:** `DNI` es atómico. `Domicilio` ("Av. Siempreviva 742, Springfield") no lo es, ya que puede descomponerse en `calle`, `altura` y `ciudad`.
    *   **Decisión de Diseño:** El nivel de atomicidad necesario depende de los requerimientos del sistema. Si se necesita buscar, ordenar o filtrar por `ciudad`, entonces es necesario separar `ciudad` en su propia columna.

*   **Redundancia:**
    *   La repetición innecesaria de datos.
    *   **Ejemplo:** Si en una tabla de proveedores se repite "Corrientes" y "Ctes." para la misma ciudad, se genera redundancia e inconsistencia. La solución a esto es la **normalización**, que se verá en clases futuras, y a menudo implica crear tablas separadas para entidades como `Localidad`.

---

#### **IV. Reglas de Mapeo Detalladas y Aplicadas (El Núcleo de la Clase)**

El profesor utiliza un caso de estudio complejo para demostrar en vivo cómo aplicar las reglas de mapeo en la herramienta ERDPlus.

1.  **Mapeo de Entidad Débil:**
    *   **Concepto:** Una entidad débil no puede existir sin su entidad "propietaria" (fuerte). Ejemplo clásico: `Factura` (fuerte) y `Detalle_Factura` (débil).
    *   **Regla de Mapeo:**
        1.  Tanto la entidad fuerte como la débil se convierten en **tablas**.
        2.  La clave primaria de la entidad **fuerte** se propaga como **clave foránea** a la tabla de la entidad **débil**.
        3.  La clave primaria de la tabla **débil** es una **clave primaria compuesta** formada por:
            *   La clave foránea de la entidad fuerte.
            *   El atributo que funcionaba como "clave parcial" en la entidad débil.

2.  **Mapeo de Relación Muchos a Muchos (N:M):**
    *   **Concepto:** Las relaciones N:M no se pueden representar directamente en el modelo relacional.
    *   **Regla de Mapeo:**
        1.  La relación N:M se convierte en una **nueva tabla intermedia**.
        2.  Esta tabla intermedia **"acarrea" las claves primarias** de las dos tablas que conecta. Estas claves primarias se convierten en **claves foráneas** dentro de la tabla intermedia.
        3.  La clave primaria de la tabla intermedia es, por regla general, una **clave primaria compuesta** formada por ambas claves foráneas.
        4.  Si la relación tenía atributos propios en el DER (ej: `fecha` en `reserva`), estos se convierten en columnas adicionales en la tabla intermedia.
    *   **Aclaración Importante:** Se debatió si se podría usar un ID autoincremental como clave primaria simple en la tabla intermedia. El profesor fue enfático: **es un mal diseño en el modelo relacional**. La clave compuesta es la que garantiza la unicidad de la relación (ej: un autor no puede aparecer dos veces para el mismo libro) y mantiene la integridad del modelo.

3.  **Mapeo de Relaciones Recursivas (1:N):**
    *   **Ejemplo:** Un `Empleado` puede tener un `Jefe`, que a su vez es otro `Empleado`.
    *   **Regla de Mapeo:** Se crea una **clave foránea en la misma tabla** que hace referencia a su propia clave primaria (ej: la tabla `empleado` tendría una columna `id_jefe` que es una FK a `id_empleado`).
    *   Si la relación es opcional (un empleado puede no tener jefe), esta columna FK debe **permitir valores nulos**.

---

#### **V. Conclusiones y Tareas para la Próxima Clase**

*   **Tarea Principal:** Para la clase práctica de mañana, los estudiantes deben:
    1.  Mapear los ejercicios de la **Serie 1** (excepto el ejercicio 4 que estaba en revisión) de su modelo conceptual al modelo relacional.
    2.  Resolver la **Serie 2**, que se encuentra en la pestaña de "Diseño Conceptual" del aula virtual.
*   **Recomendación:** Para la Serie 2, lo ideal es que presenten **ambos modelos (conceptual y relacional)**. El objetivo final es llegar a un modelo relacional correcto, pero el conceptual ayuda a justificar las decisiones de diseño.
*   **Proyecto de Estudio:** Deben empezar a definir su caso de estudio para poder desarrollar el modelo conceptual y relacional del mismo. La asignación de temas técnicos de investigación se realizará más adelante.
*   **Guía de Mapeo:** El profesor se comprometió a subir un documento guía (en borrador) con todas las reglas de mapeo explicadas.
---

### **Resumen Completo de la Clase de Proyecto/Laboratorio (27/08/2025) Septima Clase Virtual**

---

#### **Tema Principal del Video**

El tema central de la clase es la **Normalización de Bases de Datos**, una técnica fundamental dentro del diseño lógico para optimizar un esquema relacional. La sesión se enfoca en explicar las tres primeras Formas Normales (1FN, 2FN y 3FN) y cómo aplicarlas de manera práctica para mejorar la calidad del modelo de datos.

---

#### **I. Introducción y Herramientas de Diseño**

Antes de abordar el tema principal, se realizó una breve discusión sobre herramientas de modelado de bases de datos.

*   **Problemas con ERDPlus (Versión Beta):** Varios alumnos, incluido el profesor, reportaron problemas para guardar diagramas en la nueva versión beta de ERDPlus. Se recomendó tener paciencia mientras la herramienta se estabiliza.
*   **Alternativa Profesional: Vertabelo**
    *   El profesor presentó **Vertabelo** como una de las mejores herramientas profesionales para el diseño de bases de datos.
    *   **Ventajas:** Permite crear diagramas lógicos y físicos, genera scripts de SQL para diferentes motores de bases de datos (SQL Server, Oracle, etc.), realiza ingeniería inversa (crear un modelo a partir de un script) y genera documentación detallada del modelo.
    *   **Desventaja:** Es una herramienta de pago.
    *   **Oportunidad para Estudiantes:** Vertabelo ofrece una **licencia académica gratuita** a quienes posean una cuenta de correo institucional (ej: `@exa.unne.edu.ar`). El profesor animó a los estudiantes a solicitar estas cuentas de forma individual a la facultad, justificando la necesidad de acceder a recursos académicos.

---

#### **II. El Proceso de Normalización: ¿Qué es y Por Qué es Importante?**

La normalización es un proceso sistemático que se aplica al modelo relacional para mejorar su estructura. Sus objetivos principales son:

1.  **Evitar la Redundancia de Datos:** Eliminar la repetición innecesaria de información.
2.  **Disminuir Problemas de Actualización:** Asegurar que un cambio en un dato se realice en un solo lugar, evitando inconsistencias (conocidas como "anomalías de actualización").
3.  **Proteger la Integridad de los Datos:** Garantizar que los datos sean consistentes, coherentes y reflejen la realidad del negocio.

El proceso se realiza a través de una serie de reglas o "etapas" llamadas **Formas Normales**. En esta clase se cubrieron las tres primeras.

---

#### **III. Las Tres Primeras Formas Normales (1FN, 2FN, 3FN)**

##### **1. Primera Forma Normal (1FN): Atomicidad**

*   **Regla:** Una tabla está en 1FN si y solo si **todos sus atributos son atómicos**.
*   **Concepto de Atomicidad:** Un atributo es atómico si contiene un **único valor** y no puede ser descompuesto en partes más pequeñas con significado propio.
*   **Violaciones Comunes de la 1FN:**
    1.  **Atributos Multivaluados:** Una columna que almacena múltiples valores para una misma fila (ej: una columna `telefono` que guarda "3794-112233, 3794-445566").
        *   **Solución:** Se crea una **nueva tabla separada** para el atributo multivaluado, con una clave foránea que la relacione con la tabla original.
    2.  **Atributos Compuestos:** Una columna que agrupa varios datos distintos (ej: `localidad_provincia` que contiene "Resistencia, Chaco").
        *   **Solución:** Se **descompone** el atributo en múltiples columnas (`localidad`, `provincia`). El nivel de descomposición depende de los requerimientos del sistema.
*   **Conclusión:** Un buen diseño conceptual y mapeo al modelo relacional ya debería resolver la mayoría de los problemas de 1FN.

##### **2. Segunda Forma Normal (2FN): Dependencia Funcional Completa**

*   **Regla:** Una tabla está en 2FN si:
    1.  Está en 1FN.
    2.  **Todos los atributos que no forman parte de la clave primaria dependen funcionalmente de la clave primaria completa**.
*   **Punto Clave:** Esta regla es relevante **únicamente para tablas con claves primarias compuestas**. Si una tabla está en 1FN y tiene una clave primaria simple, automáticamente está en 2FN.
*   **Concepto de Dependencia Funcional Parcial (Violación de 2FN):** Ocurre cuando un atributo no clave depende solo de **una parte** de la clave primaria compuesta, y no de toda la clave.
*   **Ejemplo Práctico:**
    *   **Tabla `Matricula(id_alumno, id_curso, nombre_alumno, nombre_curso)`**.
    *   **Clave Primaria Compuesta:** `(id_alumno, id_curso)`.
    *   **Análisis:**
        *   `nombre_alumno` depende solo de `id_alumno`. **(Dependencia parcial)**.
        *   `nombre_curso` depende solo de `id_curso`. **(Dependencia parcial)**.
    *   **Solución:** Se **descompone** la tabla original en nuevas tablas para eliminar las dependencias parciales:
        1.  `Alumno(id_alumno, nombre_alumno)`
        2.  `Curso(id_curso, nombre_curso)`
        3.  `Matricula(id_alumno, id_curso)` (La tabla original queda solo con la clave primaria compuesta).

##### **3. Tercera Forma Normal (3FN): Sin Dependencias Transitivas**

*   **Regla:** Una tabla está en 3FN si:
    1.  Está en 2FN.
    2.  **No existen dependencias transitivas** entre los atributos no clave.
*   **Concepto de Dependencia Transitiva (Violación de 3FN):** Ocurre cuando un atributo no clave (C) depende de otro atributo no clave (B), que a su vez depende de la clave primaria (A). Es decir: A → B → C.
*   **Ejemplo Práctico:**
    *   **Tabla `Alumno(id_alumno, localidad, provincia)`**.
    *   **Clave Primaria Simple:** `id_alumno`.
    *   **Análisis:** `provincia` no depende directamente de `id_alumno`. `provincia` depende de `localidad`, y `localidad` depende de `id_alumno`. Se da una dependencia transitiva.
    *   **Solución:** Se **descompone** la tabla para eliminar la dependencia transitiva, creando una nueva tabla:
        1.  `Alumno(id_alumno, id_localidad)` (Se usa un ID o el nombre si es único).
        2.  `Localidad(id_localidad, nombre_localidad, provincia)`.

---

#### **IV. Aplicación Práctica y Redundancia**

El profesor demostró el proceso completo de normalización sobre una tabla `Matricula` inicial que violaba las tres formas normales. El resultado fue un modelo descompuesto en varias tablas (`Matricula`, `Alumno`, `Curso`, `Docente`, `Localidad`, `Provincia`), cada una cumpliendo con la 3FN.

*   **Proceso Natural:** Se destacó que un buen diseño conceptual, seguido de un mapeo correcto al modelo relacional, ya incorpora gran parte de la normalización de forma intuitiva. Sin embargo, las formas normales sirven como una técnica formal para revisar y refinar el diseño.
*   **Objetivo Final:** A partir de ahora, todos los diseños de bases de datos en la materia deben, como mínimo, **cumplir con la Tercera Forma Normal (3FN)**.

---

#### **V. Tareas y Próximos Pasos**

*   **Nueva Serie de Ejercicios (Serie 3):** Se ha publicado una nueva guía de ejercicios en el aula virtual. Estos ejercicios tienen una mayor complejidad y están diseñados para practicar la normalización.
*   **Tarea para Mañana:** Los estudiantes deben resolver los ejercicios de la Serie 3 y estar preparados para presentarlos y discutirlos en la clase práctica presencial. Se recomienda analizar los datos de prueba proporcionados en la guía para entender mejor el comportamiento de los datos antes de normalizar.
  ---
