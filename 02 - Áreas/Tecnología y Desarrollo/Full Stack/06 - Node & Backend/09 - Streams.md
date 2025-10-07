¡Perfecto! Has conquistado el corazón de Node.js: la asincronía (`async/await`). Ahora sabes cómo manejar operaciones lentas sin bloquear el Event Loop. Sin embargo, ¿qué sucede si esa operación lenta involucra una cantidad masiva de datos?

Ahí es donde entran los **Streams**, el mecanismo de Node.js para manejar grandes volúmenes de datos de manera eficiente en memoria.

Aquí tienes la nota para la clase anterior: **[[08 - Asincronía en Node.js]]**.

---

# 09 - Streams

## 📚 Introducción: El Problema de la Memoria

En la clase **[[05 - File System (fs)]]**, vimos que funciones como `fs.readFile()` cargan **todo el contenido del archivo en la memoria RAM** del servidor antes de poder procesarlo o enviarlo. Si tu servidor tiene 8GB de RAM y un usuario sube un archivo de 5GB, tu servidor podría colapsar (agotamiento de memoria).

Un **Stream (Flujo)** resuelve esto. Un Stream es una interfaz abstracta para trabajar con datos secuenciales en Node.js. Permite que los datos se lean o escriban en **pequeños trozos (chunks)**, sin tener que cargar toda la fuente de datos en memoria a la vez.

**Analógicamente:**

- `fs.readFile()` es como llenar una piscina (RAM) antes de vaciarla.
    
- Un Stream es como una tubería: los datos fluyen continuamente de un extremo a otro.
    

---

## 🌊 Tipos de Streams

Existen cuatro tipos de Streams en Node.js, todos basados en la interfaz `EventEmitter` (que veremos en [[10 - Eventos y EventEmitter]]):

|Tipo de Stream|Propósito|Uso Común|Métodos Clave|
|---|---|---|---|
|**Readable**|Fuente de datos. Desde donde los datos fluyen.|Lectura de archivos (`fs.createReadStream`), Peticiones HTTP de cliente.|`.read()`, `.on('data')`, `.on('end')`|
|**Writable**|Destino de datos. Hacia donde los datos fluyen.|Escritura en archivos (`fs.createWriteStream`), Respuestas HTTP de servidor (`res`).|`.write(chunk)`, `.end()`|
|**Duplex**|Combina Readable y Writable. Puede leer Y escribir.|Sockets de red.|Ambos conjuntos de métodos.|
|**Transform**|Un Duplex Stream que puede modificar datos a medida que se mueven.|Compresión/descompresión de datos (ej: `zlib`), cifrado.|Ambos conjuntos de métodos.|

---

## 🔗 El Concepto de Piping (Tubería)

El método `.pipe()` es la forma más sencilla y poderosa de usar Streams. Simplemente **conecta un Stream de lectura (Readable) directamente a un Stream de escritura (Writable)**. Node.js gestiona automáticamente la velocidad para que el lector no abrume al escritor (un mecanismo llamado **Backpressure**).

### Ejemplo Práctico: Servir un Archivo Grande

Vamos a modificar un servidor HTTP (de [[06 - Módulo HTTP]]) para servir un archivo grande usando Streams. Esto es mucho más eficiente que cargarlo con `fs.readFile()`.

1. Crea un archivo grande de ejemplo para probar:
    
    Bash
    
    ```
    # (En Linux/macOS) crea un archivo de 100MB relleno de ceros
    # En Windows, puedes copiar un archivo grande que tengas a mano
    head -c 100M /dev/zero > archivo_grande.bin 
    ```
    
2. Crea el archivo `server-streams.js`:
    

JavaScript

```
// server-streams.js
const http = require('http');
const fs = require('fs'); // Usamos la interfaz sin promises para createReadStream
const path = require('path');

const ARCHIVO_PESADO = path.join(process.cwd(), 'archivo_grande.bin'); 

const server = http.createServer((req, res) => {
    if (req.url === '/archivo-stream') {
        // 1. Crear el Readable Stream
        const readStream = fs.createReadStream(ARCHIVO_PESADO);
        
        // 2. Configurar el encabezado de la respuesta
        res.writeHead(200, {
            'Content-Type': 'application/octet-stream', // Tipo de archivo binario
            'Content-Length': fs.statSync(ARCHIVO_PESADO).size // Tamaño del archivo
        });

        // 3. ¡Conectar el lector al escritor (response)!
        // La respuesta (res) es un Writable Stream.
        readStream.pipe(res); 

        // Manejo de errores (importante)
        readStream.on('error', (err) => {
            console.error('Error al leer el stream:', err);
            res.end('Error interno al servir el archivo.');
        });
        
    } else {
        res.end('Ruta no encontrada. Intenta /archivo-stream');
    }
});

server.listen(3000, () => {
    console.log('Servidor de Streams corriendo en http://localhost:3000/');
});
```

