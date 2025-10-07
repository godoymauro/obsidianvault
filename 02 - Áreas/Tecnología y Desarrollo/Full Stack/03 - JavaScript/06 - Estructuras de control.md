Hemos llegado al final de los fundamentos. En esta clase, cerraremos el Nivel 1 aprendiendo a controlar el **flujo** de tu programa, que es cómo hacemos que la computadora tome **decisiones** y ejecute tareas **repetitivas**.

---

Aquí tienes los apuntes de la clase anterior: [[05 - Operadores y expresiones]]

# 06 - Estructuras de control

## ✍️ Introducción: Dando Órdenes Condicionales

Hasta ahora, tu código se ejecuta línea por línea, de arriba abajo. Las **estructuras de control** son herramientas que cambian este flujo. Nos permiten decirle al programa: "Si pasa **A**, haz **X**; si no, haz **Y**" (condicionales) o "Haz **Z** cien veces" (bucles).

Aquí cubriremos los dos tipos principales:

1. **Condicionales:** `if/else` y `switch`.
    
2. **Bucles (Loops):** `for`, `while`, `do...while`, `for...of`, `for...in`.
    

---

## 💻 Explicación Detallada: Condicionales (Decisiones)

Las estructuras condicionales se basan en la evaluación de una expresión a un valor **booleano** (`true` o `false`), que aprendimos en la clase anterior (ver [[05 - Operadores y expresiones]]).

### 1. `if` y `else`

Es la estructura de decisión más fundamental.

#### **Sintaxis Básica (`if`):**

JavaScript

```
if (condicion_es_verdadera) {
    // Código que se ejecuta SOLO si la condición es TRUE
}
```

#### **Sintaxis Completa (`if/else`):**

JavaScript

```
if (es_de_noche) {
    console.log("Enciendo la luz.");
} else {
    // Código que se ejecuta SOLO si la condición es FALSE
    console.log("Apago la luz.");
}
```

#### **Múltiples Condiciones (`if/else if/else`):**

Para evaluar varias posibilidades, usamos `else if`. El programa prueba las condiciones de arriba abajo y ejecuta solo el **primer bloque** que es `true`, ignorando el resto.

JavaScript

```
const puntuacion = 85;

if (puntuacion >= 90) {
    console.log("Calificación: A");
} else if (puntuacion >= 80) {
    console.log("Calificación: B"); // Esta se ejecuta para 85
} else if (puntuacion >= 70) {
    console.log("Calificación: C");
} else {
    console.log("Calificación: F");
}
```

### 2. `switch`

El `switch` es una alternativa limpia y legible a una cadena larga de `if/else if/else` cuando se evalúa una **única variable** contra múltiples **valores específicos**.

JavaScript

```
const diaSemana = 3; // 1=Lunes, 2=Martes, etc.

switch (diaSemana) {
    case 1:
        console.log("Inicio de semana: Lunes.");
        break; // IMPORTANTE: El 'break' detiene la ejecución del switch.
    case 5:
        console.log("Viernes. ¡Casi fin de semana!");
        break;
    case 6: // Múltiples casos pueden compartir el mismo código si omites el break
    case 7:
        console.log("Fin de semana.");
        break;
    default:
        // Si no coincide con NINGÚN 'case'
        console.log("Día de trabajo normal.");
        break;
}
```

---

## 💻 Explicación Detallada: Bucles (Repetición)

Los **bucles** (o ciclos) se usan para ejecutar el mismo bloque de código múltiples veces. Son esenciales para procesar listas de datos (arrays).

### 1. Bucle `for` (El más común)

Se usa cuando sabes de antemano **cuántas veces** debe repetirse el código (ej: 10 veces, o la longitud de un array).

|Parte|Descripción|
|---|---|
|**Inicialización**|`let i = 0;` Se ejecuta _solo una vez_ al principio. Define un contador.|
|**Condición**|`i < 10;` Se evalúa _antes_ de cada iteración. Si es `true`, sigue; si es `false`, el bucle termina.|
|**Expresión final**|`i++` (o `i = i + 1;`) Se ejecuta _después_ de cada iteración. Suele ser para aumentar el contador.|

Exportar a Hojas de cálculo

#### **Sintaxis `for`:**

JavaScript

```
// Contamos del 0 al 9 (10 veces)
for (let i = 0; i < 10; i++) { 
    console.log("El número actual es: " + i);
}
```

### 2. Bucle `while` (Condición incierta)

Se usa cuando la repetición depende de una condición que puede cambiar **dentro** del bucle, y no sabemos de antemano cuántas veces se ejecutará.

