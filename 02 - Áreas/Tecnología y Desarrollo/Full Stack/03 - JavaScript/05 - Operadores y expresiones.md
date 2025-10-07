Ya sabes qué tipos de datos existen. Ahora, vamos a aprender a _hacer cosas_ con esos datos: sumar, comparar, asignar y tomar decisiones. Para eso, usamos los **operadores**.

---

Aquí tienes los apuntes de la clase anterior: [[04 - Tipos de datos en JavaScript]]

# 05 - Operadores y expresiones

## ✍️ Introducción: Manipulando los Datos

Un **operador** es un símbolo que le dice al motor de JavaScript que realice algún tipo de operación. Una **expresión** es cualquier pieza de código que produce un valor (por ejemplo, `5 + 3` es una expresión, y su resultado es `8`).

En esta clase, cubriremos los operadores más comunes, agrupados por su función: Aritméticos, de Asignación, de Comparación y Lógicos.

---

## 💻 Explicación Detallada: Tipos de Operadores

### 1. Operadores Aritméticos (Matemáticas Básicas)

Estos operadores se usan para realizar cálculos matemáticos con valores de tipo `Number`.

|Operador|Nombre|Descripción|Ejemplo|Resultado|
|---|---|---|---|---|
|**`+`**|Suma|Suma dos números.|`10 + 5`|`15`|
|**`-`**|Resta|Resta dos números.|`10 - 5`|`5`|
|**`*`**|Multiplicación|Multiplica dos números.|`10 * 5`|`50`|
|**`/`**|División|Divide dos números.|`10 / 5`|`2`|
|**`%`**|Módulo (Resto)|Devuelve el resto de una división.|`10 % 3`|`1`|
|**`**`**|Exponenciación|Eleva un número a la potencia de otro.|`2 ** 3`|`8`|
|**`++`**|Incremento|Suma 1 a una variable (ej: `i++`).|`let i=1; i++`|`i` es `2`|
|**`--`**|Decremento|Resta 1 a una variable (ej: `i--`).|`let i=1; i--`|`i` es `0`|

Exportar a Hojas de cálculo

> **⚠️ Atención al `+`:** Si lo usas con _strings_, el operador `+` realiza una **concatenación** (unión de textos), no una suma.
> 
> JavaScript
> 
> ```
> console.log("Hola " + "Mundo"); // Muestra: "Hola Mundo"
> console.log("2" + 2); // ¡Muestra: "22"! JS lo trata como concatenación.
> ```

---

### 2. Operadores de Asignación

El operador de asignación principal es el signo igual (`=`). Sin embargo, existen atajos para realizar una operación aritmética y una asignación al mismo tiempo.

|Operador|Nombre|Descripción|Ejemplo|Equivale a...|
|---|---|---|---|---|
|**`=`**|Asignación|Asigna el valor de la derecha a la variable de la izquierda.|`let x = 10`||
|**`+=`**|Suma y Asignación|Suma un valor y reasigna el resultado.|`x += 5`|`x = x + 5`|
|**`-=`**|Resta y Asignación|Resta un valor y reasigna el resultado.|`x -= 2`|`x = x - 2`|
|**`*=`**|Multiplicación y Asignación|Multiplica por un valor y reasigna.|`x *= 3`|`x = x * 3`|
|**`/=`**|División y Asignación|Divide por un valor y reasigna.|`x /= 2`|`x = x / 2`|

Exportar a Hojas de cálculo

---

### 3. Operadores de Comparación (Devuelven un `Boolean`)

Estos operadores comparan dos valores y **siempre devuelven un valor booleano** (`true` o `false`). Son la base para tomar decisiones en el código (ver [[06 - Estructuras de control]]).

|Operador|Nombre|Descripción|Ejemplo|Resultado|
|---|---|---|---|---|
|**`==`**|Igualdad Débil|Compara solo el valor, ignorando el tipo de dato. **(No recomendado)**|`10 == "10"`|`true`|
|**`===`**|Igualdad Estricta|Compara el **valor** Y el **tipo de dato**.|`10 === "10"`|`false`|
|**`!=`**|Desigualdad Débil|¿Son diferentes (ignorando el tipo)? **(No recomendado)**|`10 != "10"`|`false`|
|**`!==`**|Desigualdad Estricta|¿Son diferentes el valor O el tipo?|`10 !== "10"`|`true`|
|**`>`**|Mayor que||`10 > 5`|`true`|
|**`<`**|Menor que||`10 < 5`|`false`|
|**`>=`**|Mayor o igual que||`10 >= 10`|`true`|
|**`<=`**|Menor o igual que||`10 <= 9`|`false`|

