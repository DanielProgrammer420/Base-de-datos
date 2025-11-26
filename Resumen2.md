
### 1. Resumen General del Tema
Las **Funciones de Filas** (o escalares) son herramientas en SQL que operan sobre un **único valor** de una fila y devuelven un **único resultado**. Su función principal es transformar, formatear o calcular datos (limpieza, estandarización) sin agruparlos. Se utilizan fundamentalmente en las cláusulas SELECT, WHERE y HAVING, y su comportamiento o sintaxis puede variar ligeramente entre diferentes motores de base de datos (como SQL Server y MySQL) y el estándar ANSI.

---

### 2. Conceptos Clave Indispensables (Para memorizar)

* **Operación Fila por Fila:** A diferencia de las funciones de agregación, el resultado de una función escalar es totalmente independiente de las otras filas del conjunto de datos.
* **Visibilidad y Atomicidad:** El resultado es un valor atómico que puede usarse directamente en cualquier parte de la consulta que admita una expresión de columna.
* **Transformación en WHERE:** Permiten transformar temporalmente un valor para evaluarlo en una condición de filtrado (ej: extraer el año de una fecha para compararlo), respetando el Orden Lógico de Procesamiento.
* **Manejo de Nulos:** Son vitales para la robustez de las consultas, permitiendo reemplazar valores desconocidos o evitar errores matemáticos fatales.

---

### 3. Definiciones Exactas (Listas para MC y V/F)

* **Función Escalar:** Operación que toma un solo valor de una tupla (fila) y retorna un solo valor.
* **COALESCE():** Función estándar que retorna el **primer valor NO NULO** de una lista de expresiones proporcionada.
* **NULLIF():** Retorna **NULL** si las dos expresiones comparadas son iguales; de lo contrario, retorna la primera expresión.
* **TRIM():** Elimina espacios en blanco tanto del inicio como del final de una cadena de texto.
* **DATEDIFF():** Calcula la diferencia o el intervalo de tiempo entre dos fechas.

---

### 4. Diferencias que Suelen Aparecer en Exámenes

* **Funciones Escalares vs. Funciones de Agregación:**
    * *Escalares:* 1 fila de entrada $\rightarrow$ 1 valor de salida (independiente).
    * *Agregación (SUM, AVG):* Múltiples filas de entrada $\rightarrow$ 1 valor de salida (dependiente del grupo).

* **Sintaxis de Longitud de Cadena (Portabilidad):**
    * *SQL Server:* `LEN(cadena)`.
    * *MySQL / ANSI:* `CHAR_LENGTH(cadena)` o `CHARACTER_LENGTH`.

* **Reemplazo de Nulos (Específico del motor):**
    * *SQL Server:* `ISNULL(expresion, reemplazo)`.
    * *MySQL:* `IFNULL(expresion, reemplazo)`.
    * *Estándar/Ambos:* `COALESCE` (Es la opción más segura porque soporta múltiples argumentos y es estándar).

---

### 5. Propiedades y Características

* **Categorías Principales:**
    * *Strings (Cadenas):* Estandarización (`UPPER`/`LOWER`), Limpieza (`TRIM`), Extracción (`SUBSTRING`, `LEFT`, `RIGHT`), Concatenación (`CONCAT`).
    * *Date/Time (Fechas):* Extracción de partes (`YEAR`, `MONTH`), Aritmética (`DATEDIFF`, `DATEADD`), Formato (`DATE_FORMAT`).
    * *Numéricas:* Redondeo (`ROUND`), Truncado o Techo (`FLOOR`/`CEILING`), Valor Absoluto (`ABS`).
    * *Conversión:* Cambio explícito de tipo de dato (`CAST`, `CONVERT`).
* **Integración en OLP:** Respetan el Orden Lógico de Procesamiento, siendo muy útiles para filtrar datos en el paso 4 (cláusula WHERE) antes de la proyección final.

---

### 6. Advertencias y Trampas Comunes de Examen (¡OJO AQUÍ!)

* **Trampa de DATEDIFF:** El orden de los parámetros cambia drásticamente según el motor.
    * *SQL Server:* `DATEDIFF(unidad, fecha_inicio, fecha_fin)`.
    * *MySQL:* `DATEDIFF(fecha_fin, fecha_inicio)` (Generalmente solo devuelve días).
* **Trampa de NULLIF:** A menudo se pregunta qué devuelve si los valores son *diferentes*. Respuesta: Devuelve la **primera expresión**, NO devuelve null.
* **Trampa de División por Cero:** Para evitar errores que detengan la consulta, se debe usar `NULLIF(divisor, 0)` para convertir el 0 en NULL; cualquier número dividido por NULL da NULL (no error).
* **Trampa de CONCAT:** Aunque en SQL Server se puede usar `+` y en ANSI `||`, la función `CONCAT()` es la más segura para evitar problemas con nulos al unir textos.

---

### 7. Mini Ejemplos Explicativos

* **Filtrado por Año (WHERE):**
    `WHERE YEAR(Fecha_Nacimiento) = 1990` $\rightarrow$ La función transforma la fecha *solo para la comparación*, no altera el dato guardado en la tabla.
* **Manejo de Nulos (COALESCE):**
    `COALESCE(Telefono, 'No Disponible')` $\rightarrow$ Si el campo *Telefono* es NULL, muestra 'No Disponible'; si tiene número, muestra el número.
* **Comparación de Texto (UPPER):**
    `WHERE UPPER(Ciudad) = 'MADRID'` $\rightarrow$ Permite encontrar 'Madrid', 'madrid' o 'MADRID' sin importar cómo fue escrito originalmente.


