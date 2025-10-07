Ya sabes cómo guardar información usando `let` y `const`. Ahora, aprenderemos _qué tipos_ de información puede manejar JavaScript. Esto es crucial, porque la forma en que el lenguaje trata un número es muy diferente a cómo trata un texto.

---

Aquí tienes los apuntes de la clase anterior: [[03 - Sintaxis básica y variables]]

# 04 - Tipos de datos en JavaScript

## ✍️ Introducción: La Clasificación de la Información

En la vida real, no es lo mismo un número de teléfono que un nombre, o un valor de "verdadero/falso". En programación, clasificamos estos valores con **tipos de datos**. JavaScript tiene una naturaleza flexible (es un lenguaje de tipado débil y dinámico), lo que significa que no tienes que decirle explícitamente a una variable qué tipo de dato guardará, pero internamente, el motor sí clasifica todo.

En esta clase, exploraremos los dos grandes grupos de tipos de datos: **Primitivos** y **Estructurados**.

---

## 💻 Explicación Detallada: Tipos Primitivos y Estructurados

### 1. Tipos de Datos Primitivos (Los Fundamentales)

Los tipos primitivos son la base. Son inmutables (no se pueden modificar directamente), y cuando se copian, se copia el valor en sí.

|Tipo Primitivo|Descripción|Ejemplos|
|---|---|---|
|**`String`**|Secuencias de caracteres (texto). Se encierran entre comillas (`""`, `''` o `` ` ``).|`"Hola Mundo"`, `'JS'`, `` `Template` ``|
|**`Number`**|Números, incluyen tanto enteros como decimales.|`10`, `3.14`, `-500`|
|**`Boolean`**|Solo dos valores posibles: **`true`** (verdadero) o **`false`** (falso).|`true`, `false`|
|**`Undefined`**|El valor asignado automáticamente a una variable que ha sido declarada, pero a la que _aún no se le ha asignado un valor_.|`let x;` // x es `undefined`|
|**`Null`**|Representa la **ausencia intencional** de cualquier valor de objeto. Se asigna de forma explícita.|`let datos = null;`|
|**`Symbol`** (ES6+)|Se usa para crear identificadores únicos (lo veremos en avanzado).|`Symbol('id')`|
|**`BigInt`** (ES2020+)|Se usa para números enteros extremadamente grandes que exceden el límite de `Number`.|`9007199254740991n`|

Exportar a Hojas de cálculo

#### **`String` en Detalle**

Puedes usar comillas simples, dobles o _backticks_ (comillas invertidas). Las _backticks_ se usan para crear **Template Strings** (cadenas de plantilla) que permiten incrustar variables directamente, lo cual es muy útil (ver [[18 - ES6+ y nuevas características]]):

JavaScript

```
const nombre = "Alice";
const edad = 30;

// Opción clásica (concatenación)
const saludoClasico = "Hola, mi nombre es " + nombre + " y tengo " + edad + " años.";

// Template String (moderno y legible)
const saludoModerno = `Hola, mi nombre es ${nombre} y tengo ${edad} años.`; 

console.log(saludoModerno); 
```

### 2. Tipos de Datos Estructurados (Referencia)

A diferencia de los primitivos, estos son contenedores de información más complejos. Se les llama de **referencia** porque cuando copias una variable de este tipo, no copias el valor en sí, sino una "referencia" o un puntero a la ubicación en memoria donde está el dato.

|Tipo Estructurado|Descripción|Ejemplos|
|---|---|---|
|**`Object`**|La base de todo lo complejo. Un contenedor de pares clave-valor.|`{ nombre: "Ana", edad: 25 }`|
|**`Array`**|Un tipo especial de objeto para almacenar una lista ordenada de valores.|`[1, 2, 3, "texto"]`|
|**`Function`**|El bloque de código ejecutable (técnicamente, una función es un tipo de objeto).|`function() { ... }`|

Exportar a Hojas de cálculo

#### **`Object` en Detalle**

Los objetos se usan para modelar entidades de la vida real con sus propiedades.

JavaScript

```
const persona = {
    // clave (propiedad): valor
    nombre: "Carlos", 
    profesion: "Desarrollador",
    tieneTrabajo: true,
    edad: 45
};

// Accediendo a las propiedades:
console.log(persona.nombre);      // Muestra: "Carlos"
console.log(persona.tieneTrabajo); // Muestra: true 
```

👉 Ver también [[09 - Objetos y POO en JavaScript]]

#### **`Array` en Detalle**

Los arrays son listas que nos permiten guardar varios valores bajo una única variable. Cada elemento tiene un **índice** (posición), que siempre empieza en **cero (0)**.

JavaScript

```
const colores = ["rojo", "verde", "azul"];

// Accediendo al primer elemento (índice 0)
console.log(colores[0]); // Muestra: "rojo"

// El array es un tipo de objeto, y podemos ver cuántos elementos tiene:
console.log(colores.length); // Muestra: 3
```

👉 Ver también [[08 - Arrays en profundidad]]

### 3. El Operador `typeof`

¿Cómo sé de qué tipo es un valor? JavaScript nos da una herramienta: el operador `typeof`.

JavaScript

```
let texto = "Hola";
let numero = 2024;
let esMayor = false;
let usuario = { id: 1 };
let lista = ["A", "B"];

console.log(typeof texto);   // Muestra: "string"
console.log(typeof numero);  // Muestra: "number"
console.log(typeof esMayor); // Muestra: "boolean"

// ¡Ojo! Hay una peculiaridad histórica de JS
console.log(typeof null);    // Muestra: "object" (es un error histórico del lenguaje, pero debes saberlo)

console.log(typeof usuario); // Muestra: "object"
console.log(typeof lista);   // Muestra: "object" (Arrays son objetos especiales)
```

---

## 🛠️ Ejemplo Práctico: Declaración de Tipos

Crea un archivo `clase04.js` y prueba la declaración de diferentes tipos:

JavaScript

```
// 1. Strings (Texto)
const nombreProducto = 'Laptop Gamer X1';
const descripcion = `Ideal para programar y jugar. Incluye ${nombreProducto}.`;

console.log(nombreProducto);
console.log(descripcion);
console.log("-------------------");

// 2. Numbers (Números)
const precio = 1250.99;
const unidadesVendidas = 50;

console.log(`Precio Total: ${precio * unidadesVendidas}`); // Multiplicación

// 3. Boolean (Verdadero o Falso)
let enStock = true;
let agotado = false;

console.log("¿Hay stock?", enStock); 

if (enStock) {
    console.log("¡Puedes comprar!");
} else {
    console.log("Producto no disponible.");
}

console.log("-------------------");

// 4. Undefined y Null
let miVariable; // Declarada, pero sin valor
console.log("Mi variable (sin asignar):", miVariable); // Muestra undefined

miVariable = null; // Asignamos intencionalmente la ausencia de valor
console.log("Mi variable (asignada a null):", miVariable); // Muestra null
```

---

## ✅ Resumen de Puntos Clave

- **Tipo de dato** es la clasificación de la información (texto, número, etc.).
    
- Hay dos grandes grupos: **Primitivos** (inmutables, copia por valor) y **Estructurados** (mutables, copia por referencia).
    
- Los Primitivos clave son: **`String`**, **`Number`**, **`Boolean`**, **`Undefined`**, y **`Null`**.
    
- Usa **Template Strings** (` `` `) para construir texto de forma más limpia con variables incrustadas (`${variable}`).
    
- Los Estructurados clave son: **`Object`** (pares clave-valor) y **`Array`** (listas ordenadas).
    
- El operador **`typeof`** te permite verificar el tipo de dato de cualquier valor.
    

---

Con los tipos de datos dominados, el siguiente paso es aprender a manipularlos, combinarlos y compararlos. Para eso, necesitamos los operadores.

👉 Continuar con [[05 - Operadores y expresiones]]