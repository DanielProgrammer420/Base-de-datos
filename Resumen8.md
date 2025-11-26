Aquí tienes el material de estudio definitivo sobre **Optimización de consultas a través de índices**, diseñado para dominar la teoría y la práctica en SQL Server de cara a tu examen parcial.

-----

# Optimización de Consultas a través de Índices

## Resumen Maestro Completo

### 1\. Definiciones y Propósito

Un **Índice** en SQL Server es una estructura de datos en disco (generalmente un Árbol-B o **B-Tree**) asociada a una tabla o vista, que acelera la recuperación de filas.

  * **Analogía Clásica:** Un índice es como el índice alfabético al final de un libro. Sin él, para encontrar el tema "Transacciones", tendrías que leer el libro página por página (**Table Scan**). Con él, vas directo a la página exacta (**Index Seek**).
  * **Propósito:** El objetivo principal es reducir las operaciones de Entrada/Salida (I/O) al minimizar la cantidad de datos que el motor debe leer para satisfacer una consulta.

### 2\. Conceptos Fundamentales

#### A. Estructura B-Tree (Árbol Balanceado)

Los índices se organizan en páginas de 8KB en una estructura jerárquica:

1.  **Root Node (Raíz):** El punto de entrada.
2.  **Intermediate Levels:** Nodos de navegación.
3.  **Leaf Nodes (Hojas):** El nivel inferior. Aquí es donde reside la diferencia crucial entre tipos de índices.

#### B. Tipos de Índices (La pregunta obligada de examen)

1.  **Índice Agrupado (Clustered Index):**

      * **Concepto:** Ordena **físicamente** los datos de la tabla según la columna(s) del índice.
      * **Regla:** Solo puede haber **1** por tabla (los datos no pueden estar ordenados físicamente de dos formas distintas a la vez).
      * **Nivel Hoja:** Las hojas del árbol *son* las páginas de datos reales.
      * **Nota:** Por defecto, al crear una `PRIMARY KEY`, SQL Server crea un índice Clustered, pero no es obligatorio que sea así.

2.  **Índice No Agrupado (Non-Clustered Index):**

      * **Concepto:** Es una estructura separada de los datos (como el índice al final del libro). Contiene el valor de la clave y un puntero hacia la fila real de datos.
      * **Regla:** Puede haber múltiples (hasta 999 en versiones recientes).
      * **Nivel Hoja:** Contiene punteros. Si la tabla tiene un Clustered Index, el puntero es la Clustered Key. Si es un Heap, es el RID (Row ID).

3.  **Heap (Montículo):**

      * Es una tabla **sin** índice Clustered. Los datos se almacenan desordenados, tal como van llegando.

#### C. Operaciones del Optimizador (Plan de Ejecución)

Debes conocer estos términos para interpretar si una consulta está optimizada:

  * **Table Scan:** Lectura completa de la tabla (Malo en tablas grandes).
  * **Clustered Index Scan:** Lectura completa del índice agrupado (Equivalente a Table Scan, suele ser lento).
  * **Index Seek:** El motor navega por el árbol y va directo al dato (Ideal, muy rápido).
  * **Key Lookup:** El índice Non-Clustered encontró la fila, pero faltan columnas solicitadas en el `SELECT`, así que debe saltar al índice Clustered para buscarlas. (Costoso).

### 3\. Reglas y Propiedades

  * **Selectividad y Cardinalidad:** Los índices son útiles en columnas con *alta cardinalidad* (muchos valores únicos, ej: DNI, Email). Son inútiles en columnas con *baja cardinalidad* (ej: Género: 'M'/'F'), ya que el motor preferirá escanear todo a usar el índice.
  * **Trade-off (Costo-Beneficio):**
      * Los índices **aceleran los SELECT**.
      * Los índices **ralentizan los INSERT, UPDATE, DELETE**. (Cada vez que escribes datos, SQL Server debe reordenar y actualizar los árboles de los índices).
  * **Left-most Prefix:** En índices compuestos (ej: Apellido, Nombre), el índice sirve si buscas por "Apellido" o por "Apellido + Nombre", pero **no** sirve si buscas solo por "Nombre".

-----

## Errores Comunes en Parciales

1.  **"Más índices es mejor":** Falso. El exceso de índices mata el rendimiento de escritura y ocupa espacio en disco.
2.  **Confundir Primary Key con Clustered Index:** Aunque suelen ir juntos por defecto, puedes tener una PK que sea *Non-Clustered* y un campo Fecha que sea *Clustered*.
3.  **Olvidar el `INCLUDE`:** Para evitar el costoso *Key Lookup*, se usa `INCLUDE` para agregar columnas al nivel hoja del índice sin que formen parte del ordenamiento del árbol. Esto crea un **Índice Cubierto (Covering Index)**.

