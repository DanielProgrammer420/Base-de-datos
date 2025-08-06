# Diseño Conceptual - Resumen

---

## Introducción al diseño conceptual y el modelo entidad-relación (M E-R)

El diseño conceptual es la fase inicial del proceso de diseño de bases de datos, enfocada en crear un modelo abstracto y lógico de los datos, independiente de cualquier sistema gestor de bases de datos (DBMS) o consideración técnica. Su objetivo es representar de manera clara y precisa los requerimientos del negocio mediante el modelo entidad-relación (M E-R), herramienta gráfica y conceptual ampliamente utilizada.

Este modelo permite:

- Identificar entidades (objetos relevantes del dominio).  
- Definir relaciones entre entidades.  
- Especificar atributos que describen propiedades de entidades y relaciones.  
- Establecer restricciones (cardinalidad, participación, claves).

El M E-R sirve como puente entre el mundo real y la implementación técnica, facilitando la comunicación entre analistas, diseñadores y usuarios finales. Es crucial para evitar ambigüedades y garantizar que la base de datos refleje fielmente las necesidades del sistema.

---

## Componentes clave del modelo entidad-relación

### Entidades

Una entidad es cualquier objeto del mundo real sobre el que queremos guardar información y que se puede distinguir de los demás. Piensa en sustantivos.

**Ejemplo**: en una base de datos para una universidad, las entidades podrían ser:

- ESTUDIANTE
- PROFESOR
- ASIGNATURA

Cada entidad tiene atributos que la describen.  
**Representación gráfica**: Rectángulos.

#### Tipos de entidades:

- **Entidades Fuertes**:  
  - Pueden existir por sí mismas.  
  - Tienen un atributo clave único que las identifica.  
  - Se representan con rectángulos simples.

- **Entidades Débiles**:  
  - Dependen de una entidad fuerte para existir.  
  - Requieren una clave parcial y se identifican mediante una clave compuesta (clave primaria de la entidad fuerte + clave parcial).  
  - Se dibujan con rectángulos dobles.

---

### Atributos

Los atributos son las propiedades o características que describen a una entidad. Son los adjetivos o datos específicos que nos interesan de cada entidad.

**Ejemplo para la entidad ESTUDIANTE**:

- NumeroDeMatricula
- Nombre
- Apellido
- FechaDeNacimiento

**Representación gráfica**: óvalos conectados a su entidad.

#### Tipos de atributos:

- **Atributo Clave (o Llave Primaria)**:  
  - Identifica de forma única a cada instancia de la entidad.  
  - Se representa con el nombre del atributo subrayado.

- **Atributos Simples**:  
  - No se pueden dividir en partes más pequeñas (ej. Género).

- **Atributos Compuestos**:  
  - Se pueden dividir en sub-partes con significado propio (ej. Dirección → Calle, Ciudad, CodigoPostal).

- **Atributos Monovaluados**:  
  - Solo pueden tener un valor por entidad (ej. FechaDeNacimiento).

- **Atributos Multivaluados**:  
  - Pueden tener múltiples valores para una misma entidad (ej. TitulosUniversitarios).  
  - Se representan con un doble óvalo.

- **Atributos Derivados**:  
  - Su valor se puede calcular o derivar (ej. Edad a partir de FechaDeNacimiento).  
  - Se representan con un óvalo punteado.

---

### Claves

- **Clave primaria**:  
  - Atributo único que identifica inequívocamente a una entidad (ej.: ID_Empleado).

- **Clave candidata**:  
  - Atributos alternativos válidos como clave primaria (ej.: Correo_Electrónico en Usuario).

- **Clave foránea**:  
  - Atributo que referencia la clave primaria de otra entidad (ej.: ID_Departamento en Empleado).

---

### Relaciones

Una relación es una asociación o vínculo entre dos o más entidades. Describe cómo se conectan las entidades entre sí.  
**Ejemplo**: un PROFESOR imparte una ASIGNATURA.

**Representación gráfica**: rombo que conecta las entidades relacionadas.

#### Características de las relaciones:

##### Grado de la relación

- **Binarias**: 2 entidades  
- **Ternarias**: 3 entidades  
- **n-arias**: n entidades

##### Cardinalidades

Define cuántas instancias de una entidad pueden estar asociadas a otra:

- **Uno a Uno (1:1)**  
  - Ej.: Un GERENTE dirige un único DEPARTAMENTO y viceversa.

- **Uno a Muchos (1:N)**  
  - Ej.: Un PROFESOR imparte muchas ASIGNATURAS, pero cada ASIGNATURA tiene un solo PROFESOR.

- **Muchos a Muchos (N:M)**  
  - Ej.: Un ESTUDIANTE cursa varias ASIGNATURAS y una ASIGNATURA es cursada por muchos ESTUDIANTES.

**Representación gráfica**: números (1, N, M) o símbolos "pata de gallo" en las líneas que unen entidades con relaciones.

##### Participación

- **Total**: Todas las instancias deben participar (línea doble).  
  - Ej.: Todo Empleado pertenece a un Departamento.

- **Parcial**: Solo algunas instancias participan (línea simple).  
  - Ej.: No todos los Clientes han realizado una Compra.

---

## Entidades Débiles e Identificadores

- **Entidad débil**:  
  - No puede existir sin una entidad fuerte (ej.: Pedido_Detalle depende de Pedido).

- **Relación de identificación**:  
  - Conecta la entidad débil con su entidad fuerte.  
  - Se representa con rombo doble.

- **Clave parcial**:  
  - Atributo que identifica entidades débiles solo dentro de su entidad fuerte (ej.: Número_Ítem en Pedido_Detalle).

- **Clave completa**:  
  - Combinación de la clave de la entidad fuerte + clave parcial.

---

## Notación gráfica del diagrama E-R

Se utiliza la notación de **Chen**, estándar en diseño conceptual.

**Símbolos:**

- **Rectángulos**: Entidades (simples para fuertes, dobles para débiles)
- **Diamantes**: Relaciones
- **Óvalos**: Atributos  
  - Doble contorno: atributos multivaluados  
  - Punteado: atributos derivados
- **Líneas**: Conectan entidades con relaciones y atributos

**Símbolos de cardinalidad**:  
`1`, `N`, o `M` junto a las líneas  
**Ejemplo**:  
`Departamento (1) — Empleado (N)` → relación 1:N

---

## Índice de secciones:

- Introducción al diseño conceptual y el modelo entidad-relación (M E-R)  
- Componentes clave del modelo entidad-relación  
- Entidades Débiles e Identificadores  
- Notación gráfica del diagrama E-R
