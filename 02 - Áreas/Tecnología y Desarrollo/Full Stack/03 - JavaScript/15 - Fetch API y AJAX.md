¡Continuemos! Ya dominas la **Asincronía** con Promesas y `async/await`. Es momento de aplicar ese conocimiento al objetivo principal de la asincronía en el _frontend_: comunicarte con el mundo exterior para obtener o enviar datos, generalmente a través de una **API**.

---

Aquí tienes los apuntes de la clase anterior: [[14 - Asincronía en JavaScript]]

# 15 - Fetch API y AJAX

## ✍️ Introducción: Conectando la Web

Una aplicación web que no se comunica es una aplicación estática. El término **AJAX (Asynchronous JavaScript and XML)** se refiere a la técnica de enviar y recibir datos del servidor de forma asíncrona **sin recargar la página**.

Históricamente, esto se hacía con el objeto `XMLHttpRequest`. Hoy, el estándar moderno, más limpio y basado en **Promesas**, es la **Fetch API**.

**Fetch API** es una interfaz de JavaScript que permite realizar peticiones de red (solicitudes HTTP) de forma sencilla y eficiente.

---

## 💻 Explicación Detallada: La Fetch API

La función `fetch()` es global y toma un argumento obligatorio: la URL del recurso que quieres solicitar.

### 1. La Sintaxis Básica (GET)

Cuando llamas a `fetch()`, esta inmediatamente devuelve una **Promesa** que se resuelve con un objeto **`Response`** cuando el servidor responde (no cuando se descargan los datos).

JavaScript

```
// Petición GET simple (para obtener datos)
fetch('https://jsonplaceholder.typicode.com/users/1')
    .then(response => {
        // 1. La primera promesa se resuelve cuando la cabecera llega.
        // Verificamos si la respuesta es exitosa (status 200-299)
        if (!response.ok) {
             // Si el estado HTTP no es OK (ej: 404, 500), lanzamos un error
            throw new Error(`Error HTTP: ${response.status}`);
        }
        // 2. Necesitamos extraer los datos del cuerpo. Usamos .json() para Promesas basadas en JSON
        return response.json(); 
    })
    .then(datos => {
        // 3. La segunda promesa se resuelve con los datos reales
        console.log("Datos recibidos:", datos.name); 
    })
    .catch(error => {
        // Capturamos cualquier error de red o los errores lanzados con 'throw'
        console.error("Algo salió mal:", error.message);
    });
```

### 2. El Objeto `Response`

El objeto `Response` devuelto por `fetch()` es clave y tiene propiedades importantes:

- **`response.status`**: El código de estado HTTP (ej: 200, 404, 500).
    
- **`response.ok`**: Un booleano (`true` si el estado está en el rango 200-299, `false` en caso contrario).
    
- **`response.json()`**: Método asíncrono que lee la respuesta y la convierte en un Objeto JS (devuelve una Promesa).
    
- **`response.text()`**: Método asíncrono para leer la respuesta como texto plano (devuelve una Promesa).
    

### 3. Usando `async/await` con Fetch (¡Recomendado!)

La sintaxis `async/await` es mucho más limpia para manejar `fetch`, ya que encapsula el manejo del éxito (`.then()`) y el error (`.catch()`) en la estructura `try/catch`.

JavaScript

```
async function obtenerUsuario(id) {
    try {
        // 1. Await espera la respuesta (objeto Response)
        const response = await fetch(`https://jsonplaceholder.typicode.com/users/${id}`);
        
        // Manejo de errores HTTP
        if (!response.ok) {
            throw new Error(`¡Fallo en la solicitud! Status: ${response.status}`);
        }
        
        // 2. Await espera que el cuerpo sea leído y parseado como JSON
        const datos = await response.json();
        
        return datos; // Devolvemos los datos finales
        
    } catch (error) {
        // Captura errores de red, errores HTTP lanzados o errores de parseo
        console.error("Error al obtener datos:", error.message);
        return null; 
    }
}