-----

## Ejemplos en SQL Server

### Sintaxis y Optimización

```sql
-- ESCENARIO: Tabla de Empleados
CREATE TABLE Empleados (
    ID int IDENTITY(1,1),
    DNI varchar(20),
    Apellido varchar(50),
    Nombre varchar(50),
    DepartamentoID int,
    Salario decimal(10,2)
);

-- 1. Crear el CLUSTERED INDEX (Ordena físico)
-- Generalmente sobre la PK
CREATE UNIQUE CLUSTERED INDEX IX_Clustered_ID 
ON Empleados(ID);

-- 2. Crear un NON-CLUSTERED INDEX (Búsqueda rápida)
-- Optimizamos búsquedas por Apellido
CREATE NONCLUSTERED INDEX IX_NC_Apellido
ON Empleados(Apellido);

-- 3. PROBLEMA: Key Lookup
-- La siguiente consulta usa el índice de Apellido, pero pide 'Salario'.
-- El índice IX_NC_Apellido NO tiene el salario, debe ir a buscarlo al Clustered.
SELECT Apellido, Salario FROM Empleados WHERE Apellido = 'Gomez';

-- 4. SOLUCIÓN: COVERING INDEX con INCLUDE
-- Modificamos el índice para que incluya el salario en la hoja.
CREATE NONCLUSTERED INDEX IX_NC_Apellido_Includes
ON Empleados(Apellido)
INCLUDE (Salario);

-- Ahora la consulta anterior se resuelve 100% en este índice (Index Seek puro).
```

-----

## Tabla Comparativa: Clustered vs. Non-Clustered

| Característica | Índice Agrupado (Clustered) | Índice No Agrupado (Non-Clustered) |
| :--- | :--- | :--- |
| **Cantidad** | Máximo **1** por tabla. | Múltiples (hasta 999). |
| **Datos** | Las hojas **contienen** los datos reales. | Las hojas contienen **punteros** a los datos. |
| **Orden** | Ordena físicamente las filas en disco. | Orden lógico, independiente del físico. |
| **Espacio** | No requiere espacio extra (es la tabla). | Requiere espacio extra en disco. |
| **Velocidad** | El más rápido para recuperar rangos. | Rápido, pero puede requerir *Lookups*. |

-----

## Checklist para Aprobar el Parcial

  * [ ] ¿Entiendo que una tabla sin índice Clustered se llama **Heap**?
  * [ ] ¿Sé por qué solo puede haber **un** índice Clustered?
  * [ ] ¿Puedo explicar la diferencia entre **Seek** (Búsqueda directa) y **Scan** (Barrido secuencial)?
  * [ ] ¿Entiendo que los índices mejoran la lectura pero perjudican la escritura (DML)?
  * [ ] ¿Sé qué es la cardinalidad y cómo afecta la decisión de indexar?
  * [ ] ¿Comprendo el concepto de **Covering Index** (Índice Cubierto) y `INCLUDE`?

-----

## Cuestionario de Preparación (15 Preguntas)

### Sección 1: Selección Múltiple (Solo una correcta)

**1. ¿Cuál es el límite máximo de índices agrupados (Clustered) que puede tener una tabla en SQL Server?**
A) 999
B) 1
C) Tantos como columnas tenga la tabla.
D) 255

**2. ¿Cómo se denomina a una tabla que no tiene un índice agrupado?**
A) Stack
B) Queue
C) Heap (Montículo)
D) Tree

**3. ¿Qué impacto tienen los índices en las operaciones de modificación de datos (INSERT, UPDATE, DELETE)?**
A) Las aceleran considerablemente.
B) No tienen ningún impacto.
C) Las ralentizan, porque el motor debe actualizar la estructura del índice.
D) Solo afectan a los DELETE, no a los INSERT.

**4. En un plan de ejecución, ¿qué operador indica que el motor tuvo que leer toda la tabla o índice secuencialmente porque no pudo usar una búsqueda eficiente?**
A) Index Seek
B) Key Lookup
C) Nested Loops
D) Index/Table Scan

**5. ¿Qué es un "Covering Index" (Índice Cubierto)?**
A) Un índice que cubre todas las tablas de la base de datos.
B) Un índice que contiene todas las columnas solicitadas por una consulta, evitando accesos adicionales a la tabla base.
C) Un índice encriptado por seguridad.
D) Un índice que ocupa más del 50% del disco.

