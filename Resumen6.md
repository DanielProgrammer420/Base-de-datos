Aquí tienes el material de estudio completo, diseñado específicamente para garantizar la comprensión teórica y práctica del manejo de permisos en SQL Server, optimizado para un examen parcial universitario.

-----

# Manejo de Permisos en Bases de Datos (SQL Server)

## Resumen Maestro Completo

### 1\. Definiciones y Contexto

El **Manejo de Permisos** es el componente central de la seguridad en bases de datos que responde a la pregunta de **Autorización** (¿Qué puede hacer el usuario?), a diferencia de la **Autenticación** (¿Quién es el usuario?).

En el contexto del modelo relacional y SQL Server, los permisos se gestionan a través del sub-lenguaje **DCL (Data Control Language)**.

### 2\. Conceptos Fundamentales: La Tríada de Seguridad

Para entender cualquier permiso, debes visualizar la relación entre tres entidades:

1.  **Principal (Sujeto):** Es la entidad que solicita acceso.
      * **Login (Inicio de sesión):** A nivel de *Servidor*. Puede ser una cuenta de Windows o una cuenta de SQL Server.
      * **User (Usuario):** A nivel de *Base de Datos*. Un Login se mapea a un User para acceder a una BD específica.
      * **Role (Rol):** Un grupo de usuarios/logins que comparten los mismos privilegios.
2.  **Securable (Objeto protegible):** Es el recurso al que se quiere acceder.
      * Jerarquía: Servidor $\rightarrow$ Base de Datos $\rightarrow$ Esquema $\rightarrow$ Objeto (Tabla, Vista, SP) $\rightarrow$ Columna.
3.  **Permission (Permiso):** La acción que se desea realizar (ej. `SELECT`, `INSERT`, `ALTER`, `EXECUTE`).

### 3\. Los Comandos DCL (Reglas de Oro)

Existen tres comandos fundamentales que dictan el acceso. Es vital entender la diferencia precisa entre ellos para el examen:

  * **GRANT (Conceder):** Otorga un permiso positivo. Permite al *Principal* realizar una acción sobre el *Securable*.
      * *Opción:* `WITH GRANT OPTION` permite que ese usuario conceda el mismo permiso a otros.
  * **DENY (Denegar):** Prohíbe explícitamente un permiso.
      * **Regla de Oro:** Un `DENY` siempre tiene precedencia sobre un `GRANT`. Si un usuario pertenece a un grupo que tiene `GRANT` pero él tiene un `DENY` individual (o en otro grupo), el acceso será denegado.
  * **REVOKE (Revocar):** Elimina un permiso previamente otorgado (`GRANT`) o denegado (`DENY`).
      * **Importante:** `REVOKE` no es lo mismo que `DENY`. `REVOKE` devuelve al usuario a un estado "neutral" (ni sí, ni no). En SQL Server, el estado por defecto es "denegado implícitamente", por lo que un `REVOKE` suele resultar en falta de acceso, a menos que obtenga el permiso por otro Rol.

### 4\. Roles (Agrupación de Permisos)

Para facilitar la administración, no se suelen dar permisos usuario por usuario, sino a través de roles.

  * **Fixed Server Roles:** Roles fijos del servidor. El más importante es `sysadmin` (tiene control total, ignora los `DENY`).
  * **Fixed Database Roles:** Roles fijos de la BD.
      * `db_owner`: Control total de la BD.
      * `db_datareader`: Puede hacer `SELECT` en todas las tablas.
      * `db_datawriter`: Puede hacer `INSERT`, `UPDATE`, `DELETE` en todas las tablas.
  * **User-Defined Roles:** Roles creados por el administrador para necesidades específicas (ej. `Rol_Ventas`).

### 5\. Cadena de Propiedad (Ownership Chaining)

Un concepto avanzado de SQL Server. Si una Vista y una Tabla pertenecen al mismo dueño (Schema Owner), y le das permiso a un usuario para hacer `SELECT` en la Vista, SQL Server **no verificará** los permisos en la Tabla base. Esto permite encapsular el acceso y ocultar datos crudos.

-----

## Errores Comunes en Parciales

1.  **Confundir Login con User:**
      * *Login:* Llave para entrar al edificio (Servidor).
      * *User:* Llave para entrar a una oficina específica (Base de Datos).
2.  **Creer que REVOKE es Denegar:** `REVOKE` simplemente "borra" la entrada en la lista de permisos. `DENY` escribe un "PROHIBIDO" en rojo.
3.  **Olvidar la Jerarquía:** Si das `GRANT SELECT` sobre un Esquema entero, todos los objetos nuevos creados en ese esquema heredarán el permiso automáticamente.
4.  **El poder de SA:** El usuario `sa` o los miembros de `sysadmin` no pueden ser bloqueados con `DENY`.

