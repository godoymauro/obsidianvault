¡Fabuloso! Dejamos atrás los fundamentos para entrar en el mundo práctico del desarrollo de APIs. Ya sabes crear un servidor con el módulo `http` nativo, pero ahora vamos a usar la herramienta que lo simplifica todo: **Express.js**.

Entramos al **Nivel 3: Express.js y APIs REST**.

Aquí tienes la nota para la clase anterior: **[[11 - CLI con Node.js]]**.

---

# 12 - Introducción a Express

## 📚 Introducción: ¿Qué es Express.js?

Vimos en **[[06 - Módulo HTTP]]** que crear un servidor con el módulo nativo de Node.js es tedioso. Tuvimos que analizar manualmente la URL, el método, y gestionar el _stream_ del cuerpo de la petición (POST/PUT).

**Express.js** es un _framework_ de desarrollo web minimalista y flexible para Node.js. Es la solución más popular para construir **APIs RESTful** y aplicaciones web de manera más rápida y organizada.

### 🧠 ¿Por qué Express?

1. **Simplifica HTTP:** Abstrae la complejidad del módulo nativo, proporcionando una interfaz limpia para definir rutas y gestionar peticiones.
    
2. **Rutas Inteligentes:** Permite definir rutas por método HTTP (`app.get()`, `app.post()`) y patrones de URL de forma sencilla.
    
3. **Middlewares:** Su núcleo se basa en el concepto de _Middleware_, un sistema potente para ejecutar funciones en secuencia antes de llegar a la lógica final de la ruta.
    

---

## 🛠 Setup y Primer Servidor con Express

### Paso 1: Inicialización del Proyecto

Asegúrate de estar en una carpeta de proyecto e inicialízala si no lo has hecho:

Bash

```
npm init -y
```

### Paso 2: Instalación de Express

Instalamos Express como dependencia principal:

Bash

```
npm install express
```

### Paso 3: Servidor Básico en `app.js`

Crea el archivo `app.js` (o `index.js`) con el siguiente código:

JavaScript

```
// app.js
const express = require('express'); 
// O si estás usando ES Modules: import express from 'express';

// 1. Inicializar la aplicación Express
const app = express();
const PORT = 3000;

// 2. Definición de Rutas (Endpoints)

// Ruta GET /
// req: Objeto de Petición
// res: Objeto de Respuesta
app.get('/', (req, res) => {
    // Express simplifica la respuesta:
    // .send() puede enviar texto, JSON, HTML, etc.
    res.send('¡Hola desde Express!');
});

// Ruta GET /api/v1/saludo
app.get('/api/v1/saludo', (req, res) => {
    // Express simplifica el envío de JSON con .json()
    res.json({ 
        mensaje: '¡API en funcionamiento!',
        servidor: 'Express' 
    });
});

// 3. Poner el Servidor a Escuchar
app.listen(PORT, () => {
    console.log(`🚀 Servidor Express escuchando en http://localhost:${PORT}`);
    console.log(`Ruta de prueba JSON: http://localhost:${PORT}/api/v1/saludo`);
});
```

### 💻 Ejecución

Bash

```
node app.js
```

---

## 🛣 El Routing en Express

El _Routing_ es el mecanismo que Express usa para determinar cómo debe responder una aplicación a una petición de cliente en una URL y un método HTTP específicos.

|Método HTTP|Propósito|Método Express|Ejemplo|
|---|---|---|---|
|**GET**|Obtener un recurso.|`app.get(path, handler)`|`app.get('/usuarios')`|
|**POST**|Crear un nuevo recurso.|`app.post(path, handler)`|`app.post('/usuarios')`|
|**PUT/PATCH**|Actualizar un recurso.|`app.put(path, handler)` / `app.patch(path, handler)`|`app.put('/usuarios/:id')`|
|**DELETE**|Eliminar un recurso.|`app.delete(path, handler)`|`app.delete('/usuarios/:id')`|
|**ALL**|Para cualquier método HTTP.|`app.all(path, handler)`|`app.all('/secreto')`|

### Objeto `req` y `res` (Simplificados)

Express mantiene los objetos `req` y `res`, pero los enriquece:

| Objeto    | Propiedad/Método    | Descripción                                                           |
| --------- | ------------------- | --------------------------------------------------------------------- |
| **`req`** | `req.params`        | Parámetros de ruta (ver [[13 - Rutas avanzadas y parámetros]]).       |
|           | `req.query`         | Parámetros de consulta (filtros).                                     |
|           | `req.body`          | Cuerpo de la petición (JSON, formularios).                            |
| **`res`** | `res.send()`        | Envía una respuesta HTTP.                                             |
|           | `res.json(obj)`     | Envía una respuesta JSON y establece el `Content-Type`.               |
|           | `res.status(code)`  | Establece el código de estado HTTP (ej: `res.status(404).json(...)`). |
|           | `res.redirect(url)` | Redirige al cliente a una nueva URL.                                  |

---

## 🧩 Middlewares: El Corazón de Express

Un **Middleware** es simplemente una **función** que tiene acceso a los objetos de petición (`req`), respuesta (`res`), y al siguiente _middleware_ en el ciclo de petición-respuesta (`next`).

Los _middlewares_ se ejecutan **en orden** y tienen la capacidad de:

1. Ejecutar cualquier código.
    
2. Hacer cambios a los objetos `req` y `res`.
    
3. Finalizar el ciclo de respuesta (llamando a `res.send()`, etc.).
    
4. Llamar a `next()` para pasar el control al siguiente _middleware_ o al _handler_ de la ruta final.
    

### Estructura de un Middleware

JavaScript

```
const miLogger = (req, res, next) => {
    // 1. Ejecuta lógica
    console.log(`[${new Date().toISOString()}] Petición a ${req.url}`);
    
    // 2. Modifica el objeto req (opcional)
    req.fechaPeticion = new Date();
    
    // 3. Llama a next()
    next(); // Pasa el control al siguiente middleware/ruta.
};

