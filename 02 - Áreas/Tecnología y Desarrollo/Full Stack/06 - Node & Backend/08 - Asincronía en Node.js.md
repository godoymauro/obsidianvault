¡Absolutamente! Has concluido exitosamente el Nivel 1. Ahora, vamos a la médula de Node.js y JavaScript moderno: la **asincronía**. Dominar este tema te transformará de un programador de scripts a un verdadero desarrollador backend de Node.js.

Aquí tienes la nota para la clase anterior: **[[07 - Gestión de dependencias]]**.

---

# 08 - Asincronía en Node.js

## 📚 Introducción: ¿Por Qué es Necesaria la Asincronía?

Como vimos en la clase **[[03 - Node.js Runtime]]**, el modelo de Node.js es de **hilo único**. Esto significa que cualquier operación que tome tiempo (I/O, red, base de datos, disco) debe ser ejecutada de forma **asíncrona** para evitar **bloquear** el único hilo de ejecución.

La asincronía en JavaScript ha evolucionado para hacer este manejo de operaciones lentas cada vez más legible y mantenible.

---

## 📞 1. Callbacks (El Origen)

El _callback_ fue el método original de manejar la asincronía en Node.js. Un _callback_ es simplemente una función que se pasa como argumento a otra función y se espera que sea ejecutada _después_ de que la tarea asíncrona haya terminado.

### El Patrón `(error, data)`

La convención en Node.js es que el _callback_ siempre reciba el **error como primer argumento** y los datos (`data`) como segundo.

JavaScript

```
// Ejemplo con el módulo 'fs' original (antes de promises)
const fs = require('fs');

fs.readFile('archivo.txt', 'utf8', (err, data) => {
    // 1. Siempre revisamos si hubo un error primero
    if (err) {
        console.error('Error al leer el archivo:', err);
        return; // Salir si hay error
    }

    // 2. Si no hay error, procesamos los datos
    console.log('Contenido del archivo:', data);
});

console.log("Este mensaje se imprime primero, demostrando la asincronía."); 
// La función fs.readFile es delegada, y el hilo principal sigue.
```

### 😩 El Problema: Callback Hell

Cuando tienes múltiples operaciones asíncronas que dependen una de la otra, terminas anidando _callbacks_ en una estructura en forma de pirámide invertida, haciendo el código ilegible, frágil y difícil de mantener.

JavaScript

```
// Callback Hell
leerUsuario(1, (err, user) => {
    if (err) return handleError(err);
    obtenerRoles(user.id, (err, roles) => {
        if (err) return handleError(err);
        comprobarPermisos(roles[0], (err, permiso) => {
            if (err) return handleError(err);
            console.log('Permiso obtenido:', permiso);
        });
    });
});
```

---

## ✨ 2. Promises (La Solución Estructural)

Las **Promises (Promesas)** se introdujeron para solucionar el _Callback Hell_. Una Promesa es un objeto que representa el eventual resultado (o error) de una operación asíncrona.

Una Promesa tiene tres estados posibles:

1. **Pending (Pendiente):** Estado inicial, ni cumplida ni rechazada.
    
2. **Fulfilled (Cumplida/Resuelta):** La operación terminó con éxito.
    
3. **Rejected (Rechazada):** La operación falló.
    

### Consumiendo Promesas

En lugar de anidar _callbacks_, usamos el método **`.then()`** para manejar el éxito y **`.catch()`** para manejar el error.

JavaScript

```
// Ejemplo: una función que devuelve una Promesa
function esperar(ms) {
    return new Promise((resolve, reject) => {
        if (ms < 0) {
            // Rechazar la promesa si la condición no se cumple
            reject(new Error("El tiempo no puede ser negativo."));
        }
        // Llamar a 'resolve' cuando la tarea asíncrona termina con éxito
        setTimeout(() => resolve(`Esperamos ${ms} milisegundos.`), ms);
    });
}

// Encadenamiento de Promesas
esperar(1000)
    .then((resultado) => {
        console.log(resultado); // "Esperamos 1000 milisegundos."
        return esperar(500); // Devolver otra Promesa para encadenar
    })
    .then((segundoResultado) => {
        console.log(segundoResultado); // "Esperamos 500 milisegundos."
        // Esto se ejecuta después de la primera y la segunda promesa
    })
    .catch((error) => {
        // Un solo .catch al final captura errores de CUALQUIER .then anterior
        console.error('Se capturó un error en el encadenamiento:', error.message);
    });
```