Exportar a Hojas de cálculo

> **💡 Regla de Oro:** **Usa siempre `===` y `!==` (estrictos).** Esto evita errores sutiles de conversión de tipos que son muy difíciles de depurar.

---

### 4. Operadores Lógicos (Combinando Condiciones)

Se usan para combinar o modificar los resultados `true`/`false` de múltiples comparaciones.

|Operador|Nombre|Descripción|Ejemplo|
|---|---|---|---|
|**`&&`**|AND (Y)|Devuelve `true` si **ambas** condiciones son `true`.|`(5 > 3) && (10 > 8)` → `true`|
|**`||`**|OR (O)|
|**`!`**|NOT (No)|Invierte el valor booleano (`true` se vuelve `false`, y viceversa).|`!(5 > 10)` → `true`|

Exportar a Hojas de cálculo

---

### 5. Operador Ternario (Condicional Corto)

Es un atajo para escribir una sentencia `if/else` simple en una sola línea.

**Sintaxis:** `condición ? valor_si_es_verdadero : valor_si_es_falso;`

JavaScript

```
const edad = 18;
const puedeVotar = edad >= 18 ? "Sí puede votar" : "No puede votar";

console.log(puedeVotar); // Muestra: "Sí puede votar"
```

---

## 🛠️ Ejemplos Prácticos: Aplicando Operadores

Crea un archivo `clase05.js` y ejecuta con Node.js.

JavaScript

```
// Usaremos let porque vamos a reasignar el valor
let saldo = 1000;
let compra = 250;
let esVip = true;

// 1. Aritméticos y Asignación
console.log(`Saldo inicial: ${saldo}`);

// Aplicamos un descuento por ser VIP (20% de descuento en la compra)
if (esVip) {
    compra *= 0.8; // Equivalente a: compra = compra * 0.8;
}

// Restamos el valor de la compra al saldo
saldo -= compra; // Equivalente a: saldo = saldo - compra;

console.log(`Compra con descuento: ${compra}`); // Muestra 200 (250 * 0.8)
console.log(`Saldo final: ${saldo}`); // Muestra 800 (1000 - 200)
console.log("-------------------");


// 2. Comparación Estricta (¡La buena práctica!)
const numero = 50;
const texto = "50";

console.log("¿Son iguales en valor (==)?", numero == texto); // true (¡Peligro de la igualdad débil!)
console.log("¿Son iguales en valor y tipo (===)?", numero === texto); // false (¡Correcto! Uno es number, el otro es string)
console.log("-------------------");


// 3. Lógicos
let tieneBeca = true;
let promedioAlto = 9;
let promedioRequerido = 8.5;

// ¿El estudiante gana la beca? (Debe tener beca Y promedio alto)
let calificaBeca = tieneBeca && (promedioAlto >= promedioRequerido);

console.log("Califica para beca:", calificaBeca); // true

// Operador Ternario
const estado = calificaBeca ? "Aprobado para la beca" : "No cumple los requisitos";
console.log("Resultado:", estado); 
```

---

## ✅ Resumen de Puntos Clave

- Los **operadores aritméticos** (`+`, `-`, `*`, `/`, `%`) realizan cálculos. Recuerda que `+` concatena _strings_.
    
- Los **operadores de asignación** (`=`, `+=`, `-=`, etc.) modifican el valor de una variable.
    
- Los **operadores de comparación** (`===`, `!==`, `>`, `<`) devuelven un `Boolean` (`true` o `false`).
    
- **Usa siempre la igualdad estricta (`===`)** para evitar conversiones de tipo inesperadas.
    
- Los **operadores lógicos** (`&&`, `||`, `!`) combinan condiciones.
    
- El **operador ternario** (`condición ? verdadero : falso`) es un atajo para decisiones sencillas.
    

---

Ya sabes cómo crear y manipular datos. El siguiente paso es usar los resultados de esas comparaciones para hacer que tu código tome **decisiones** y ejecute **acciones repetitivas**.

👉 Continuar con [[06 - Estructuras de control]]