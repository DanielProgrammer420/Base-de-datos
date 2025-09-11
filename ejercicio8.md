¡Por supuesto! Este ejercicio es la culminación de todos los conceptos de diseño lógico que han estado viendo: normalización, entidades de catálogo, múltiples relaciones N:M, y la importantísima restricción de un evento transaccional contra una relación de configuración.

Basado en el análisis del ejercicio y las mejores prácticas discutidas en clase, aquí tienes la resolución completa del Ejercicio 8 en un script de SQL Server, tal como lo esperarían los profesores.

A continuación del script, encontrarás una explicación detallada de las decisiones de diseño clave.

---

### **El Script SQL Completo (Solución Ejercicio 8)**

```sql
-- =============================================================================
-- Script de Creación: Base de Datos de Transporte (Ejercicio 8 - Serie 3)
-- Curso: Bases de Datos I
-- Descripción: Este script crea la estructura de tablas y restricciones para
-- gestionar los viajes de una empresa de transporte, implementando una
-- restricción de asignación de chofer-camión en la tabla de viajes.
-- =============================================================================

-- =============================================================================
-- SECCIÓN 1: CREACIÓN Y USO DE LA BASE DE DATOS
-- =============================================================================
-- Si la base de datos ya existe, la elimina para una ejecución limpia
IF DB_ID('transporte_db') IS NOT NULL
    DROP DATABASE transporte_db;
GO

-- Crea la nueva base de datos
CREATE DATABASE transporte_db;
GO

-- Selecciona la base de datos recién creada para trabajar en ella
USE transporte_db;
GO

-- =============================================================================
-- SECCIÓN 2: CREACIÓN DE TABLAS DE CATÁLOGO Y JERARQUÍA GEOGRÁFICA (DDL)
-- =============================================================================

CREATE TABLE Paises (
    id_pais INT IDENTITY(1,1),
    nom_pais VARCHAR(100) NOT NULL,
    CONSTRAINT PK_Paises PRIMARY KEY (id_pais)
);
GO

CREATE TABLE Provincias (
    id_provincia INT IDENTITY(1,1),
    nom_provincia VARCHAR(100) NOT NULL,
    id_pais INT NOT NULL,
    CONSTRAINT PK_Provincias PRIMARY KEY (id_provincia)
);
GO

CREATE TABLE Localidad (
    id_localidad INT IDENTITY(1,1),
    nom_localidad VARCHAR(100) NOT NULL,
    id_provincia INT NOT NULL,
    CONSTRAINT PK_Localidad PRIMARY KEY (id_localidad)
);
GO

CREATE TABLE Sucursal (
    id_sucursal INT IDENTITY(1,1),
    nom_sucursal VARCHAR(100) NOT NULL,
    id_localidad INT NOT NULL,
    CONSTRAINT PK_Sucursal PRIMARY KEY (id_sucursal)
);
GO

CREATE TABLE Categoria_licencia (
    id_categoria INT IDENTITY(1,1),
    nom_categoria VARCHAR(50) NOT NULL,
    CONSTRAINT PK_Categoria_licencia PRIMARY KEY (id_categoria)
);
GO

CREATE TABLE Materiales (
    id_material INT IDENTITY(1,1),
    nom_material VARCHAR(100) NOT NULL,
    CONSTRAINT PK_Materiales PRIMARY KEY (id_material)
);
GO

-- =============================================================================
-- SECCIÓN 3: CREACIÓN DE TABLAS DE ENTIDADES PRINCIPALES (DDL)
-- =============================================================================

CREATE TABLE Camion (
    id_camion INT IDENTITY(1,1),
    patente VARCHAR(10) NOT NULL,
    capacidad_max_tn DECIMAL(10, 2) NOT NULL,
    -- El kilometraje total es un dato DERIVADO (se calcula sumando los viajes), no se almacena.
    CONSTRAINT PK_Camion PRIMARY KEY (id_camion),
    CONSTRAINT UQ_Camion_Patente UNIQUE (patente)
);
GO

CREATE TABLE Chofer (
    id_chofer INT IDENTITY(1,1),
    dni VARCHAR(20) NOT NULL,
    nombre VARCHAR(100) NOT NULL,
    apellido VARCHAR(100) NOT NULL,
    id_sucursal INT NOT NULL,
    id_categoria INT NOT NULL,
    CONSTRAINT PK_Chofer PRIMARY KEY (id_chofer),
    CONSTRAINT UQ_Chofer_DNI UNIQUE (dni)
);
GO

-- =============================================================================
-- SECCIÓN 4: CREACIÓN DE TABLAS INTERMEDIAS (RELACIONES N:M)
-- =============================================================================

-- Define qué materiales puede transportar cada camión
CREATE TABLE Camion_Materiales (
    id_camion INT NOT NULL,
    id_material INT NOT NULL,
    CONSTRAINT PK_Camion_Materiales PRIMARY KEY (id_camion, id_material)
);
GO

-- Define qué choferes están asignados a qué camiones
CREATE TABLE Chofer_Camion (
    id_chofer INT NOT NULL,
    id_camion INT NOT NULL,
    CONSTRAINT PK_Chofer_Camion PRIMARY KEY (id_chofer, id_camion)
);
GO

-- =============================================================================
-- SECCIÓN 5: CREACIÓN DE LA TABLA DE EVENTOS (VIAJE)
-- Esta es la tabla central que registra cada transacción de viaje.
-- =============================================================================
CREATE TABLE Viaje (
    id_viaje INT IDENTITY(1,1),
    distancia_recorrida_km DECIMAL(10, 2) NOT NULL,
    duracion_dias INT NOT NULL,
    peso_transportado_tn DECIMAL(10, 2) NOT NULL,
    id_material INT NOT NULL,
    id_sucursal_origen INT NOT NULL,
    id_sucursal_destino INT NOT NULL,
    
    -- Claves foráneas que referencian la ASIGNACIÓN VÁLIDA (la restricción clave)
    id_chofer INT NOT NULL,
    id_camion INT NOT NULL,

    CONSTRAINT PK_Viaje PRIMARY KEY (id_viaje)
);
GO

-- =============================================================================
-- SECCIÓN 6: DEFINICIÓN DE CLAVES FORÁNEAS Y RESTRICCIONES (DDL - ALTER TABLE)
-- =============================================================================

-- Jerarquía Geográfica
ALTER TABLE Provincias ADD CONSTRAINT FK_Provincias_Paises FOREIGN KEY (id_pais) REFERENCES Paises(id_pais);
ALTER TABLE Localidad ADD CONSTRAINT FK_Localidad_Provincias FOREIGN KEY (id_provincia) REFERENCES Provincias(id_provincia);
ALTER TABLE Sucursal ADD CONSTRAINT FK_Sucursal_Localidad FOREIGN KEY (id_localidad) REFERENCES Localidad(id_localidad);

-- Relaciones de Chofer
ALTER TABLE Chofer ADD CONSTRAINT FK_Chofer_Sucursal FOREIGN KEY (id_sucursal) REFERENCES Sucursal(id_sucursal);
ALTER TABLE Chofer ADD CONSTRAINT FK_Chofer_CategoriaLicencia FOREIGN KEY (id_categoria) REFERENCES Categoria_licencia(id_categoria);

-- Relaciones de Tablas Intermedias
ALTER TABLE Camion_Materiales ADD CONSTRAINT FK_CamionMateriales_Camion FOREIGN KEY (id_camion) REFERENCES Camion(id_camion);
ALTER TABLE Camion_Materiales ADD CONSTRAINT FK_CamionMateriales_Materiales FOREIGN KEY (id_material) REFERENCES Materiales(id_material);
ALTER TABLE Chofer_Camion ADD CONSTRAINT FK_ChoferCamion_Chofer FOREIGN KEY (id_chofer) REFERENCES Chofer(id_chofer);
ALTER TABLE Chofer_Camion ADD CONSTRAINT FK_ChoferCamion_Camion FOREIGN KEY (id_camion) REFERENCES Camion(id_camion);

-- Relaciones de la Tabla de Viaje (la más importante)
ALTER TABLE Viaje ADD CONSTRAINT FK_Viaje_Material FOREIGN KEY (id_material) REFERENCES Materiales(id_material);
ALTER TABLE Viaje ADD CONSTRAINT FK_Viaje_SucursalOrigen FOREIGN KEY (id_sucursal_origen) REFERENCES Sucursal(id_sucursal);
ALTER TABLE Viaje ADD CONSTRAINT FK_Viaje_SucursalDestino FOREIGN KEY (id_sucursal_destino) REFERENCES Sucursal(id_sucursal);

-- !! LA RESTRICCIÓN CLAVE !!
-- Se crea una FK compuesta en Viaje que apunta a la PK compuesta de Chofer_Camion.
-- Esto garantiza que un viaje solo puede ser realizado por una asignación válida.
ALTER TABLE Viaje ADD CONSTRAINT FK_Viaje_ChoferCamion FOREIGN KEY (id_chofer, id_camion) REFERENCES Chofer_Camion(id_chofer, id_camion);

-- Restricciones CHECK
ALTER TABLE Camion ADD CONSTRAINT CHK_Camion_CapacidadMax CHECK (capacidad_max_tn > 0);
ALTER TABLE Viaje ADD CONSTRAINT CHK_Viaje_Distancia CHECK (distancia_recorrida_km > 0);
ALTER TABLE Viaje ADD CONSTRAINT CHK_Viaje_Peso CHECK (peso_transportado_tn > 0);
ALTER TABLE Viaje ADD CONSTRAINT CHK_Viaje_SucursalesDistintas CHECK (id_sucursal_origen <> id_sucursal_destino);
GO

-- =============================================================================
-- SECCIÓN 7: PRUEBAS DE VALIDACIÓN (DML - CASO INVÁLIDO)
-- =============================================================================
PRINT 'Insertando datos para la prueba...';
-- Insertar un camión, un chofer, y una asignación válida para ellos
INSERT INTO Camion (patente, capacidad_max_tn) VALUES ('AA123BB', 25.5);
INSERT INTO Paises (nom_pais) VALUES ('Argentina');
INSERT INTO Provincias (nom_provincia, id_pais) VALUES ('Corrientes', 1);
INSERT INTO Localidad (nom_localidad, id_provincia) VALUES ('Corrientes Capital', 1);
INSERT INTO Sucursal (nom_sucursal, id_localidad) VALUES ('Sucursal Central', 1);
INSERT INTO Categoria_licencia (nom_categoria) VALUES ('Cargas Peligrosas');
INSERT INTO Chofer (dni, nombre, apellido, id_sucursal, id_categoria) VALUES ('12345678', 'Juan', 'Perez', 1, 1);
INSERT INTO Chofer_Camion (id_chofer, id_camion) VALUES (1, 1); -- Asignación válida
INSERT INTO Materiales (nom_material) VALUES ('Arena');
GO

PRINT 'Probando la restricción principal (esta inserción debe fallar)...';
-- Intentar registrar un viaje con un chofer y camión que existen,
-- pero cuya combinación NO es una asignación válida.
-- (Chofer 1 NO está asignado al Camión 2, asumimos que existe un camión 2)
BEGIN TRY
    -- Primero creamos un chofer y camión no relacionados para la prueba
    INSERT INTO Camion (patente, capacidad_max_tn) VALUES ('CC456DD', 30.0);
    INSERT INTO Chofer (dni, nombre, apellido, id_sucursal, id_categoria) VALUES ('87654321', 'Carlos', 'Gomez', 1, 1);

    -- Ahora el intento de inserción inválida
    INSERT INTO Viaje (distancia_recorrida_km, duracion_dias, peso_transportado_tn, id_material, id_sucursal_origen, id_sucursal_destino, id_chofer, id_camion)
    VALUES (500, 2, 20, 1, 1, 1, 1, 2); -- Error: Chofer 1 no está asignado a Camión 2
END TRY
BEGIN CATCH
    PRINT '-> ÉXITO: Falló la inserción por FK_Viaje_ChoferCamion, como se esperaba. La base de datos impidió un viaje con una asignación inválida.';
END CATCH;
GO
```

