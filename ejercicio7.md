¡Por supuesto! Basado en el análisis del ejercicio y las mejores prácticas discutidas en clase (patrón cabecera-detalle, snapshot de precios, nombrado de constraints, etc.), aquí tienes la resolución completa del Ejercicio 7 en un script de SQL Server, tal como lo esperarían los profesores.

A continuación del script, encontrarás una explicación detallada de las decisiones de diseño clave para que comprendas el "porqué" de cada sección.

---

### **El Script SQL Completo (Solución Ejercicio 7)**

```sql
-- =============================================================================
-- Script de Creación: Base de Datos para Supermercado (Ejercicio 7 - Serie 3)
-- Curso: Bases de Datos I
-- Descripción: Este script crea la estructura de tablas y restricciones para
-- gestionar las ventas de un supermercado, implementando el patrón
-- cabecera-detalle y haciendo "snapshot" de datos transaccionales.
-- =============================================================================

-- =============================================================================
-- SECCIÓN 1: CREACIÓN Y USO DE LA BASE DE DATOS
-- =============================================================================
-- Si la base de datos ya existe, la elimina para una ejecución limpia
IF DB_ID('supermercado_db') IS NOT NULL
    DROP DATABASE supermercado_db;
GO

-- Crea la nueva base de datos
CREATE DATABASE supermercado_db;
GO

-- Selecciona la base de datos recién creada para trabajar en ella
USE supermercado_db;
GO

-- =============================================================================
-- SECCIÓN 2: CREACIÓN DE TABLAS DE CATÁLOGO (DDL)
-- Se crean primero las tablas maestras o de catálogo que son independientes
-- o tienen menos dependencias.
-- =============================================================================

CREATE TABLE Proveedor (
    id_proveedor INT IDENTITY(1,1),
    nombre VARCHAR(100) NOT NULL,
    CONSTRAINT PK_Proveedor PRIMARY KEY (id_proveedor)
);
GO

CREATE TABLE Rubro (
    id_rubro INT IDENTITY(1,1),
    descripcion VARCHAR(100) NOT NULL,
    CONSTRAINT PK_Rubro PRIMARY KEY (id_rubro)
);
GO

CREATE TABLE Metodo_Pago (
    id_metodo_pago INT IDENTITY(1,1),
    descripcion VARCHAR(100) NOT NULL,
    CONSTRAINT PK_Metodo_Pago PRIMARY KEY (id_metodo_pago)
);
GO

CREATE TABLE Vendedor (
    id_vendedor INT IDENTITY(1,1),
    nombre VARCHAR(100) NOT NULL,
    CONSTRAINT PK_Vendedor PRIMARY KEY (id_vendedor)
);
GO

-- =============================================================================
-- SECCIÓN 3: CREACIÓN DE TABLAS DEPENDIENTES Y TRANSACCIONALES (DDL)
-- =============================================================================

CREATE TABLE Articulo (
    id_articulo INT IDENTITY(1,1),
    descripcion VARCHAR(200) NOT NULL,
    stock INT NOT NULL,
    precio_unitario_actual DECIMAL(10, 2) NOT NULL,
    id_rubro INT NOT NULL,
    id_proveedor INT NOT NULL,

    CONSTRAINT PK_Articulo PRIMARY KEY (id_articulo),
    CONSTRAINT FK_Articulo_Rubro FOREIGN KEY (id_rubro) REFERENCES Rubro(id_rubro),
    CONSTRAINT FK_Articulo_Proveedor FOREIGN KEY (id_proveedor) REFERENCES Proveedor(id_proveedor)
);
GO

-- Tabla Cabecera de la transacción
CREATE TABLE Ticket (
    id_ticket INT IDENTITY(1,1),
    fecha DATETIME NOT NULL,
    id_vendedor INT NOT NULL,
    id_metodo_pago INT NOT NULL,
    
    -- El monto total del ticket es un valor calculado, no se almacena.
    -- Se obtiene sumando los subtotales de Detalle_Ticket.

    CONSTRAINT PK_Ticket PRIMARY KEY (id_ticket),
    CONSTRAINT FK_Ticket_Vendedor FOREIGN KEY (id_vendedor) REFERENCES Vendedor(id_vendedor),
    CONSTRAINT FK_Ticket_MetodoPago FOREIGN KEY (id_metodo_pago) REFERENCES Metodo_Pago(id_metodo_pago)
);
GO

-- Tabla Detalle de la transacción (Entidad Débil / Asociativa)
CREATE TABLE Detalle_Ticket (
    id_ticket INT NOT NULL,
    id_articulo INT NOT NULL,
    cantidad INT NOT NULL,
    precio_unitario_venta DECIMAL(10, 2) NOT NULL, -- Snapshot del precio al momento de la venta

    -- El subtotal de la línea (cantidad * precio) es un valor calculado, no se almacena.
    
    -- Clave Primaria Compuesta: Garantiza que un artículo no puede aparecer dos veces en el mismo ticket.
    CONSTRAINT PK_Detalle_Ticket PRIMARY KEY (id_ticket, id_articulo),
    CONSTRAINT FK_Detalle_Ticket_Ticket FOREIGN KEY (id_ticket) REFERENCES Ticket(id_ticket),
    CONSTRAINT FK_Detalle_Ticket_Articulo FOREIGN KEY (id_articulo) REFERENCES Articulo(id_articulo)
);
GO

-- =============================================================================
-- SECCIÓN 4: CREACIÓN DE RESTRICCIONES DE LÓGICA DE NEGOCIO (CHECK)
-- =============================================================================

ALTER TABLE Articulo
ADD CONSTRAINT CHK_Articulo_Stock
CHECK (stock >= 0);
GO

ALTER TABLE Articulo
ADD CONSTRAINT CHK_Articulo_Precio
CHECK (precio_unitario_actual > 0);
GO

ALTER TABLE Detalle_Ticket
ADD CONSTRAINT CHK_Detalle_Cantidad
CHECK (cantidad > 0);
GO

ALTER TABLE Detalle_Ticket
ADD CONSTRAINT CHK_Detalle_PrecioVenta
CHECK (precio_unitario_venta > 0);
GO

-- =============================================================================
-- SECCIÓN 5: INSERCIÓN DE DATOS DE PRUEBA (DML - CASOS VÁLIDOS)
-- =============================================================================
PRINT 'Insertando datos de prueba válidos...';

-- Catálogos
INSERT INTO Rubro (descripcion) VALUES ('Perfumería'), ('Lácteos'), ('Juguetería');
INSERT INTO Proveedor (nombre) VALUES ('Proveedor A'), ('Proveedor B');
INSERT INTO Metodo_Pago (descripcion) VALUES ('Contado'), ('Tarjeta de Crédito');
INSERT INTO Vendedor (nombre) VALUES ('Juan Pérez'), ('Ana Martínez');

-- Artículos
INSERT INTO Articulo (descripcion, stock, precio_unitario_actual, id_rubro, id_proveedor) VALUES
('Shampoo ABC', 100, 5.50, 1, 1),
('Leche XYZ', 200, 1.20, 2, 2),
('Auto de Juguete', 50, 15.00, 3, 1);

-- Simular un Ticket
-- 1. Crear la cabecera del Ticket
INSERT INTO Ticket (fecha, id_vendedor, id_metodo_pago) VALUES ('2025-09-08 10:30:00', 1, 2);
DECLARE @ultimo_ticket_id INT = SCOPE_IDENTITY(); -- Captura el ID del ticket recién creado

-- 2. Añadir las líneas de detalle a ese ticket
-- Se venden 2 shampoos al precio de ese momento (5.50)
INSERT INTO Detalle_Ticket (id_ticket, id_articulo, cantidad, precio_unitario_venta) VALUES (@ultimo_ticket_id, 1, 2, 5.50);
-- Se venden 5 leches al precio de ese momento (1.20)
INSERT INTO Detalle_Ticket (id_ticket, id_articulo, cantidad, precio_unitario_venta) VALUES (@ultimo_ticket_id, 2, 5, 1.20);
GO

-- =============================================================================
-- SECCIÓN 6: PRUEBAS DE VALIDACIÓN DE RESTRICCIONES (DML - CASOS INVÁLIDOS)
-- =============================================================================
PRINT 'Probando restricciones (estas inserciones deben fallar)...';

-- Prueba 1: Intentar insertar el mismo artículo dos veces en el mismo ticket
BEGIN TRY
    INSERT INTO Detalle_Ticket (id_ticket, id_articulo, cantidad, precio_unitario_venta) VALUES (1, 1, 1, 5.50);
END TRY
BEGIN CATCH
    PRINT '-> ÉXITO: Falló la inserción por PK_Detalle_Ticket, como se esperaba.';
END CATCH;
GO

-- Prueba 2: Intentar registrar una cantidad negativa en el detalle
BEGIN TRY
    INSERT INTO Detalle_Ticket (id_ticket, id_articulo, cantidad, precio_unitario_venta) VALUES (1, 3, -1, 15.00);
END TRY
BEGIN CATCH
    PRINT '-> ÉXITO: Falló la inserción por CHK_Detalle_Cantidad, como se esperaba.';
END CATCH;
GO
```

