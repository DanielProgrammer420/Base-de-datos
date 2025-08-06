<!-- Sección 1 -->
# Introducción al diseño conceptual y el modelo entidad-relación (M E-R)

El diseño conceptual es la fase inicial del proceso de diseño de bases de datos, enfocada en crear un modelo abstracto y lógico de los datos, independiente de cualquier sistema gestor de bases de datos (DBMS) o consideración técnica. Su objetivo es representar de manera clara y precisa los requerimientos del negocio mediante el modelo entidad-relación (M E-R), herramienta gráfica y conceptual ampliamente utilizada. Este modelo permite:  
- Identificar entidades (objetos relevantes del dominio).  
- Definir relaciones entre entidades.  
- Especificar atributos que describen propiedades de entidades y relaciones.  
- Establecer restricciones (cardinalidad, participación, claves).

El M E-R sirve como puente entre el mundo real y la implementación técnica, facilitando la comunicación entre analistas, diseñadores y usuarios finales. Es crucial para evitar ambigüedades y garantizar que la base de datos refleje fielmente las necesidades del sistema.

---

<!-- Sección 2 -->
## Componentes clave del modelo entidad-relación

### Entidades

Una entidad es cualquier objeto del mundo real sobre el que queremos guardar información y que se puede distinguir de los demás. Piensa en sustantivos. Por ejemplo, en una base de datos para una universidad, las entidades podrían ser: ESTUDIANTE, PROFESOR, ASIGNATURA.

Cada entidad tiene atributos que la describen.

**Representación gráfica:** Rectángulos.

**Tipos de entidades:**

- **Entidades Fuertes:** Son aquellas que pueden existir por sí mismas. Tienen un atributo clave único que las identifica. La mayoría de las entidades son fuertes. Se representan con rectángulos simples.
- **Entidades débiles:** Dependen de una entidad fuerte para existir. Requieren una clave parcial y se identifican mediante una clave compuesta (clave primaria de la entidad fuerte + clave parcial). Se dibujan con rectángulos dobles.

---

### Atributos

Los atributos son las propiedades o características que describen a una entidad. Son los adjetivos o datos específicos que nos interesan de cada entidad. Para la entidad ESTUDIANTE, los atributos podrían ser: NumeroDeMatricula, Nombre, Apellido, FechaDeNacimiento.

Propiedades que describen características de entidades o relaciones.

Se representan con óvalos conectados a su entidad.

Los atributos se clasifican en varios tipos:

- **Atributo Clave (o Llave Primaria):** Es un atributo que identifica de forma única a cada instancia de la entidad. No puede haber dos estudiantes con el mismo NumeroDeMatricula. Se representa con el nombre del atributo subrayado.
- **Atributos Simples:** No se pueden dividir en partes más pequeñas (ej. Género).
- **Atributos Compuestos:** Se pueden dividir en sub-partes con significado propio (ej. el atributo Dirección se puede componer de Calle, Ciudad, CodigoPostal).
- **Atributos Monovaluados:** Solo pueden tener un valor para una entidad (ej. una persona solo tiene una FechaDeNacimiento).
- **Atributos Multivaluados:** Pueden tener múltiples valores para una misma entidad (ej. un PROFESOR puede tener varios TitulosUniversitarios). Se representan con un doble óvalo.
- **Atributos Derivados:** Su valor se puede calcular o "derivar" a partir de otro atributo (ej. la Edad de un estudiante se puede derivar de su FechaDeNacimiento). Se representan con un óvalo punteado.

**Claves:**

- **Clave primaria:** Atributo único que identifica inequívocamente a una entidad (ej.: ID_Empleado).
- **Clave candidata:** Atributos alternativos válidos como clave primaria (ej.: Correo_Electrónico en Usuario).
- **Clave foránea:** Atributo que referencia la clave primaria de otra entidad (ej.: ID_Departamento en Empleado).

---

### Relaciones

Una relación es una asociación o vínculo entre dos o más entidades. Describe cómo se conectan las entidades entre sí. Por ejemplo, un PROFESOR imparte una ASIGNATURA.

Se representan con un rombo que conecta las entidades relacionadas.

Las relaciones también tienen características importantes:

#### Grado de la relación

Se refiere al número de entidades participantes:

- **Binarias** (2 entidades).
- **Ternarias** (3 entidades).
- **n-arias** (n entidades).

#### Cardinalidades

La cardinalidad es la característica más importante de una relación, ya que define el número de instancias de una entidad que pueden estar asociadas con las instancias de otra entidad. Los tipos de cardinalidad son:

- **Uno a Uno (1:1):** Una instancia de la entidad A se relaciona con, como máximo, una instancia de la entidad B.  
  **Ejemplo:** Un GERENTE dirige un único DEPARTAMENTO, y un DEPARTAMENTO es dirigido por un único GERENTE.
- **Uno a Muchos (1:N):** Una instancia de la entidad A se puede relacionar con muchas instancias de la entidad B, pero una de B solo con una de A.  
  **Ejemplo:** Un PROFESOR puede impartir muchas ASIGNATURAS, pero una ASIGNATURA es impartida por un solo PROFESOR.
- **Muchos a Muchos (N:M):** Una instancia de la entidad A se puede relacionar con muchas de B, y una de B se puede relacionar con muchas de A.  
  **Ejemplo:** Un ESTUDIANTE puede cursar varias ASIGNATURAS, y una ASIGNATURA es cursada por muchos ESTUDIANTES.

La cardinalidad se dibuja en las líneas que unen las entidades con la relación, usando números (1, N, M) o símbolos de "pata de gallo".

#### Participación:

- **Total:** Todas las instancias deben participar en la relación (ej.: todo Empleado debe pertenecer a un Departamento; se marca con línea doble).
- **Parcial:** Solo algunas instancias participan (ej.: no todos los Clientes han realizado una Compra; línea simple).

---

<!-- Sección 3 -->
### Entidades Débiles e Identificadores (p. 93-95)

- **Entidad débil:** No puede existir sin una entidad fuerte (ej: Pedido_Detalle depende de Pedido).
- **Relación de identificación:** Conecta la entidad débil con su entidad fuerte (rombo doble).
- **Clave parcial:** Atributo que identifica entidades débiles solo dentro de su entidad fuerte (ej: Número_Ítem en Pedido_Detalle).
- **Clave completa:** Combinación de la clave de la entidad fuerte + clave parcial.

---

<!-- Sección 4 -->
## Notación gráfica del diagrama E-R

El libro utiliza la notación de Chen (estándar en diseño conceptual), con los siguientes símbolos:  
- **Rectángulos:** Entidades (simples para fuertes, dobles para débiles).
- **Diamantes:** Relaciones.
- **Óvalos:** Atributos (doble contorno para multivaluados, punteado para derivados).
- **Líneas:** Conectan entidades con relaciones y atributos.

**Símbolos de cardinalidad:** 1 (uno), N (muchos) o M (muchos) junto a las líneas (ej.: Departamento (1) — Empleado (N) para una relación 1:N).
