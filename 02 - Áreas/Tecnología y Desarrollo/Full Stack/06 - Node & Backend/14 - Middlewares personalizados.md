¡Muy bien! Has aprendido a recibir datos del cliente usando Express, lo cual es la mitad de la batalla. La otra mitad es asegurar que esos datos sean válidos, que el usuario esté autorizado, y registrar lo que sucede. Todo esto se hace de forma limpia y organizada con **Middlewares**.

Esta clase te enseñará a construir tus propias funciones de _middleware_ para modularizar tareas críticas como validación y autenticación.

Aquí tienes la nota para la clase anterior: **[[13 - Rutas avanzadas y parámetros]]**.

---

# 14 - Middlewares personalizados

## 📚 Introducción: El Poder de la Cadena

Como vimos en **[[12 - Introducción a Express]]**, un **Middleware** es una función que se interpone entre la llegada de la petición y la lógica final de la ruta. Es el patrón de diseño central de Express.

La belleza del _middleware_ reside en su capacidad para formar una **cadena de responsabilidad**. Cada función puede realizar una tarea específica (ej: registrar hora, verificar JWT, validar datos) y luego decidir si la petición avanza (`next()`) o se detiene (enviando una respuesta, ej: `res.status(401).send()`).

### Estructura de un Middleware

Un _middleware_ siempre recibe tres argumentos: `(req, res, next)`.

JavaScript

```
const miMiddleware = (req, res, next) => {
    // 1. Ejecutar código (ej: logging, verificación de tokens)
    console.log(`Petición recibida en: ${new Date().toISOString()}`);
    
    // 2. Modificar req o res (opcional)
    req.horaInicio = Date.now();
    
    // 3. Pasar el control al siguiente handler en la cadena
    next(); 
    
    // 🛑 IMPORTANTE: Si falta next() y no hay res.send(), la petición queda colgada.
};
```

---

## 💻 Ejemplo 1: Middleware de Logging (Registro)

Un _logger_ (registrador) es el _middleware_ más común. Sirve para ver qué está haciendo el servidor y cuánto tiempo tarda.

JavaScript

```
// middlewares/logger.js (Buena práctica: separarlos en su propia carpeta)

// Función para calcular el tiempo de respuesta y registrar la actividad
const requestLogger = (req, res, next) => {
    const start = Date.now();
    
    // El 'listener' se activa justo antes de que se envíe la respuesta
    res.on('finish', () => {
        const duration = Date.now() - start;
        console.log(`[${req.method}] ${req.originalUrl} - STATUS: ${res.statusCode} - ${duration}ms`);
    });

    next(); // Permite que la petición continúe
};

module.exports = requestLogger;
// O si usas ESM: export default requestLogger;
```

**Uso en `app.js`:**

JavaScript

```
const express = require('express');
const app = express();
const requestLogger = require('./middlewares/logger');

// Aplicación a nivel de aplicación (afecta a todas las rutas)
app.use(requestLogger); 

app.get('/', (req, res) => {
    res.send('Ruta principal.');
});
// Cuando accedas a '/', el requestLogger imprimirá un log.
```

---

## 🔒 Ejemplo 2: Middleware de Validación y Seguridad

El _middleware_ de seguridad y validación se usa para revisar si una petición tiene la estructura correcta o si el usuario tiene permiso.

### Middleware de Validación de ID

JavaScript

```
// middlewares/validator.js

const validarIdNumerico = (req, res, next) => {
    // Asume que este middleware se usa en una ruta con :id (ej: /usuarios/:id)
    const id = req.params.id;

    // Usamos parseInt() y Number.isNaN para asegurar que es un número entero
    if (Number.isNaN(parseInt(id))) {
        // 🛑 Detenemos la cadena y enviamos una respuesta de error
        return res.status(400).json({ 
            error: 'ID de recurso inválido', 
            mensaje: 'El ID debe ser un número entero.'
        });
    }

    // Si es válido, pasamos el control al siguiente handler (la lógica de la ruta)
    next();
};

module.exports = { validarIdNumerico };
```

**Uso en `app.js`:**

JavaScript

```
const { validarIdNumerico } = require('./middlewares/validator');

// Esta ruta usará primero el middleware de validación
app.get('/usuarios/:id', validarIdNumerico, (req, res) => {
    // Si llegamos aquí, sabemos que req.params.id es un número válido
    res.json({ mensaje: `Datos del usuario ${req.params.id}` });
});
```

Si el cliente intenta acceder a `/usuarios/abc`, el _middleware_ `validarIdNumerico` detectará el error y enviará la respuesta `400` antes de que la lógica de la ruta se ejecute.

---

## ➡️ Tipos de Middlewares y Dónde Aplicarlos

|Tipo de Aplicación|Función|Propósito|Ejemplo|
|---|---|---|---|
|**Global** (`app.use`)|Se aplica a _todas_ las peticiones.|Logging, Body Parsing (`express.json`), Seguridad Global (`helmet`, `cors`).|`app.use(express.json());`|
|**Ruta Específica** (`app.get(..., middleware, handler)`)|Se aplica solo a esa ruta y método.|Validación, Autenticación (¿Usuario logeado?).|`app.post('/new', checkAuth, validator, handler)`|
|**Router** (`router.use`)|Se aplica a todas las rutas dentro de un grupo (ej: `/api/admin/*`).|Autenticación de administrador para un grupo de _endpoints_.|`adminRouter.use(isAdmin);`|

---

## ✅ Buenas Prácticas y Errores Comunes

|Categoría|Buena Práctica|Error Común|
|---|---|---|
|**Organización**|**Separar _middlewares_ en archivos** y carpetas dedicadas (ej: `/middlewares`).|Escribir _middlewares_ largos e inmanejables en el archivo principal (`app.js`).|
|**Fallar Rápido**|El _middleware_ debe fallar (enviar la respuesta de error) lo más pronto posible si algo es inválido (ej: token ausente, datos incorrectos).|Dejar la validación para el _handler_ final de la ruta, desperdiciando recursos.|
|**`next()`**|Entender la diferencia entre `next()` y `next(err)`. **`next(err)`** salta el resto de los _middlewares_ y rutas y va directamente al _middleware_ de manejo de errores (ver [[16 - Manejo de errores]]).|Usar `next()` para pasar un error. Esto puede causar _unhandled errors_.|
|**Mutación**|Usar los _middlewares_ para adjuntar datos a la petición, haciendo que los datos estén disponibles en `req.usuario`, `req.datosValidados`, etc.|Usar variables globales o dependencias externas para pasar datos entre _middlewares_.|

---

## 🔑 Resumen de Puntos Clave

- Un **Middleware** es una función `(req, res, next)` que se ejecuta en la cadena de procesamiento de la petición.
    
- Es la forma más efectiva de **modularizar** tareas transversales como logging, validación, y autenticación.
    
- El método **`next()`** es crucial para pasar el control al siguiente _middleware_ o al _handler_ final de la ruta.
    
- Un _middleware_ debe hacer una de dos cosas: 1) Llamar a `next()`, o 2) Terminar el ciclo con un envío de respuesta (`res.send()`, `res.json()`).
    
- La aplicación de _middlewares_ puede ser **global** (`app.use()`) o **específica de la ruta**.
    

Has aprendido a controlar el flujo de la petición. Ahora podemos armar la estructura completa de una API web profesional.

Tu próxima clase te enseñará a construir una API RESTful completa: **[[15 - APIs RESTful]]**.