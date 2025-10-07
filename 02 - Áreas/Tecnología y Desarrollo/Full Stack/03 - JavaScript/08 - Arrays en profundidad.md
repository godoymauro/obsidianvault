Acabas de aprender a crear bloques de código reutilizables con funciones. Ahora, vamos a aplicar ese poder a la estructura de datos más usada en JavaScript: las listas, conocidas como **Arrays**.

---

Aquí tienes los apuntes de la clase anterior: [[07 - Funciones en JavaScript]]

# 08 - Arrays en profundidad

## ✍️ Introducción: Listas Ordenadas y Acciones Rápidas

En la [[04 - Tipos de datos en JavaScript]], aprendimos que un **Array** es una lista ordenada de valores. Piensa en un Array como una **lista de supermercado** 🛒.

1. Cada artículo (valor) tiene una **posición** específica.
    
2. La posición (llamada **índice**) siempre comienza en **0**.
    
3. Puedes guardar cualquier tipo de dato en la misma lista (números, textos, incluso otras listas).
    

Dominar los métodos de Array es lo que realmente te convertirá en un desarrollador JavaScript eficiente.

---

## 💻 Explicación Detallada: Conceptos Clave y Métodos de Mutación

### 1. Creación y Acceso

Los Arrays se crean usando corchetes `[]` y sus elementos se separan por comas.

JavaScript

```
// Un Array heterogéneo (contiene diferentes tipos de datos)
const inventario = ["Laptop", 1200.50, true, null]; 

// Acceder a un elemento por su índice (la posición)
console.log(inventario[0]); // Muestra: "Laptop" (índice 0)
console.log(inventario[2]); // Muestra: true (índice 2)

// Obtener la longitud del Array
console.log(inventario.length); // Muestra: 4
```

> **Nota de Índice:** Para acceder al **último elemento**, puedes usar: `array[array.length - 1]`.

### 2. Métodos que Mutan (Cambian el Array Original)

Estos métodos modifican el Array en el que se aplican.

|Método|Descripción|Ejemplo|Resultado|
|---|---|---|---|
|**`push()`**|Añade uno o más elementos **al final** del Array.|`colores.push("Amarillo")`|`["Rojo", "Verde", "Azul", "Amarillo"]`|
|**`pop()`**|**Elimina** y **devuelve** el último elemento del Array.|`colores.pop()`|Devuelve `"Azul"`, el array ahora es `["Rojo", "Verde"]`|
|**`unshift()`**|Añade uno o más elementos **al principio** del Array.|`colores.unshift("Morado")`|`["Morado", "Rojo", "Verde", "Azul"]`|
|**`shift()`**|**Elimina** y **devuelve** el primer elemento del Array.|`colores.shift()`|Devuelve `"Rojo"`, el array ahora es `["Verde", "Azul"]`|
|**`splice()`**|El más potente y complejo. Permite **añadir**, **eliminar** o **reemplazar** elementos en cualquier posición.|`array.splice(indiceInicio, numElementosAEliminar, elementosAAgregar...)`||

Exportar a Hojas de cálculo

JavaScript

```
let lista = ["A", "B", "C", "D"];
// Ejemplo de splice: Desde el índice 1, elimina 2 elementos, y añade "X"
lista.splice(1, 2, "X"); 
console.log(lista); // Muestra: ["A", "X", "D"]
```

---

## 💻 Explicación Detallada: Métodos de Iteración (Funciones de Orden Superior)

Estas son las funciones más importantes para procesar colecciones de datos. Se les llama de **Orden Superior** porque **reciben otra función** como argumento (el _callback_).

|Método|Propósito|¿Mutan?|¿Qué devuelven?|
|---|---|---|---|
|**`forEach()`**|**Recorrer** el Array. Ejecuta una función para cada elemento.|No|`undefined` (No devuelve nada)|
|**`map()`**|**Transformar** el Array. Crea un **nuevo Array** con los resultados de aplicar una función a cada elemento.|No|Nuevo Array|
|**`filter()`**|**Filtrar** el Array. Crea un **nuevo Array** solo con los elementos que cumplen una condición.|No|Nuevo Array|
|**`find()`**|**Buscar** un elemento. Devuelve el **primer** elemento que cumpla una condición.|No|El elemento o `undefined`|
|**`reduce()`**|**Reducir** el Array. Aplica una función para reducir todos los elementos a un único valor (ej: una suma).|No|Un único valor|

Exportar a Hojas de cálculo

