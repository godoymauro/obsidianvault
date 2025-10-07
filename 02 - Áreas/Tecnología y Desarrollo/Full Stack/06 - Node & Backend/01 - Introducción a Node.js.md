# 01 - Introducción a Node.js

## 📚 Introducción: ¿Por qué Node.js?

Antes de Node.js, **JavaScript** vivía exclusivamente en el navegador (el **Frontend**). Para el **Backend** (servidores, bases de datos, lógica de negocio), se usaban lenguajes como PHP, Java, Python o Ruby.

La llegada de Node.js cambió esto. Permitió que los desarrolladores usaran JavaScript para **todo**, desde el navegador hasta el servidor.

Aquí tienes los apuntes del curso anterior: [[01 - Introducción a Build Tools]]

---

## 📝 ¿Qué es Node.js?

Node.js no es un lenguaje de programación (ese es JavaScript), ni es un _framework_ (como Express.js o React).

**Definición:** **Node.js** es un **entorno de ejecución (runtime)** de JavaScript del lado del servidor.

El corazón de Node.js es el **motor V8 de Google**, el mismo que utiliza el navegador Chrome para ejecutar JavaScript. Ryan Dahl tomó este motor y le añadió librerías de bajo nivel (escritas en C++) para que JavaScript pudiera interactuar con el sistema operativo: acceder a archivos, gestionar redes, crear servidores, etc.

### Diagrama Conceptual

Fragmento de código

```
graph TD
    A[Tu Código JavaScript] --> B(Node.js Runtime);
    B --> C(Motor V8 - Ejecución Rápida de JS);
    B --> D(Librerías C++ - Acceso al Sistema);
    D --> E(Sistema Operativo);
    E --> F(Red, Archivos, Base de Datos);
```

---

## ⏳ Historia Breve

|Año|Evento|Impacto|
|---|---|---|
|**2009**|**Ryan Dahl presenta Node.js.**|Libera JavaScript del navegador. Introduce el concepto de **I/O No Bloqueante**.|
|**2010**|**NPM (Node Package Manager)** es lanzado.|Se convierte rápidamente en el ecosistema de librerías más grande del mundo.|
|**2015**|**Fundación Node.js** creada.|Asegura la longevidad y el desarrollo continuo del proyecto.|
|**Hoy**|**Uso masivo.**|Empresas como Netflix, Uber y PayPal lo usan para backend.|

Exportar a Hojas de cálculo

---

## 🔄 Diferencias Clave con el Backend Tradicional

Esta es la parte más importante para entender la filosofía de Node.js.

|Característica|Backend Tradicional (e.g., PHP, Java Servlets)|Node.js|
|---|---|---|
|**Modelo de Concurrencia**|**Multi-Hilo (Multi-Threaded)**|**Hilo Único (Single-Threaded)**|
|**Manejo de I/O**|**Bloqueante (Blocking I/O)**|**No Bloqueante (Non-Blocking I/O)**|
|**Filosofía**|Cada nueva solicitud obtiene un nuevo hilo. Si una solicitud espera (I/O), el hilo se bloquea hasta terminar.|Usa un solo hilo para la lógica. Las operaciones que toman tiempo (I/O) se delegan y se gestionan con _callbacks_ o _Promises_. El hilo principal nunca se detiene.|
|**Rendimiento**|Bueno, pero consume más memoria y recursos por el manejo de muchos hilos.|**Excelente** para aplicaciones I/O intensivas (APIs, _streaming_, chats) porque maneja miles de conexiones con un solo hilo eficiente.|
|**Estructura**|Pesada, con más configuraciones y _boilerplate_.|**Ligera**, modular y minimalista por naturaleza.|

Exportar a Hojas de cálculo

### 🧠 Concepto Clave: I/O No Bloqueante

Imagina un chef (el hilo principal de Node.js):

1. **Modelo Bloqueante (Chef Tradicional):** Un cliente pide una sopa (tarea I/O: hervir agua). El chef se sienta y mira la olla hasta que el agua hierve. Mientras tanto, no puede atender a ningún otro cliente. **(Lento, desperdicio de recursos)**.
    
