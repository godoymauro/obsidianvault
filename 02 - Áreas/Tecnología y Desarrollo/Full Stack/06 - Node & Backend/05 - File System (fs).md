¡Estupendo! Con una comprensión sólida de la arquitectura (Event Loop) y la estructura (Módulos), estamos listos para que Node.js haga algo útil en el sistema operativo.

La interacción con archivos y directorios es una de las tareas más comunes en el backend.

Aquí tienes la nota para la clase anterior: [[04 - Módulos en Node.js]]

---

# 05 - File System (fs)

## 📚 Introducción: El Módulo `fs`

El módulo **`fs`** (File System) es uno de los módulos nativos de Node.js más importantes. Permite interactuar con el sistema de archivos del servidor (leer, escribir, modificar, eliminar archivos y carpetas).

Debido a que las operaciones de I/O (entrada/salida) de disco son inherentemente **lentas**, el módulo `fs` sigue estrictamente la filosofía **Non-Blocking I/O** de Node.js, ofreciendo tres interfaces para casi todas sus funciones:

1. **Síncrona (Blocking):** Nombres terminados en `Sync` (ej: `fs.readFileSync`). **⚠️ No usar en el hilo principal.**
    
2. **Asíncrona con Callbacks:** La versión original (ej: `fs.readFile`).
    
3. **Asíncrona con Promesas (Recomendada):** Versión moderna (ej: `fs.promises.readFile`).
    

---

## 🚀 Trabajando con Promesas (La Interfaz Moderna)

Como desarrolladores backend modernos, debemos **priorizar la interfaz de Promesas** para escribir código asíncrono limpio y legible.

Para acceder a esta interfaz, usamos la propiedad `promises` del módulo `fs`.

JavaScript

```
// Usando CommonJS o ES Modules, la importación es la misma
const fs = require('fs').promises; // CJS
// o
// import * as fs from 'node:fs/promises'; // ESM (más explícito y moderno)
```

### Ejemplo Práctico: Lectura y Escritura Asíncrona

Crea un archivo llamado `fs-promesas.js` y, si usas `package.json` con `"type": "module"`, usa la sintaxis `import`.

JavaScript

```
// fs-promesas.js
import * as fs from 'node:fs/promises';
import { join } from 'node:path';

// Definimos la ruta del archivo a crear/leer
const ARCHIVO_RUTA = join(process.cwd(), 'datos_ejemplo.txt'); 
// join nos ayuda a construir rutas seguras para cualquier SO

async function manejarArchivos() {
    console.log('--- 1. ESCRIBIENDO Archivo ---');
    const contenido = 'Hola Mundo. Estos datos se escribieron con Node.js y Promesas.';

    try {
        // fs.writeFile() sobrescribe el archivo si existe.
        // El tercer argumento es la codificación (encoding), por defecto 'utf8'.
        await fs.writeFile(ARCHIVO_RUTA, contenido, { encoding: 'utf8' });
        console.log('✅ Archivo escrito con éxito.');

    } catch (error) {
        console.error('❌ Error al escribir el archivo:', error);
        return; // Detener si falla la escritura
    }

    console.log('\n--- 2. LEYENDO Archivo ---');
    try {
        // fs.readFile() lee el contenido completo del archivo en memoria.
        const data = await fs.readFile(ARCHIVO_RUTA, 'utf8');
        console.log('Contenido leído:');
        console.log(`"${data}"`);

    } catch (error) {
        console.error('❌ Error al leer el archivo:', error);
    }
    
    console.log('\n--- 3. ELIMINANDO Archivo ---');
    try {
        await fs.unlink(ARCHIVO_RUTA); // fs.unlink elimina un archivo
        console.log(`✅ Archivo '${ARCHIVO_RUTA}' eliminado.`);
    } catch (error) {
        console.error('❌ Error al eliminar el archivo:', error);
    }
}

// Ejecutar la función principal
manejarArchivos();

// Nota: El uso de 'await' pausa la función *solo* localmente, 
// no bloquea el Event Loop.
```

### 💻 Comando para Ejecutar

Bash

```
node fs-promesas.js
```

---

## 🛑 Advertencia: Las Versiones Síncronas

