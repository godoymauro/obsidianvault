¡Perfecto! Ya controlamos el **CRUD** básico. Ahora, daremos un paso adelante para hacer que nuestros datos no solo se lean, sino que también se **transformen y analicen**. Esto lo logramos con **Operadores y Funciones**.

---

# 07 - Operadores y funciones

## 🧮 ¿Qué son Operadores y Funciones?

En SQL, los **operadores** y **funciones** nos permiten realizar cálculos, comparaciones y transformaciones sobre los datos que recuperamos. Son esenciales para ir más allá de la simple recuperación de datos y comenzar con el análisis y el procesamiento.

### Categorías Principales

1. **Operadores:** Símbolos utilizados para comparar valores (`>`, `=`, `LIKE`), realizar cálculos (`+`, `-`, `*`), o combinar condiciones (`AND`, `OR`).
    
2. **Funciones Escalares:** Operan sobre **una fila** y devuelven un **único valor** para esa fila (ej: convertir texto a mayúsculas, calcular la raíz cuadrada).
    
3. **Funciones de Agregación:** Operan sobre un **conjunto de filas** y devuelven un **único valor resumen** para todo el conjunto (ej: la suma total, el promedio).
    

---

## 1. Operadores Matemáticos y de Comparación

Estos se utilizan principalmente en la cláusula `WHERE` (para filtrar) y en la cláusula `SELECT` (para calcular).

### Operadores Matemáticos

|Operador|Propósito|Ejemplo en `SELECT`|Resultado|
|---|---|---|---|
|`+`|Suma|`SELECT Precio + 10 FROM Productos`|Devuelve el precio aumentado en 10.|
|`-`|Resta|`SELECT Stock - 5 FROM Productos`|Devuelve el stock reducido en 5.|
|`*`|Multiplicación|`SELECT Precio * Stock FROM Productos`|Devuelve el valor total del inventario por producto.|
|`/`|División|`SELECT Total / Cantidad FROM Pedidos`|Devuelve el precio unitario promedio.|
|`%` (o `MOD`)|Módulo (Resto)|`SELECT ID % 2 FROM Productos`|Útil para identificar filas pares/impares.|

### Operadores de Comparación

|Operador|Propósito|Ejemplo en `WHERE`|
|---|---|---|
|`=`|Igual a|`WHERE Nombre = 'Teclado'`|
|`!=` o `<>`|Diferente de|`WHERE Precio <> 100`|
|`>` , `<`|Mayor que, Menor que|`WHERE Stock > 10`|
|`>=` , `<=`|Mayor/Menor o igual que|`WHERE Precio <= 50.00`|
|`BETWEEN`|Valor dentro de un rango inclusivo|`WHERE Precio BETWEEN 50 AND 150`|
|`IN`|Valor coincide con cualquier valor en una lista|`WHERE ID IN (1, 5, 10)`|
|`IS NULL`|El valor es nulo (desconocido)|`WHERE Email IS NULL`|
|`LIKE`|Búsqueda de patrones de texto (ver más adelante)|`WHERE Nombre LIKE 'Laptop%'`|

---

## 2. Funciones Escalares Comunes (Por Fila)

Estas funciones transforman el valor de una columna, fila por fila.

|Categoría|Función|Propósito|Ejemplo|
|---|---|---|---|
|**Texto**|`UPPER(col)` / `LOWER(col)`|Convierte el texto a mayúsculas o minúsculas.|`SELECT UPPER(Nombre) FROM Productos`|
||`LENGTH(col)`|Devuelve la longitud de una cadena.|`SELECT LENGTH(Nombre) FROM Productos`|
||`CONCAT(a, b, ...)`|Une varias cadenas de texto.|`SELECT CONCAT(Nombre, ' ', Apellido) FROM Clientes`|
|**Números**|`ROUND(col, decimales)`|Redondea un número al número de decimales especificado.|`SELECT ROUND(Precio, 0) FROM Productos`|
||`ABS(col)`|Valor absoluto.|`SELECT ABS(-10.5)`|
|**Fecha**|`NOW()` / `CURRENT_DATE()`|Devuelve la fecha y/o hora actual del servidor.|`SELECT NOW()`|
||`DATE_FORMAT(col, formato)`|Formatea una fecha (varía según SGBD).|`SELECT DATE_FORMAT(Fecha, '%Y-%m-%d')`|

