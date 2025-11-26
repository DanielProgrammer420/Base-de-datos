### 1. Resumen General del Tema
Este tema trata sobre cómo unificar datos dispersos en una base de datos relacional. Existen dos mecanismos fundamentales:
1.  **Joins (Reuniones):** Combinan datos **horizontalmente** (agregan columnas de otra tabla) basándose en una relación lógica (generalmente PK = FK).
2.  **Operaciones de Conjuntos:** Combinan resultados de consultas **verticalmente** (agregan filas a un listado existente) basándose en la estructura compatible de las tablas.

---

### 2. Conceptos Clave Indispensables (Para memorizar)

* **Cláusula ON:** Es el corazón del JOIN. Define la condición lógica para emparejar filas (ej. `TablaA.ID = TablaB.ID_Cliente`).
* **Compatibilidad de Esquemas:** Requisito obligatorio para operaciones de conjuntos (UNION, INTERSECT, etc.). Las consultas deben tener el **mismo número de columnas** y **tipos de datos compatibles** en el mismo orden.
* **Producto Cartesiano:** Ocurre cuando se combinan tablas sin condición de filtrado. El resultado es la multiplicación de todas las filas de A por todas las de B.
* **Restricción vs. Expansión:** El INNER JOIN restringe datos (elimina lo que no coincide), mientras que los OUTER JOINs (Left, Right, Full) preservan datos aunque no tengan pareja (rellenando con NULL).

---

### 3. Definiciones Exactas (Listas para MC y V/F)

* **INNER JOIN:** Retorna **solo** las filas que tienen coincidencia en **ambas** tablas. Es la reunión más restrictiva.
* **LEFT JOIN:** Retorna **todas** las filas de la tabla izquierda y las coincidentes de la derecha (o NULL si no hay coincidencia).
* **RIGHT JOIN:** Retorna **todas** las filas de la tabla derecha y las coincidentes de la izquierda (o NULL si no hay coincidencia).
* **FULL OUTER JOIN:** Retorna filas cuando hay coincidencia en **cualquiera** de las tablas (todo A + todo B).
* **CROSS JOIN:** Combina cada fila de la primera tabla con cada fila de la segunda (Producto Cartesiano).
* **UNION:** Combina resultados de dos consultas, **eliminando duplicados** (hace un ordenamiento interno).
* **UNION ALL:** Combina resultados de dos consultas, **manteniendo duplicados** (es más rápido porque no ordena ni verifica).
* **INTERSECT:** Devuelve solo las filas que existen en **ambos** conjuntos de resultados.
* **EXCEPT:** Devuelve las filas que están en el **primer** conjunto pero **no** en el segundo (Resta).

---

### 4. Diferencias que Suelen Aparecer en Exámenes

* **JOIN vs. UNION:**
    * *JOIN:* Crecimiento horizontal (más columnas).
    * *UNION:* Crecimiento vertical (más filas).
* **UNION vs. UNION ALL:**
    * *UNION:* Lento (procesa distinct). No muestra duplicados.
    * *UNION ALL:* Rápido (solo concatena). Muestra duplicados.
* **NATURAL JOIN (Estándar) vs. SQL Server:**
    * *Estándar:* Une automático por columnas con mismo nombre.
    * *SQL Server:* **No existe**. Debes usar INNER JOIN explícito.

---

### 5. Propiedades y Características

* **Cardinalidad del CROSS JOIN:** Si Tabla A tiene $N$ filas y Tabla B tiene $M$ filas, el resultado tendrá $N \times M$ filas.
* **Manejo de Nulos en Outer Joins:** Si un registro de la tabla principal (ej. Izquierda en Left Join) no tiene pareja, las columnas de la tabla secundaria se llenan con **NULL**.
* **Equivalencia de Nombres:**
    * `EXCEPT` (SQL Server) = `MINUS` (Oracle).

---

### 6. Advertencias y Trampas Comunes de Examen (¡OJO AQUÍ!)

* **Trampa del NATURAL JOIN:** En SQL Server NO existe el `NATURAL JOIN`. Si una pregunta de V/F dice "En SQL Server, NATURAL JOIN une tablas automáticamente...", es **FALSO**.
* **Trampa del Orden en EXCEPT:** `A EXCEPT B` **no es lo mismo** que `B EXCEPT A`. El orden importa (es una resta: 5 - 3 no es igual a 3 - 5).
* **Trampa de Performance:** Si te preguntan cuál es más eficiente para unir listas disjuntas (ej. ventas 2023 y ventas 2024), la respuesta es **UNION ALL**, porque evita el costo de buscar duplicados que ya sabes que no existen.
* **Trampa de columnas en UNION:** Si la consulta A tiene 3 columnas y la consulta B tiene 4, la operación dará **error**. Deben tener la misma cantidad.

---

### 7. Mini Ejemplos Explicativos

* **INNER JOIN:** ¿Clientes con pedidos?
    `Clientes INNER JOIN Pedidos` $\rightarrow$ Solo muestra clientes que compraron.
* **LEFT JOIN:** ¿Todos los clientes y sus pedidos (si tienen)?
    `Clientes LEFT JOIN Pedidos` $\rightarrow$ Muestra todos los clientes; si uno no compró, sale NULL en los datos del pedido.
* **CROSS JOIN:**
    Tabla Colores (3 filas) x Tabla Talles (4 filas) = 12 combinaciones posibles.
* **EXCEPT:**
    `ClientesActivos EXCEPT Pedidos2024` $\rightarrow$ Clientes que están activos pero **NO** compraron este año.
