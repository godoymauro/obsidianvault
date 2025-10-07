¡Excelente progreso! Hemos visto cómo los **Streams** manejan los datos que fluyen. Ahora, volvamos a la raíz de la filosofía de Node.js: la **programación basada en eventos**.

En esta clase, aprenderás a crear tus propios sistemas de eventos, un concepto vital ya que casi todos los módulos nativos de Node.js (como `http`, `fs` y los propios _Streams_) heredan esta capacidad.

Aquí tienes la nota para la clase anterior: **[[09 - Streams]]**.

---

# 10 - Eventos y EventEmitter

## 📚 Introducción: El Corazón Event-Driven de Node.js

Node.js está construido sobre un modelo **Event-Driven (Impulsado por Eventos)**. Esto significa que la aplicación espera pasivamente a que ocurra un evento (ej. una solicitud HTTP llega, un archivo termina de leerse, un temporizador expira) y luego ejecuta el código asociado (el _listener_ o _callback_).

El módulo nativo **`events`** y la clase **`EventEmitter`** te permiten crear tus propios objetos que emiten eventos con nombres específicos, permitiendo una comunicación limpia y desacoplada entre diferentes partes de tu aplicación.

---

## 🛠 La Clase `EventEmitter`

La clase `EventEmitter` es el cimiento de la programación basada en eventos en Node.js. Para usarla, la importamos del módulo nativo `events`.

### Conceptos Clave

|Concepto|Descripción|Métodos Clave|
|---|---|---|
|**Emisión**|Disparar o "lanzar" un evento con un nombre específico.|`.emit(eventName, [arg1], [arg2], ...)`|
|**Escucha (_Listening_)**|Registrar una función _callback_ que se ejecutará cuando un evento sea emitido.|`.on(eventName, listener)` o `.addListener(eventName, listener)`|
|**Una sola vez**|Registrar una función que se ejecutará solo la primera vez que se emita el evento.|`.once(eventName, listener)`|
|**Remoción**|Eliminar un _listener_ específico de un evento.|`.removeListener(eventName, listener)`|

### Ejemplo Práctico: Un "Centro de Notificaciones"

Crea un archivo llamado `event-emitter-example.js`:

JavaScript

```
// event-emitter-example.js
// 1. Importamos el módulo nativo
const EventEmitter = require('events');

// 2. Creamos una instancia de EventEmitter
const miNotificador = new EventEmitter();

// --- 3. Definición de Listeners (Escuchas) ---

// Listener 1: Escucha el evento 'usuarioLogeado'
miNotificador.on('usuarioLogeado', (user) => {
    console.log(`[LOG] 🧑‍💻 El usuario ${user.username} ha iniciado sesión.`);
});

// Listener 2: Escucha el mismo evento 'usuarioLogeado'
miNotificador.on('usuarioLogeado', (user) => {
    // Aquí podríamos notificar a la base de datos o enviar un email
    console.log(`[EMAIL] Enviando correo de bienvenida a ${user.email}...`);
});

// Listener 3: Escucha el evento 'alertaSistema' UNA sola vez
miNotificador.once('alertaSistema', (mensaje) => {
    console.warn(`🚨 ALERTA CRÍTICA: ${mensaje} (Solo se mostrará una vez)`);
});


// --- 4. Emisión de Eventos ---

console.log('// --- Emisión 1: Inicio de Sesión ---');
const usuario = { username: 'experto_backend', email: 'exp@backend.com' };
// Emitimos el evento. Todos los listeners asociados se ejecutan síncronamente.
miNotificador.emit('usuarioLogeado', usuario);


console.log('\n// --- Emisión 2: Alerta de Sistema (Primera vez) ---');
miNotificador.emit('alertaSistema', 'Memoria al 95%');


console.log('\n// --- Emisión 3: Alerta de Sistema (Segunda vez) ---');
// Este evento se ignora porque el listener se registró con .once()
miNotificador.emit('alertaSistema', 'Memoria al 98%'); 

// Si emitimos 'usuarioLogeado' de nuevo, ambos listeners se ejecutarán.
console.log('\n// --- Emisión 4: Segundo Logeo ---');
miNotificador.emit('usuarioLogeado', { username: 'otro_dev', email: 'otro@dev.com' });
```

### 💻 Comando para Ejecutar

Bash

```
node event-emitter-example.js
```