// Llamada y uso de la función async
obtenerUsuario(5).then(usuario => {
    if (usuario) {
        console.log("Usuario 5:", usuario.name);
    }
});
```

---

## 💻 Explicación Detallada: Peticiones POST (Envío de Datos)

Para enviar datos (ej: registrar un nuevo usuario), se requiere una petición **POST**. Aquí, `fetch` necesita un segundo argumento: un objeto de configuración.

|Propiedad de Configuración|Valor|Descripción|
|---|---|---|
|**`method`**|`'POST'`, `'PUT'`, `'DELETE'`, etc.|Define el tipo de solicitud.|
|**`headers`**|Objeto con cabeceras HTTP.|**Crucial:** Define el tipo de contenido que envías.|
|**`body`**|El cuerpo de la solicitud.|El dato que quieres enviar, **siempre como String JSON**.|

Exportar a Hojas de cálculo

#### **Ejemplo de Petición POST (Crear Recurso)**

JavaScript

```
async function crearTarea(nuevaTarea) {
    const url = 'https://jsonplaceholder.typicode.com/todos';
    
    // 1. Convertimos el Objeto JS a String JSON
    const bodyJSON = JSON.stringify(nuevaTarea);
    
    try {
        const response = await fetch(url, {
            method: 'POST', // Definimos el método
            headers: {
                // Indicamos que el contenido que enviamos es JSON
                'Content-Type': 'application/json' 
            },
            body: bodyJSON // Incluimos el JSON en el cuerpo
        });

        if (!response.ok) {
            throw new Error(`Fallo POST. Status: ${response.status}`);
        }
        
        // El servidor suele devolver el objeto recién creado (con un ID)
        const tareaCreada = await response.json();
        console.log("Tarea creada con ID:", tareaCreada.id);
        return tareaCreada;

    } catch (error) {
        console.error("Error al crear tarea:", error.message);
        return null;
    }
}
```

---

## 🛠️ Ejemplos Prácticos: Consumo de APIs

Crea un archivo `clase15.js` (si usas Node.js, recuerda que `fetch` es nativo en las versiones modernas, si no, es mejor probarlo en la consola del navegador).

JavaScript

```
// Datos a enviar en la petición POST
const datosACrear = {
    title: 'Aprender Fetch API y Asincronía',
    body: 'Dominar las Promesas y Async/Await.',
    userId: 1
};

// --- FUNCIÓN PRINCIPAL ASÍNCRONA ---
async function ejecutarDemo() {
    console.log("--- 1. Ejecutando Petición POST ---");
    const nuevaPublicacion = await crearTarea(datosACrear);

    if (nuevaPublicacion) {
        console.log("POST Exitoso. ID de la nueva publicación:", nuevaPublicacion.id);
        
        console.log("\n--- 2. Ejecutando Petición GET con el nuevo ID ---");
        // Ahora, consultamos la publicación que acabamos de crear
        const publicacionLeida = await obtenerUsuario(nuevaPublicacion.id);
        
        if (publicacionLeida) {
             console.log(`GET Exitoso. Título: ${publicacionLeida.title.substring(0, 30)}...`);
             console.log(`Cuerpo: ${publicacionLeida.body.substring(0, 30)}...`);
        }
    }
}

// Definiciones de las funciones (las mismas que en la explicación)
async function crearTarea(data) {
    const response = await fetch('https://jsonplaceholder.typicode.com/posts', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(data)
    });
    if (!response.ok) throw new Error(`Fallo POST. Status: ${response.status}`);
    return response.json();
}

async function obtenerUsuario(id) {
    const response = await fetch(`https://jsonplaceholder.typicode.com/posts/${id}`);
    // ¡Ojo! Las IDs de prueba se comportan extraño en este servidor.
    // Usamos el mismo manejo de errores HTTP
    if (!response.ok) throw new Error(`Fallo GET. Status: ${response.status}`);
    return response.json();
}

// Ejecutar la demo
ejecutarDemo();

console.log("\nEl programa principal sigue ejecutándose mientras Fetch trabaja...");
```

---

## ✅ Resumen de Puntos Clave

- **AJAX** es el término genérico para la comunicación asíncrona entre el navegador y el servidor sin recargar la página.
    
- La **Fetch API** es el estándar moderno basado en **Promesas** para realizar peticiones HTTP.
    
- Una llamada a `fetch(url)` devuelve una Promesa que se resuelve con un objeto **`Response`**.
    
- Debes verificar el estado **`response.ok`** o **`response.status`** para confirmar que la solicitud HTTP fue exitosa (código 200-299).
    
- El método **`response.json()`** (asíncrono) se usa para convertir el cuerpo de la respuesta a un Objeto JavaScript.
    
- Para peticiones que envían datos (`POST`, `PUT`), se requiere un objeto de configuración con el **`method`**, las **`headers`** (`Content-Type: application/json` es crucial) y el **`body`** (que debe ser un String JSON usando `JSON.stringify()`).
    
- Usar **`async/await`** con `fetch` y un bloque **`try...catch`** es la forma más legible y robusta de manejar la comunicación con APIs.
    

---

¡Felicidades! Has completado el Nivel 3. Has pasado de la lógica pura a la interacción en tiempo real y la comunicación con servidores.

Ahora, pasaremos al Nivel 4, el avanzado, donde puliremos tu código y aprenderemos a depurar, probar y usar las características más nuevas del lenguaje.

👉 Continuar con [[16 - Errores y depuración]]