### 🧠 ¿Qué logramos con `.pipe(res)`?

Cuando el cliente solicita `/archivo-stream`:

1. Node.js crea un **Readable Stream** (`readStream`).
    
2. Node.js conecta ese flujo directamente al objeto de **Respuesta HTTP** (`res`), que es un **Writable Stream**.
    
3. Los 100MB del archivo **nunca se cargan en la RAM de Node.js**; simplemente se leen por pedazos y se envían por pedazos al cliente.
    

---

## 🔄 Usos Avanzados de Streams

### A. Transform Stream (Ejemplo de Mayúsculas)

Los Streams de Transformación permiten manipular los datos mientras fluyen. El módulo nativo `stream` proporciona la clase `Transform`.

JavaScript

```
const { Transform } = require('stream');

// 1. Creamos un Transform Stream personalizado
const mayusculasTransform = new Transform({
    // La función 'transform' se llama por cada chunk
    transform(chunk, encoding, callback) {
        // Convertimos el chunk a string, lo pasamos a mayúsculas
        const upperCaseChunk = chunk.toString().toUpperCase();
        
        // 2. Empujamos el chunk transformado (el 'push')
        this.push(upperCaseChunk);
        
        // 3. Llamamos al callback para indicar que terminamos con este chunk
        callback();
    }
});

// Ejemplo: Leer, transformar y escribir en un solo flujo
fs.createReadStream('input.txt')
    .pipe(mayusculasTransform) // <- El Transform Stream en medio
    .pipe(fs.createWriteStream('output-mayusculas.txt')); 

// Los datos fluyen de input.txt -> Transform -> output-mayusculas.txt
```

### B. Backpressure (Contrapresión)

Cuando un Stream **Readable** es más rápido que el **Writable**, el escritor puede empezar a saturarse. El mecanismo de **Backpressure** le dice al lector que se detenga temporalmente, evitando la sobrecarga de la memoria.

- El método `.pipe()` maneja la contrapresión automáticamente.
    
- Si no usas `.pipe()`, debes manejar los eventos **`drain`** (el escritor está listo) y verificar si `.write()` devuelve `false` (el escritor está saturado). Por eso, **siempre que sea posible, usa `.pipe()`**.
    

---

## ✅ Buenas Prácticas y Errores Comunes

|Categoría|Buena Práctica|Error Común|
|---|---|---|
|**Uso**|Usar **Streams** para cualquier operación I/O con archivos grandes, subidas de archivos, o transferencia de datos entre servicios.|Usar `fs.readFile()` para archivos que superen unos pocos MB (ej. > 5MB).|
|**Control**|**Priorizar `.pipe()`** para que Node.js maneje la complejidad de **Backpressure**.|Intentar controlar manualmente el flujo de Streams sin `.pipe()`, complicando el manejo del evento `drain`.|
|**Errores**|Siempre escuchar el evento **`error`** en todos los Streams para evitar que el proceso Node.js falle.|No manejar errores, lo que puede causar fallos no controlados (`uncaughtException`).|
|**Recursos**|Cerrar explícitamente los Streams Writable con **`.end()`** si no usas `.pipe()`.|Olvidar `stream.end()`, manteniendo el recurso abierto y pudiendo causar _memory leaks_.|

---

## 🔑 Resumen de Puntos Clave

- **Streams** son la interfaz para manejar datos secuenciales por **trozos (chunks)**.
    
- Su principal ventaja es el **ahorro de memoria** al evitar cargar archivos grandes completamente en la RAM.
    
- Existen **Readable**, **Writable**, **Duplex** y **Transform** Streams.
    
- El método **`.pipe()`** es la herramienta clave para conectar flujos y es fundamental para un desarrollo eficiente de I/O.
    
- **Backpressure** es el mecanismo interno que evita que un Stream Readable sature a un Stream Writable.
    

Has aprendido sobre los datos que fluyen. Ahora, veremos cómo Node.js maneja la otra mitad de su arquitectura de Eventos: el mecanismo para crear y escuchar eventos personalizados.

Tu siguiente clase te enseñará a crear tus propios flujos de control: **[[10 - Eventos y EventEmitter]]**.