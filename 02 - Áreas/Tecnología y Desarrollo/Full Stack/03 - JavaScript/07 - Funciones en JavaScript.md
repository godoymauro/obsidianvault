¡En marcha con el Nivel 2! Has dominado los fundamentos. El siguiente paso es aprender a **organizar y reutilizar** tu código de manera eficiente, y la herramienta central para esto son las **funciones**.

---

Aquí tienes los apuntes de la clase anterior: [[06 - Estructuras de control]]

# 07 - Funciones en JavaScript

## ✍️ Introducción: Recetas de Cocina Reutilizables

Piensa en una **función** como una **receta de cocina** 🧑‍🍳.

1. Tiene un **nombre** (ej: _Hacer Tarta de Manzana_).
    
2. Necesita unos **ingredientes** (parámetros/entradas).
    
3. Contiene una serie de **pasos** (el código).
    
4. Produce un **resultado** (valor de retorno).
    

El gran valor de las funciones es la **reutilización**: escribes la receta una vez, y puedes "llamarla" (ejecutarla) mil veces sin tener que escribir los pasos de nuevo.

---

## 💻 Explicación Detallada: Definición, Partes y Tipos

Una función es un bloque de código diseñado para realizar una tarea particular.

### 1. Partes de una Función

|Parte|Descripción|Palabra Clave|
|---|---|---|
|**Declaración**|Indica que se va a crear una función.|`function`|
|**Nombre**|Un nombre descriptivo (`camelCase` recomendado) para invocarla.|`calcularArea`|
|**Parámetros**|Las variables que actúan como "ingredientes" que la función necesita para trabajar.|`(ancho, alto)`|
|**Cuerpo**|El bloque de código que se ejecuta, encerrado entre llaves `{}`.|`{ ... }`|
|**Retorno**|La palabra clave que devuelve el resultado de la función.|`return`|

Exportar a Hojas de cálculo

### 2. Formas de Definir Funciones

Hay tres formas principales de crear funciones en JavaScript moderno:

#### A) Declaración de Función (Function Declaration)

Es la forma clásica. Se beneficia del _hoisting_ (elevación), lo que significa que puedes llamarla incluso antes de que aparezca en el código.

JavaScript

```
// 1. Declaración
function saludar(nombre) { // 'nombre' es un parámetro
    // Cuerpo de la función
    return `Hola, ${nombre}. Bienvenido al curso.`; // Valor de retorno
}

// 2. Llamada o Invocación
const mensaje = saludar("Estudiante Experto"); // "Estudiante Experto" es el argumento
console.log(mensaje); // Muestra: "Hola, Estudiante Experto. Bienvenido al curso."
```

#### B) Expresión de Función (Function Expression)

La función se define como parte de una expresión (se asigna a una variable). Esta forma **no se beneficia del _hoisting_**, por lo que debes declararla antes de usarla.

JavaScript

```
// La función (que no tiene nombre en sí) se asigna a la constante 'sumar'
const sumar = function(a, b) {
    return a + b;
};

// Llamada
console.log(sumar(5, 3)); // Muestra: 8
```

#### C) Funciones Flecha (Arrow Functions - ES6+) 🏹

Es la sintaxis más moderna, compacta y limpia. Se usa la flecha `=>` en lugar de la palabra `function`. Se usa muy a menudo, especialmente en el Nivel 2 y 3.

JavaScript

```
// Sintaxis compacta y moderna
const multiplicar = (num1, num2) => {
    return num1 * num2;
};

// Si la función solo tiene una línea y lo único que hace es retornar, 
// puedes simplificar aún más (retorno implícito):
const duplicar = num => num * 2; // Paréntesis del parámetro omitidos si es solo uno

console.log(multiplicar(4, 5)); // Muestra: 20
console.log(duplicar(10)); // Muestra: 20
```

👉 Ver también [[18 - ES6+ y nuevas características]]

### 3. Alcance (Scope) de las Variables

El **Scope** es el **área de visibilidad** de una variable. Define dónde puedes acceder a ella.

- **Scope Global:** Variables declaradas **fuera** de cualquier función. Son accesibles desde cualquier parte del código. (¡Evítalo para mantener el código limpio!)
    
- **Scope Local (o de Función/Bloque):** Variables declaradas **dentro** de una función o un bloque (`{}`). Solo son accesibles dentro de ese ámbito.
    

JavaScript

```
const variableGlobal = "Soy global"; // Scope Global

function miFuncion() {
    // variableLocal solo existe dentro de esta función
    const variableLocal = "Soy local"; 
    console.log(variableGlobal); // Sí puedo acceder a la global
}

miFuncion();

// console.log(variableLocal); // ¡ERROR! No puedes acceder a una variable local desde afuera.
```

---

## 🛠️ Ejemplos Prácticos: Usando Parámetros y Retorno

Crea un archivo `clase07.js` y ejecútalo con Node.js.

JavaScript

```
// 1. Función Declarada con Parámetros y Retorno
function calcularIva(montoSinIva, tasaIva = 0.21) { 
    // Si no se proporciona 'tasaIva', usará el valor por defecto de 0.21
    const ivaCalculado = montoSinIva * tasaIva;
    const montoFinal = montoSinIva + ivaCalculado;
    return montoFinal;
}

// Llamada 1: Usando el valor por defecto
let precioLaptop = 1000;
let precioFinal1 = calcularIva(precioLaptop);
console.log(`Precio final de Laptop (IVA 21%): $${precioFinal1}`); // Muestra: 1210

// Llamada 2: Sobrescribiendo el valor por defecto (ej: IVA reducido)
let precioLibro = 50;
let precioFinal2 = calcularIva(precioLibro, 0.10);
console.log(`Precio final de Libro (IVA 10%): $${precioFinal2}`); // Muestra: 55

console.log("-------------------");

// 2. Función Flecha (Arrow Function) para verificar un límite
const verificarLimite = (saldo, limite) => {
    if (saldo > limite) {
        return true;
    } 
    return false;
    
    // O aún más corto con operador ternario (ver Clase 05):
    // return saldo > limite ? true : false;
};

let saldoCuenta = 5000;
let limiteAlerta = 4500;

let alerta = verificarLimite(saldoCuenta, limiteAlerta);
console.log("¿Supera el límite de alerta?", alerta); // Muestra: true
```

---

## ✅ Resumen de Puntos Clave

- Una **función** es un bloque de código reutilizable que realiza una tarea específica.
    
- **Parámetros** son los "ingredientes" (entradas) que la función necesita; **Argumentos** son los valores que se pasan al llamar la función.
    
- La palabra clave **`return`** devuelve el resultado de la función y detiene su ejecución.
    
- Hay tres formas de definir funciones: **Declaración** (`function nombre()`), **Expresión** (`const nombre = function()`) y **Flecha** (`const nombre = () =>`).
    
- Las **Funciones Flecha** (`=>`) son la sintaxis moderna y compacta (ES6+).
    
- El **Scope** define dónde son visibles las variables. Las variables locales (dentro de la función) no son accesibles desde fuera.
    

---

Ahora que ya puedes crear tus propias recetas de código, la siguiente clase se centrará en el tipo de dato que más a menudo procesarás: las listas ordenadas, o **Arrays**.

👉 Continuar con [[08 - Arrays en profundidad]]