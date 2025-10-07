¡Fantástico! Has construido una API RESTful completa con todas las prácticas de producción. Ahora, para alcanzar el nivel de **Experto en Backend**, integramos las arquitecturas más modernas que estudiaste en el Nivel 6.

Esta última clase te desafía a pensar en la arquitectura de un sistema empresarial moderno.

---

# 33 - Proyecto avanzado: Microservicios + GraphQL + WebSockets

## 📚 Introducción: Arquitectura Distribuida Moderna

Un proyecto avanzado en Node.js rara vez es un simple monolito REST. Las grandes aplicaciones (como Uber, Netflix, etc.) utilizan una combinación de tecnologías para gestionar la complejidad, la velocidad y la comunicación en tiempo real.

Este proyecto integra:

1. **Microservicios:** Separación de dominios de negocio (ej: `Usuarios` y `Pedidos`).
    
2. **GraphQL:** Como **API Gateway** unificado para el cliente, resolviendo el _over-fetching_.
    
3. **WebSockets (Socket.IO):** Para notificaciones en tiempo real entre los servicios.
    
4. **Docker + CI/CD:** Para la portabilidad y automatización del despliegue.
    

---

## 🏗 Estructura de la Arquitectura

El proyecto se divide en tres servicios principales, cada uno con su propio contenedor Docker y proceso:

Fragmento de código

```
graph LR
    subgraph Cliente (Frontend)
        A[App Web/Móvil]
    end

    subgraph Backend Services
        B(API Gateway GraphQL)
        C(Microservicio de Usuarios)
        D(Microservicio de Pedidos)
    end
    
    A -->|1. Consulta Única| B
    B -->|2. Resuelve User| C
    B -->|3. Resuelve Pedidos| D
    
    C -->|4. Evento Asíncrono| E(Broker de Mensajes - RabbitMQ/Kafka)
    D -->|4. Evento Asíncrono| E
    
    E -->|5. Recibe Evento| D
    E -->|5. Recibe Evento| C
    
    D -->|6. Notificación en Vivo| F(WebSockets - Socket.IO)
    F -->|7. Push en Tiempo Real| A
```

---

## 1. El Microservicio Básico (Usuarios y Pedidos)

Cada microservicio es una aplicación Node.js independiente (similar a tu [[32 - Proyecto guiado]]) pero con una única responsabilidad (SRP):

- **Responsabilidad:** CRUD completo de su dominio (ej: Servicio de Usuarios solo maneja `email`, `password`, `rol`).
    
- **Comunicación Externa:** Se exponen internamente a través de un **endpoint REST simple** (para que el GraphQL Gateway pueda consultarlos) y a través de un **Broker de Mensajes** para eventos asíncronos.
    
- **Base de Datos:** Cada uno tiene su propia BD (ej: Usuarios en MongoDB, Pedidos en PostgreSQL).
    

**Práctica Clave:** Implementa la lógica de que al crear un nuevo pedido (en el **Servicio de Pedidos**), este servicio **publica un evento** llamado `pedido_creado` en el broker de mensajes (Kafka/RabbitMQ - ver [[29 - Microservicios]]).

---

## 2. API Gateway con GraphQL

El GraphQL Gateway es el **único punto de contacto** para el cliente. Su único trabajo es componer la respuesta consultando los servicios internos.

### Resolución de Relaciones entre Servicios

El desafío es cómo el GraphQL Gateway resuelve la relación entre un usuario (en el Servicio de Usuarios) y sus pedidos (en el Servicio de Pedidos).

JavaScript

```
// GraphQL Gateway - Ejemplo de Resolver
const resolvers = {
    Query: {
        usuario: async (parent, { id }) => {
            // Llama al Microservicio de Usuarios (GET /users/:id)
            const user = await axios.get(`http://servicio-usuarios:3001/users/${id}`);
            return user.data;
        },
    },

    // 💡 Resolver para la Relación (la magia de la composición)
    Usuario: {
        pedidos: async (parent, args) => {
            // parent es el objeto Usuario que acabamos de resolver (contiene el ID)
            // Llama al Microservicio de Pedidos (GET /orders?userId=X)
            const pedidos = await axios.get(`http://servicio-pedidos:3002/orders?userId=${parent.id}`);
            return pedidos.data;
        }
    }
};
```

**Optimización Avanzada:** Para resolver el problema **N+1** que se crea aquí (si un cliente pide 100 usuarios, el resolver `Usuario.pedidos` hará 100 peticiones), implementarías **Data Loaders** (ver [[27 - GraphQL con Node.js]]).

---

## 3. WebSockets y Notificaciones en Tiempo Real

Usamos Socket.IO para notificar al cliente sobre eventos asíncronos.

**Flujo:**

1. **Servicio de Envío (hipotético):** Actualiza el estado de un pedido a "Enviado".
    
2. **Servicio de Envío** publica un evento: `pedido_enviado` en el Broker de Mensajes.
    
3. El **Servicio de Pedidos** escucha el evento `pedido_enviado`.
    
4. El **Servicio de Pedidos** usa su instancia de Socket.IO para emitir un mensaje al _Room_ del usuario afectado (ver [[28 - WebSockets y tiempo real]]).
    

### Lógica en el Servicio de Pedidos (`socket-notifier.js`)

JavaScript

```
// socket-notifier.js
// Asume que la conexión 'io' se inicializó al arrancar el servicio de Pedidos
const io = require('./socketio-instance'); 

