Aquí tienes el material de estudio definitivo sobre **Funciones y Procedimientos Almacenados**, estructurado para garantizar tu éxito en el examen parcial.

-----

# Funciones y Procedimientos Almacenados en SQL Server

## Resumen Maestro Completo

### 1\. Definiciones y Propósito

Ambos son objetos de base de datos que encapsulan lógica de programación en SQL (T-SQL), permitiendo reutilizar código, mejorar la seguridad y centralizar la lógica de negocio. Sin embargo, sus propósitos son distintos:

  * **Procedimientos Almacenados (Stored Procedures - SP):** Son rutinas preparadas para realizar una tarea o un conjunto de tareas (flujos de trabajo). Están diseñados para **ejecutar acciones** (modificar datos, realizar transacciones complejas, devolver múltiples conjuntos de resultados).
  * **Funciones Definidas por el Usuario (User Defined Functions - UDF):** Son rutinas diseñadas para **calcular y devolver valores**. Su objetivo es procesar datos y retornar un resultado (escalar o tabla) para ser usado dentro de otras consultas.

### 2\. Conceptos Fundamentales

#### A. Procedimientos Almacenados (SP)

  * **Ejecución:** Se ejecutan de manera independiente mediante el comando `EXEC` o `EXECUTE`.
  * **Flexibilidad:** Pueden contener casi cualquier comando SQL (SELECT, INSERT, UPDATE, DELETE, CREATE, DROP).
  * **Parámetros:** Aceptan parámetros de entrada y parámetros de salida (`OUTPUT`).
  * **Retorno:**
      * Pueden devolver múltiples *Result Sets* (tablas resultado de varios SELECTs).
      * Pueden devolver un código de estado entero (`RETURN`) para indicar éxito o fallo (generalmente 0 es éxito).

#### B. Funciones (UDF)

  * **Ejecución:** Se invocan como parte de una expresión T-SQL (dentro de un `SELECT`, `WHERE`, `HAVING`).
  * **Restricciones:** No pueden realizar cambios en el estado de la base de datos (no pueden hacer INSERT/UPDATE/DELETE sobre tablas base). Solo pueden modificar variables locales de la función.
  * **Tipos de Funciones:**
    1.  **Escalares:** Devuelven un único valor (int, varchar, date, etc.).
    2.  **Tabla en línea (Inline Table-Valued):** Devuelven una tabla resultado de una sola sentencia SELECT. Actúan como "vistas parametrizadas".
    3.  **Tabla multisentencia (Multi-Statement Table-Valued):** Crean una tabla en memoria, la llenan con lógica compleja y la devuelven.

### 3\. Reglas y Propiedades Clave

1.  **Determinismo:** Una función es determinista si siempre devuelve el mismo resultado para los mismos valores de entrada (ej: `SUM()`). Es no determinista si el resultado cambia (ej: `GETDATE()`).
2.  **Uso de DML:**
      * **SP:** Permitido totalmente.
      * **UDF:** **Prohibido** en tablas permanentes. Una función no puede modificar la base de datos.
3.  **Manejo de Errores:**
      * **SP:** Soporta `TRY...CATCH`.
      * **UDF:** No soporta `TRY...CATCH`.
4.  **Llamadas cruzadas:**
      * Un SP puede llamar a una Función.
      * Una Función **NO** puede llamar a un SP.

-----

## Errores Comunes en Parciales

1.  **Intentar llamar a un SP dentro de un SELECT:**
      * *Incorrecto:* `SELECT * FROM dbo.MiProcedimiento()` $\rightarrow$ Esto dará error.
      * *Correcto:* `EXEC dbo.MiProcedimiento`.
2.  **Confundir el `RETURN`:**
      * En una UDF, `RETURN` devuelve el dato calculado (el objetivo de la función).
      * En un SP, `RETURN` se usa solo para devolver un código de estado (integer) y corta la ejecución inmediatamente.
3.  **Creer que las funciones pueden modificar datos:** Si en el examen te preguntan si puedes hacer un `UPDATE` a la tabla `Clientes` dentro de una función, la respuesta es un rotundo **NO**.

-----

## Ejemplos en SQL Server

### 1\. Procedimiento Almacenado (Lógica de Negocio)

