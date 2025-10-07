Saber manejar errores es la defensa, pero escribir código limpio es el ataque. En esta clase, definiremos el estilo y los principios que distinguen el código funcional del código profesional y mantenible.

---

Aquí tienes los apuntes de la clase anterior: [[16 - Errores y depuración]]

# 17 - Buenas prácticas de código

## ✍️ Introducción: Escribir para Personas

El código se ejecuta en una computadora, pero lo leen otros programadores (¡incluido tu "yo" del futuro!). Las **buenas prácticas** son un conjunto de reglas y convenciones que buscan hacer el código más **legible**, **mantenible**, **escalable** y con menos **errores lógicos**.

Si dos programadores leen el mismo código, deberían entenderlo de la misma manera. Esto se logra con la uniformidad y el cumplimiento de principios clave de la ingeniería de software.

---

## 💻 Explicación Detallada: Principios de la Ingeniería de Software

Existen varios principios que guían la escritura de código limpio. Aquí nos centraremos en los más influyentes en JavaScript:

### 1. DRY (Don't Repeat Yourself) 🚫

Este es quizás el principio más importante. **Evita la duplicación de código**. Si te encuentras copiando y pegando el mismo bloque de código tres veces, ese bloque necesita ser refactorizado (reorganizado) en una **función** (ver [[07 - Funciones en JavaScript]]).

|MALO (WET - Write Everything Twice)|BUENO (DRY)|
|---|---|
|`console.log("Error al cargar 1");` `console.log("Error al cargar 2");`|`function logError(id) { console.error("Error al cargar " + id); }` `logError(1); logError(2);`|

Exportar a Hojas de cálculo

### 2. KISS (Keep It Simple, Stupid) 🧠

Busca siempre la solución más simple y directa. **Evita la complejidad innecesaria**. Si puedes lograr algo con una sola función `filter()` (ver [[08 - Arrays en profundidad]]), no lo hagas con un bucle `for` de diez líneas anidadas.

- **Pregúntate:** ¿Un desarrollador junior podría entender esta lógica en menos de 5 minutos? Si la respuesta es no, probablemente necesite simplificación.
    

### 3. Modularidad y Responsabilidad Única (Single Responsibility Principle - SRP) 🧱

Este principio establece que **cada función o módulo debe tener una sola razón para cambiar**.

- Una función debe hacer **una sola cosa**, y hacerla bien.
    
    - **MALO:** Una función que `descargaDatos`, `calculaImpuestos` y `actualizaDOM`.
        
    - **BUENO:** Tres funciones separadas: `descargarDatos()`, `calcularImpuestos(datos)` y `actualizarInterfaz(resultado)`.
        

---

## 💻 Explicación Detallada: Convenciones y Estilo de Código

### 1. Nombres Descriptivos y Convenciones de Nomenclatura 🏷️

Los nombres de tus variables, funciones y clases son tu principal herramienta de documentación. Deben ser claros y **autodocumentar** el código.

|Elemento|Convención|Ejemplo BUENO|Ejemplo MALO|
|---|---|---|---|
|**Variables**|`camelCase` (empezar con minúscula)|`nombreCompleto`, `contadorClics`|`nc`, `x`, `contador_clics`|
|**Funciones**|`camelCase` (deben ser verbos)|`obtenerDatosUsuario()`, `actualizarSaldo()`|`usuarioDatos()`, `saldo`|
|**Constantes**|`UPPER_SNAKE_CASE` (todo mayúsculas, guion bajo)|`TASA_IVA`, `MAX_INTENTOS`|`tasaIva`, `MaxIntentos`|
|**Clases**|`PascalCase` (empezar con mayúscula)|`Usuario`, `CarritoCompra`|`usuario`, `carrito_compra`|

Exportar a Hojas de cálculo

### 2. Comentarios Útiles y JSDoc 📝

Los comentarios (ver [[03 - Sintaxis básica y variables]]) deben explicar el **por qué** del código, no el **qué**.

- **Evita:** Comentar lo obvio. `// sumar 1 al contador`.
    
- **Prefiere:** Explicar decisiones complejas o efectos secundarios. `// Se usa 'setTimeout' para evitar un error de renderizado en navegadores antiguos.`
    

Para funciones importantes, usa **JSDoc**, un estándar que te permite documentar formalmente los parámetros y el valor de retorno.