2. **Modelo No Bloqueante (Chef Node.js):** Un cliente pide una sopa. El chef pone el agua a hervir (delega la tarea) y le dice a un ayudante (el Event Loop) que le avise cuando esté lista. Mientras espera, el chef atiende al segundo cliente, al tercero, etc. **(Rápido, eficiente, atiende a muchos a la vez)**.
    

Esto se explica en detalle en la clase **[[03 - Node.js Runtime]]**.

---

## ✅ Buenas Prácticas y Errores Comunes (Iniciales)

|Categoría|Buena Práctica|Error Común|
|---|---|---|
|**Filosofía**|**Abraza la asincronía.** Usa `async/await` o `Promises` para manejar operaciones I/O.|**Intentar usar programación sincrónica** (como en PHP o Python) para I/O. Esto **bloquea** el único hilo de Node.js, matando la performance.|
|**Entorno**|**Usa `nvm`** (Node Version Manager) para gestionar las versiones de Node.js.|Instalar Node.js globalmente sin un gestor de versiones. Esto causa conflictos en diferentes proyectos. (**Ver [[02 - Instalación y configuración]]**).|
|**Código**|**Usa `const` y `let`** sobre `var`. Adhiérete a la sintaxis moderna de JS (ES Modules, `async/await`).|Escribir código con sintaxis antigua (`var`, _callbacks_ anidados o **Callback Hell**).|

Exportar a Hojas de cálculo

---

## 🛠 Ejemplo Práctico: Nuestro Primer Servidor

No te preocupes si el código parece un poco mágico por ahora. En **[[06 - Módulo HTTP]]** lo desglosaremos. Este es el "Hello World" del backend con Node.js puro.

1. Crea un archivo llamado `server.js`.
    
2. Pega el siguiente código:
    

JavaScript

```
// server.js

// 1. Importamos el módulo HTTP nativo de Node.js
const http = require('http');

// 2. Definimos el host (tu propia máquina) y el puerto (puerta de entrada)
const hostname = '127.0.0.1'; // localhost
const port = 3000;

// 3. Creamos el servidor. Esta función es un "listener" de requests.
// Recibe la petición (req) y el objeto de respuesta (res).
const server = http.createServer((req, res) => {
  // Establecemos el código de estado HTTP (200 OK)
  // y el tipo de contenido que vamos a enviar (texto plano)
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');

  // Escribimos y enviamos la respuesta al cliente
  res.end('¡Hola desde mi primer servidor Node.js!\n');
});

// 4. Ponemos el servidor a escuchar en el host y puerto definidos
server.listen(port, hostname, () => {
  console.log(`Servidor ejecutándose en http://${hostname}:${port}/`);
});

// ¡Felicidades! Tienes un servidor web básico corriendo.
```

### 💻 Comando para Ejecutar

Abre tu terminal en la carpeta donde guardaste `server.js` y ejecuta:

Bash

```
node server.js
```

Verás el mensaje: `Servidor ejecutándose en http://127.0.0.1:3000/`.

Abre tu navegador y ve a esa dirección. ¡Verás tu mensaje!

---

## 🔑 Resumen de Puntos Clave

- **Node.js** es un **entorno de ejecución** que permite usar JavaScript fuera del navegador, típicamente en el servidor.
    
- Utiliza el **Motor V8** de Google, lo que le da una gran velocidad.
    
- Su principal fortaleza es su arquitectura **Single-Threaded** y **Non-Blocking I/O** (I/O No Bloqueante), lo que lo hace increíblemente eficiente para manejar miles de conexiones simultáneas.
    
- **No bloquees el hilo principal.** Si una tarea tarda (como una consulta a BD o leer un archivo), usa asincronía.
    

¡Hemos cubierto la base! Ahora sabes **qué es** Node.js y **por qué** es tan popular en el backend.

Tu siguiente paso es esencial: **[[02 - Instalación y configuración]]**.