```sql
-- Creación del SP
CREATE PROCEDURE Ventas.sp_RegistrarVenta
    @IdProducto INT,
    @Cantidad INT,
    @MensajeSalida VARCHAR(100) OUTPUT -- Parámetro de salida
AS
BEGIN
    SET NOCOUNT ON; -- Buena práctica: evita mensajes de "x filas afectadas"
    
    BEGIN TRY
        -- Lógica: Descontar stock e insertar venta
        UPDATE Inventario.Productos
        SET Stock = Stock - @Cantidad
        WHERE Id = @IdProducto;

        INSERT INTO Ventas.Detalle (IdProducto, Cantidad, Fecha)
        VALUES (@IdProducto, @Cantidad, GETDATE());

        SET @MensajeSalida = 'Venta registrada exitosamente.';
    END TRY
    BEGIN CATCH
        SET @MensajeSalida = 'Error: ' + ERROR_MESSAGE();
    END CATCH
END;

-- Ejecución
DECLARE @Respuesta VARCHAR(100);
EXEC Ventas.sp_RegistrarVenta 
    @IdProducto = 5, 
    @Cantidad = 2, 
    @MensajeSalida = @Respuesta OUTPUT;

PRINT @Respuesta;
```

### 2\. Función Escalar (Cálculo)

```sql
-- Creación: Calcular precio con IVA
CREATE FUNCTION Ventas.fn_CalcularPrecioConIVA (@PrecioBase DECIMAL(10,2))
RETURNS DECIMAL(10,2)
AS
BEGIN
    DECLARE @PrecioFinal DECIMAL(10,2);
    SET @PrecioFinal = @PrecioBase * 1.21;
    RETURN @PrecioFinal;
END;

-- Ejecución (Dentro de un SELECT)
SELECT Nombre, Precio, Ventas.fn_CalcularPrecioConIVA(Precio) as PrecioFinal
FROM Productos;
```

### 3\. Función de Tabla en Línea (Vista Parametrizada)

```sql
-- Creación: Obtener productos por rango de precio
CREATE FUNCTION Ventas.fn_ProductosPorRango (@Min DECIMAL, @Max DECIMAL)
RETURNS TABLE
AS
RETURN (
    SELECT Id, Nombre, Precio
    FROM Productos
    WHERE Precio BETWEEN @Min AND @Max
);

-- Ejecución (En el FROM)
SELECT * FROM Ventas.fn_ProductosPorRango(100, 500);
```

-----

## Tabla Comparativa (Vital para el examen)

| Característica | Procedimiento Almacenado (SP) | Función (UDF) |
| :--- | :--- | :--- |
| **Invocación** | `EXECUTE` (independiente) | Dentro de `SELECT`, `WHERE`, `FROM` |
| **Retorno** | Múltiples Result Sets, Parámetros Output, Return Code (int) | Un valor escalar o una Tabla |
| **DML (Insert/Update/Delete)** | Permitido en cualquier tabla. | **Prohibido** en tablas de la BD (solo en variables tabla locales). |
| **Manejo de Errores** | Soporta `TRY...CATCH`. | No soporta `TRY...CATCH`. |
| **Transacciones** | Puede iniciar y confirmar transacciones. | No puede gestionar transacciones. |
| **Uso Típico** | Modificar datos, procesos batch, lógica compleja. | Cálculos, transformaciones, filtros reutilizables. |

-----

## Checklist para Aprobar el Parcial

  * [ ] ¿Sé diferenciar cuándo usar un SP (acción) y una UDF (cálculo)?
  * [ ] ¿Entiendo que las funciones **no** pueden tener "side effects" (modificar tablas)?
  * [ ] ¿Conozco la sintaxis `CREATE PROCEDURE` vs `CREATE FUNCTION`?
  * [ ] ¿Sé qué es un parámetro `OUTPUT` en un SP?
  * [ ] ¿Puedo explicar la diferencia entre una función escalar y una de tabla?

-----

## Cuestionario de Preparación (15 Preguntas)

### Sección 1: Selección Múltiple (Solo una correcta)

**1. ¿Cuál de las siguientes afirmaciones es la razón principal para usar una Función Definida por el Usuario (UDF) en lugar de un Procedimiento Almacenado?**
A) Necesitas modificar datos en tres tablas diferentes.
B) Quieres manejar errores con `TRY...CATCH`.
C) Necesitas usar el resultado de la rutina dentro de una cláusula `WHERE` o `SELECT`.
D) Quieres devolver múltiples conjuntos de resultados (Result Sets).

**2. ¿Cómo se ejecuta un Procedimiento Almacenado en SQL Server?**
A) `SELECT * FROM NombreProcedimiento`
B) `CALL NombreProcedimiento`
C) `EXECUTE NombreProcedimiento`
D) `RUN NombreProcedimiento`

**3. ¿Qué sentencia SQL está estrictamente PROHIBIDA dentro de una Función Definida por el Usuario estándar?**
A) `DECLARE`
B) `SELECT`
C) `UPDATE` (sobre una tabla física de la base de datos)
D) `WHILE`

**4. Un Procedimiento Almacenado puede devolver valores al programa que lo llamó mediante:**
A) Únicamente la sentencia `RETURN`.
B) Conjuntos de resultados (Selects), parámetros `OUTPUT` y códigos de retorno.
C) Solo modificando tablas.
D) Las funciones del sistema.

