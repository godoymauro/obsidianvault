¡Adelante! Con los motores listos, es hora de aprender el lenguaje universal de las bases de datos relacionales: **SQL**. Empezaremos por las operaciones fundamentales para manipular los datos.

---

# 06 - Sintaxis SQL

## 🗣️ ¿Qué es SQL?

**SQL** (_Structured Query Language_ o Lenguaje de Consulta Estructurado) es el lenguaje estándar para interactuar con bases de datos relacionales. No es un lenguaje de programación completo (como Python o Java), sino un **lenguaje declarativo**: tú le dices al SGBD _qué_ quieres lograr, y el SGBD determina _cómo_ ejecutarlo.

SQL se divide en varias categorías de comandos, pero nos enfocaremos en la más crítica:

- **DML** (_Data Manipulation Language_): Comandos para manipular los datos (nuestro foco principal en esta clase).
    
- **DDL** (_Data Definition Language_): Comandos para definir la estructura (tablas, esquemas, índices).
    
- **DCL** (_Data Control Language_): Comandos para gestionar permisos y seguridad.
    
- **TCL** (_Transaction Control Language_): Comandos para gestionar transacciones (`COMMIT`, `ROLLBACK`).
    

---

## 💾 DML: Manipulación de Datos (CRUD)

El corazón de SQL son las cuatro operaciones fundamentales para trabajar con datos, conocidas como **CRUD**: **C**rear, **R**eer, **U**pdate y **D**elete.

|Operación|Sigla|Comando SQL|Propósito|
|---|---|---|---|
|**C**rear|Insert|`INSERT`|Agregar nuevas filas (registros) a una tabla.|
|**R**eer|Select|`SELECT`|Consultar y recuperar datos.|
|**U**pdate|Update|`UPDATE`|Modificar datos existentes en las filas.|
|**D**elete|Delete|`DELETE`|Eliminar filas de una tabla.|

Exportar a Hojas de cálculo

### Preparación del Escenario (DDL Rápido)

Antes de practicar el DML, necesitamos una tabla. Usaremos un comando **DDL** (`CREATE TABLE`).

SQL

```
-- 1. CREAR la tabla de ejemplo (Válido para MySQL y PostgreSQL)
CREATE TABLE Productos (
    ID INT PRIMARY KEY AUTO_INCREMENT, -- O SERIAL en PostgreSQL
    Nombre VARCHAR(100) NOT NULL,
    Precio DECIMAL(10, 2) NOT NULL,
    Stock INT DEFAULT 0
);
-- Nota: En PostgreSQL, usa ID SERIAL PRIMARY KEY; y elimina AUTO_INCREMENT.
```

---

## 1. CREATE (Crear Datos): El Comando `INSERT`

El comando `INSERT` agrega una nueva tupla (fila) a la relación (tabla).

### Sintaxis Completa

SQL

```
INSERT INTO NombreTabla (Columna1, Columna2, Columna3, ...)
VALUES (Valor1, Valor2, Valor3, ...);
```

### Ejemplos Prácticos

SQL

```
-- Ejemplo 1: Especificando todas las columnas
INSERT INTO Productos (Nombre, Precio, Stock)
VALUES ('Laptop Gaming X', 1200.00, 15);

-- Ejemplo 2: Insertando múltiples filas en una sola instrucción (Más eficiente)
INSERT INTO Productos (Nombre, Precio, Stock)
VALUES
    ('Teclado Mecánico', 85.50, 50),
    ('Monitor 4K', 450.00, 10);
```

---

## 2. READ (Leer Datos): El Comando `SELECT`

El comando `SELECT` es el más utilizado y versátil. Es el comando que implementa las operaciones de **Proyección** (π) y **Selección** (σ) del [[04 - Álgebra relacional y fundamentos teóricos|Álgebra Relacional]].

### Sintaxis Básica

SQL

```
SELECT Columna1, Columna2, ...
FROM NombreTabla
-- Cláusulas opcionales que veremos en las próximas clases
[WHERE condicion]
[ORDER BY columna]
```

### Ejemplos Prácticos

SQL

```
-- Ejemplo 1: Obtener TODAS las columnas y TODAS las filas
SELECT *
FROM Productos;

-- Ejemplo 2: Obtener solo el Nombre y el Precio (Proyección)
SELECT Nombre, Precio
FROM Productos;

-- Ejemplo 3: Usar alias para las columnas (Mejora la legibilidad)
SELECT Nombre AS Producto, Precio AS Costo
FROM Productos;
```