### Ejemplo: Uso de Funciones Escalares

SQL

```
-- Queremos una lista con el nombre del producto en mayúsculas y el precio redondeado, 
-- y un cálculo simple del valor con IVA.

SELECT
    UPPER(Nombre) AS Nombre_Mayusculas,
    ROUND(Precio, 0) AS Precio_Redondeado,
    Precio * 1.21 AS Precio_con_IVA -- Operador matemático
FROM Productos;
```

---

## 3. Funciones de Agregación (Por Conjunto)

Las funciones de agregación toman _todos_ los valores de una columna (o un grupo de ellos, como veremos en la próxima clase) y los combinan para generar una única métrica resumida.

|Función|Propósito|Ejemplo|Resultado|
|---|---|---|---|
|`COUNT()`|Cuenta el número de filas (o valores no nulos).|`COUNT(*)`|Número total de productos.|
|`SUM()`|Calcula la suma de los valores de una columna numérica.|`SUM(Stock)`|Inventario total disponible.|
|`AVG()`|Calcula el valor promedio (media) de una columna numérica.|`AVG(Precio)`|Precio promedio de todos los productos.|
|`MIN()`|Encuentra el valor mínimo en una columna.|`MIN(Precio)`|El producto más barato.|
|`MAX()`|Encuentra el valor máximo en una columna.|`MAX(Precio)`|El producto más caro.|

### Ejemplos Prácticos de Agregación

SQL

```
-- 1. ¿Cuántos productos tenemos en total?
SELECT COUNT(*) AS Total_Productos
FROM Productos;

-- 2. ¿Cuál es el precio promedio y el precio máximo?
SELECT
    AVG(Precio) AS Precio_Promedio,
    MAX(Precio) AS Precio_Maximo
FROM Productos;

-- 3. ¿Cuál es el valor total del inventario? (Suma de Precio * Stock)
SELECT
    SUM(Precio * Stock) AS Valor_Total_Inventario
FROM Productos;
```

> 📝 **Nota sobre `COUNT(*)` vs `COUNT(Columna)`:**
> 
> - `COUNT(*)` cuenta **todas las filas** (incluyendo aquellas donde los valores son `NULL`).
>     
> - `COUNT(Columna)` cuenta solo las filas donde el valor de esa **columna NO es NULL**.
>     

---

## 4. Operador `LIKE` y Wildcards (Búsqueda de Patrones)

El operador `LIKE` se usa en la cláusula `WHERE` para buscar patrones dentro de cadenas de texto, utilizando caracteres comodín (_wildcards_).

|Comodín|Significado|Ejemplo|Descripción|
|---|---|---|---|
|`%`|Representa **cero o más** caracteres.|`LIKE 'Moni%'`|Encuentra textos que empiezan con "Moni" (Monitor, Monitoreo...).|
|`_`|Representa **exactamente un** carácter.|`LIKE 'J_n'`|Encuentra "Jan", "Jen", "Jun"...|

### Ejemplos `LIKE`

SQL

```
-- 1. Productos cuyo nombre contiene la palabra 'Gaming' en cualquier parte
SELECT Nombre
FROM Productos
WHERE Nombre LIKE '%Gaming%';

-- 2. Productos que terminan con la letra 's'
SELECT Nombre
FROM Productos
WHERE Nombre LIKE '%s';

-- 3. Productos que tienen una 'a' como segundo carácter (ej: 'Tablet')
SELECT Nombre
FROM Productos
WHERE Nombre LIKE '_a%';
```

---

## ✅ Cierre: Puntos Clave

- Los **Operadores** (`+`, `=`, `AND`) realizan cálculos y comparaciones.
    
- Las **Funciones Escalares** (`UPPER`, `ROUND`) actúan sobre una fila y devuelven un valor.
    
- Las **Funciones de Agregación** (`SUM`, `AVG`, `COUNT`) actúan sobre un conjunto de filas y devuelven un resumen.
    
- El operador **`LIKE`** permite realizar búsquedas flexibles de patrones de texto usando los comodines `%` (cero o más caracteres) y `_` (un solo carácter).
    

---

**Próximo Paso:** Ahora que podemos calcular y agregar, necesitamos la herramienta clave para organizar esos datos resumidos: la cláusula `GROUP BY`, junto con los filtros `WHERE` y `HAVING`.

[[08 - Filtros y condiciones]]