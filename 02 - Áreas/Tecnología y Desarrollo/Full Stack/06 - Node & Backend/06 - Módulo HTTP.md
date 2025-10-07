¡Absolutamente! Has aprendido a organizar tu código y a interactuar con el sistema de archivos. Ahora, vamos a la razón principal por la que se usa Node.js en el backend: **crear servidores web**.

El módulo `http` es el núcleo de la capacidad de Node.js para manejar tráfico de red.

Aquí tienes la nota para la clase anterior: [[05 - File System (fs)]]

---

# 06 - Módulo HTTP

## 📚 Introducción: Creando Servidores

El módulo **`http`** es el módulo nativo de Node.js que implementa el protocolo de transferencia de hipertexto (HTTP). Es la herramienta de más bajo nivel que te permite escuchar peticiones de red y enviar respuestas.

Aunque en la práctica profesional usarás _frameworks_ de alto nivel como **Express.js** (que veremos en [[12 - Introducción a Express]]), entender el módulo `http` es fundamental para saber qué está sucediendo "bajo el capó".

---

## 🏗 El Servidor Básico

La creación de un servidor HTTP en Node.js se basa en el principio de **programación orientada a eventos** (que veremos en [[10 - Eventos y EventEmitter]]). Es decir, el servidor espera un evento (`request`) y ejecuta una función (_callback_ o _listener_) cuando ocurre.

### Ejemplo Práctico: Servidor en `server-http.js`

Crea un archivo llamado `server-http.js`:

JavaScript

