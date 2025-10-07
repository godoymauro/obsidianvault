¡Excelente! Hemos llegado al Nivel 4, el tramo final que te transforma de un codificador a un desarrollador profesional. En esta clase, aprenderemos a lidiar con el lado oscuro del código: los **errores** y cómo combatirlos.

---

Aquí tienes los apuntes de la clase anterior: [[15 - Fetch API y AJAX]]

# 16 - Errores y depuración

## ✍️ Introducción: Los Errores son Normales

En programación, los errores no son un signo de fracaso; son una **realidad constante**. Un desarrollador pasa gran parte de su tiempo corrigiendo errores (**debugging**). Si sabes cómo identificar, categorizar y solucionar los errores de manera eficiente, serás un desarrollador mucho más valioso.

En esta lección, distinguiremos los tipos de errores, aprenderemos a prevenirlos con `try...catch` y usaremos la herramienta más importante de todas: la Consola del Navegador.

---

## 💻 Explicación Detallada: Tipos de Errores

En JavaScript, podemos clasificar los errores en tres grandes categorías:

### 1. Errores de Sintaxis (Syntax Errors)

Estos ocurren antes de que el código comience a ejecutarse. El motor de JavaScript no puede ni siquiera entender lo que has escrito porque has violado las reglas del lenguaje (ver [[03 - Sintaxis básica y variables]]).

- **Causa:** Olvidar una llave `}`, un paréntesis `(`, un punto y coma `;` donde es obligatorio, o escribir mal una palabra clave (`funtion` en lugar de `function`).
    
- **Resultado:** El programa se detiene completamente y el error se muestra al inicio.
    

JavaScript

```
// Ejemplo de Error de Sintaxis
const mensaje = "Hola mundo" // Falta el punto y coma

if (mensaje === "Hola mundo" { // Falta el paréntesis de cierre
    console.log("Error");
}
```

### 2. Errores de Referencia y Tipado (Runtime Errors)

Estos son los errores más comunes. Ocurren mientras el programa se está ejecutando (_runtime_). El motor entendió la sintaxis, pero cuando intenta ejecutar la instrucción, se encuentra con algo que no puede hacer.

- **`ReferenceError`:** Intentar usar una variable o función que no ha sido definida o que está fuera de su ámbito (**Scope**).
    
    JavaScript
    
    ```
    console.log(variableInexistente); // ReferenceError: variableInexistente is not defined
    ```
    
- **`TypeError`:** Intentar realizar una operación en un tipo de dato inapropiado (ej: llamar a un método de array en una variable que es `null`).
    
    JavaScript
    
    ```
    const datos = null;
    datos.map(item => item); // TypeError: Cannot read properties of null (reading 'map')
    ```
    

### 3. Errores Lógicos

Estos son los más difíciles de detectar. El código se ejecuta sin fallar, pero el **resultado es incorrecto**.

- **Causa:** Usar un operador de comparación incorrecto (ej: `>` en lugar de `>=`), un error en una fórmula matemática, o una condición de bucle incorrecta.
    
- **Resultado:** El programa funciona, pero el cliente ve un precio final erróneo o un dato incompleto. Se detectan principalmente mediante **pruebas** (ver [[19 - Testing en JavaScript]]).
    

---

## 💻 Explicación Detallada: Manejo de Errores con `try...catch`

Para prevenir que un error de ejecución detenga toda tu aplicación, debes envolver el código que podría fallar en un bloque **`try...catch`**.

#### **Estructura `try...catch`:**

JavaScript

```
try {
    // 1. Código que intentamos ejecutar.
    // Si ocurre un error aquí, la ejecución salta inmediatamente al bloque 'catch'.
    console.log("Intentando obtener datos...");
    const resultado = JSON.parse("Esto no es JSON válido"); // Esto lanza un SyntaxError de JSON

} catch (error) {
    // 2. Código que se ejecuta SOLO si hubo un error en el bloque 'try'.
    // La variable 'error' (puede ser cualquier nombre) contiene el objeto de error.
    console.error("¡Se capturó un error!");
    console.error("Tipo de Error:", error.name);
    console.error("Mensaje:", error.message);

} finally {
    // 3. (Opcional) Código que se ejecuta SIEMPRE, haya habido éxito o error.
    console.log("El bloque try/catch ha finalizado su ejecución.");
}
```

> **Uso Común:** Esto es vital al trabajar con **APIs** y `JSON.parse()`, ya que nunca sabes si los datos de entrada serán válidos (ver [[15 - Fetch API y AJAX]]).

---

## 💻 Explicación Detallada: Herramientas de Depuración

La forma más efectiva de encontrar errores no es adivinando, sino usando las herramientas que te da el navegador.

### 1. La Consola (`console`)