-----

## Ejemplos en SQL Server

### Sintaxis Básica y Lógica de Negocio

```sql
-- 1. Crear un Login (Nivel Servidor)
CREATE LOGIN [AnaDev] WITH PASSWORD = 'StrongPassword123';

-- 2. Mapear Login a Usuario (Nivel Base de Datos)
USE VentasDB;
CREATE USER [AnaUser] FOR LOGIN [AnaDev];

-- 3. Crear un Rol personalizado
CREATE ROLE [AnalistasVentas];

-- 4. Agregar Usuario al Rol
ALTER ROLE [AnalistasVentas] ADD MEMBER [AnaUser];

-- 5. USO DE GRANT (Dar permiso)
-- Le damos permiso al Rol para leer la tabla Pedidos
GRANT SELECT ON Schema::Ventas TO [AnalistasVentas];

-- 6. USO DE DENY (La excepción)
-- Aunque Ana está en el rol 'AnalistasVentas' y debería ver todo el esquema,
-- le prohibimos ver la tabla de 'Salarios'.
DENY SELECT ON Object::Ventas.Salarios TO [AnaUser];

-- RESULTADO: Ana puede ver 'Ventas.Pedidos' (por el Rol),
-- pero NO 'Ventas.Salarios' (por el DENY explícito).

-- 7. USO DE REVOKE (Corrección)
-- Nos arrepentimos del DENY. Ahora Ana vuelve a heredar el permiso del Rol.
REVOKE SELECT ON Object::Ventas.Salarios TO [AnaUser];
```

-----

## Situaciones Reales de Uso

| Situación | Solución de Permisos |
| :--- | :--- |
| **Auditoría Externa** | Crear usuario y asignarlo al rol `db_datareader`. Solo lectura, cero riesgo de borrado. |
| **Aplicación Web** | Crear un usuario de BD que solo tenga permisos de `EXECUTE` sobre Stored Procedures, y ningún acceso directo a tablas (Seguridad por encapsulamiento). |
| **Becario/Junior** | `GRANT` permisos específicos solo sobre las tablas de desarrollo. `DENY` sobre tablas de producción sensibles (RRHH). |

-----

## Checklist para Aprobar el Parcial

  * [ ] ¿Entiendo la diferencia entre Autenticación y Autorización?
  * [ ] ¿Puedo explicar la diferencia entre Login y User?
  * [ ] ¿Sé qué prioridad tiene un `DENY` sobre un `GRANT`?
  * [ ] ¿Entiendo que `REVOKE` elimina un permiso pero no prohíbe explícitamente si hay otro rol involucrado?
  * [ ] ¿Conozco los roles fijos básicos (`sysadmin`, `db_owner`, `db_datareader`)?
  * [ ] ¿Sé escribir una sentencia `GRANT` básica?

-----

## Cuestionario de Preparación (15 Preguntas)

### Sección 1: Selección Múltiple (Solo una correcta)

**1. ¿Qué sub-lenguaje de SQL se utiliza para gestionar permisos?**
A) DML (Data Manipulation Language)
B) DDL (Data Definition Language)
C) DCL (Data Control Language)
D) TCL (Transaction Control Language)

**2. En la jerarquía de seguridad de SQL Server, ¿qué es un "Securable"?**
A) El usuario que se loguea.
B) El objeto al que se desea acceder (ej. Tabla, Vista).
C) La contraseña del usuario.
D) El rol del servidor.

**3. Si un usuario pertenece a un Rol que tiene `GRANT SELECT` sobre una tabla, pero al usuario se le aplica explícitamente un `DENY SELECT` sobre la misma tabla, ¿qué sucede?**
A) Puede seleccionar datos porque el Rol tiene prioridad.
B) No puede seleccionar datos porque el `DENY` tiene precedencia.
C) SQL Server lanza un error de conflicto.
D) Se aplica el permiso más reciente en el tiempo.

**4. ¿Cuál es la función del comando `REVOKE`?**
A) Prohibir explícitamente el acceso a un recurso.
B) Eliminar un usuario de la base de datos.
C) Eliminar un permiso `GRANT` o `DENY` previamente asignado.
D) Bloquear la cuenta del login.

**5. ¿Qué diferencia principal existe entre un `Login` y un `User`?**
A) No hay diferencia, son sinónimos.
B) El Login es a nivel de base de datos; el User es a nivel de servidor.
C) El Login autentica en el Servidor; el User autoriza dentro de una Base de Datos.
D) El User tiene contraseña; el Login no.

