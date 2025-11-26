
### 🎓 Resumen Maestro: Funciones de Agregación y Agrupamiento (T-SQL)

#### 1. Resumen General del Tema
La agregación de datos es el proceso de transformar grandes volúmenes de filas en información resumida y significativa. Este proceso sigue tres fases lógicas conocidas como **Split** (dividir los datos en grupos), **Apply** (aplicar la función de cálculo a cada grupo) y **Combine** (unificar los resultados). En SQL Server, utilizamos funciones como SUM o COUNT junto con las cláusulas `GROUP BY` y `HAVING` para controlar la granularidad (nivel de detalle) de la información.

---

#### 2. Conceptos Clave Indispensables (Memorizar)
* **Función de Agregación:** Operación que devuelve un **único valor** de resumen a partir de un conjunto de filas (input).
* **Granularidad:** Al agrupar, elevamos el nivel de abstracción pero **perdemos el detalle** de la fila individual (ya no puedes ver los datos de una venta específica, solo el total por grupo).
* **DISTINCT en Agregados:** Si usas el modificador `DISTINCT` dentro de la función (ej. `AVG(DISTINCT Precio)`), el motor primero elimina duplicados y luego calcula. Si no lo usas, calcula sobre todo el conjunto.
* **Split-Apply-Combine:** Es el concepto teórico fundamental de cómo funciona el motor internamente al agrupar datos.

---

#### 3. Definiciones Exactas (Para Opción Múltiple)
* **SUM (Expresión):** Calcula la suma algebraica total. Requiere obligatoriamente columnas de tipo **numérico**.
* **AVG (Expresión):** Calcula la media aritmética (promedio). Requiere columnas numéricas.
* **MIN / MAX:** Devuelven el valor mínimo o máximo. Funcionan con números, **fechas** (más antigua/reciente) y **cadenas de texto** (orden alfabético).
* **COUNT(\*):** Cuenta el número total de filas (tuplas). **No ignora NULLs**, pues su propósito es medir la existencia física de la fila.
* **COUNT(Columna):** Cuenta el número de filas donde la columna tiene un valor definido. **Ignora explícitamente los NULLs**.
* **GROUP BY:** Cláusula que segmenta el conjunto de resultados en grupos basados en valores idénticos de una o más columnas.
* **HAVING:** Filtro que especifica una condición de búsqueda aplicada a los **grupos** o a los **resultados agregados** (opera post-agregación).

---

#### 4. Diferencias que SIEMPRE aparecen en Exámenes

**A. WHERE vs. HAVING** (¡Pregunta fija!):
* **WHERE:** Filtra **filas individuales**. Se ejecuta **antes** de agrupar. **NO** admite funciones de agregación.
* **HAVING:** Filtra **grupos**. Se ejecuta **después** de agrupar. **SÍ** admite (y suele requerir) funciones de agregación.

**B. COUNT(\*) vs. COUNT(Columna)**:
* **COUNT(\*):** Cuenta todo (incluso si el campo es nulo). Resultado = Tamaño total de la tabla/grupo.
* **COUNT(Col):** Cuenta solo valores válidos. Resultado = Total menos los nulos.
* *Tip de examen:* La diferencia matemática entre `COUNT(*)` y `COUNT(Col)` es exactamente el número de filas que tienen NULL en esa columna (útil para auditar calidad de datos).

**C. AVG(Col) vs. AVG(DISTINCT Col)**:
* **AVG:** (Suma de todos los valores) / (Cantidad total de valores).
* **AVG DISTINCT:** (Suma de valores únicos) / (Cantidad de valores únicos). Dan resultados matemáticos diferentes.

---

#### 5. Propiedades y Manejo de NULLs
* **Regla de Oro de los Nulos:** Todas las funciones de agregación estándar (`SUM`, `AVG`, `MIN`, `MAX`, `COUNT(col)`) **IGNORAN** los valores NULL. La única excepción es `COUNT(*)` (o `COUNT(1)`).
* **Resultado en conjunto vacío:** Si aplicas `SUM`, `AVG`, `MIN` o `MAX` sobre un conjunto vacío o que solo contiene nulos, el resultado devuelto es **NULL** (no devuelve cero).
* **Resultado en vacío COUNT:** `COUNT` devuelve **0** si no encuentra filas.

---

#### 6. Advertencias y Trampas Comunes (¡Cuidado!)
1.  **Trampa del SELECT:** Si utilizas `GROUP BY`, **toda** columna listada en el `SELECT` que no esté dentro de una función de agregación (como SUM o MAX) **DEBE OBLIGATORIAMENTE** estar incluida en la cláusula `GROUP BY`.
    * *Incorrecto:* `SELECT Departamento, NombreEmpleado, SUM(Sueldo) FROM Tabla GROUP BY Departamento` (Falla porque NombreEmpleado no es parte del grupo ni de una suma).
2.  **Trampa del WHERE:** No puedes poner condiciones agregadas en el WHERE. Ejemplo: `WHERE SUM(Ventas) > 1000` dará error de sintaxis. Debes mover esa condición al `HAVING`.
3.  **Optimización:** Siempre es mejor filtrar primero con `WHERE` (ej. filtrar por año) para reducir el volumen de datos antes de que el motor gaste recursos en la operación costosa de agrupar. No dejes para el `HAVING` lo que puedes filtrar antes en el `WHERE`.

---

#### 7. Orden Lógico de Ejecución (Vital para entender el flujo)
Es imperativo entender que el motor de base de datos no ejecuta las cláusulas en el orden en que las escribes. El orden lógico de procesamiento es:

1.  **FROM / JOINs** (Determina las tablas fuente)
2.  **WHERE** (Filtra filas individuales - Fase Pre-agregación)
3.  **GROUP BY** (Agrupa las filas filtradas)
4.  **Funciones de Agregación** (Calcula los valores de resumen por grupo)
5.  **HAVING** (Filtra los grupos resultantes - Fase Post-agregación)
6.  **SELECT** (Proyecta las columnas finales)
7.  **ORDER BY** (Ordena el conjunto de resultados final)

