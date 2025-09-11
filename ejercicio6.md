¡Por supuesto! Basado en el análisis de las transcripciones de las clases y las mejores prácticas discutidas, aquí tienes la resolución completa del Ejercicio 6 en un script de SQL Server, tal como lo esperarían los profesores.

A continuación del script, encontrarás una explicación detallada de cada sección, justificando por qué se tomó cada decisión de diseño.

---

### **El Script SQL Completo (Solución Ejercicio 6)**

```sql
-- =============================================================================
-- Script de Creación: Base de Datos para Spotify (Ejercicio 6 - Serie 3)
-- Curso: Bases de Datos I
-- Descripción: Este script crea la estructura de tablas y restricciones 
-- para gestionar el historial de suscripciones de usuarios, siguiendo las
-- buenas prácticas de diseño físico y normalización.
-- =============================================================================

-- =============================================================================
-- SECCIÓN 1: CREACIÓN Y USO DE LA BASE DE DATOS
-- Siempre se debe crear y seleccionar la base de datos para no trabajar en 'master'.
-- =============================================================================
-- Si la base de datos ya existe, la elimina para una ejecución limpia
IF DB_ID('spotify_db') IS NOT NULL
    DROP DATABASE spotify_db;
GO

-- Crea la nueva base de datos
CREATE DATABASE spotify_db;
GO

-- Selecciona la base de datos recién creada para trabajar en ella
USE spotify_db;
GO

-- =============================================================================
-- SECCIÓN 2: CREACIÓN DE TABLAS (DDL)
-- Se crean primero las tablas de catálogo o maestras (Usuario, Tipo_Suscripcion)
-- y luego las tablas que dependen de ellas (Historial_Suscripcion).
-- =============================================================================

-- Tabla para almacenar la información de los usuarios
CREATE TABLE Usuario (
    id_usuario INT IDENTITY(1,1), -- Clave sustituta autoincremental
    nombre_usuario VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL,
    fecha_registro DATE NOT NULL,

    -- Definición de la Clave Primaria con un nombre descriptivo
    CONSTRAINT PK_Usuario PRIMARY KEY (id_usuario),

    -- Definición de restricciones de Unicidad con nombres descriptivos
    CONSTRAINT UQ_Usuario_NombreUsuario UNIQUE (nombre_usuario),
    CONSTRAINT UQ_Usuario_Email UNIQUE (email)
);
GO

-- Tabla de catálogo para los diferentes planes de suscripción
CREATE TABLE Tipo_Suscripcion (
    id_tipo_suscripcion INT IDENTITY(1,1),
    descripcion VARCHAR(100) NOT NULL,
    costo DECIMAL(10, 2) NOT NULL,

    -- Definición de la Clave Primaria
    CONSTRAINT PK_Tipo_Suscripcion PRIMARY KEY (id_tipo_suscripcion)
);
GO

-- Tabla para registrar el historial de cambios de suscripción de cada usuario
-- Esta es una entidad asociativa que se comporta como una entidad débil.
CREATE TABLE Historial_Suscripcion (
    id_usuario INT NOT NULL,     -- Parte de la PK, y FK a Usuario
    fecha_inicio DATE NOT NULL,  -- Parte de la PK, define el inicio de la vigencia
    fecha_fin DATE NULL,         -- NULL significa que es la suscripción activa
    id_tipo_suscripcion INT NOT NULL, -- FK a Tipo_Suscripcion

    -- Definición de la CLAVE PRIMARIA COMPUESTA (El punto más importante)
    -- Garantiza que un usuario no puede iniciar dos suscripciones en la misma fecha.
    CONSTRAINT PK_Historial_Suscripcion PRIMARY KEY (id_usuario, fecha_inicio)
);
GO

-- =============================================================================
-- SECCIÓN 3: CREACIÓN DE CLAVES FORÁNEAS (DDL - ALTER TABLE)
-- Se definen las relaciones entre las tablas después de su creación.
-- Esto mejora la organización y es útil para cargas masivas de datos.
-- =============================================================================

ALTER TABLE Historial_Suscripcion
ADD CONSTRAINT FK_Historial_Usuario
FOREIGN KEY (id_usuario) REFERENCES Usuario(id_usuario);
GO

ALTER TABLE Historial_Suscripcion
ADD CONSTRAINT FK_Historial_TipoSuscripcion
FOREIGN KEY (id_tipo_suscripcion) REFERENCES Tipo_Suscripcion(id_tipo_suscripcion);
GO

-- =============================================================================
-- SECCIÓN 4: CREACIÓN DE RESTRICCIONES DE LÓGICA DE NEGOCIO (CHECK)
-- Se añaden validaciones para asegurar la coherencia de los datos.
-- =============================================================================

ALTER TABLE Tipo_Suscripcion
ADD CONSTRAINT CHK_TipoSuscripcion_Costo
CHECK (costo >= 0);
GO

ALTER TABLE Historial_Suscripcion
ADD CONSTRAINT CHK_Historial_FechasLogicas
CHECK (fecha_fin IS NULL OR fecha_fin > fecha_inicio);
GO

-- =============================================================================
-- SECCIÓN 5: INSERCIÓN DE DATOS DE PRUEBA (DML - CASOS VÁLIDOS)
-- Se insertan datos para demostrar que el modelo funciona.
-- =============================================================================
PRINT 'Insertando datos de prueba válidos...';

-- Insertar tipos de suscripción
INSERT INTO Tipo_Suscripcion (descripcion, costo) VALUES
('Free', 0.00),
('Premium', 9.99),
('Family', 14.99);

-- Insertar usuarios
INSERT INTO Usuario (nombre_usuario, email, fecha_registro) VALUES
('juanperez', 'juan.perez@email.com', '2023-01-10'),
('anamartinez', 'ana.martinez@email.com', '2023-02-05');

-- Simular historial para Juan Pérez
-- 1. Se registra con el plan Free
INSERT INTO Historial_Suscripcion (id_usuario, fecha_inicio, fecha_fin, id_tipo_suscripcion)
VALUES (1, '2023-01-10', '2023-03-15', 1); -- Plan Free (ID=1)

-- 2. Cambia al plan Premium
INSERT INTO Historial_Suscripcion (id_usuario, fecha_inicio, fecha_fin, id_tipo_suscripcion)
VALUES (1, '2023-03-16', NULL, 2); -- Plan Premium (ID=2) -> NULL en fecha_fin indica que es el plan actual

-- Simular historial para Ana Martínez
-- 1. Se registra directamente con el plan Family
INSERT INTO Historial_Suscripcion (id_usuario, fecha_inicio, fecha_fin, id_tipo_suscripcion)
VALUES (2, '2023-02-05', NULL, 3); -- Plan Family (ID=3)
GO

-- =============================================================================
-- SECCIÓN 6: PRUEBAS DE VALIDACIÓN DE RESTRICCIONES (DML - CASOS INVÁLIDOS)
-- Se intentan insertar datos que violan las reglas para demostrar que los
-- constraints funcionan correctamente. Cada una de estas sentencias debe fallar.
-- =============================================================================
PRINT 'Probando restricciones (estas inserciones deben fallar)...';

-- Prueba 1: Intentar insertar un usuario con un email duplicado
BEGIN TRY
    INSERT INTO Usuario (nombre_usuario, email, fecha_registro) VALUES ('jperez2', 'juan.perez@email.com', '2023-05-01');
END TRY
BEGIN CATCH
    PRINT '-> ÉXITO: Falló la inserción por UQ_Usuario_Email, como se esperaba.';
END CATCH;
GO

-- Prueba 2: Intentar insertar un registro de historial con una fecha de inicio que ya existe para ese usuario
BEGIN TRY
    INSERT INTO Historial_Suscripcion (id_usuario, fecha_inicio, fecha_fin, id_tipo_suscripcion) VALUES (1, '2023-03-16', NULL, 3);
END TRY
BEGIN CATCH
    PRINT '-> ÉXITO: Falló la inserción por PK_Historial_Suscripcion, como se esperaba.';
END CATCH;
GO

-- Prueba 3: Intentar insertar un registro de historial con una fecha de fin anterior a la de inicio
BEGIN TRY
    INSERT INTO Historial_Suscripcion (id_usuario, fecha_inicio, fecha_fin, id_tipo_suscripcion) VALUES (2, '2024-01-01', '2023-12-31', 2);
END TRY
BEGIN CATCH
    PRINT '-> ÉXITO: Falló la inserción por CHK_Historial_FechasLogicas, como se esperaba.';
END CATCH;
GO
```