JavaScript

```
/**
 * Calcula el monto final de una compra aplicando el IVA.
 * @param {number} subtotal El precio base sin impuestos.
 * @param {number} tasa La tasa de impuesto a aplicar (ej: 0.21).
 * @returns {number} El monto total a pagar.
 */
function calcularMontoTotal(subtotal, tasa) {
    return subtotal * (1 + tasa);
}
```

### 3. Estructura y Formato (Identación y Espacios)

- **Indentación:** Usa siempre 2 o 4 espacios (nunca tabulaciones, a menos que tu equipo lo exija) para anidar bloques de código.
    
- **Espacios:** Usa espacios alrededor de operadores (`+`, `=`, `===`) y después de comas.
    

JavaScript

```
// MALO: Difícil de leer
if(saldo>100){console.log('ok')} 

// BUENO: Legible
if (saldo > 100) {
    console.log('ok');
}
```

> **Herramienta Útil:** Usa un **Linter** (como ESLint) y un **Formateador** (como Prettier) en VSCode. Estas herramientas fuerzan automáticamente estas buenas prácticas de formato, haciendo que tu código siempre luzca profesional.

### 4. Uso Consistente de `const` y `let`

Como aprendimos en [[03 - Sintaxis básica y variables]]:

- **Usa `const` por defecto.** Esto evita que reasignes variables accidentalmente y hace que el lector sepa inmediatamente que ese valor no va a cambiar.
    
- **Solo usa `let`** cuando sepas que el valor será reasignado (ej: contadores de bucles, saldos que cambian).
    

---

## 🛠️ Ejemplo Práctico: Refactorización a Buenas Prácticas

Aquí tienes un ejemplo de código "malo" refactorizado a uno "bueno" aplicando los principios.

JavaScript

```
// CÓDIGO MALO (No DRY, nombres pobres, formato descuidado)
let s = 1000;
let iva = 0.21;
let c = true;

if (c) {
    s = s - 50;
    s = s * (1 + iva);
    console.log("Total:", s);
} else {
    s = s * (1 + iva);
    console.log("Total:", s);
}
// Fin del código
```

JavaScript

```
// CÓDIGO BUENO (DRY, Nombres descriptivos, Modularidad)

// Constante en UPPER_SNAKE_CASE
const TASA_IVA = 0.21;

/**
 * Calcula el total final aplicando el IVA.
 * (Responsabilidad Única)
 * @param {number} subtotal 
 * @returns {number} Monto final
 */
function calcularTotalConIVA(subtotal) {
    return subtotal * (1 + TASA_IVA);
}

// Variables con nombres claros
let saldoCuenta = 1000;
const esClienteVIP = true;
const MONTO_DESCUENTO = 50;

if (esClienteVIP) {
    // Si es VIP, aplicamos el descuento
    saldoCuenta -= MONTO_DESCUENTO;
}

// Reutilizamos la función para el cálculo final (DRY)
const montoFinal = calcularTotalConIVA(saldoCuenta);

console.log(`Monto final calculado: ${montoFinal.toFixed(2)}`); // toFixed(2) para 2 decimales
// Fin del código
```

---

## ✅ Resumen de Puntos Clave

- **DRY (Don't Repeat Yourself)**: Evita la duplicación de código usando funciones.
    
- **KISS (Keep It Simple, Stupid)**: Busca siempre la solución más simple.
    
- **SRP (Single Responsibility Principle)**: Cada función debe hacer una sola cosa.
    
- Usa **nombres descriptivos** que auto-documenten el código (ej: `obtenerUsuario` vs `oU`).
    
- Sigue las convenciones: `camelCase` para variables/funciones, `PascalCase` para clases y `UPPER_SNAKE_CASE` para constantes.
    
- Los **comentarios** deben explicar el _por qué_ de la lógica, no el _qué_ obvio. Usa JSDoc para documentar funciones clave.
    
- **Usa `const` por defecto** y `let` solo si el valor necesita ser reasignado.
    
- Usa **Formateadores** (como Prettier) para mantener un estilo consistente.
    

---

Ahora que tu código es profesional, vamos a dotarlo de las características que hacen a JavaScript un lenguaje moderno.

👉 Continuar con [[18 - ES6+ y nuevas características]]