**Resultado Clave:** Observa cómo dos funciones diferentes (el `[LOG]` y el `[EMAIL]`) se ejecutan cuando un solo evento (`usuarioLogeado`) es emitido. Esto es **desacoplamiento** puro.

---

## 🏗 Extendiendo `EventEmitter` (Patrón de Clase)

En aplicaciones reales, en lugar de usar una instancia cruda de `EventEmitter`, es común crear una clase que **extienda** de ella. Esto convierte a tu propia clase en una fuente de eventos.

### Ejemplo: Clase `ServicioDeDatos`

JavaScript

```
const EventEmitter = require('events');

class ServicioDeDatos extends EventEmitter {
    constructor() {
        super();
        this.datos = [];
    }

    // Método que emite un evento después de una operación
    agregarDato(dato) {
        this.datos.push(dato);
        console.log(`Agregado: ${dato}`);
        
        // 💡 Emisión del evento: 'datoAgregado'
        this.emit('datoAgregado', dato, this.datos.length);
    }
}

// Uso
const servicio = new ServicioDeDatos();

// Escuchamos el evento de la clase
servicio.on('datoAgregado', (nuevoDato, total) => {
    console.log(`[Listener]: Nuevo dato "${nuevoDato}" agregado. Total: ${total}`);
});

servicio.agregarDato('registro_A');
servicio.agregarDato('registro_B');
```

De esta manera, la lógica de _qué_ sucede cuando se agrega un dato (`servicio.agregarDato()`) queda separada de la lógica de _reacción_ a ese evento (el `listener`).

---

## 🔄 `EventEmitter` y Módulos Nativos

Es fundamental entender que esta clase `EventEmitter` es la que está detrás de la mayoría de las APIs de Node.js que ves:

|Módulo/Clase|Tipo de Objeto|Eventos Comunes|
|---|---|---|
|**`http.Server`**|Extiende de `EventEmitter`|`request`, `listening`, `close`|
|**`req`** (Petición HTTP)|Es un `Readable Stream` (que extiende de `EventEmitter`)|`data`, `end`, `error`|
|**`res`** (Respuesta HTTP)|Es un `Writable Stream` (que extiende de `EventEmitter`)|`finish`, `close`|
|**`fs.createReadStream`**|Es un `Readable Stream`|`data`, `end`, `error`|

Entender `EventEmitter` te da la clave para saber por qué usas `req.on('data', ...)` o `server.on('request', ...)`: **simplemente estás registrando un _listener_ para un objeto que es un emisor de eventos.**

---

## ✅ Buenas Prácticas y Errores Comunes

|Categoría|Buena Práctica|Error Común|
|---|---|---|
|**Desacoplamiento**|Usar `EventEmitter` para notificar a otras partes del sistema en lugar de llamar a sus funciones directamente.|Acoplar módulos llamando directamente a sus funciones, haciendo el código difícil de modificar.|
|**Errores**|**Siempre escuchar el evento `error`**. Si se emite un evento `error` y no hay _listener_, Node.js considera que es un error no controlado y **cierra el proceso**.|Olvidar `emitter.on('error', (err) => {...})` en emisores críticos.|
|**Sincronía**|Recordar que los _listeners_ se ejecutan **síncronamente** y en orden.|Esperar que los _listeners_ se ejecuten de forma asíncrona o que Node.js los ejecute en paralelo (no lo hace).|
|**Limpieza**|Usar `.removeListener()` si un _listener_ ya no es necesario (especialmente en bucles o conexiones de larga duración) para prevenir _memory leaks_.|Registrar _listeners_ sin fin en un mismo emisor, consumiendo memoria innecesariamente.|

---

## 🔑 Resumen de Puntos Clave

- Node.js se basa en la programación **Event-Driven**.
    
- La clase **`EventEmitter`** del módulo `events` es la base para crear tu propio sistema de eventos.
    
- Los métodos clave son **`.emit()`** (disparar evento) y **`.on()`** (escuchar evento).
    
- Los _listeners_ se ejecutan **síncronamente** cuando se emite el evento.
    
- La mayoría de los módulos nativos de Node.js (HTTP, FS, Streams) extienden o usan `EventEmitter`.
    
- **Es crucial manejar el evento `error`** para evitar que el proceso Node.js falle.
    

Has dominado la arquitectura fundamental. Como última clase de este nivel, veremos cómo usar Node.js para tareas fuera de los servidores web.

Tu próxima clase te enseñará a crear herramientas de línea de comandos: **[[11 - CLI con Node.js]]**.