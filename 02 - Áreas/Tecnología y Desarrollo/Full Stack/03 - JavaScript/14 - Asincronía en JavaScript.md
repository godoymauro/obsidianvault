Hemos llegado al corazón de la programación moderna. En el mundo web, las tareas como cargar datos de un servidor tardan tiempo, y no queremos que la aplicación se congele mientras espera. El manejo de este tiempo de espera es la **Asincronía**.

---

Aquí tienes los apuntes de la clase anterior: [[13 - Manipulación avanzada del DOM]]

# 14 - Asincronía en JavaScript

## ✍️ Introducción: La Naturaleza de JavaScript

JavaScript es un lenguaje **single-threaded** (de un solo hilo). Esto significa que solo puede ejecutar **una tarea a la vez** en la secuencia principal.

Si tuviéramos que esperar sincrónicamente (en secuencia) por una solicitud de red que tarda 5 segundos, la página se "congelaría" y el usuario no podría hacer clic en nada.

La **Asincronía** es la capacidad de iniciar una tarea que tardará un tiempo y, en lugar de esperar, dejar que el hilo principal (el _thread_) continúe ejecutando otras tareas. Una vez que la tarea lenta finaliza, JavaScript nos notifica y ejecuta el código final.

Las tres etapas de la asincronía son: **Callbacks**, **Promesas** (la base moderna) y **Async/Await** (la sintaxis más limpia).

---

## 💻 Explicación Detallada: Evolución de la Asincronía

### 1. Callbacks (El Inicio)

Un **Callback** es simplemente una función que se pasa como argumento a otra función y se espera que se ejecute _más tarde_, cuando una tarea finalice.

JavaScript

```
// Ejemplo de una función que simula una operación lenta
function cargarDatos(url, callback) {
    console.log(`Iniciando carga de datos de ${url}...`);
    // Simulamos un retraso de 2 segundos
    setTimeout(() => { 
        const datos = { id: 1, mensaje: "Datos cargados" };
        callback(datos); // Llama al callback con los datos
    }, 2000); // 2000 milisegundos = 2 segundos
}

cargarDatos('http://api.com/users', (data) => {
    // ESTA es la función callback
    console.log("¡Tarea terminada! Datos recibidos:", data.mensaje);
});

console.log("Mientras se cargan los datos, yo sigo ejecutando."); 
// Esta línea se imprime ANTES que el mensaje de "Tarea terminada".
```

> **⚠️ El Problema del Callback Hell:** Si tienes muchas operaciones asíncronas que dependen una de otra, terminas anidando callbacks dentro de callbacks, creando un código ilegible y difícil de mantener (la famosa "pirámide de la perdición").

### 2. Promesas (Promises)

Una **Promesa** es un objeto que representa el eventual resultado (o error) de una operación asíncrona. Es la solución moderna al Callback Hell, ya que permite encadenar operaciones con mayor claridad.

Una Promesa puede estar en uno de tres estados:

|Estado|Significado|Acción|
|---|---|---|
|**Pending**|Pendiente. El trabajo aún no ha terminado.||
|**Fulfilled**|Cumplida. La operación se completó con éxito.|Ejecuta **`.then()`**|
|**Rejected**|Rechazada. La operación falló.|Ejecuta **`.catch()`**|

Exportar a Hojas de cálculo

#### **Manejo de Promesas:**

JavaScript

```
function simularCarga() {
    return new Promise((resolve, reject) => {
        // Simulamos un éxito aleatorio
        if (Math.random() > 0.3) {
            setTimeout(() => {
                resolve("¡Promesa cumplida! Datos obtenidos."); // Llama a 'resolve' si tiene éxito
            }, 1500);
        } else {
            setTimeout(() => {
                reject(new Error("Error de conexión.")); // Llama a 'reject' si falla
            }, 1500);
        }
    });
}

// 1. Llamar a la función y manejar el resultado con .then()
simularCarga()
    .then(resultado => {
        // Se ejecuta si la promesa se cumplió (resolve)
        console.log("ÉXITO:", resultado);
    })
    .catch(error => {
        // Se ejecuta si la promesa fue rechazada (reject)
        console.error("FALLO:", error.message);
    })
    .finally(() => {
        // Se ejecuta SIEMPRE, haya éxito o fallo
        console.log("Proceso de promesa finalizado.");
    });
```