JavaScript

```
let contador = 0;

while (contador < 3) {
    console.log("Mientras sea menor que 3: " + contador);
    contador++; // ESENCIAL: Debemos cambiar la condición para evitar un bucle infinito
}
// Cuando contador llega a 3, la condición es false y el bucle termina.
```

### 3. Bucle `do...while` (Ejecución al menos una vez)

Similar a `while`, pero la condición se verifica al final. Esto garantiza que el código **dentro del `do` se ejecute al menos una vez**, incluso si la condición es falsa desde el inicio.

JavaScript

```
let intentos = 0;
let contraseñaCorrecta = false;

do {
    // Este código se ejecuta la primera vez, ¡incluso si 'contraseñaCorrecta' es falso!
    console.log("Intento número: " + (intentos + 1));
    intentos++;
    // Aquí iría el código para verificar la contraseña...
} while (contraseñaCorrecta === true && intentos < 3);

// En este caso, se ejecutará una vez y luego se detendrá porque la condición es falsa.
```

### 4. Bucles Específicos para Arrays y Objetos (Iteradores Modernos)

JavaScript moderno tiene bucles optimizados para colecciones de datos.

|Bucle|Uso principal|¿Qué obtiene?|
|---|---|---|
|**`for...of`**|Recorrer elementos de **Arrays** (o Mapas, Sets).|El **valor** de cada elemento.|
|**`for...in`**|Recorrer propiedades de **Objetos**.|El **nombre** (clave) de cada propiedad.|

Exportar a Hojas de cálculo

JavaScript

```
const frutas = ["Manzana", "Pera", "Mango"];
const datosUsuario = { nombre: "Leo", pais: "Chile", edad: 35 };

// for...of (Recorre la lista y obtiene el VALOR)
for (const fruta of frutas) {
    console.log(`Compraré una ${fruta}.`);
}

// for...in (Recorre el OBJETO y obtiene la CLAVE)
for (const clave in datosUsuario) {
    console.log(`Clave: ${clave}, Valor: ${datosUsuario[clave]}`);
}
```

👉 Ver también [[08 - Arrays en profundidad]] y [[09 - Objetos y POO en JavaScript]]

---

## 🛠️ Ejemplos Prácticos: Aplicando Control

Crea un archivo `clase06.js` y ejecútalo con Node.js.

JavaScript

```
// Ejemplo 1: Condicionales y Lógica (if/else)
const hora = 15; // Las 3 de la tarde

if (hora < 12) {
    console.log("Buenos días.");
} else if (hora >= 12 && hora < 18) { // Usamos el operador Lógico AND (&&)
    console.log("Buenas tardes.");
} else {
    console.log("Buenas noches.");
}

console.log("-------------------");

// Ejemplo 2: Bucle for para Array
const listaClientes = ["Maria", "Juan", "Pedro", "Luisa"];

console.log("Iniciando envío de correo a clientes:");
// i < listaClientes.length asegura que no intentemos leer fuera del array
for (let i = 0; i < listaClientes.length; i++) {
    const cliente = listaClientes[i]; // Accedemos al valor usando el índice 'i'
    console.log(`Enviando a [${i}]: ${cliente}`);
}

console.log("-------------------");

// Ejemplo 3: Bucle for...of (más limpio para arrays)
console.log("Usando for...of:");
for (const cliente of listaClientes) {
    console.log(`Procesando cliente: ${cliente}`);
}
```

---

## ✅ Resumen de Puntos Clave

- Las **estructuras de control** dirigen el flujo de ejecución del código.
    
- **`if/else`** y **`else if`** se usan para tomar decisiones basadas en condiciones booleanas.
    
- **`switch`** es útil para evaluar una única variable contra múltiples valores fijos (**`case`**). Siempre usa **`break`** para evitar que el código siga ejecutándose en el siguiente caso.
    
- Los **bucles** se usan para la repetición de tareas.
    
- **`for`** es ideal cuando se conoce el número exacto de repeticiones.
    
- **`while`** repite mientras una condición sea `true` (cuidado con los bucles infinitos).
    
- **`for...of`** es la forma moderna y recomendada para iterar sobre los **valores** de un array.
    
- **`for...in`** se usa para iterar sobre las **claves** (propiedades) de un objeto.
    

---

¡Felicidades! Has completado el Nivel 1. Dominas los fundamentos: variables, tipos de datos, operadores y estructuras de control.

Ahora, pasaremos al Nivel 2, donde la complejidad aumenta y aprenderemos a organizar nuestro código en bloques reutilizables.

👉 Continuar con [[07 - Funciones en JavaScript]]