|Interfaz|Función|¿Bloquea el Event Loop?|Recomendación|
|---|---|---|---|
|**Síncrona**|`fs.readFileSync()`|**SÍ**. Detiene la ejecución de todo el programa hasta que el disco termine.|Usar **solo** en el arranque del servidor (ej: leer archivos de configuración iniciales). **Nunca** dentro de una ruta de API o request.|
|**Asíncrona (Promesas)**|`fs.promises.readFile()`|**NO**. Delega la tarea I/O y libera el hilo principal.|**Prioridad 1.** Úsala siempre.|

### Ejemplo de Bloqueo (¡Para Entender, No Para Usar!)

JavaScript

```
// fs-sincrono.js

const fsSincrono = require('fs');
const path = require('path');
const inicio = Date.now();

// 🛑 Esto detendrá el programa por el tiempo que tarde el disco.
try {
    const data = fsSincrono.readFileSync(path.join(process.cwd(), 'archivo_grande.dat'));
    console.log(`Leído archivo en ${Date.now() - inicio}ms`);
} catch (err) {
    // Manejo de errores
}

console.log('Este mensaje solo aparece cuando la lectura síncrona termina.');
```

---

## 🌊 Streams (Flujos): El Gran Poder de `fs`

Cuando trabajas con archivos muy grandes (ej: subir videos, archivos de log enormes), leer el archivo completo en memoria (`fs.readFile`) puede **colapsar tu servidor** por falta de RAM.

La solución son los **Streams (Flujos)**.

- Un _Stream_ permite que los datos se lean o escriban en **pequeños trozos (chunks)** secuenciales, sin cargar el archivo completo en la memoria a la vez.
    

|Tipo de Stream|Función|Propósito|
|---|---|---|
|**Readable Stream**|`fs.createReadStream()`|Leer datos de una fuente (ej: un archivo).|
|**Writable Stream**|`fs.createWriteStream()`|Escribir datos a un destino (ej: otro archivo o una respuesta HTTP).|

Profundizaremos en Streams en **[[09 - Streams]]**, pero aquí está el concepto inicial:

JavaScript

```
// Ejemplo de copia de archivo con Streams (Mucho más eficiente en memoria)
import * as fs from 'node:fs';

const lector = fs.createReadStream('entrada.txt');
const escritor = fs.createWriteStream('salida_copia.txt');

// 💡 Piping: Conecta el flujo de lectura directamente al flujo de escritura.
// Los datos fluyen de a poco, sin llenar la RAM.
lector.pipe(escritor);

lector.on('end', () => {
    console.log('Copia de archivo grande terminada usando Streams.');
});
```

---

## ✅ Buenas Prácticas y Errores Comunes

|Categoría|Buena Práctica|Error Común|
|---|---|---|
|**Asincronía**|**Usar `fs.promises` y `async/await`** para toda la I/O de archivos en tiempo de ejecución.|Usar `fs.readFileSync` dentro de la lógica de una API, lo que inevitablemente **bloqueará el Event Loop**.|
|**Rutas**|Usar el módulo **`path`** (`path.join()`) para construir rutas, en lugar de concatenar cadenas.|Concatenar rutas directamente (`/ruta/archivo.txt`), lo que falla en Windows o Linux debido a las diferentes barras.|
|**Memoria**|Usar **Streams** (`fs.createReadStream`) para archivos de gran tamaño (más de unos pocos MB).|Leer archivos muy grandes con `fs.readFile`, agotando la memoria RAM del servidor.|

---

## 🔑 Resumen de Puntos Clave

- El módulo **`fs`** permite a Node.js interactuar con el sistema de archivos del servidor.
    
- **Prioriza `fs.promises`** con `async/await` para mantener el modelo de **I/O No Bloqueante**.
    
- **Evita las funciones `*Sync`** en la lógica de tiempo de ejecución.
    
- El módulo **`path`** es crucial para crear rutas compatibles con cualquier sistema operativo.
    
- Para archivos grandes, usa **Streams** (`fs.createReadStream`/`fs.createWriteStream`) para ahorrar memoria.
    

Hemos aprendido a manejar archivos en el servidor. El siguiente paso natural es el networking: usar Node.js para hacer lo que mejor sabe hacer: **servir contenido web**.

Tu siguiente clase te enseñará a crear un servidor web desde cero: **[[06 - Módulo HTTP]]**.