### 3. Async/Await (Sintaxis Moderna)

Introducido en ES2017, **Async/Await** es simplemente una forma más limpia, legible y **sincrónica** de escribir código asíncrono basado en Promesas.

- **`async`:** Se usa para declarar que una función es asíncrona. Una función `async` siempre devuelve implícitamente una Promesa.
    
- **`await`:** Solo puede usarse dentro de una función `async`. **Detiene la ejecución** de la función `async` hasta que la Promesa que le sigue se **resuelva** (`fulfilled`).
    

JavaScript

```
// 1. La función debe ser marcada como 'async'
async function procesarDatos() {
    console.log("Iniciando proceso asíncrono...");
    
    try {
        // 2. 'await' detiene la función aquí hasta que simularCarga() termine
        const resultado = await simularCarga(); 
        
        // Esta línea solo se ejecuta si la Promesa se resolvió
        console.log("Resultado de await:", resultado);
        
    } catch (error) {
        // Usamos try/catch para manejar los errores (reject) de la promesa
        console.error("Error capturado con try/catch:", error.message);
    }
    
    console.log("Fin del procesamiento.");
}

procesarDatos();
console.log("¡El hilo principal sigue su curso!"); // Se ejecuta inmediatamente
```

> **Clave:** La función `async` se detiene en el `await`, pero el **hilo principal de JavaScript NO se detiene**. Permite que otras tareas se ejecuten. La función `async` simplemente se "pausa" a sí misma y se reanuda cuando la Promesa se resuelve.

---

## 🛠️ Ejemplos Prácticos: Promesas y Async/Await

Crea un archivo `clase14.js` y ejecútalo con Node.js. Incluye la función `simularCarga` del ejemplo anterior.

JavaScript

```
// La función simularCarga definida previamente
function simularCarga() {
    return new Promise((resolve, reject) => {
        const segundos = Math.floor(Math.random() * 3) + 1; // 1 a 3 segundos
        
        if (segundos > 2) {
            setTimeout(() => reject(new Error(`Falló después de ${segundos}s`)), segundos * 1000);
        } else {
            setTimeout(() => resolve(`Éxito en ${segundos} segundos`), segundos * 1000);
        }
    });
}

// ------------------------------------------------
// Opción 1: Encadenamiento con .then()
// ------------------------------------------------
simularCarga()
    .then(mensaje => {
        console.log("[.then] Paso 1 -", mensaje);
        // Si quiero otra operación asíncrona, la devuelvo aquí:
        return simularCarga(); 
    })
    .then(mensaje2 => {
        console.log("[.then] Paso 2 -", mensaje2);
    })
    .catch(error => {
        console.error("[.catch] Error:", error.message);
    });
    
// ------------------------------------------------
// Opción 2: Usando Async/Await (dentro de una función auto-invocada)
// ------------------------------------------------
(async () => {
    try {
        const resultado1 = await simularCarga();
        console.log("[Async/Await] Primer resultado:", resultado1);
        
        const resultado2 = await simularCarga();
        console.log("[Async/Await] Segundo resultado:", resultado2);
        
    } catch (error) {
        console.error("[Async/Await] Error capturado:", error.message);
    }
})(); // Función anónima ejecutada inmediatamente (IIFE)
```

---

## ✅ Resumen de Puntos Clave

- JavaScript es **single-threaded**; la asincronía evita que el hilo principal se bloquee esperando tareas lentas (como solicitudes de red).
    
- **Callbacks** son funciones pasadas para ejecutarse después, pero pueden llevar al _Callback Hell_.
    
- Una **Promesa** es un objeto con tres estados: **Pending**, **Fulfilled** (resuelto con `.then()`) y **Rejected** (fallido con `.catch()`).
    
- **`async/await`** es la sintaxis moderna que permite escribir código asíncrono con la apariencia de código síncrono, mejorando la legibilidad.
    
- Una función marcada con **`async`** permite usar **`await`** dentro, lo que pausa la ejecución de esa función (pero no del hilo principal) hasta que una Promesa se resuelve.
    
- El manejo de errores en `async/await` se realiza con los bloques **`try...catch`**.
    

---

Ahora tienes el poder de manejar el tiempo. El siguiente paso es aplicar esto a la tarea asíncrona más común en la web: la comunicación con APIs a través de la red, usando `Fetch`.

👉 Continuar con [[15 - Fetch API y AJAX]]