¡Claro! Pasamos de la eficiencia de datos con GraphQL a la necesidad de **comunicación instantánea** y bidireccional. Las APIs REST y GraphQL están diseñadas para un modelo de "petición-respuesta", pero no sirven para eventos en tiempo real, como un chat o notificaciones en vivo.

Esta clase te introducirá a los **WebSockets** y a **Socket.IO**, la herramienta estándar de Node.js para el desarrollo en tiempo real.

Aquí tienes la nota para la clase anterior: **[[27 - GraphQL con Node.js]]**.

---

# 28 - WebSockets y tiempo real

## 📚 Introducción: El Límite de HTTP

El protocolo **HTTP** (el usado por REST y GraphQL) es **unidireccional y sin estado (_stateless_)**. Esto significa que:

1. El cliente debe iniciar siempre la comunicación.
    
2. Una vez que el servidor responde, la conexión se cierra.
    

Para simular el tiempo real, las soluciones antiguas (como _Polling_ o _Long Polling_) eran ineficientes, ya que bombardeaban al servidor con peticiones repetidas.

**WebSockets** resuelve esto. Es un protocolo de comunicación que permite una conexión **persistente y bidireccional** entre el cliente y el servidor, sobre un único _socket_ TCP.

---

## 📡 WebSockets: Conexión Persistente

El protocolo WebSockets comienza con un _handshake_ HTTP normal (una petición de `Upgrade`). Si el servidor acepta, la conexión se **actualiza** y permanece abierta indefinidamente.

|Característica|WebSockets|HTTP (REST/GraphQL)|
|---|---|---|
|**Conexión**|Persistente, abierta, bidireccional.|Transitoria, se cierra después de la respuesta.|
|**Uso**|Tiempo real, baja latencia, transferencia constante de datos.|Petición de datos, CRUD, operaciones de escritura.|
|**Overhead**|Muy bajo (solo los datos, sin encabezados HTTP repetitivos).|Alto (se envían encabezados en cada petición).|

---

## 🚀 Socket.IO: La Capa de Abstracción en Node.js

Programar con la API nativa de WebSockets de Node.js puede ser complejo. **Socket.IO** es una librería que abstrae el protocolo y añade características cruciales para el desarrollo en tiempo real:

1. **Compatibilidad:** Funciona incluso si el cliente no soporta WebSockets, recurriendo automáticamente a otras tecnologías (_Long Polling_).
    
2. **Reconexión Automática:** Si la conexión cae, Socket.IO intenta reconectar automáticamente.
    
3. **Rooms & Namespaces:** Permite segmentar usuarios (ej: separar el chat de un juego de las notificaciones de otro chat).
    

### Paso 1: Instalación

Necesitas el servidor y el cliente (el cliente se usa en el frontend, pero la librería principal es para el backend de Node.js).

Bash

```
# Servidor Node.js
npm install socket.io
```

### Paso 2: Creación del Servidor Socket.IO

Socket.IO requiere un servidor **HTTP** existente (como el de Express) para poder montarse sobre él.

**Archivo:** `socket-server.js`

JavaScript

```
// socket-server.js
const express = require('express');
const http = require('http'); // Módulo HTTP nativo
const { Server } = require('socket.io');

const app = express();
const httpServer = http.createServer(app); // Creamos el servidor HTTP
const io = new Server(httpServer, {
    // Configuramos CORS para permitir conexión desde el frontend
    cors: {
        origin: "http://localhost:8080", // Domínio del cliente
        methods: ["GET", "POST"]
    }
});

const PORT = 3000;

// 💡 1. Manejo de la Conexión
io.on('connection', (socket) => {
    // 'socket' es el objeto de conexión de CADA cliente individual
    console.log(`[CONN] Cliente conectado: ${socket.id}`);

    // --- 2. Escuchando eventos desde el Cliente ---
    socket.on('mensajeChat', (msg) => {
        console.log(`Mensaje recibido de ${socket.id}: ${msg}`);
        
        // --- 3. Emisión de eventos ---
        // io.emit(): Envía el mensaje a TODOS los clientes conectados
        io.emit('nuevoMensaje', { usuarioId: socket.id, texto: msg });
    });
    
    // Escuchando la desconexión
    socket.on('disconnect', () => {
        console.log(`[DISCONN] Cliente desconectado: ${socket.id}`);
    });
});

httpServer.listen(PORT, () => {
    console.log(`🚀 Socket.IO Server corriendo en puerto ${PORT}`);
});
```