Es tu mejor amiga. Los métodos del objeto `console` (que vimos en [[02 - Configuración del entorno]]) te permiten "ver" lo que pasa dentro de tu código.

|Método de Consola|Propósito|Ejemplo|
|---|---|---|
|**`console.log()`**|Imprime una variable o mensaje.|`console.log('Valor:', miVariable);`|
|**`console.error()`**|Imprime un mensaje con formato de error.|`console.error('Fallo en la función X');`|
|**`console.warn()`**|Imprime un mensaje de advertencia (amarillo).|`console.warn('Función obsoleta.');`|
|**`console.table()`**|Muestra datos (especialmente arrays y objetos) en formato de tabla. **¡Súper útil!**|`console.table(listaDeUsuarios);`|

Exportar a Hojas de cálculo

### 2. El Debugger del Navegador

Es la herramienta más poderosa para la depuración, ya que te permite pausar el código y revisarlo línea por línea.

- **Punto de Interrupción (`Breakpoint`):** Es una parada intencional que colocas en tu código (en la pestaña **"Sources"** o **"Fuentes"** del navegador). Cuando el motor llega a esa línea, se detiene.
    
- **La Palabra Clave `debugger`:** Puedes insertar la palabra `debugger;` directamente en tu código JavaScript. Cuando el navegador se encuentra con esa línea y las herramientas de desarrollador están abiertas, el código se pausará automáticamente.
    

**Pasos para depurar con Breakpoints:**

1. Abre las Herramientas de Desarrollador (F12 o Ctrl+Shift+I).
    
2. Ve a la pestaña **Sources** (Fuentes).
    
3. Busca tu archivo JavaScript.
    
4. Haz clic en el número de línea donde quieres que el código se detenga (aparecerá un punto azul).
    
5. Recarga la página o ejecuta la función. Cuando el código se detiene, puedes:
    
    - Inspeccionar variables en tiempo real.
        
    - Usar los botones de "Step Over" (Saltar), "Step Into" (Entrar) y "Step Out" (Salir) para avanzar línea por línea.
        

---

## 🛠️ Ejemplos Prácticos: `try...catch` y Consola

Crea un archivo `clase16.js` y ejecútalo con Node.js.

JavaScript

```
function calcularYGuardar(jsonInput) {
    let datos = null;

    // 1. Bloque try...catch para manejar posibles errores en JSON.parse
    try {
        datos = JSON.parse(jsonInput);
        
        // 2. Manejamos un posible TypeError (si 'datos' fuera null o undefined)
        const total = datos.items.reduce((acc, item) => acc + item.precio, 0);
        
        console.log("Cálculo exitoso. Total:", total);
        
    } catch (e) {
        // e.name es el tipo de error (SyntaxError, TypeError, etc.)
        console.error(`ERROR en la función: ${e.name}`); 
        console.error(`Mensaje detallado: ${e.message}`);
        console.warn("Se usará un valor por defecto.");
        
        // Devolvemos un valor seguro en caso de fallo
        return { total: 0, items: [] }; 
    } finally {
        console.log("Proceso de cálculo finalizado, con o sin errores.");
    }

    return datos;
}

// Caso 1: JSON Válido (éxito)
console.log("--- Caso 1: Válido ---");
const jsonValido = '{"items": [{"precio": 10}, {"precio": 20}]}';
calcularYGuardar(jsonValido);

console.log("\n--- Caso 2: JSON Inválido (Syntax Error) ---");
// Caso 2: Error de sintaxis en JSON (lo capturamos)
const jsonInvalido = '{"items": [precio: 10}]}';
calcularYGuardar(jsonInvalido);
```

---

## ✅ Resumen de Puntos Clave

- Los errores de **Sintaxis** impiden la ejecución del código.
    
- Los errores de **Runtime** (`ReferenceError`, `TypeError`) ocurren durante la ejecución.
    
- El bloque **`try...catch`** se usa para capturar errores de ejecución, prevenir que el programa colapse y ejecutar una lógica de recuperación.
    
- El bloque **`finally`** se ejecuta siempre al terminar el `try` o el `catch`.
    
- El objeto **`error`** capturado contiene propiedades clave como **`error.name`** (tipo) y **`error.message`** (descripción).
    
- La **Consola** del navegador (`console.log`, `console.table`, `console.error`) es fundamental para inspeccionar valores.
    
- Los **Breakpoints** y la palabra clave **`debugger;`** son las herramientas más potentes para pausar el código y depurar paso a paso.
    

---

Ahora que sabes cómo limpiar tus errores, vamos a aprender a escribir código que tenga menos errores desde el principio, siguiendo las reglas de la profesión.

👉 Continuar con [[17 - Buenas prácticas de código]]