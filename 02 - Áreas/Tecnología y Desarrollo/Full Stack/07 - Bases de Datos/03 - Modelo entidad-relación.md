¡Claro que sí! Pasamos a la fase de diseño, la más crítica de todas. Antes de crear tablas o colecciones, debemos modelar.

---

# 03 - Modelo Entidad-Relación (MER)

## 📐 ¿Qué es el Modelo Entidad-Relación (MER)?

El **Modelo Entidad-Relación (MER)** es una herramienta conceptual de alto nivel que nos permite diseñar la estructura de una base de datos de manera visual, independientemente del motor que usemos (MySQL, PostgreSQL, MongoDB, etc.).

Piensa en el MER como el **plano arquitectónico** de tu base de datos. Un arquitecto no construye una casa sin un plano; un especialista no crea una BD sin un MER.

### Componentes Fundamentales del MER

El MER utiliza tres conceptos básicos que se interrelacionan:

1. **Entidades:** Son objetos o cosas del mundo real que se quieren representar y de las que se almacenan datos.
    
2. **Atributos:** Son las propiedades o características de una entidad.
    
3. **Relaciones:** Son las asociaciones o vínculos lógicos entre dos o más entidades.
    

---

## 🧍 1. Entidades

Una **Entidad** representa una clase de objetos, personas o conceptos. En una base de datos relacional, una entidad se convertirá en una **Tabla**.

|Concepto|Entidad (Ejemplo)|Equivalente en SQL|
|---|---|---|
|Cosa|Producto, Vehículo|Tabla|
|Persona|Cliente, Empleado, Profesor|Tabla|
|Concepto|Pedido, Factura, Curso|Tabla|

### Diagrama: Notación de Entidad

