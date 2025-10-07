¡Estupendo! Hemos dominado las operaciones dentro de una tabla. Ahora, vamos a la verdadera clave de las bases de datos relacionales: **combinar información de múltiples tablas**. Esto se logra con la cláusula **`JOIN`**.

---

# 09 - Joins

## 🤝 ¿Qué son los Joins?

Un **`JOIN`** es una operación SQL que combina filas de dos o más tablas basándose en una columna o condición relacionada entre ellas.

En términos del [[03 - Modelo entidad-relación|Modelo Entidad-Relación]], los `JOINs` son la forma en que el lenguaje SQL implementa las **relaciones (1:1, 1:N, N:M)** que definimos usando **claves foráneas (FK)**.

### Preparación del Escenario

Usaremos dos tablas con una relación **Uno a Muchos (1:N)**: Un departamento tiene muchos empleados.

|Tabla: **Departamentos**|Tabla: **Empleados**|
|---|---|
|`ID_Depto` (PK)|`ID_Empleado` (PK)|
|`NombreDepto`|`Nombre`|
||`ID_Depto` (FK)|

**Datos de Ejemplo:**

|ID_Depto|NombreDepto||ID_Empleado|Nombre|ID_Depto|
|---|---|---|---|---|---|
|1|Ventas||101|Ana|1|
|2|Marketing||102|Bruno|2|
|3|I+D||103|Carlos|1|
|4|Contabilidad||104|David|_NULL_|
||||105|Elena|5|

---

## 1. INNER JOIN (Reunión Interna)

El `INNER JOIN` es el tipo más común. Devuelve solo las filas que tienen **valores coincidentes** en ambas tablas. Las filas que no tienen coincidencia en la otra tabla se descartan.

### Diagrama Conceptual

### Sintaxis

SQL

```
SELECT columnas_deseadas
FROM TablaA
INNER JOIN TablaB ON TablaA.FK = TablaB.PK;
```

### Ejemplo Práctico

SQL

```
-- Queremos una lista de empleados Y el nombre de su departamento.
-- El empleado David (ID_Depto NULL) y el Depto Contabilidad (sin empleados) NO aparecerán.

SELECT
    E.Nombre,
    D.NombreDepto
FROM Empleados E -- Usamos alias E
INNER JOIN Departamentos D ON E.ID_Depto = D.ID_Depto;
```

|Nombre|NombreDepto|
|---|---|
|Ana|Ventas|
|Bruno|Marketing|
|Carlos|Ventas|
|Elena|_Ninguna Coincidencia_|

_(El resultado **solo** incluirá a Ana, Bruno y Carlos, porque Elena tiene `ID_Depto` 5, que no existe en la tabla `Departamentos`, y `Contabilidad` no tiene empleados asignados.)_

---

## 2. LEFT JOIN (Reunión Izquierda)

El `LEFT JOIN` (o `LEFT OUTER JOIN`) devuelve **todas las filas de la tabla izquierda** (la primera tabla mencionada en `FROM`), y las filas coincidentes de la tabla derecha.

Si no hay coincidencia en la tabla derecha, las columnas de la tabla derecha se rellenan con **`NULL`**.

### Diagrama Conceptual

### Ejemplo Práctico

SQL

```
-- Queremos TODOS los empleados, y si tienen departamento, su nombre.
-- El empleado David (ID_Depto NULL) y Elena (ID_Depto 5) aparecerán.

SELECT
    E.Nombre,
    D.NombreDepto
FROM Empleados E -- Esta es la tabla IZQUIERDA
LEFT JOIN Departamentos D ON E.ID_Depto = D.ID_Depto;
```

|Nombre|NombreDepto|
|---|---|
|Ana|Ventas|
|Bruno|Marketing|
|Carlos|Ventas|
|**David**|**NULL**|
|**Elena**|**NULL**|

---

## 3. RIGHT JOIN (Reunión Derecha)

El `RIGHT JOIN` (o `RIGHT OUTER JOIN`) es el espejo del `LEFT JOIN`. Devuelve **todas las filas de la tabla derecha**, y las filas coincidentes de la tabla izquierda.

Si no hay coincidencia en la tabla izquierda, sus columnas se rellenan con **`NULL`**.

### Diagrama Conceptual

### Ejemplo Práctico

SQL

```
-- Queremos TODOS los departamentos, y si tienen empleados, sus nombres.
-- El Depto Contabilidad (sin empleados) aparecerá.

SELECT
    D.NombreDepto,
    E.Nombre
FROM Empleados E
RIGHT JOIN Departamentos D ON E.ID_Depto = D.ID_Depto; -- Esta es la tabla DERECHA
```

|NombreDepto|Nombre|
|---|---|
|Ventas|Ana|
|Marketing|Bruno|
|Ventas|Carlos|
|I+D|NULL|
|**Contabilidad**|**NULL**|

---

## 4. FULL JOIN (Reunión Completa)

El `FULL JOIN` (o `FULL OUTER JOIN`) devuelve todas las filas cuando hay una coincidencia en una de las tablas. Combina los resultados del `LEFT` y `RIGHT JOIN`. Las filas sin coincidencia se rellenan con `NULL`.

### Diagrama Conceptual

### Ejemplo Práctico (Combinando todos los casos)

SQL

```
-- Queremos TODOS los empleados Y TODOS los departamentos.
-- Aparecerán David (Empleado sin Depto), Elena (sin Depto) y Contabilidad (Depto sin Empleados).

SELECT
    E.Nombre,
    D.NombreDepto
FROM Empleados E
FULL OUTER JOIN Departamentos D ON E.ID_Depto = D.ID_Depto;
```

---

## 🛠️ Buenas Prácticas y Estrategias con Joins

1. **Usar Alias (Muy Recomendado):** Siempre usa alias cortos (`E`, `D`) para las tablas (`FROM Empleados E`) y referencia las columnas con el alias (`E.Nombre`). Esto evita ambigüedades cuando ambas tablas tienen columnas con el mismo nombre (ej: `ID`).
    
2. **La Cláusula `ON` es Esencial:** Define **explícitamente** la condición de la relación, generalmente `TablaA.FK = TablaB.PK`.
    
3. **Filtrar `NULL` con `WHERE` para Exclusiones:**
    
    - Para obtener solo los empleados **sin** departamento, usa un `LEFT JOIN` y filtra el resultado:
        
        SQL
        
        ```
        SELECT E.Nombre FROM Empleados E
        LEFT JOIN Departamentos D ON E.ID_Depto = D.ID_Depto
        WHERE D.ID_Depto IS NULL; -- ¡Esto filtra solo las filas sin coincidencia!
        ```
        

---

## ✅ Cierre: Puntos Clave

- Los **`JOINs`** son la herramienta para combinar datos de múltiples tablas usando una condición relacionada (usualmente una clave foránea).
    
- **`INNER JOIN`:** Solo devuelve filas con **coincidencias** en ambas tablas.
    
- **`LEFT JOIN`:** Devuelve **todas las filas de la izquierda** (y las coincidentes de la derecha, rellenando con `NULL` si no hay).
    
- **`RIGHT JOIN`:** Devuelve **todas las filas de la derecha** (y las coincidentes de la izquierda, rellenando con `NULL` si no hay).
    
- Siempre especifica la condición de unión usando la cláusula **`ON`**.
    

---

**Próximo Paso:** Hemos aprendido a combinar tablas horizontalmente con `JOINs`. Ahora, aprenderemos a anidar consultas, una técnica fundamental para resolver problemas complejos: las **Subconsultas**.

[[10 - Subconsultas]]