---

### **Explicación Paso a Paso (El "Porqué" de cada Decisión)**

1.  **Secciones 1 y 2 (Creación de BD y Catálogos):** Son buenas prácticas. Se crean primero las tablas independientes (`Proveedor`, `Rubro`, etc.) que actuarán como "listas de opciones" para las tablas más complejas.

2.  **Sección 3 (Tablas Dependientes y Transaccionales):**
    *   **`Articulo`:** Es una tabla maestra pero depende de `Rubro` y `Proveedor`, por eso se crea después de ellas. Contiene el `precio_unitario_actual`, que es el precio de lista que puede cambiar en cualquier momento.
    *   **`Ticket`:** Es la **cabecera** de la transacción. Contiene la información general que aplica a toda la venta: cuándo se hizo, quién la hizo y cómo se pagó. No contiene el `monto_total` porque, como se discutió en clase, es un valor **calculado/derivado**.
    *   **`Detalle_Ticket` - La Clave del Diseño:**
        *   **Clave Primaria Compuesta `(id_ticket, id_articulo)`:** Esta es la decisión de diseño más importante. Garantiza que un artículo solo puede aparecer **una vez** en cada ticket. Si un cliente compra 3 unidades del mismo artículo, se registra en una sola fila con `cantidad = 3`. Esto es típico de un sistema de supermercado.
        *   **`precio_unitario_venta` (El "Snapshot"):** Esta columna es **crucial**. No se usa el precio de la tabla `Articulo`. En su lugar, se **copia** el precio al momento de la venta en esta columna. Esto "congela" el precio de la transacción y asegura que si el precio del artículo cambia en el futuro, los informes de ventas pasadas seguirán siendo correctos.
        *   **No hay columna `total`:** El subtotal de la línea (`cantidad * precio_unitario_venta`) es un valor calculado y no debe almacenarse.

3.  **Sección 4 (Restricciones `CHECK`):** Se añaden reglas de negocio simples para garantizar la coherencia de los datos (stock no negativo, cantidades y precios positivos).

4.  **Secciones 5 y 6 (Pruebas):**
    *   Se muestra cómo se realizaría una transacción real: primero se inserta la cabecera en `Ticket`, se captura el `id_ticket` generado, y luego se usa ese ID para insertar las múltiples líneas en `Detalle_Ticket`.
    *   Se incluyen pruebas explícitas que intentan violar las restricciones para demostrar que el modelo es robusto y se protege a sí mismo de datos inconsistentes.