---

### **Explicación Paso a Paso (El "Porqué" de cada Decisión)**

1.  **Sección 1 (Creación de la BD):** Es una buena práctica fundamental. Asegura que tu script sea auto-contenido y no modifique por error otras bases de datos como `master`.

2.  **Sección 2 (Creación de Tablas):**
    *   **`Usuario` y `Tipo_Suscripcion`:** Son las entidades "maestras" o de catálogo. Se crean primero porque `Historial_Suscripcion` dependerá de ellas.
    *   **`IDENTITY(1,1)`:** Se utiliza para que el motor de base de datos gestione automáticamente las claves primarias (`id_usuario`, `id_tipo_suscripcion`), asegurando su unicidad sin intervención manual.
    *   **`Historial_Suscripcion` - La Clave del Diseño:**
        *   **No tiene un `id_historial`:** Como se discutió en clase, usar una clave artificial aquí es una mala práctica porque no garantiza la lógica del negocio.
        *   **Clave Primaria Compuesta `(id_usuario, fecha_inicio)`:** Esta es la decisión de diseño más importante. Define lo que hace único a un registro de historial: la combinación de **quién** (`id_usuario`) y **cuándo** (`fecha_inicio`) comenzó el plan. Esto **físicamente impide** que un usuario inicie dos planes en la misma fecha, haciendo que la base de datos sea "inteligente".
        *   **`fecha_fin DATE NULL`:** Es crucial que esta columna permita valores nulos. Un `NULL` en esta columna es la forma estándar de indicar cuál es la **suscripción actualmente activa** del usuario.

3.  **Sección 3 (Claves Foráneas):**
    *   Se definen con `ALTER TABLE` por separado. Esto hace el código más legible y es una práctica común en entornos profesionales, especialmente antes de cargas masivas de datos.
    *   **Nomenclatura `FK_TablaOrigen_TablaDestino`:** Se usa un nombre descriptivo para la restricción, como pidieron los profesores, para que los mensajes de error sean claros.

4.  **Sección 4 (Restricciones `CHECK`):**
    *   **`CHK_TipoSuscripcion_Costo`:** Una validación simple para asegurar que no se puedan registrar costos negativos.
    *   **`CHK_Historial_FechasLogicas`:** Una restricción de lógica de negocio fundamental. Asegura que un período de suscripción no puede terminar antes de haber comenzado. El `fecha_fin IS NULL OR ...` es necesario para permitir la fila de la suscripción activa.

5.  **Secciones 5 y 6 (Pruebas):**
    *   Estas secciones son lo que diferencia un simple script de una solución completa. Demuestran que no solo has escrito el código, sino que has **verificado que funciona** y que las **restricciones se comportan como esperas**. Incluir pruebas de casos que deben fallar es una marca de profesionalismo.