### El Patrón `map`, `filter`, `reduce` (¡Inmutabilidad!)

Estos tres métodos son la base del desarrollo moderno porque fomentan la **inmutabilidad**: en lugar de modificar el Array original, **siempre crean uno nuevo**. Esto hace que el código sea más predecible y seguro.

#### A) `map()`: Transformación

JavaScript

```
const numeros = [2, 4, 6];

// Quiero un nuevo Array con cada número al cuadrado
const cuadrados = numeros.map(num => num * num);

console.log(cuadrados); // Muestra: [4, 16, 36]
console.log(numeros);   // El Array original NO cambia: [2, 4, 6]
```

#### B) `filter()`: Selección

JavaScript

```
const edades = [15, 22, 17, 30];

// Quiero un nuevo Array solo con mayores de edad (>= 18)
const mayoresEdad = edades.filter(edad => edad >= 18);

console.log(mayoresEdad); // Muestra: [22, 30]
```

#### C) `reduce()`: Acumulación

JavaScript

```
const precios = [10, 25, 40];

// Quiero sumar todos los precios para obtener un total
// 'acumulador' guarda la suma en cada paso
const total = precios.reduce((acumulador, precioActual) => {
    return acumulador + precioActual;
}, 0); // El '0' es el valor inicial del acumulador

console.log(total); // Muestra: 75
```

---

## 🛠️ Ejemplos Prácticos: Array Completo

Crea un archivo `clase08.js` y ejecútalo con Node.js.

JavaScript

```
const productos = [
    { nombre: "Mouse", precio: 25, stock: 10 },
    { nombre: "Teclado", precio: 80, stock: 0 },
    { nombre: "Monitor", precio: 200, stock: 5 },
    { nombre: "Webcam", precio: 40, stock: 15 }
];

// 1. forEach: Recorrer e imprimir
console.log("--- 1. forEach (Recorrido simple) ---");
productos.forEach(producto => {
    console.log(`Producto: ${producto.nombre} - $${producto.precio}`);
});

// 2. map: Obtener solo los nombres
console.log("\n--- 2. map (Transformación) ---");
const nombres = productos.map(p => p.nombre);
console.log(nombres); // Muestra: ["Mouse", "Teclado", "Monitor", "Webcam"]

// 3. filter: Obtener solo productos en stock
console.log("\n--- 3. filter (Filtro de condición) ---");
const enStock = productos.filter(p => p.stock > 0);
console.log(enStock); 
/* Muestra: 
[
  { nombre: "Mouse", precio: 25, stock: 10 },
  { nombre: "Monitor", precio: 200, stock: 5 },
  { nombre: "Webcam", precio: 40, stock: 15 }
]
*/

// 4. find: Encontrar un producto específico
console.log("\n--- 4. find (Búsqueda del primero) ---");
const monitor = productos.find(p => p.nombre === "Monitor");
console.log("Monitor encontrado:", monitor); // Devuelve el objeto { nombre: "Monitor", ... }

// 5. reduce: Calcular el valor total de todo el inventario (precio * stock)
console.log("\n--- 5. reduce (Acumulación total) ---");
const valorTotalInventario = productos.reduce((acumulador, p) => {
    return acumulador + (p.precio * p.stock);
}, 0); // Empezamos la suma en 0

console.log(`Valor total del inventario: $${valorTotalInventario}`); // (25*10) + (80*0) + (200*5) + (40*15) = 250 + 0 + 1000 + 600 = 1850
```

---

## ✅ Resumen de Puntos Clave

- Los **Arrays** son listas ordenadas de valores, indexadas desde **0**.
    
- Los métodos de **mutación** (`push()`, `pop()`, `shift()`, `unshift()`, `splice()`) modifican el Array original.
    
- Los métodos de **iteración** (`map()`, `filter()`, `reduce()`, `forEach()`, `find()`) son **funciones de orden superior** que toman otra función como argumento.
    
- **`map()`** se usa para crear un nuevo Array **transformado**.
    
- **`filter()`** se usa para crear un nuevo Array **filtrado** por una condición.
    
- **`reduce()`** se usa para **acumular** un único valor a partir de todos los elementos del Array.
    
- La buena práctica moderna es usar los métodos de iteración para mantener la **inmutabilidad** (no cambiar el Array original).
    

---

Ahora que puedes gestionar listas de datos, el siguiente paso es la estructura de datos más importante y flexible: el **Objeto**, que es la base de la Programación Orientada a Objetos (POO).

👉 Continuar con [[09 - Objetos y POO en JavaScript]]