// 🛑 ¡Si no llamas a next() ni envías una respuesta, la petición se congela!
```

### Aplicando Middlewares

Puedes aplicar _middlewares_ a toda la aplicación (`app.use`) o a rutas específicas:

JavaScript

```
// A. Middleware a nivel de aplicación (para TODAS las rutas)
app.use(miLogger);

// B. Middleware para una ruta específica
app.get('/admin', miLogger, (req, res) => {
    // Este código solo se ejecuta si miLogger llama a next()
    res.send(`Petición recibida en: ${req.fechaPeticion}`);
});
```

---

## ✅ Buenas Prácticas y Errores Comunes

|Categoría|Buena Práctica|Error Común|
|---|---|---|
|**Organización**|**Usar `Router`** de Express para agrupar rutas lógicamente (ej: `/api/users`, `/api/products`).|Poner todas las rutas y lógica en el archivo `app.js` principal.|
|**Middlewares**|Usar _middlewares_ para tareas repetitivas (logging, autenticación, validación, parsing).|Poner lógica de _middleware_ dentro del _handler_ de la ruta final.|
|**`next()`**|Entender que un _middleware_ debe llamar a `next()` o enviar una respuesta.|Olvidar `next()` en un _middleware_, causando que la petición se quede "colgada" para siempre.|
|**Body Parsing**|Usar `express.json()` o `express.urlencoded()` para leer el cuerpo de la petición.|Intentar leer el cuerpo de la petición manualmente con _Streams_ como en el módulo `http` nativo.|

---

## 🔑 Resumen de Puntos Clave

- **Express.js** es un _framework_ minimalista que simplifica la creación de servidores HTTP en Node.js.
    
- El **Routing** en Express es simple: `app.get()`, `app.post()`, etc., mapean el método y la URL a un _handler_.
    
- **Middlewares** son funciones intermedias que se ejecutan secuencialmente. Son esenciales para la modularidad.
    
- Los _middlewares_ deben llamar a **`next()`** para pasar el control al siguiente eslabón de la cadena o enviar la respuesta final.
    
- Express enriquece los objetos `req` y `res` con métodos útiles como `res.json()`, `res.status()`, `req.query`, etc.
    

Hemos sentado las bases. Tu siguiente paso es dominar la lectura de datos que llegan en la URL o en el cuerpo de la petición.

Tu próxima clase te enseñará sobre los datos que viajan con la ruta: **[[13 - Rutas avanzadas y parámetros]]**.