---

## 🥇 3. Async/Await (La Forma Moderna)

**`async/await`** es la sintaxis moderna (introducida en ES2017) construida **encima de las Promesas**. Su objetivo es permitirte escribir código asíncrono que se vea y se lea como si fuera síncrono. Esto es la práctica estándar en el backend de Node.js.

### Reglas de Oro

1. La palabra clave **`await`** solo se puede usar **dentro** de una función marcada como **`async`**.
    
2. `await` pausa la ejecución de la **función `async`** (sin bloquear el Event Loop) hasta que la Promesa a la que se aplica se resuelva o se rechace.
    

### Ejemplo Práctico con `async/await`

JavaScript

```
// Reutilizamos la función 'esperar' basada en Promesas

async function ejecutarFlujo() {
    try {
        console.log('Iniciando el proceso...');
        
        // 1. Usar await hace que la ejecución espere aquí
        const resultado1 = await esperar(1500);
        console.log(`Paso 1: ${resultado1}`);

        // 2. El código es secuencial y fácil de leer
        const resultado2 = await esperar(750);
        console.log(`Paso 2: ${resultado2}`);
        
        // 3. Probar el error
        await esperar(-100); 

    } catch (error) {
        // El bloque try...catch maneja el rechazo (reject) de la Promesa
        console.error('Flujo fallido:', error.message);
    } finally {
        console.log('Proceso finalizado (siempre se ejecuta).');
    }
}

ejecutarFlujo();
```

---

## ⚖️ Comparativa de Asincronía

|Característica|Callbacks|Promises (`.then/.catch`)|Async/Await (`try/catch`)|
|---|---|---|---|
|**Sintaxis**|Anidada, dependiente del orden de argumentos.|Encadenada, basada en métodos.|Secuencial, parece código síncrono.|
|**Manejo de Errores**|Repetitivo (revisar `if (err)` en cada paso).|Un `.catch()` al final maneja todos los errores anteriores.|Un `try...catch` simple y elegante.|
|**Legibilidad**|Baja (Callback Hell).|Media/Alta.|**Máxima.**|
|**Uso Moderno**|**Obsoleto** (Solo para librerías legacy).|Bueno, pero a menudo reemplazado por `async/await`.|**Estándar de Oro.**|

---

## ✅ Buenas Prácticas y Errores Comunes

|Categoría|Buena Práctica|Error Común|
|---|---|---|
|**Adoptar**|Usar **`async/await`** para consumir todas las Promesas.|Seguir usando callbacks o mezclarlos con Promesas (`.then`) innecesariamente.|
|**Errores**|**Siempre envolver `await` en un `try...catch`** para manejar el rechazo de la Promesa.|Olvidar el `try...catch` en funciones `async`. Si la Promesa se rechaza, el proceso puede fallar sin ser manejado (`UnhandledPromiseRejection`).|
|**Simultaneidad**|Usar `Promise.all()` para ejecutar Promesas **independientes** en paralelo.|Usar `await` secuencialmente para operaciones que no dependen una de otra, desperdiciando tiempo.|
|**Función Top-Level**|Recordar que si llamas a un `await` en el nivel superior, tu archivo `.js` debe estar en formato **ES Modules** (`"type": "module"` en `package.json`).|Intentar usar `await` sin envolverlo en una función `async` en CommonJS.|

---

## 🔑 Resumen de Puntos Clave

- **Asincronía** es clave para mantener el **Event Loop** de Node.js libre.
    
- **Callbacks** son el mecanismo original, pero llevan al _Callback Hell_.
    
- **Promises** ofrecen una estructura limpia con `.then()` y `.catch()` para encadenar operaciones asíncronas.
    
- **`async/await`** es la sintaxis más limpia y moderna, permitiendo manejar Promesas con la lógica de `try...catch` síncrona.
    
- **La regla número uno:** Nunca dejes un `await` sin un manejo de errores (sea con `.catch()` o con `try...catch`).
    

Ahora que dominas los fundamentos de la asincronía, podemos ver cómo se aplica a la gestión eficiente de datos grandes.

Tu próxima clase te introducirá a los **Flujos de Datos**: **[[09 - Streams]]**.