---

### **Explicación Paso a Paso (El "Porqué" de cada Decisión)**

1.  **Secciones 1, 2 y 3 (Estructura Básica):** Se crean todas las tablas de catálogo y las entidades principales. `Camion` no tiene `kilometraje` porque es un dato **derivado** que se calcula sumando las distancias de sus viajes.

2.  **Sección 4 (Tablas Intermedias):** Se crean las dos tablas que definen las **reglas de negocio estáticas**:
    *   `Camion_Materiales`: Define el "menú" de qué camión puede llevar qué material.
    *   `Chofer_Camion`: Define las "asignaciones válidas" de qué chofer puede conducir qué camión.
    Ambas tienen una **clave primaria compuesta** por las claves foráneas de las tablas que conectan, garantizando la unicidad de cada relación.

3.  **Sección 5 (Tabla `Viaje`):** Esta es la tabla del **evento**.
    *   No contiene claves foráneas directas a `Chofer` y `Camion` por separado.
    *   Contiene una **clave foránea compuesta `(id_chofer, id_camion)`**. Esta es la decisión de diseño más importante del ejercicio.

4.  **Sección 6 (Restricciones):**
    *   **LA RESTRICCIÓN CLAVE:** La línea `FK_Viaje_ChoferCamion` es la que implementa la regla de negocio. Le dice a la base de datos: "Antes de aceptar un nuevo viaje, asegúrate de que la combinación de chofer y camión que te están dando **exista como una clave primaria válida** en la tabla `Chofer_Camion`".
    *   Esto hace **físicamente imposible** registrar un viaje con una asignación inválida, moviendo la lógica de negocio a la propia estructura de la base de datos.
    *   `CHK_Viaje_SucursalesDistintas`: Es una restricción de `CHECK` simple pero importante que añade coherencia, asegurando que un viaje no puede tener el mismo origen y destino.

5.  **Sección 7 (Pruebas):** La prueba final demuestra el poder del diseño. Intenta insertar un viaje con una combinación de chofer y camión que, aunque existen por separado, no forman una asignación válida. La base de datos, gracias a la `FK_Viaje_ChoferCamion`, **rechaza la inserción**, protegiendo la integridad de los datos.
