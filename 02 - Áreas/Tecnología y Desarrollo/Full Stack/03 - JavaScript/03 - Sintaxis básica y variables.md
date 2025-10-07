Con el entorno configurado, es hora de escribir código real. En esta clase, aprenderemos el abecé de JavaScript: cómo se estructura el código y cómo empezamos a guardar información.

---

Aquí tienes los apuntes de la clase anterior: [[02 - Configuración del entorno]]

# 03 - Sintaxis básica y variables

## ✍️ Introducción: Las Reglas del Lenguaje

Cada lenguaje, ya sea el español, el inglés o JavaScript, tiene reglas. Estas reglas se llaman **sintaxis**. Si rompemos la sintaxis, el programa no entiende lo que le pedimos y arroja un error.

En esta lección, nos enfocaremos en:

1. La sintaxis básica (cómo se escriben las instrucciones y los comentarios).
    
2. La herramienta fundamental para almacenar datos: las **variables** (`var`, `let`, `const`).
    

---

## 💻 Explicación Detallada: Sintaxis y Contenedores de Datos

### 1. Instrucciones y Terminadores

En JavaScript, cada **instrucción** (una orden que le das a la computadora) debe terminar con un **punto y coma** (`;`).

JavaScript

```
// Una instrucción simple: mostrar un mensaje
console.log("¡Hola!"); 

// Otra instrucción: una operación matemática
var suma = 5 + 3; 

// Es una buena práctica usar siempre el punto y coma, aunque JS a veces lo infiera.
// La omisión puede causar errores difíciles de encontrar.
```

### 2. Comentarios: Notas para el Futuro

Los **comentarios** son líneas de código que el motor de JavaScript **ignora por completo**. Son vitales para dejar notas, explicar por qué se escribió cierta parte del código o desactivar temporalmente una línea.

|Tipo de Comentario|Símbolo|Uso|
|---|---|---|
|**Línea Única**|`//`|Para explicaciones cortas o notas rápidas.|
|**Bloque**|`/* ... */`|Para comentarios extensos que ocupan varias líneas.|

Exportar a Hojas de cálculo

JavaScript

```
// Esto es un comentario de línea única, la computadora lo ignora.
var precio = 100; // Aquí explico qué almaceno.

/*
Este es un comentario de bloque.
Puedo usarlo para desactivar un segmento grande de código
o para dar una explicación detallada sobre una función compleja.
*/
```

### 3. Variables: Cajas para Guardar Información

Una **variable** es un espacio en la memoria de la computadora que usamos para **almacenar información** (datos). Piensa en ellas como **etiquetas** que ponemos en **cajas** para poder recuperar su contenido más tarde.

Antes de usar una variable, debemos **declararla**. JavaScript moderno nos ofrece tres formas de declarar variables: `var`, `let`, y `const`.

|Palabra Clave|Uso|¿Se puede reasignar?|Ámbito (Scope)|
|---|---|---|---|
|**`var`**|**(Anticuada)** No se recomienda para código nuevo. Tiene problemas de ámbito.|Sí|Función (Function Scope)|
|**`let`**|**(Recomendada)** Para valores que _van a cambiar_ (ej: un contador, la edad de alguien).|Sí|Bloque (Block Scope)|
|**`const`**|**(Recomendada)** Para valores que _no van a cambiar_ (ej: el valor de Pi, una contraseña fija).|**NO**|Bloque (Block Scope)|

Exportar a Hojas de cálculo

#### **`let` (Variable reasignable)**

Usamos `let` cuando sabemos que el valor de la variable cambiará en el futuro.

JavaScript

```
// 1. Declaración e inicialización
let puntaje = 0; 
console.log(puntaje); // Muestra: 0

// 2. Reasignación (el valor cambia)
puntaje = 100;
console.log(puntaje); // Muestra: 100
```

#### **`const` (Constante, no reasignable)**

Usamos `const` cuando el valor debe permanecer fijo durante toda la ejecución del programa. Si intentas cambiarla, JavaScript te dará un error.

JavaScript

```
// 1. Declaración e inicialización (requerido)
const TASA_IMPUESTO = 0.16;
console.log(TASA_IMPUESTO); // Muestra: 0.16

// 2. Intento de reasignación (¡ERROR!)
// TASA_IMPUESTO = 0.18; 
// Si descomentas la línea de arriba, verás un error en la consola: 
// "Assignment to constant variable."
```

👉 **Regla de oro:** Empieza siempre declarando con `const`. Si más tarde te das cuenta de que necesitas cambiar el valor, cámbiala a `let`. **Evita `var`.**

### 4. Nombres de Variables (¡Convenciones!)

El nombre de una variable debe ser descriptivo. Hay reglas y convenciones de estilo que debes seguir:

|Regla|Descripción|Ejemplo|
|---|---|---|
|**Regla 1 (Legal)**|No pueden empezar con un número.|`let 1nombre;` (Ilegal)|
|**Regla 2 (Legal)**|No pueden contener espacios ni guiones.|`let primer nombre;` (Ilegal)|
|**Regla 3 (Legal)**|Son sensibles a mayúsculas/minúsculas.|`Edad` es diferente de `edad`.|
|**Convención (Estilo)**|Usa **`camelCase`** (la primera palabra minúscula, las siguientes empiezan con mayúscula).|`let nombreCompletoUsuario;`|

Exportar a Hojas de cálculo

---

## 🛠️ Ejemplo Práctico: Usando `let` y `const`

Abre tu VSCode, crea un archivo `clase03.js` y ejecútalo con Node.js (`node clase03.js`).

JavaScript

```
// Declaramos constantes (valores que no cambiarán)
const PI = 3.14159;
const URL_API = "https://miservidor.com/data";

// Un ejemplo de uso de const
let radio = 5;
const areaCirculo = PI * radio * radio; // PI es fija.

console.log("Área calculada:", areaCirculo); 
console.log("-------------------");

// Declaramos variables reasignables con let
let contadorTareas = 0; // Inicialmente, no hay tareas.
let nombreUsuario = "Invitado";

console.log("Contador inicial:", contadorTareas);
console.log("Usuario actual:", nombreUsuario);

// El programa avanza y los valores cambian (reasignación)
contadorTareas = contadorTareas + 3; // ¡Se han añadido 3 tareas!
nombreUsuario = "Alejandro Pérez"; // El usuario ha iniciado sesión.

console.log("-------------------");
console.log("Nuevo contador:", contadorTareas); // Muestra: 3
console.log("Nuevo usuario:", nombreUsuario); // Muestra: Alejandro Pérez
```

---

## ✅ Resumen de Puntos Clave

- La **sintaxis** son las reglas que rigen la escritura del código.
    
- Cada instrucción debe terminar con un **punto y coma** (`;`).
    
- Los **comentarios** (`//` o `/* */`) son ignorados por el motor y sirven para documentar el código.
    
- Las **variables** son contenedores con etiquetas para guardar datos.
    
- **`const`** se usa para valores fijos (constantes).
    
- **`let`** se usa para valores que cambiarán (variables).
    
- Se debe usar la convención **`camelCase`** para nombrar variables.
    
- **Evita** usar `var` en código nuevo.
    

---

Ahora que sabes cómo etiquetar y guardar información, el siguiente paso es entender _qué tipo_ de información puedes guardar.

👉 Continuar con [[04 - Tipos de datos en JavaScript]]