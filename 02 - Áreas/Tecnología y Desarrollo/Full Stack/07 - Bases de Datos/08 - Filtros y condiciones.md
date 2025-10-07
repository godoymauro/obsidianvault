Hemos aprendido a manipular datos (CRUD) y a transformarlos (Funciones de Agregación). El siguiente paso es ganar **control absoluto** sobre qué datos se consultan y cómo se agrupan. Esto lo logramos con las cláusulas `WHERE`, `ORDER BY`, `GROUP BY` y `HAVING`.

---

# 08 - Filtros y condiciones

En SQL, la cláusula `FROM` te dice _de dónde_ vienen los datos, y la cláusula `SELECT` te dice _qué_ columnas quieres. Las cláusulas que veremos ahora son las que te dicen **cuáles, cómo y dónde** se presentarán esos datos.

## 1. Filtrado de Filas: `WHERE` (Selección σ)

La cláusula `WHERE` es el filtro fundamental, el que implementa la operación de **Selección (σ)** del [[04 - Álgebra relacional y fundamentos teóricos|Álgebra Relacional]]. Se utiliza para especificar una condición que deben cumplir las filas para ser incluidas en el resultado.

### Uso y Sintaxis

La cláusula `WHERE` utiliza los [[07 - Operadores y funciones|operadores de comparación y lógicos]] para crear condiciones complejas.

SQL

```
SELECT Columna1, Columna2
FROM NombreTabla
WHERE Condicion1 [AND|OR] Condicion2;
```

### Ejemplos Prácticos

Tomando nuestra tabla `Productos`:

SQL

```
-- 1. Productos con precio mayor a 100.00 y stock mayor a 5
SELECT Nombre, Precio, Stock
FROM Productos
WHERE Precio > 100.00 AND Stock > 5;

-- 2. Productos que cuestan entre 50 y 90 O cuyo nombre empieza con 'T'
SELECT Nombre, Precio
FROM Productos
WHERE (Precio BETWEEN 50 AND 90) OR (Nombre LIKE 'T%');
```

---

## 2. Ordenamiento: `ORDER BY`

La cláusula `ORDER BY` se utiliza para ordenar el conjunto de resultados de una consulta.

### Uso y Sintaxis

Se ejecuta **al final** del proceso de consulta. El orden puede ser ascendente (`ASC`, por defecto) o descendente (`DESC`).

SQL

```
SELECT Columna1, Columna2
FROM NombreTabla
WHERE condicion
ORDER BY Columna_a_Ordenar [ASC | DESC];
```

### Ejemplos Prácticos

SQL

```
-- 1. Listar productos del más caro al más barato
SELECT Nombre, Precio
FROM Productos
ORDER BY Precio DESC;

-- 2. Listar productos ordenados primero por Stock (ascendente)
--    y si el stock es el mismo, ordenarlos por Nombre (alfabético)
SELECT Nombre, Stock, Precio
FROM Productos
ORDER BY Stock ASC, Nombre ASC;
```

---

## 3. Agrupación: `GROUP BY`

La cláusula `GROUP BY` es fundamental para el análisis de datos. Divide el conjunto de filas en grupos, permitiendo que las [[07 - Operadores y funciones|funciones de agregación]] (como `SUM`, `AVG`, `COUNT`) operen sobre cada grupo por separado.

### Uso y Sintaxis

Cuando utilizas una función de agregación y quieres ver el resultado por categoría, debes incluir la(s) columna(s) de categoría en la cláusula `GROUP BY`.

SQL

```
SELECT Columna_Categoria, Funcion_Agregacion(Columna_Valor)
FROM NombreTabla
GROUP BY Columna_Categoria;
```

### Ejemplo Práctico (Requiere una tabla con categorías)

Creamos una tabla auxiliar `Pedidos` para este ejemplo.

|ID_Pedido|ID_Cliente|Region|Total|
|---|---|---|---|
|101|1|Norte|150.00|
|102|2|Sur|200.00|
|103|1|Norte|50.00|
|104|3|Oeste|100.00|

Exportar a Hojas de cálculo

SQL

```
-- Queremos saber la suma total de los pedidos por cada región

SELECT
    Region, -- Columna de agrupación
    COUNT(ID_Pedido) AS Total_Pedidos,
    SUM(Total) AS Suma_Total_Ventas
FROM Pedidos
GROUP BY Region;
```

**Resultado Esperado:**

|Region|Total_Pedidos|Suma_Total_Ventas|
|---|---|---|
|Norte|2|200.00|
|Sur|1|200.00|
|Oeste|1|100.00|

Exportar a Hojas de cálculo

---

## 4. Filtrado de Grupos: `HAVING`

La cláusula `WHERE` filtra **filas individuales** _antes_ de la agrupación. La cláusula `HAVING` filtra **grupos** _después_ de que se han aplicado las funciones de agregación.

### Uso y Sintaxis

Se utiliza justo después de `GROUP BY`. Su condición debe referenciar una función de agregación o una columna de agrupación.

SQL

```
SELECT Columna_Categoria, SUM(Total)
FROM Pedidos
GROUP BY Region
HAVING SUM(Total) > 150.00; -- Filtra los grupos
```

### Ejemplo Práctico

Continuando con el ejemplo anterior:

SQL

```
-- Queremos solo aquellas regiones cuya suma total de ventas sea superior a 150.00

SELECT
    Region,
    SUM(Total) AS Total_Ventas
FROM Pedidos
GROUP BY Region
HAVING SUM(Total) > 150.00
ORDER BY Total_Ventas DESC;
```

**Resultado Esperado:**

|Region|Total_Ventas|
|---|---|
|Norte|200.00|
|Sur|200.00|

Exportar a Hojas de cálculo

---

## 🧠 La Jerarquía de Ejecución (¡Clave para la Optimización!)

El orden en que escribes la consulta no es el orden en que el SGBD la ejecuta. El SGBD siempre sigue esta secuencia lógica. Entender esto te ayuda a evitar errores (`WHERE` no puede usar agregaciones) y a optimizar.

|Orden|Cláusula|Propósito|
|---|---|---|
|**1.**|`FROM`|Define la fuente de datos.|
|**2.**|`WHERE`|**Filtra filas individuales** (antes de agrupar).|
|**3.**|`GROUP BY`|Agrupa las filas restantes.|
|**4.**|`HAVING`|**Filtra los grupos** resultantes (puede usar agregaciones).|
|**5.**|`SELECT`|Selecciona las columnas o calcula las agregaciones finales.|
|**6.**|`ORDER BY`|Ordena el conjunto de resultados final.|

Exportar a Hojas de cálculo

---

## ✅ Cierre: Puntos Clave

- **`WHERE`** filtra **filas individuales** _antes_ de la agrupación.
    
- **`GROUP BY`** agrupa las filas por una o más categorías para aplicar funciones de agregación.
    
- **`HAVING`** filtra **grupos** _después_ de que se han calculado las agregaciones.
    
- **`ORDER BY`** organiza el resultado final y se ejecuta siempre al último.
    
- **Recuerda la jerarquía:** `WHERE` → `GROUP BY` → `HAVING` → `SELECT` → `ORDER BY`.
    

---

**Próximo Paso:** Hasta ahora, hemos trabajado con una sola tabla. El mundo real exige combinar información de múltiples entidades. Aprenderemos a hacer esto con los poderosos **`JOINs`**.

[[09 - Joins]]