---

## 💬 3. Comunicación: Emisión y Escucha de Eventos

La comunicación en Socket.IO se basa en el patrón de **[[10 - Eventos y EventEmitter]]**, pero sobre la red.

|Objeto de Emisión|Método|Audiencia|
|---|---|---|
|**`io.emit('evento', datos)`**|Global|Envía el evento a **TODOS** los clientes conectados.|
|**`socket.emit('evento', datos)`**|Individual|Envía el evento **SOLO** al cliente que corresponde a ese `socket`.|
|**`socket.broadcast.emit('evento', datos)`**|Global, excepto emisor|Envía el evento a **TODOS MENOS** al cliente que lo envió.|
|**`io.to('roomName').emit('evento', datos)`**|Grupo (Room)|Envía el evento a **TODOS** los clientes unidos a esa sala.|

### Rooms (Salas)

Las **Rooms** son esenciales para la escalabilidad. Si tienes un chat con 1000 usuarios pero solo 5 están en el chat de "Soporte", solo debes enviar los mensajes al _room_ "Soporte".

JavaScript

```
io.on('connection', (socket) => {
    // Unirse a una sala (ej: al iniciar sesión)
    socket.on('joinRoom', (roomName) => {
        socket.join(roomName);
        console.log(`Cliente ${socket.id} se unió a la sala ${roomName}`);
        
        // Emitir SOLO a esa sala
        io.to(roomName).emit('notificacion', `Bienvenido a ${roomName}`);
    });

    // Enviar un mensaje dentro de una sala
    socket.on('mensajeSala', (data) => {
        io.to(data.room).emit('nuevoMensaje', data.msg);
    });
});
```

---

## ✅ Buenas Prácticas y Errores Comunes

|Categoría|Buena Práctica|Error Común|
|---|---|---|
|**Seguridad**|Aplicar **Autenticación** y **Autorización** al evento `connection` o a cada evento. No confíes en el `socket.id`.|Asumir que un `socket.id` es suficiente para identificar a un usuario (son efímeros) o no verificar los permisos.|
|**Mensajería**|Usar **Rooms** para segmentar el tráfico y optimizar el rendimiento.|Usar `io.emit` (emisión global) para enviar mensajes privados o de grupo reducido, saturando a todos los clientes.|
|**Performance**|**Evitar enviar grandes volúmenes de datos** por Socket.IO. Úsalo para notificaciones y datos pequeños.|Intentar transferir imágenes o videos completos a través de WebSockets (mejor usar un enlace y HTTP/CDN).|
|**Escalabilidad**|Usar un **Adaptador** (ej: Redis Adapter) para Socket.IO. Esto permite que múltiples instancias del servidor Node.js se comuniquen entre sí y compartan los _rooms_.|Ejecutar múltiples instancias de Node.js sin un adaptador, lo que resulta en clientes conectados a diferentes instancias sin recibir todos los mensajes.|

---

## 🔑 Resumen de Puntos Clave

- **WebSockets** es un protocolo que establece una conexión **persistente y bidireccional** sobre un único _socket_ TCP, ideal para el tiempo real.
    
- **Socket.IO** es la librería más utilizada en Node.js, añadiendo reconexión, _fallbacks_ y gestión de **Rooms** y **Namespaces**.
    
- La comunicación se basa en **eventos** (`.emit()` y `.on()`), no en peticiones HTTP.
    
- Debes **montar** el servidor de Socket.IO sobre un servidor HTTP existente (ej: Express con `http.createServer`).
    
- Las **Rooms** son esenciales para el rendimiento, permitiendo segmentar la audiencia de los eventos.
    

Has aprendido a construir APIs, gestionar datos y comunicarte en tiempo real. Ahora, veremos cómo organizar sistemas más grandes, compuestas por muchos servicios.

Tu próxima clase se centrará en la arquitectura de sistemas distribuidos: **[[29 - Microservicios]]**.