En la notación más común (Chen o _Crow's Foot_), la entidad se representa con un **rectángulo**.

Fragmento de código

```
graph TD
    A[CLIENTE]
    B[PRODUCTO]
```

---

## 🏷️ 2. Atributos

Los **Atributos** describen las propiedades de una entidad. Cada atributo tiene un nombre y un tipo de dato asociado (ej: texto, número, fecha). En una tabla, un atributo se convierte en una **Columna**.

### Tipos de Atributos Clave

1. **Atributo Simple:** No se puede dividir (ej: `Precio`, `Edad`).
    
2. **Atributo Compuesto:** Se puede dividir en atributos más pequeños (ej: `Dirección` se divide en `Calle`, `Ciudad`, `Código Postal`).
    
3. **Atributo Derivado:** Se calcula a partir de otros (ej: `Edad` se deriva de `Fecha de Nacimiento`).
    
4. **Clave Primaria (PK):** Es un atributo (o conjunto de ellos) que identifica **de forma única** a cada instancia (fila) de la entidad. ¡Es el atributo más importante! Se subraya en los diagramas.
    

### Diagrama: Notación de Atributos

Los atributos se representan con **óvalos** conectados a la entidad. La clave primaria va subrayada.

Fragmento de código

```
graph TD
    subgraph CLIENTE
        Dirección
        Nombre
        ID_Cliente
    end
    ID_Cliente --- CLIENTE(CLIENTE)
    Nombre --- CLIENTE
    Dirección --- CLIENTE
    style ID_Cliente fill:#add8e6,stroke:#333
```

_(En este ejemplo, `ID_Cliente` es la clave primaria.)_

---

## 🔗 3. Relaciones y Cardinalidad

Una **Relación** es una asociación lógica entre dos o más entidades.

### La Cardinalidad

La **Cardinalidad** define cuántas instancias de una entidad se relacionan con cuántas instancias de otra. Es el concepto más importante al modelar. Se expresa en tres tipos:

#### A. Uno a Uno (1:1)

- **Definición:** Una instancia de la Entidad A se relaciona con, como máximo, una instancia de la Entidad B, y viceversa.
    
- **Ejemplo:** Un **Empleado** tiene **un** solo **Pasaporte**, y **un** Pasaporte pertenece a **un** solo Empleado.
    

#### B. Uno a Muchos (1:N)

- **Definición:** Una instancia de la Entidad A puede relacionarse con _muchas_ instancias de la Entidad B, pero una instancia de B solo se relaciona con _una_ instancia de A.
    
- **Ejemplo:** Un **Departamento** tiene **muchos** **Empleados**, pero cada Empleado pertenece a **un** solo Departamento. (Es la relación más común).
    

#### C. Muchos a Muchos (N:M)

- **Definición:** Una instancia de la Entidad A puede relacionarse con _muchas_ instancias de B, y una instancia de B puede relacionarse con _muchas_ instancias de A.
    
- **Ejemplo:** Un **Estudiante** puede estar matriculado en **muchos** **Cursos**, y un Curso tiene **muchos** Estudiantes.
    

> 📝 **NOTA CRÍTICA:** Una relación N:M **no puede implementarse directamente** en una BD relacional. Siempre se resuelve creando una tercera entidad (o **Tabla Intermedia**) que contendrá las claves primarias de ambas entidades originales.

### Diagrama: Notación de Relaciones (Crow's Foot)

Usaremos la notación **Pata de Gallo** (_Crow's Foot_), la más usada en la industria para diagramas lógicos.

|Notación|Cardinalidad|Significado|
|---|---|---|
|**—|**|Cero o Uno|
|**—||**|
|**—0<**|Cero a Muchos|Opcional, puede tener muchos|
|**—|–<**|Uno a Muchos|

### Ejemplo Práctico de MER

Diseñaremos el núcleo de un sistema de ventas:

|Entidad|Clave Primaria|Otros Atributos|
|---|---|---|
|**CLIENTE**|`ID_Cliente`|Nombre, Email, Teléfono|
|**PRODUCTO**|`ID_Producto`|Nombre, Precio, Stock|
|**PEDIDO**|`ID_Pedido`|Fecha, Estado, Total|

**Relaciones:**

1. **CLIENTE** y **PEDIDO**: Un cliente hace _muchos_ pedidos. Un pedido es hecho por _un_ solo cliente. (1:N)
    
2. **PEDIDO** y **PRODUCTO**: Un pedido incluye _muchos_ productos. Un producto puede estar en _muchos_ pedidos. (N:M)
    

#### Diagrama Lógico del MER

Fragmento de código

```
erDiagram
    CLIENTE ||--o{ PEDIDO : hace
    PRODUCTO }o--o{ PEDIDO_DETALLE : está_en
    PEDIDO ||--o{ PEDIDO_DETALLE : incluye

    CLIENTE {
        int ID_Cliente PK
        varchar Nombre
        varchar Email
    }

    PEDIDO {
        int ID_Pedido PK
        date Fecha
        decimal Total
        int ID_Cliente FK "ID del cliente que hace el pedido"
    }

    PRODUCTO {
        int ID_Producto PK
        varchar Nombre
        decimal Precio
    }

    PEDIDO_DETALLE {
        int ID_Detalle PK
        int ID_Pedido FK
        int ID_Producto FK
        int Cantidad
    }
```

- **Observación Clave:** La relación N:M entre `PEDIDO` y `PRODUCTO` se resolvió con la tabla intermedia **PEDIDO_DETALLE**. Esta tabla contiene las claves foráneas (`ID_Pedido` y `ID_Producto`) de las dos tablas que relaciona.
    

---

## 🛠️ Buenas Prácticas al Modelar

1. **Enfoque Iterativo:** El MER nunca es perfecto a la primera. Dibújalo, revísalo y ajústalo con las [[11 - Normalización y desnormalización|reglas de normalización]].
    
2. **Identificar PK/FK:** La clave de un buen diseño relacional es identificar correctamente las **Claves Primarias (PK)** y **Claves Foráneas (FK)**.
    
    - **PK** define la entidad.
        
    - **FK** implementa la relación (el "lazo" que conecta las tablas).
        
3. **Resolver N:M:** Siempre que encuentres una relación Muchos a Muchos, ¡crea una tabla intermedia!
    

---

## ✅ Cierre: Puntos Clave

- El **Modelo Entidad-Relación (MER)** es el plano de la BD, compuesto por **Entidades** (tablas), **Atributos** (columnas) y **Relaciones**.
    
- La **Clave Primaria (PK)** identifica una fila de forma única. La **Clave Foránea (FK)** implementa las relaciones entre tablas.
    
- La **Cardinalidad** (1:1, 1:N, N:M) define las reglas de las relaciones.
    
- Las relaciones **N:M** siempre deben resolverse con una **Tabla Intermedia**.
    

---

**Próximo Paso:** El MER nos da la estructura visual, pero ahora debemos entender la base matemática que hace funcionar a SQL: el **Álgebra Relacional**.

[[04 - Álgebra relacional y fundamentos teóricos]]