```
// server-http.js
// Usamos CommonJS para el módulo nativo
const http = require('http'); 

// 1. Definición de constantes
const PORT = 3000;
const HOSTNAME = '127.0.0.1'; // localhost

// 2. Función de manejo de peticiones (El Handler)
// Esta función se ejecuta CADA VEZ que el servidor recibe una petición.
// 'req' (request): Objeto de la petición del cliente (lo que el cliente pide).
// 'res' (response): Objeto de la respuesta que el servidor enviará (lo que respondemos).
const requestListener = (req, res) => {
    // 3. Análisis de la Petición (req)
    console.log(`Petición recibida: Método=${req.method}, URL=${req.url}`);

    // A. Analizar la URL (ruta)
    if (req.url === '/') {
        // 4. Configuración de la Respuesta (res) para la ruta raíz
        res.writeHead(200, { 'Content-Type': 'text/plain' }); // Status Code 200 (OK)
        res.end('¡Bienvenido a la API nativa de Node.js!'); // El cuerpo de la respuesta
    } 
    
    else if (req.url === '/info') {
        res.writeHead(200, { 'Content-Type': 'application/json' }); // Tipo JSON
        // Creamos un JSON de respuesta
        const data = JSON.stringify({
            nombre: "Servidor Nativo",
            version: "1.0",
            metodo: req.method
        });
        res.end(data);
    } 
    
    else {
        // 5. Manejo de errores (Ruta no encontrada)
        res.writeHead(404, { 'Content-Type': 'text/plain' }); // Status Code 404 (Not Found)
        res.end('Error 404: Ruta no encontrada.');
    }
};

// 6. Creación del Servidor
const server = http.createServer(requestListener);

// 7. Poner el Servidor a Escuchar
// Este método es asíncrono y delega la tarea al sistema operativo.
server.listen(PORT, HOSTNAME, () => {
    console.log(`Servidor ejecutándose en http://${HOSTNAME}:${PORT}/`);
    console.log(`Rutas de prueba:`);
    console.log(` - http://${HOSTNAME}:${PORT}/`);
    console.log(` - http://${HOSTNAME}:${PORT}/info`);
});
```

### 💻 Comando para Ejecutar

Bash

```
node server-http.js
```

Abre tu navegador o una herramienta como **Postman** o **cURL** y prueba las URL.

---

## 🔍 Objetos Request (`req`) y Response (`res`)

El _callback_ de `http.createServer` recibe estos dos objetos, que son los pilares de la comunicación HTTP.

### Objeto `req` (Petición)

Contiene toda la información enviada por el cliente:

|Propiedad|Descripción|Ejemplo|
|---|---|---|
|`req.method`|El método HTTP usado (GET, POST, PUT, DELETE).|`'GET'`|
|`req.url`|La URL solicitada por el cliente.|`'/users/1?active=true'`|
|`req.headers`|Encabezados de la petición (como _User-Agent_ o _Accept_).|`{ 'accept': 'application/json' }`|
|`req.on('data', ...)`|Un **Stream** para leer el cuerpo de la petición (para POST/PUT).|_Ver sección Cuerpo de Petición_|

### Objeto `res` (Respuesta)

El objeto que usamos para construir la respuesta y enviarla al cliente:

|Propiedad/Método|Descripción|
|---|---|
|`res.statusCode`|El código de estado HTTP (200, 404, 500).|
|`res.setHeader(name, value)`|Establece un encabezado de respuesta.|
|`res.writeHead(code, headers)`|Una forma concisa de establecer el código de estado y encabezados.|
|`res.end(data)`|Finaliza la respuesta y envía opcionalmente los datos al cliente. **Debe llamarse una vez por petición.**|

---

## 📥 Lectura del Cuerpo de la Petición (POST/PUT)

Cuando un cliente envía datos (ej. un formulario JSON en un POST), el cuerpo de la petición no llega completo. Llega en **pedazos (chunks)** a través de un **Readable Stream** (`req`).

Para obtener el cuerpo completo, debemos escuchar los eventos de ese _Stream_ de forma asíncrona:

JavaScript

```
// Fragmento de código para un método POST
if (req.method === 'POST') {
    let body = '';
    
    // 1. Escuchar el evento 'data' para obtener los chunks
    req.on('data', (chunk) => {
        body += chunk.toString(); // Acumular los chunks
    });
    
    // 2. Escuchar el evento 'end' cuando el Stream ha terminado
    req.on('end', () => {
        try {
            const data = JSON.parse(body); // Asumimos que es JSON
            console.log('Datos recibidos:', data);
            
            res.writeHead(201, { 'Content-Type': 'application/json' });
            res.end(JSON.stringify({ mensaje: "Datos procesados", id: 1 }));
        } catch (error) {
            res.writeHead(400);
            res.end(JSON.stringify({ error: "JSON inválido" }));
        }
    });

    // 3. Manejo de error del stream
    req.on('error', (err) => {
        console.error('Error en el stream de petición:', err);
        res.writeHead(500);
        res.end('Error interno del servidor.');
    });

}
```

**Importante:** La necesidad de manejar manualmente estos _Streams_ es la razón principal por la que se usa **Express.js**, que lo hace por ti automáticamente con _middlewares_.

---

## ✅ Buenas Prácticas y Errores Comunes

|Categoría|Buena Práctica|Error Común|
|---|---|---|
|**Finalizar**|**Siempre llamar a `res.end()`** una sola vez por petición.|Olvidar `res.end()`, dejando la conexión abierta y al cliente en espera infinita.|
|**Headers**|Establecer el `Content-Type` correcto (`application/json`, `text/html`, `text/plain`).|Olvidar `Content-Type` o establecerlo mal, haciendo que el navegador interprete mal la respuesta.|
|**Errores**|Usar los **códigos de estado HTTP** correctos (200, 404, 500, 401, 201).|Usar siempre 200 (OK) incluso para errores, lo cual confunde al cliente.|
|**I/O**|Si el servidor hace I/O (ej: lee un archivo), usar las versiones asíncronas para **no bloquear el Event Loop**.|Usar `fs.readFileSync` dentro del `requestListener` (ver [[03 - Node.js Runtime]]).|

---

## 🔑 Resumen de Puntos Clave

- El módulo **`http`** es la base para crear servidores web en Node.js.
    
- `http.createServer(listener)` crea el servidor y el _listener_ recibe los objetos **`req`** (petición) y **`res`** (respuesta).
    
- El manejo de peticiones se basa en el **método (`req.method`)** y la **URL (`req.url`)**.
    
- El cuerpo de la petición (para POST/PUT) debe leerse asíncronamente escuchando los eventos **`data`** y **`end`** del _Stream_ `req`.
    
- Debes usar `res.writeHead()` o `res.setHeader()` para los encabezados y **`res.end()`** para enviar la respuesta y finalizar la conexión.
    

Hemos creado un servidor funcional. Ahora, antes de pasar a la organización con Express, necesitamos la última pieza del rompecabezas de Node.js: **cómo gestionar las librerías externas**.

Tu siguiente clase te enseñará a gestionar dependencias: **[[07 - Gestión de dependencias]]**.