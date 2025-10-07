¡Excelente! Ya sabes cómo juntar datos de diferentes tablas con `JOINs`. Ahora, llevaremos la lógica de consulta a un nivel superior, aprendiendo a **anidar consultas** para resolver problemas que no se pueden solucionar en un solo paso: las **Subconsultas**.

---

# 10 - Subconsultas

## 🧩 ¿Qué son las Subconsultas?

Una **Subconsulta** (o _subquery_) es una consulta `SELECT` que está anidada dentro de otra consulta SQL. Actúa como un _input_ para la consulta principal. El SGBD ejecuta primero la subconsulta y luego utiliza su resultado para completar la consulta principal.

Las subconsultas permiten resolver preguntas que requieren dos pasos lógicos:

1. **Encontrar un valor** (la subconsulta).
    
2. **Usar ese valor** para filtrar, calcular o comparar en la consulta principal.
    

---

## 1. Tipos de Subconsultas por su Salida

Clasificamos las subconsultas según el tipo de resultado que devuelven, ya que esto determina dónde pueden ser utilizadas:

### A. Subconsultas Escalares (Un Solo Valor)

- **Salida:** Devuelven **una sola fila** y **una sola columna** (un valor único).
    
- **Uso:** Pueden usarse en casi cualquier lugar donde se espera un valor único: en la cláusula `SELECT`, `WHERE`, o `HAVING`.
    
- **Ejemplo:** _Encontrar el precio de los productos que son más caros que el precio promedio._
    

SQL

```
-- Queremos el precio promedio para usarlo como referencia
SELECT AVG(Precio) FROM Productos; -- Esto devuelve un valor escalar, digamos 250.00

-- Consulta principal
SELECT Nombre, Precio
FROM Productos
WHERE Precio > (
    -- Subconsulta escalar: calcula el promedio
    SELECT AVG(Precio)
    FROM Productos
);
```

### B. Subconsultas de Múltiples Filas (Lista de Valores)

- **Salida:** Devuelven **múltiples filas**, pero **una sola columna** (una lista de valores).
    
- **Uso:** Se utilizan principalmente en la cláusula `WHERE` con operadores como `IN`, `NOT IN`, `ANY`, o `ALL`.
    
- **Ejemplo:** _Encontrar a todos los empleados que trabajan en el departamento de 'Ventas'._
    

SQL

```
-- 1. Encontrar los IDs de los departamentos llamados 'Ventas' (Lista: 1, 5)
SELECT ID_Depto FROM Departamentos WHERE NombreDepto = 'Ventas';

-- 2. Consulta principal (usando la lista)
SELECT Nombre
FROM Empleados
WHERE ID_Depto IN (
    -- Subconsulta de múltiples filas: devuelve una lista de IDs
    SELECT ID_Depto
    FROM Departamentos
    WHERE NombreDepto = 'Ventas'
);
```

---

## 2. Tipos de Subconsultas por su Ubicación

### C. Subconsultas Correlacionadas

Una subconsulta es **correlacionada** si se refiere a una columna de la tabla de la consulta principal. En esencia, la subconsulta **se ejecuta una vez por cada fila** procesada por la consulta principal.

- **Uso:** Resuelve problemas complejos de "por grupo".
    
- **Ejemplo:** _Encontrar el producto más caro dentro de **cada** categoría._
    

SQL

```
SELECT P1.Nombre, P1.Precio, P1.Categoria
FROM Productos P1
WHERE P1.Precio = (
    -- La subconsulta correlacionada se ejecuta por cada fila de P1
    SELECT MAX(P2.Precio)
    FROM Productos P2
    WHERE P2.Categoria = P1.Categoria -- <-- Correlación: usa P1.Categoria
);
```

_(Para la primera fila de `P1`, la subconsulta encuentra el máximo de esa categoría. Luego, el `WHERE` principal verifica si el precio de `P1` coincide con ese máximo)._

### D. Subconsultas en la Cláusula `FROM` (Tablas Derivadas)

Cuando una subconsulta se coloca en la cláusula `FROM`, su resultado se trata como una **tabla temporal** (llamada **Tabla Derivada**) para el resto de la consulta. **Siempre** deben tener un alias.