**6. Si ejecutas `SELECT * FROM Clientes WHERE Apellido = 'Perez'`, y tienes un índice Non-Clustered en `Apellido`, pero la tabla tiene 50 columnas más, ¿qué operación costosa realizará probablemente el motor?**
A) Table Scan
B) Key Lookup (o RID Lookup)
C) Hash Match
D) Merge Join

**7. ¿Para cuál de las siguientes columnas sería MENOS eficiente crear un índice?**
A) DNI (Valores únicos).
B) FechaTransaccion (Alta variabilidad).
C) EstadoCivil (Valores: 'S', 'C', 'V', 'D' - Baja cardinalidad).
D) Email (Valores únicos).

**8. ¿Qué cláusula se utiliza en `CREATE INDEX` para agregar columnas al nivel hoja del índice sin que formen parte de la clave de ordenamiento?**
A) `ADD`
B) `WITH`
C) `INCLUDE`
D) `ATTACH`

**9. ¿Qué estructura de datos utilizan internamente los índices en SQL Server?**
A) Listas enlazadas simples.
B) Árboles B (B-Trees).
C) Tablas Hash (por defecto).
D) Vectores.

**10. Tienes un índice compuesto en las columnas `(Pais, Ciudad, Calle)`. ¿Cuál de los siguientes `WHERE` NO aprovechará el índice eficientemente?**
A) `WHERE Pais = 'Argentina'`
B) `WHERE Pais = 'Argentina' AND Ciudad = 'Corrientes'`
C) `WHERE Ciudad = 'Rosario'`
D) `WHERE Pais = 'Argentina' AND Ciudad = 'Corrientes' AND Calle = 'San Martin'`

-----

### Sección 2: Verdadero o Falso

**11.** La Llave Primaria (Primary Key) debe ser obligatoriamente el índice Clustered de la tabla.
**( ) Verdadero / ( ) Falso**

**12.** Un Index Seek es generalmente más eficiente que un Index Scan cuando se buscan pocos registros.
**( ) Verdadero / ( ) Falso**

**13.** Los índices ocupan espacio físico adicional en el disco duro (excepto el Clustered que reorganiza los datos existentes).
**( ) Verdadero / ( ) Falso**

**14.** Si fragmentas mucho un índice (por muchos inserts/updates), el rendimiento de lectura mejora.
**( ) Verdadero / ( ) Falso**

**15.** Se puede crear un índice Non-Clustered sobre una Vista (Indexed View) para materializar sus resultados.
**( ) Verdadero / ( ) Falso**

-----

## Soluciones y Explicaciones

### Respuestas Selección Múltiple

1.  **B**. Por definición física, los datos solo pueden ordenarse de una manera.
2.  **C**. Heap. Los datos están "tirados" sin orden.
3.  **C**. Es el costo de mantenimiento ("overhead"). Cada cambio en datos implica reacomodar el árbol del índice.
4.  **D**. Scan significa "leer todo". Es lo opuesto a ir directo al punto.
5.  **B**. Es el escenario ideal de optimización. El motor saca todo del índice y no toca la tabla real.
6.  **B**. El índice tiene el Apellido, pero para traer el `*` (resto de columnas) debe saltar a buscar los datos faltantes.
7.  **C**. Baja cardinalidad. Si un índice devuelve el 25-30% de la tabla (ej. todos los 'Casados'), el motor prefiere hacer un Scan.
8.  **C**. `INCLUDE` permite agregar datos payload a las hojas del índice Non-Clustered.
9.  **B**. B-Trees permiten búsquedas logarítmicas rápidas.
10. **C**. Rompe la regla del **Left-most prefix**. El índice empieza por `Pais`. Si no especificas `Pais`, el índice no sirve para buscar `Ciudad` directamente.

### Respuestas Verdadero/Falso

11. **Falso**. Es el comportamiento *por defecto*, pero puedes definir una PK como Non-Clustered y poner el Clustered en otra columna (ej. Fecha).
12. **Verdadero**. Seek es quirúrgico; Scan es masivo.
13. **Verdadero**. Los Non-Clustered son estructuras redundantes, copias parciales de datos organizadas de otra forma.
14. **Falso**. La fragmentación dispersa las páginas en disco, aumentando el I/O y empeorando el rendimiento. Se debe hacer `REORGANIZE` o `REBUILD`.
15. **Verdadero**. Es una técnica avanzada para guardar físicamente el resultado de una vista compleja.

### Siguiente paso sugerido:

¿Te gustaría que analicemos **cómo leer un Plan de Ejecución** gráfico en SQL Server para identificar visualmente cuándo faltan índices?
