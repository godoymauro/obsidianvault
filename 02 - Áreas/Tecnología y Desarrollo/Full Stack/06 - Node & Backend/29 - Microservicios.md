¡Continuemos con las arquitecturas modernas! Has aprendido a construir y asegurar servicios individuales (monolitos o APIs REST/GraphQL), pero las aplicaciones empresariales grandes se dividen en muchos servicios pequeños e interdependientes.

Esta clase te introducirá a los **Microservicios**, el patrón arquitectónico dominante para la escalabilidad moderna, y cómo se comunican entre sí.

Aquí tienes la nota para la clase anterior: **[[28 - WebSockets y tiempo real]]**.

---

# 29 - Microservicios

## 📚 Introducción: Del Monolito a los Microservicios

Históricamente, las aplicaciones se construían como un **Monolito**: un solo bloque grande de código que contenía toda la lógica de negocio (usuarios, pedidos, pagos, inventario) y que se desplegaba como una única unidad.

|Característica|Monolito|Microservicios|
|---|---|---|
|**Arquitectura**|Una sola aplicación gigante.|Conjunto de servicios pequeños e independientes.|
|**Despliegue**|Despliegue único. Un pequeño cambio requiere desplegar todo.|Despliegues independientes. Cada servicio se despliega por separado.|
|**Tecnología**|Usa una única tecnología/lenguaje (ej: todo Node.js).|Permite usar la mejor tecnología para cada servicio (ej: Node.js para API, Python para ML).|
|**Escalabilidad**|Dificultad para escalar partes específicas; se debe escalar todo.|**Escalabilidad granular.** Se escala solo el servicio que lo necesita (ej: solo el servicio de Pedidos).|

Un **Microservicio** es un enfoque para desarrollar una única aplicación como un conjunto de servicios más pequeños, cada uno ejecutándose en su propio proceso y comunicándose a través de mecanismos ligeros.

---

## 🧱 Estructura de Microservicios en Node.js

En Node.js, cada microservicio es una **aplicación Express/Koa/Hapi independiente**, con su propia base de datos (o parte de ella) y su propia lógica de negocio.

|Servicio Típico|Propósito|Tecnología Común|
|---|---|---|
|**Servicio de Usuarios**|Autenticación, perfiles, roles.|Node.js (Express) + MongoDB/PostgreSQL|
|**Servicio de Inventario**|Gestión de stock, precios.|Java/Node.js + PostgreSQL (Transacciones)|
|**API Gateway**|Punto de entrada único para el cliente, maneja autenticación, _rate limiting_ y enrutamiento.|Node.js (Express, para simplicidad)|

---

## 🤝 Comunicación entre Microservicios

La comunicación es el aspecto más complejo de esta arquitectura. Los servicios necesitan hablar entre sí, y hay dos patrones principales:

### 1. Comunicación Síncrona (RPC y HTTP)

El servicio cliente **espera** una respuesta inmediata del servicio proveedor.

|Mecanismo|Descripción|Librería Node.js|Uso|
|---|---|---|---|
|**REST/HTTP**|Comunicación estándar de petición/respuesta. Un servicio llama al _endpoint_ REST de otro.|`axios`, `node-fetch`|Consultas simples (ej: obtener datos de usuario).|
|**gRPC (RPC)**|Protocolo de llamada a procedimiento remoto. Más eficiente que REST, usa HTTP/2 y _Payloads_ binarios (Protocol Buffers).|`@grpc/grpc-js`|Comunicación interna de alta performance y baja latencia.|

**Problema de Sincronía:** Si el Servicio A llama al Servicio B, y el Servicio B está caído, el Servicio A también falla (**Acoplamiento Temporal**).

### 2. Comunicación Asíncrona (Mensajería)

Los servicios se comunican a través de un **Broker de Mensajes** (cola). El servicio A envía un mensaje a la cola y no espera una respuesta; el servicio B lee ese mensaje cuando puede. Esto desacopla el emisor del receptor.

|Mecanismo|Descripción|Librería Node.js|Uso|
|---|---|---|---|
|**Colas de Mensajes**|Un servicio publica un mensaje en un _Topic_; otro servicio lo consume.|**RabbitMQ** (`amqplib`), **Kafka** (`kafkajs`).|Eventos de dominio, tareas largas (ej: procesamiento de video, envío de emails).|