---

## 3. UPDATE (Actualizar Datos): El Comando `UPDATE`

El comando `UPDATE` modifica los valores de las columnas en filas existentes.

### Sintaxis

SQL

```
UPDATE NombreTabla
SET Columna1 = NuevoValor1, Columna2 = NuevoValor2, ...
[WHERE condicion]; -- ¡La cláusula WHERE es CRÍTICA aquí!
```

### Ejemplos Prácticos

SQL

```
-- Ejemplo 1: Aumentar el precio del producto con ID 1 en un 10%
UPDATE Productos
SET Precio = Precio * 1.10
WHERE ID = 1;

-- Ejemplo 2: Poner el stock de "Teclado Mecánico" a 60
UPDATE Productos
SET Stock = 60
WHERE Nombre = 'Teclado Mecánico';

-- ⚠️ ERROR COMÚN: Olvidar la cláusula WHERE. ¡Modificas TODAS las filas!
UPDATE Productos
SET Stock = 0; -- ¡Esto pone el stock de TODOS los productos a cero! ¡Cuidado!
```

---

## 4. DELETE (Eliminar Datos): El Comando `DELETE`

El comando `DELETE` elimina filas enteras de una tabla.

### Sintaxis

SQL

```
DELETE FROM NombreTabla
[WHERE condicion]; -- ¡La cláusula WHERE también es CRÍTICA aquí!
```

### Ejemplos Prácticos

SQL

```
-- Ejemplo 1: Eliminar el producto con ID 3
DELETE FROM Productos
WHERE ID = 3;

-- Ejemplo 2: Eliminar todos los productos cuyo Stock sea 0
DELETE FROM Productos
WHERE Stock = 0;

-- ⚠️ ERROR COMÚN: Olvidar la cláusula WHERE. ¡Eliminas TODAS las filas!
DELETE FROM Productos; -- ¡Esto vacía COMPLETAMENTE la tabla!
```

> 📝 **Nota sobre `TRUNCATE` vs `DELETE`:** Para eliminar _todos_ los registros, `DELETE` es más lento porque registra cada eliminación. `TRUNCATE TABLE NombreTabla;` es un comando **DDL** más rápido que elimina todos los datos y reinicia el contador de auto-incremento, pero no permite la reversión mediante transacciones.

---

## 🛠️ Buenas Prácticas y Errores Comunes

|Categoría|Buena Práctica|Error Común|
|---|---|---|
|**Prueba**|Siempre usa `SELECT` con el `WHERE` que planeas usar antes de ejecutar un `UPDATE` o un `DELETE`.|Ejecutar `UPDATE` o `DELETE` sin la cláusula `WHERE`, borrando/modificando datos críticos.|
|**Integridad**|Usar la sintaxis de `INSERT` que especifica las columnas (`INSERT INTO Tabla (C1, C2)`).|Asumir el orden de las columnas (`INSERT INTO Tabla VALUES (...)`) y romper el _script_ si cambia el esquema.|
|**Lectura**|Usar el alias `AS` para hacer las columnas de resultado más legibles.|Dejar nombres de columna genéricos o muy largos que confunden al usuario o a la aplicación.|

Exportar a Hojas de cálculo

---

## ✅ Cierre: Puntos Clave

- **SQL** es un lenguaje **declarativo** estándar para manejar bases de datos relacionales.
    
- Las operaciones **DML** fundamentales son el **CRUD**: **C**rear (`INSERT`), **R**eer (`SELECT`), **U**pdate (`UPDATE`) y **D**elete (`DELETE`).
    
- La cláusula **`WHERE`** es esencial para el `SELECT`, `UPDATE` y `DELETE`, ya que especifica qué filas serán afectadas, evitando cambios o lecturas masivas no deseadas.
    
- **`SELECT *`** recupera todas las columnas; lo ideal es especificar solo las columnas necesarias para mejorar el rendimiento (Proyección).
    

---

**Próximo Paso:** Ya sabemos manipular los datos. Ahora, aprenderemos a transformarlos y analizarlos utilizando **Operadores y Funciones** como `SUM`, `AVG` y `COUNT`.

[[07 - Operadores y funciones]]