**5. ¿Qué tipo de función devuelve una tabla resultante de una sola sentencia SELECT y no tiene cuerpo `BEGIN...END`?**
A) Función Escalar.
B) Función de Tabla Multisentencia.
C) Función de Tabla en Línea (Inline Table-Valued).
D) Procedimiento Almacenado Extendido.

**6. ¿Cuál es el propósito del comando `SET NOCOUNT ON` en un Procedimiento Almacenado?**
A) Evitar que el procedimiento devuelva datos.
B) Mejorar el rendimiento evitando el mensaje de red sobre el número de filas afectadas.
C) Impedir que cuente las filas de la tabla.
D) Es obligatorio para que el SP compile.

**7. Con respecto al manejo de errores, ¿cuál es la diferencia clave?**
A) Las funciones usan `TRY...CATCH` y los procedimientos no.
B) Los procedimientos usan `TRY...CATCH` y las funciones no.
C) Ambos soportan `TRY...CATCH`.
D) Ninguno soporta manejo de errores nativo.

**8. ¿Puede un Procedimiento Almacenado llamar a una Función?**
A) Sí, siempre.
B) No, nunca.
C) Solo si la función es escalar.
D) Solo si el procedimiento es del sistema.

**9. ¿Qué devuelve la sentencia `RETURN` dentro de un Procedimiento Almacenado?**
A) Una tabla.
B) Un valor de cualquier tipo de dato.
C) Un valor entero (Integer), usado típicamente para el estado.
D) El último registro insertado.

**10. ¿Qué ventaja de seguridad ofrecen los Procedimientos Almacenados?**
A) Encriptan toda la base de datos.
B) Permiten dar permiso de `EXECUTE` al usuario sin darle acceso directo (`SELECT/INSERT`) a las tablas base.
C) Eliminan la necesidad de contraseñas.
D) Evitan que se borren los logs.

-----

### Sección 2: Verdadero o Falso

**11.** Es posible invocar un Procedimiento Almacenado dentro de una sentencia `SELECT` (ej: `SELECT * FROM dbo.MiStoredProc`).
**( ) Verdadero / ( ) Falso**

**12.** Una Función de Tabla Multisentencia permite declarar una variable tipo `@tabla`, llenarla con datos usando lógica compleja y devolverla al final.
**( ) Verdadero / ( ) Falso**

**13.** Los procedimientos almacenados se compilan y su plan de ejecución se almacena en caché para mejorar el rendimiento en futuras ejecuciones.
**( ) Verdadero / ( ) Falso**

**14.** Una función escalar puede devolver múltiples valores si se separan por comas.
**( ) Verdadero / ( ) Falso**

**15.** Se pueden usar parámetros de salida (`OUTPUT`) en las Funciones Definidas por el Usuario.
**( ) Verdadero / ( ) Falso**

-----

## Soluciones y Explicaciones

### Respuestas Selección Múltiple

1.  **C**. Las funciones están diseñadas para integrarse en expresiones SQL. Los SPs no pueden usarse en un `WHERE` o `SELECT`.
2.  **C**. La sintaxis correcta en T-SQL es `EXEC` o `EXECUTE`. `CALL` es de otros motores como MySQL/Oracle.
3.  **C**. Las funciones no pueden tener efectos secundarios en la base de datos (no pueden alterar tablas permanentes).
4.  **B**. Los SPs son muy versátiles en el retorno de información.
5.  **C**. Las *Inline Table-Valued Functions* son básicamente un `SELECT` parametrizado y son muy eficientes.
6.  **B**. Reduce el tráfico de red ("ruido") y mejora levemente el rendimiento en procesos masivos.
7.  **B**. Limitación técnica importante: no puedes usar bloques `TRY...CATCH` dentro de una UDF.
8.  **A**. Los SPs pueden usar funciones libremente. Lo inverso (Función llamando a SP) no es posible normalmente.
9.  **C**. En SPs, `RETURN` es estrictamente para códigos de estado numéricos (ej. 0=OK, -1=Error).
10. **B**. Se conoce como **encapsulamiento de seguridad**. El usuario solo necesita permiso para ejecutar el SP, no para tocar las tablas.

### Respuestas Verdadero/Falso

11. **Falso**. Los SPs se ejecutan con `EXEC`. No pueden ser parte de un `SELECT`.
12. **Verdadero**. Es la característica principal de las funciones multisentencia (aunque suelen ser más lentas que las en línea).
13. **Verdadero**. Es una de las grandes ventajas de rendimiento (Plan Caching).
14. **Falso**. "Escalar" significa, por definición, un único valor atómico.
15. **Falso**. Los parámetros `OUTPUT` son exclusivos de los Procedimientos Almacenados. Las funciones solo retornan su valor definido en `RETURNS`.