**Ventaja de Asincronía:** El servicio emisor no se bloquea si el receptor está caído, ya que el _Broker_ guarda el mensaje hasta que el receptor esté listo (**Resiliencia**).

---

## 🚀 gRPC: Comunicación Interna de Alto Rendimiento

**gRPC** (desarrollado por Google) es la opción preferida para la comunicación interna entre microservicios de alto rendimiento.

### ¿Por qué gRPC sobre REST?

- **HTTP/2:** Usa HTTP/2, que permite _multiplexing_ (múltiples peticiones en una sola conexión) y es más rápido.
    
- **Protocol Buffers (Protobuf):** Los datos se serializan en un formato binario compacto y tipado, en lugar de JSON.
    
- **Definición de Contrato:** La comunicación se define en un archivo `.proto` estricto, lo que garantiza el tipado entre servicios escritos en diferentes lenguajes.
    

### Ejemplo de Definición Protobuf

Protocol Buffers

```
// users.proto
// Definición del servicio y los mensajes (datos)

syntax = "proto3";

// 1. Definición de la estructura de datos (Mensajes)
message UserRequest {
  string userId = 1; // Campo 1
}

message UserResponse {
  string userId = 1;
  string name = 2;
  string email = 3;
}

// 2. Definición del servicio RPC
service UserService {
  // Un método RPC llamado GetUser que recibe UserRequest y devuelve UserResponse
  rpc GetUser (UserRequest) returns (UserResponse);
}
```

**Node.js:** Usando el código generado a partir de este archivo `.proto`, puedes llamar a `UserService.GetUser({ userId: '123' })` desde otro servicio Node.js, como si fuera una función local.

---

## 📦 Colas de Mensajes (RabbitMQ/Kafka)

Las colas de mensajes son esenciales para la arquitectura impulsada por eventos (_Event-Driven Architecture_) en microservicios.

**Ejemplo de Flujo (RabbitMQ):**

1. **Servicio de Pedidos:** Recibe un pago. Publica un mensaje: `Pedido_Completado` en la cola.
    
2. **Servicio de Envío:** Escucha la cola, recibe el mensaje, e inicia la etiqueta de envío.
    
3. **Servicio de Emails:** Escucha la cola, recibe el mensaje, y envía el email de confirmación al cliente.
    

El servicio de Pedidos nunca necesitó saber dónde estaban el Servicio de Envío o el Servicio de Emails; solo le importaba publicar el evento.

---

## ✅ Buenas Prácticas y Errores Comunes

|Categoría|Buena Práctica|Error Común|
|---|---|---|
|**Límites de Servicio**|Diseñar microservicios alrededor de límites de **Dominio de Negocio** (ej: un servicio para **Pedidos**, otro para **Facturación**).|Dividir servicios por capas técnicas (ej: un servicio para la capa de presentación, otro para la capa de datos).|
|**API Gateway**|Usar un **API Gateway** (Proxy) como el único punto de entrada para los clientes externos.|Exponer cada microservicio directamente a Internet.|
|**Transacciones**|Usar patrones de **Saga** para gestionar transacciones que abarcan múltiples servicios (No hay _joins_ de BD entre servicios).|Intentar implementar transacciones distribuidas complejas (muy difícil y arriesgado).|
|**Comunicación**|Usar **gRPC** para llamadas sincrónicas internas de alta performance y **Colas de Mensajes** para eventos asíncronos.|Usar REST/JSON para todas las comunicaciones internas de alto volumen.|

---

## 🔑 Resumen de Puntos Clave

- Los **Microservicios** dividen una aplicación en servicios pequeños, independientes y escalables.
    
- El **desacoplamiento** y la **escalabilidad granular** son las principales ventajas.
    
- La comunicación puede ser **Síncrona** (REST, **gRPC**—el más eficiente para interno) o **Asíncrona** (Colas de Mensajes como **RabbitMQ** o **Kafka**).
    
- **gRPC** usa HTTP/2 y _Protocol Buffers_ para una comunicación interna más rápida y tipada.
    
- La **Mensajería Asíncrona** es clave para la **resiliencia**, ya que el servicio emisor no espera por el receptor.
    

Has cubierto la arquitectura tradicional (Microservicios) y asíncrona (WebSockets). Ahora, veremos cómo simplificar aún más la infraestructura, delegando la gestión del servidor por completo.

Tu próxima clase se centrará en el desarrollo sin servidor: **[[30 - Serverless]]**.