// 💡 Función que se dispara al recibir el evento 'pedido_enviado' del Broker
const handleOrderSentEvent = (eventData) => {
    const { userId, orderId } = eventData;
    
    // Envía la notificación SOLO al usuario que posee el pedido
    io.to(userId).emit('pedidoActualizado', {
        id: orderId,
        estado: 'ENVIADO',
        mensaje: 'Tu pedido está en camino!'
    });
};

// ... escuchar broker de mensajes y llamar a handleOrderSentEvent
```

**Práctica Clave:** Para que Socket.IO funcione en un entorno de microservicios con múltiples réplicas (escalabilidad), necesitas usar un **Adaptador de Socket.IO** (ej: Redis Adapter) para que todos los procesos worker puedan comunicarse y enviar mensajes a las _Rooms_ correctas.

---

## 4. Despliegue con Docker Compose (CI/CD)

Para ejecutar este sistema de múltiples servicios, la herramienta más simple es **Docker Compose** (parte de Docker).

El archivo `docker-compose.yml` define y conecta todos tus servicios:

YAML

```
version: '3.8'
services:
  # Servicio 1: GraphQL Gateway
  gateway:
    build: ./gateway-graphql
    ports:
      - "4000:4000"
    environment:
      USER_SERVICE_URI: http://users:3001 # 💡 Uso del nombre del servicio Docker
      ORDER_SERVICE_URI: http://orders:3002
      
  # Servicio 2: Microservicio de Usuarios
  users:
    build: ./service-users
    ports:
      - "3001:3001"
    environment:
      # ... Variables de entorno del usuario ...
    depends_on:
      - rabbitmq # Espera que RabbitMQ esté arriba

  # Servicio 3: Microservicio de Pedidos
  orders:
    build: ./service-orders
    ports:
      - "3002:3002"
    depends_on:
      - rabbitmq
      
  # Servicio 4: Broker de Mensajes
  rabbitmq:
    image: rabbitmq:3-management-alpine
    ports:
      - "5672:5672" # Puerto de mensajería
      - "15672:15672" # Puerto de gestión web
```

**Comando de Ejecución:** `docker compose up --build` inicia los cuatro servicios, crea su red interna y los expone en sus respectivos puertos.

---

## ✅ Lecciones Finales para el Experto

|Principio|Lección Aprendida|Integración|
|---|---|---|
|**Desacoplamiento**|Los microservicios no dependen directamente de los demás; se comunican por **eventos asíncronos** (Broker) o por **API**.|El Servicio de Pedidos no llama directamente al Servicio de Usuarios para obtener el email; el GraphQL Gateway hace la composición.|
|**Composición**|El **GraphQL Gateway** compone una vista unificada para el cliente, ocultando la complejidad de los microservicios subyacentes.|Un cliente solo ve `/graphql`, no `/users` y `/orders` separados.|
|**Observabilidad**|El _logging_ y el monitoreo deben configurarse para el **entorno distribuido** (ver [[24 - Logs y monitoreo]]) con un ID de correlación único para rastrear una petición a través de los tres servicios.|Usar herramientas APM para seguir el _trace_ de una consulta GraphQL a través del Gateway, Usuarios, y Pedidos.|
|**Consistencia**|La consistencia de datos debe ser gestionada por los **eventos de dominio** (SAGA pattern).|Cuando se elimina un usuario, el Servicio de Usuarios publica un evento `usuario_eliminado`, y el Servicio de Pedidos **escucha** y marca sus pedidos como huérfanos.|

---

# 🎓 ¡Fin del Curso!

¡Felicidades! Has completado el plan de estudios completo para convertirte en un **Experto en Backend con Node.js**. Desde el Event Loop hasta los Microservicios, ahora posees las herramientas y el conocimiento para construir cualquier aplicación moderna y escalable.

Tu siguiente paso es poner todo esto en práctica. ¿Qué proyecto te gustaría abordar ahora, quizás implementar el **Data Loader** para optimizar el GraphQL Gateway?