- **Uso:** Simplifica consultas complejas y permite aplicar funciones de agregación sobre resultados de agregaciones previas.
    
- **Ejemplo:** _Calcular el promedio de los totales de ventas que son superiores a 500._ (Se requiere el total de ventas antes de promediar).
    

SQL

```
SELECT AVG(Ventas_Netas) AS Promedio_Ventas_Altas
FROM (
    -- Tabla Derivada: Primero calculamos las ventas por cliente
    SELECT SUM(Total) AS Ventas_Netas
    FROM Pedidos
    GROUP BY ID_Cliente
    HAVING SUM(Total) > 500 -- Solo clientes con más de 500 en ventas
) AS VentasFiltradas; -- ¡El alias (VentasFiltradas) es obligatorio!
```

---

## 3. Subconsultas con Operadores `EXISTS` y `NOT EXISTS`

Los operadores `EXISTS` y `NOT EXISTS` son muy eficientes y se utilizan para verificar la **presencia** o **ausencia** de filas en el resultado de una subconsulta correlacionada.

- **Resultado:** Devuelve `TRUE` o `FALSE`.
    
- **Ventaja:** El SGBD puede dejar de buscar tan pronto como encuentra la primera coincidencia, lo que a menudo es más rápido que usar un `JOIN` o `IN`.
    
- **Ejemplo:** _Encontrar todos los clientes que **sí** han hecho al menos un pedido._
    

SQL

```
SELECT Nombre, Email
FROM Clientes C
WHERE EXISTS (
    -- Subconsulta: ¿Existe ALGÚN pedido (P) cuyo ID_Cliente coincida con el cliente actual (C)?
    SELECT 1 -- No importa qué seleccionas, solo importa si devuelve filas
    FROM Pedidos P
    WHERE P.ID_Cliente = C.ID_Cliente -- Correlación
);
```

### Ejemplo `NOT EXISTS`

- _Encontrar todos los clientes que **nunca** han hecho un pedido (clientes inactivos)._
    

SQL

```
SELECT Nombre, Email
FROM Clientes C
WHERE NOT EXISTS (
    -- Si no existe NINGÚN pedido con este ID_Cliente, entonces lo incluye
    SELECT 1
    FROM Pedidos P
    WHERE P.ID_Cliente = C.ID_Cliente
);
```

---

## 🛠️ Buenas Prácticas y Rendimiento

1. **Prioriza `JOINs` sobre Subconsultas:** Aunque las subconsultas son potentes, los `JOINs` a menudo son más legibles y, en muchos SGBD, más rápidos, especialmente con grandes volúmenes de datos. Usa subconsultas solo cuando un `JOIN` no pueda resolver el problema (ej: `HAVING` anidado o la necesidad de un escalar).
    
2. **Evita `SELECT *` en Subconsultas:** Siempre proyecta solo las columnas que necesitas. Si usas `EXISTS`, usa `SELECT 1`.
    
3. **Alias Obligatorios:** Las tablas derivadas (subconsultas en `FROM`) siempre deben llevar un alias.
    

---

## ✅ Cierre: Puntos Clave

- Una **Subconsulta** es un `SELECT` anidado que provee un _input_ a la consulta principal.
    
- **Escalares:** Devuelven un valor único (usados en `SELECT`, `WHERE`).
    
- **Múltiples Filas:** Devuelven una lista (usados con `IN`, `NOT IN`).
    
- **Correlacionadas:** Se ejecutan una vez por fila de la consulta principal y usan una columna de esta.
    
- **Tablas Derivadas:** Subconsultas en la cláusula `FROM` que actúan como tablas temporales (deben tener alias).
    
- **`EXISTS` / `NOT EXISTS`** son operadores eficientes para verificar la presencia/ausencia de datos, a menudo más rápidos que `IN`.
    

---

**Próximo Paso:** Con las herramientas de consulta dominadas (`SELECT`, `JOIN`, Subconsultas), volvemos al diseño para optimizar la estructura de nuestra base de datos. La siguiente clase es fundamental para la integridad y el rendimiento: la **Normalización**.

[[11 - Normalización y desnormalización]]