**6. ¿Qué rol fijo de base de datos permite leer datos de todas las tablas, pero no modificarlos?**
A) `db_owner`
B) `db_accessadmin`
C) `db_datawriter`
D) `db_datareader`

**7. Si deseas que un usuario pueda otorgar a otros usuarios el mismo permiso que tú le estás dando, ¿qué cláusula debes usar?**
A) `WITH ADMIN OPTION`
B) `WITH GRANT OPTION`
C) `ALLOW CASCADE`
D) `ENABLE SHARING`

**8. ¿Cuál es el principio de seguridad que dicta que un usuario debe tener solo los permisos estrictamente necesarios para hacer su trabajo?**
A) Principio de Máxima Autoridad.
B) Principio de Autenticación Doble.
C) Principio de Menor Privilegio (Least Privilege).
D) Principio de Redundancia de Datos.

**9. Al eliminar un Login del servidor, ¿qué sucede con los usuarios de base de datos asociados a él?**
A) Se eliminan automáticamente.
B) Se convierten en usuarios "huérfanos" (Orphaned Users).
C) Se bloquea la base de datos.
D) El Login no se puede eliminar si tiene usuarios.

**10. Tienes un Stored Procedure que selecciona datos de una Tabla. Si das permiso `EXECUTE` en el SP a un usuario, pero NO le das permiso `SELECT` en la Tabla, ¿funcionará? (Asumiendo mismo dueño/owner).**
A) No, necesita permisos en ambos objetos.
B) Sí, debido a la Cadena de Propiedad (Ownership Chaining).
C) Sí, pero solo si es administrador.
D) No, SQL Server requiere acceso directo a los datos siempre.

-----

### Sección 2: Verdadero o Falso

**11.** El rol `sysadmin` puede ser restringido mediante un `DENY` explícito para que no pueda ver una tabla específica.
**( ) Verdadero / ( ) Falso**

**12.** Los permisos asignados a un Esquema (Schema) son heredados automáticamente por los objetos contenidos en ese esquema.
**( ) Verdadero / ( ) Falso**

**13.** `REVOKE` es el opuesto exacto de `GRANT`; si haces `REVOKE`, el usuario queda explícitamente prohibido de entrar, igual que con `DENY`.
**( ) Verdadero / ( ) Falso**

**14.** Es posible otorgar permisos a nivel de columna (por ejemplo, permitir ver el `Nombre` pero no el `Salario` en una tabla de empleados).
**( ) Verdadero / ( ) Falso**

**15.** Un usuario de base de datos puede pertenecer a múltiples roles de base de datos simultáneamente.
**( ) Verdadero / ( ) Falso**

-----

## Soluciones y Explicaciones

### Respuestas Selección Múltiple

1.  **C**. DCL es el subconjunto para permisos (`GRANT`, `REVOKE`, `DENY`).
2.  **B**. Securable (Protegible) es el recurso (tabla, BD, etc.).
3.  **B**. El `DENY` es el permiso más fuerte ("Trump card") y sobrescribe cualquier `GRANT` proveniente de roles.
4.  **C**. `REVOKE` limpia el permiso, devolviendo al estado neutral.
5.  **C**. Concepto clave de arquitectura: Login = Servidor (Entrada al edificio), User = BD (Entrada a la oficina).
6.  **D**. `db_datareader` es el rol estándar para lectura global.
7.  **B**. `WITH GRANT OPTION` permite delegar la capacidad de dar permisos.
8.  **C**. Es el estándar de oro en seguridad informática.
9.  **B**. El usuario queda en la BD pero sin un Login que lo active (huérfano).
10. **B**. Si tienen el mismo *owner*, SQL solo chequea el permiso en el objeto inicial (el SP).

### Respuestas Verdadero/Falso

11. **Falso**. Los miembros de `sysadmin` ignoran todas las comprobaciones de permisos. No se les puede denegar nada.
12. **Verdadero**. La herencia de permisos facilita la administración; si das acceso al esquema, das acceso a sus tablas.
13. **Falso**. `REVOKE` quita el permiso otorgado, pero si el usuario tiene permiso por *otro* rol, podrá entrar. `DENY` es el único que prohíbe incondicionalmente.
14. **Verdadero**. Se puede hacer `GRANT SELECT (Columna1, Columna2) ON Tabla...`.
15. **Verdadero**. Un usuario puede sumar permisos de `db_datareader`, `Rol_Ventas` y `Rol_Reportes` a la vez (los permisos son aditivos, excepto si hay un `DENY`).

### Siguiente paso sugerido:

¿Te gustaría que profundicemos en la sintaxis de **Roles de Aplicación** o que te genere un ejercicio práctico de código (`script`) para simular un escenario de error de permisos y cómo solucionarlo?
