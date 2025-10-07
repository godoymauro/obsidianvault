¡Excelente! Hemos diseñado la estructura con el MER. Ahora vamos a la base teórica que sustenta el funcionamiento de **SQL**: el **Álgebra Relacional**. Comprender esto es clave para escribir consultas eficientes.

---

# 04 - Álgebra Relacional y Fundamentos Teóricos

## 🧠 ¿Qué es el Álgebra Relacional?

El **Álgebra Relacional** es un conjunto de operaciones matemáticas abstractas que nos permite manipular las relaciones (tablas) de una base de datos. Fue desarrollado por E.F. Codd como la base teórica para el modelo relacional.

Piensa en el Álgebra Relacional como el _motor_ que ejecuta las consultas **SQL**. Cuando escribes un `SELECT * FROM ... WHERE ...`, el SGBD lo traduce internamente a una o varias de estas operaciones.

### Conceptos Clave

1. **Relación (Tabla):** Una estructura de datos bidimensional con filas y columnas.
    
2. **Tupla (Fila):** Un registro individual en la tabla.
    
3. **Atributo (Columna):** Una propiedad con un nombre y tipo de dato.
    

El Álgebra Relacional utiliza un conjunto de operadores para tomar una o dos relaciones de entrada y producir una **nueva relación de salida**.

---

## ⚙️ Operadores Fundamentales

Existen cinco operaciones básicas a partir de las cuales se construyen todas las demás consultas SQL.

### 1. Selección (σ) - Filtrar Filas

- **Símbolo:** σ (sigma minúscula)
    
- **Propósito:** Elige un subconjunto de **tuplas** (filas) de una relación que cumplen una condición específica.
    
- **Equivalente en SQL:** La cláusula `WHERE`.
    
- **Ejemplo Teórico:** σPrecio>50​(Productos)
    
- **Equivalente en SQL:**
    
    SQL
    
    ```
    SELECT *
    FROM Productos
    WHERE Precio > 50;
    ```
    

### 2. Proyección (π) - Filtrar Columnas

- **Símbolo:** π (pi minúscula)
    
- **Propósito:** Elige un subconjunto de **atributos** (columnas) de una relación.
    
- **Equivalente en SQL:** La lista de columnas después de `SELECT`.
    
- **Ejemplo Teórico:** πNombre,Precio​(Productos)
    
- **Equivalente en SQL:**
    
    SQL
    
    ```
    SELECT Nombre, Precio
    FROM Productos;
    ```
    

### 3. Unión (∪) - Combinar Conjuntos

- **Símbolo:** ∪
    
- **Propósito:** Combina las tuplas de dos relaciones **compatibles** (deben tener el mismo número y tipo de atributos). Elimina duplicados.
    
- **Equivalente en SQL:** `UNION`.
    
- **Ejemplo Teórico:** ClientesEspan˜a∪ClientesFrancia
    

### 4. Diferencia de Conjuntos (− ) - Excluir

- **Símbolo:** −
    
- **Propósito:** Devuelve las tuplas que están en la Relación A pero **no** están en la Relación B (deben ser compatibles).
    
- **Equivalente en SQL:** `EXCEPT` (o `MINUS` en Oracle/SQLite).
    
- **Ejemplo Teórico:** Clientes−ClientesConPedidos (Obtiene clientes que no han hecho pedidos).
    

### 5. Producto Cartesiano (×) - Combinar Todas las Filas

- **Símbolo:** ×
    
- **Propósito:** Combina cada tupla de la Relación A con cada tupla de la Relación B. El resultado es una relación con la suma de los atributos y un número de tuplas igual al producto de las tuplas originales (FilasA​×FilasB​).
    
- **Equivalente en SQL:** `CROSS JOIN` (o simplemente listar tablas sin `WHERE`).
    
- **Ejemplo Teórico:** Empleados×Departamentos
    

> ⚠️ **IMPORTANTE:** El Producto Cartesiano **casi nunca se usa solo**, ya que genera resultados irrelevantes. Es la base teórica de los _JOINs_, donde luego se usa la **Selección (σ)** para filtrar las combinaciones válidas.

---

## ➕ Operadores Derivados (Combinando Fundamentales)

Estos operadores se construyen a partir de los cinco fundamentales, pero son tan comunes que tienen su propia notación.

### 6. Intersección (∩)

- **Símbolo:** ∩
    
- **Propósito:** Devuelve las tuplas que son comunes a ambas relaciones.
    
- **Equivalente en SQL:** `INTERSECT`.
    
- **Construcción:** R∩S≡R−(R−S) (Restar de R, todo lo que no está en R y S a la vez).
    

### 7. Reunión (Join) Natural (⋈) - La Base de las Relaciones

- **Símbolo:** ⋈ (Símbolo de pajarita)
    
- **Propósito:** Es la operación más crucial. Combina el **Producto Cartesiano (×)** seguido de una **Selección (σ)** y una **Proyección (π)**.
    
- **En palabras:** Combina dos tablas basándose en que los atributos de nombres idénticos (y valores iguales) coincidan, eliminando las columnas duplicadas en el resultado.
    
- **Equivalente en SQL:** `INNER JOIN` (generalmente usado con la cláusula `ON`).
    
- **Fórmula Teórica (Simplificada):** R⋈S≡σR.Clave=S.Clave​(R×S)
    

#### Ejemplo de Join Natural

**Tablas:**

- **Empleados:** (`ID`, `Nombre`, `ID_Depto`)
    
- **Departamentos:** (`ID`, `NombreDepto`)
    

**Consulta Teórica:** πNombre,NombreDepto​(Empleados⋈Departamentos)

**Equivalente en SQL:**

SQL

```
SELECT E.Nombre, D.NombreDepto
FROM Empleados E
INNER JOIN Departamentos D ON E.ID_Depto = D.ID;
```

---

## 🛠️ La Importancia de Comprender el Álgebra

Comprender estos conceptos te convierte en un mejor especialista por tres razones:

1. **Optimización de Consultas:** Cuando escribes una consulta compleja, saber que una **Selección (σ)** se ejecuta _antes_ de una **Proyección (π )** te ayuda a diseñar _queries_ que reducen la cantidad de datos a procesar lo antes posible.
    
2. **Comprensión de `JOINs`:** Un _JOIN_ no es magia; es un **Producto Cartesiano** seguido de un **Filtro** muy específico. Entender esto te ayuda a depurar _JOINs_ que devuelven resultados incorrectos.
    
3. **Base Teórica:** Es el lenguaje formal que define lo que _debe_ hacer un SGBD para adherirse al modelo relacional.
    

---

## ✅ Cierre: Puntos Clave

- El **Álgebra Relacional** es la base matemática de todas las consultas SQL.
    
- Los cinco operadores fundamentales son: **Selección (σ)** (filtra filas/`WHERE`), **Proyección (π)** (filtra columnas/`SELECT`), **Unión (∪ )**, **Diferencia (− )** y **Producto Cartesiano (×)**.
    
- La operación más usada, el **Join Natural (⋈)**, es una combinación de Producto Cartesiano y Selección (filtrado).
    
- Un buen diseño de consulta aplica la **Selección** y **Proyección** lo más pronto posible para reducir la carga de trabajo del motor.
    

---

**Próximo Paso:** Con el diseño (MER) y la teoría (Álgebra Relacional) establecidos, estamos listos para ensuciarnos las manos. Instalaremos y configuraremos los motores de bases de datos que usaremos en el curso.

[[05 - Instalación de motores]]