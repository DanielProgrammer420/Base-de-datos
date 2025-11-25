### 1. Resumen General del Tema
La Unidad 5 abarca **SQL (Structured Query Language)** desde su base hasta conceptos avanzados. Se divide fundamentalmente en dos mundos: **DDL** (Definición, el "arquitecto" que crea estructuras) y **DML** (Manipulación, el usuario que gestiona datos).
A medida que avanzamos, el tema integra la recuperación compleja de datos mediante **JOINs**, **Subconsultas** y **Operaciones de Conjuntos**. Finalmente, se tratan aspectos de administración y seguridad como **Vistas**, **Integridad**, **Autorización** y la programación en base de datos (**Procedimientos/Funciones**), cerrando con la distinción entre SQL Incorporado y Dinámico.

---

### 2. Conceptos Clave Indispensables (Jerarquía de Examen)

* **DDL (Data Definition Language):** Crea y modifica la estructura (tablas, índices, vistas). Comandos principales: `CREATE`, `ALTER`, `DROP`.
* **DML (Data Manipulation Language):** Consulta y modifica los datos internos. Comandos principales: `SELECT`, `INSERT`, `UPDATE`, `DELETE`.
* **Restricciones (Constraints):** Reglas impuestas para asegurar la integridad y consistencia de los datos (`PK`, `FK`, `UNIQUE`, `CHECK`, `NOT NULL`).
* **JOINs:** Mecanismo fundamental para combinar filas de dos o más tablas basándose en una columna común.
* **Agregación:** Operaciones matemáticas sobre grupos de filas para devolver un único valor (`GROUP BY`, `HAVING`).
* **Vistas:** Tablas virtuales que no almacenan datos propios, sino una definición de consulta lógica.
* **Autorización:** Control de seguridad y acceso mediante `GRANT` (dar permisos) y `REVOKE` (quitar permisos).

---

### 3. Definiciones Exactas (Memorizables)

* **NULL:** Representa la ausencia de valor, desconocido o no aplicable. **No** es lo mismo que cero, espacio en blanco o cadena vacía.
* **Primary Key (PK):** Identificador único de cada fila. Garantiza unicidad y no permite valores nulos.
* **Foreign Key (FK):** Crea un vínculo entre tablas para la integridad referencial. El valor debe existir en la tabla referenciada o ser NULL.
* **Esquema (Schema):** Colección lógica de objetos (tablas, vistas, funciones) agrupados bajo un nombre, útil para organizar y gestionar permisos.
* **Subconsulta:** Una sentencia `SELECT` anidada dentro de otra instrucción SQL (como otro SELECT, INSERT, UPDATE o DELETE).
* **SQL Dinámico:** Construcción y ejecución de sentencias SQL como cadenas de texto en tiempo de ejecución, útil cuando la consulta no se conoce de antemano.
* **Procedimiento Almacenado:** Bloque de código guardado en la BD que realiza operaciones; puede aceptar parámetros y no necesariamente devuelve un resultado directo como una función.
* **Función Almacenada:** Bloque de código guardado que realiza cálculos y **siempre** devuelve un único valor.

---

### 4. Diferencias Críticas (Preguntas Fijas de Examen)

| Concepto A | Concepto B | Diferencia Clave para el Examen |
| :--- | :--- | :--- |
| **WHERE** | **HAVING** | `WHERE` filtra filas individuales **antes** del agrupamiento. `HAVING` filtra grupos **después** de calcular las agregaciones. |
| **DELETE** | **DROP** | `DELETE` elimina filas (datos) pero la tabla sigue existiendo. `DROP` elimina la tabla completa (estructura y datos). |
| **UNION** | **UNION ALL** | `UNION` combina resultados eliminando duplicados (más costoso). `UNION ALL` incluye todo, incluso duplicados (mejor rendimiento). |
| **INNER JOIN** | **OUTER JOIN** | `INNER` devuelve solo filas con coincidencias en ambas tablas. `OUTER` (Left/Right) devuelve filas aunque no haya coincidencia en la otra tabla (rellenando con NULL). |
| **CHAR** | **VARCHAR** | `CHAR` es para cadenas de longitud fija. `VARCHAR` es para longitud variable (optimiza espacio). |
| **ON** | **USING** | `ON` especifica explícitamente la condición (`t1.id = t2.id`). `USING` es un atajo solo si la columna se llama igual en ambas tablas (`USING (id)`). |

---

### 5. Propiedades y Características (Listas para V/F)

* **Funciones de Agregación y NULL:**
    * La mayoría de funciones (`SUM`, `AVG`, `MAX`, `MIN`) **ignoran** los valores NULL.
    * La excepción es `COUNT(*)` que cuenta todas las filas, incluyendo las que tienen valores NULL.
* **Vistas:**
    * Actúan como filtros personalizados o ventanas a los datos.
    * No almacenan datos físicamente (se expanden a su consulta subyacente al ejecutarse).
    * No todas son actualizables (especialmente si tienen `GROUP BY`, `DISTINCT` o agregaciones).
* **Operaciones de Conjunto:**
    * Para funcionar, las consultas deben tener el **mismo número de columnas**.
    * Los tipos de datos de las columnas correspondientes deben ser **compatibles**.
* **Integridad Referencial (Acciones):**
    * `CASCADE`: Elimina o actualiza automáticamente las filas dependientes.
    * `RESTRICT` / `NO ACTION`: Impide la operación si existen dependencias (más seguro por defecto).

---

### 6. Advertencias y Trampas Comunes de Examen (¡Cuidado!)

> **ATENCIÓN:** Lee esto dos veces antes de rendir.

1.  **Comparar NULL con `=`:** Jamás uses `WHERE columna = NULL`. La comparación siempre dará "Desconocido". Debes usar siempre los operadores `IS NULL` o `IS NOT NULL`.
2.  **El peligro del DELETE/UPDATE sin WHERE:** Si olvidas la cláusula `WHERE` en un `UPDATE` o `DELETE`, modificarás o borrarás **todas** las filas de la tabla.
3.  **Orden Lógico:** Recuerda que no puedes usar funciones de agregación (como `SUM` o `COUNT`) directamente en la cláusula `WHERE`. Debes usarlas en el `HAVING` o en el `SELECT`.
4.  **NATURAL JOIN:** Es riesgoso porque une automáticamente por columnas con el mismo nombre. Si hay columnas coincidentes por casualidad que no son claves, la unión será incorrecta.
5.  **Inyección SQL:** Es la principal vulnerabilidad del **SQL Dinámico** si no se usan parámetros. Ocurre cuando se concatenan cadenas de usuario directamente en la consulta.

---

### 7. Mini Ejemplos Explicativos

* **Subconsulta Correlacionada:** Una subconsulta que depende de un valor de la consulta externa y se re-evalúa para cada fila.
    * *Ej:* Empleados que ganan más que el promedio de *su propio* departamento.
* **Uso de ALIAS:**
    * `SELECT e.nombre FROM Empleados AS e` -> Ayuda a la legibilidad y brevedad.
* **Left Join:**
    * `Departamentos LEFT JOIN Empleados`: Muestra todos los departamentos, incluso los que tienen 0 empleados (en ese caso, los datos del empleado serán NULL).
* **Check Constraint:**
    * `CHECK (salario > 0)`: Asegura que nadie pueda insertar un salario negativo.


