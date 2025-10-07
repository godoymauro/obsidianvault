¡Claro! Nuestra API RESTful en la clase anterior ( **[[15 - APIs RESTful]]**) funciona, pero notaste un problema: tuvimos que manejar errores (`if (!descripcion) return res.status(400)`) dentro de cada _handler_ de ruta. Esto es repetitivo y desordenado.

Una API robusta requiere un sistema centralizado para gestionar y reportar errores. Express tiene una forma elegante de hacer esto.

Aquí tienes la nota para la clase anterior: **[[15 - APIs RESTful]]**.

---

# 16 - Manejo de errores

## 📚 Introducción: Centralizar el Control

En una aplicación Express, los errores pueden surgir en muchos lugares:

1. **Sincrónicos:** Errores de sintaxis o lógica simple (ej: intentar acceder a una propiedad de `undefined`).
    
2. **Asincrónicos:** Errores de operaciones delegadas (ej: falla en la conexión a la base de datos, error de I/O en un `fs.writeFile`).
    
3. **Intencionales:** Errores de negocio (ej: usuario no encontrado, datos inválidos).
    

Express ofrece un mecanismo para atrapar y manejar todos estos errores en un único lugar: el **Middleware de Error**.

---

## 🛠 1. El Middleware de Error

Un _middleware_ normal recibe tres argumentos: `(req, res, next)`. Un **Middleware de Error** es especial porque recibe **cuatro** argumentos: `(err, req, res, next)`.

Cuando llamas a **`next(errorObjeto)`** en cualquier parte de tu cadena de procesamiento (ruta o _middleware_), Express se da cuenta de que es un error, **salta el resto de la cadena** y dirige la petición directamente al primer _middleware_ de cuatro argumentos que encuentra.

### Estructura Esencial del Middleware de Error

JavaScript

```
// Este debe ser el ÚLTIMO app.use() en tu archivo de configuración
const manejadorErrores = (err, req, res, next) => {
    // 1. Logear el error para el desarrollador (IMPORTANTE)
    console.error(`[ERROR] en ${req.originalUrl}:`, err);

    // 2. Determinar el Status Code (400, 404, 500, etc.)
    // Si el error no tiene un status code, asumimos 500 (Internal Server Error)
    const statusCode = err.statusCode || 500;
    
    // 3. Enviar una respuesta consistente al cliente
    res.status(statusCode).json({
        status: 'error',
        message: err.message || 'Error interno del servidor. Intente de nuevo más tarde.',
        // Opcional: solo mostrar el stack trace en modo desarrollo
        stack: process.env.NODE_ENV === 'development' ? err.stack : undefined 
    });
};

module.exports = manejadorErrores;
```

---

## 2. Creando Errores Personalizados (HTTP Errors)

Para tener un manejo de errores limpio, debemos estandarizar los errores que generamos intencionalmente.

Lo más común es crear una clase que extienda de `Error` y que añada la propiedad `statusCode`.

JavaScript

```
// utils/ApiError.js

/**
 * Clase para errores de API con un código de estado HTTP.
 * Permite comunicar errores específicos de negocio/cliente.
 */
class ApiError extends Error {
    constructor(statusCode, message) {
        super(message); // Llama al constructor de la clase Error
        this.statusCode = statusCode;
        this.name = 'ApiError';
        // Capturamos el stack trace para un mejor debug
        Error.captureStackTrace(this, this.constructor);
    }
}

// Exportamos errores comunes para fácil acceso
ApiError.notFound = (message = 'Recurso no encontrado.') => 
    new ApiError(404, message);

ApiError.badRequest = (message = 'Datos de petición inválidos.') => 
    new ApiError(400, message);

ApiError.unauthorized = (message = 'Acceso no autorizado.') => 
    new ApiError(401, message);

module.exports = ApiError;
```

---

## 3. Estrategias de Uso (`throw` y `next()`)

### A. Errores de Negocio Sincrónicos (Usando `throw` con _Custom Errors_)

En código síncrono, puedes usar `throw` para lanzar tu error.

JavaScript

```
const ApiError = require('./utils/ApiError');

app.get('/usuarios/:id', (req, res, next) => {
    const id = parseInt(req.params.id);

    if (isNaN(id)) {
        // 🛑 Usar 'throw' para errores síncronos
        throw ApiError.badRequest('El ID debe ser numérico.'); 
    }
    
    // Simulación: Buscar en la BD
    if (id > 1000) {
        throw ApiError.notFound(`Usuario con ID ${id} no existe.`);
    }

    res.json({ id: id, nombre: 'Usuario' });
});
```

### B. Errores Asincrónicos (Usando `async/await` y `try...catch`)

Cuando trabajamos con `async/await`, el patrón cambia ligeramente. Necesitas usar un bloque `try...catch` y, si se atrapa un error, se debe pasar explícitamente al manejador de errores con **`next(error)`**.

JavaScript

```
app.post('/datos', async (req, res, next) => {
    try {
        // Simulación de una operación asíncrona que puede fallar
        const data = await algunServicioExterno.guardar(req.body); 
        
        res.status(201).json(data);
    } catch (error) {
        // 🛑 En async/await, capturamos el error y lo pasamos con next()
        next(error); 
        // Esto envía el error al manejador de 4 argumentos.
    }
});
```

**Nota sobre `express-async-errors`:** Para evitar escribir tanto `try...catch(next(err))`, existe una dependencia popular (`express-async-errors`) que _patchéa_ Express para que detecte y envíe automáticamente los errores lanzados (`throw`) dentro de funciones `async` al manejador de errores. Es una práctica recomendada en el desarrollo moderno.

---

## 4. Ensamblaje Final en `app.js`

El orden es crítico: el _middleware_ de error debe ser lo **último** en ser definido, después de todas las rutas y _middlewares_ normales.

JavaScript

```
// app.js
const express = require('express');
const app = express();
const manejadorErrores = require('./middlewares/manejadorErrores');
// ... Tus rutas y routers

// 1. Middlewares normales (Logger, JSON parser, etc.)
app.use(express.json());

// 2. Definición de Rutas (Aquí van tus routers, ej: app.use('/api', apiRouter))
app.get('/test-error', (req, res, next) => {
    // Generar un error intencional (simulando un 401)
    const ApiError = require('./utils/ApiError');
    next(ApiError.unauthorized('Token JWT inválido.'));
});

// 3. Middleware para 404 (Si ninguna ruta anterior coincidió)
app.use((req, res, next) => {
    const ApiError = require('./utils/ApiError');
    next(ApiError.notFound(`Ruta: ${req.originalUrl} no encontrada.`));
});

// 4. ¡El Middleware de Error DEBE ir al final!
app.use(manejadorErrores); 

app.listen(3000, () => console.log('Servidor corriendo...'));
```

---

## ✅ Buenas Prácticas y Errores Comunes

|Categoría|Buena Práctica|Error Común|
|---|---|---|
|**Ubicación**|El _middleware_ de 4 argumentos debe ser el **último** `app.use()` en el archivo.|Colocar el manejador de errores antes de las rutas, impidiendo que los errores lleguen a él.|
|**API Errors**|Crear clases de error personalizadas (`ApiError`) para estandarizar los códigos de estado.|Usar solo `res.status(500).send('Error')` sin dar detalles útiles, o usar un código 200 para reportar un fallo.|
|**Asincronía**|Usar `try...catch` y **`next(err)`** o una librería como `express-async-errors`.|Olvidar `next(err)` en el `catch` de una función `async`, haciendo que Express no se entere del error (`unhandled rejection`).|
|**Seguridad**|Nunca enviar el `stack trace` del error al cliente en entornos de **producción**.|Exponer detalles sensibles del servidor o de la base de datos al cliente.|

---

## 🔑 Resumen de Puntos Clave

- Un **Middleware de Error** es una función con cuatro argumentos: `(err, req, res, next)`.
    
- Para pasar un error a este manejador, se llama a **`next(errorObjeto)`**.
    
- Las **clases de error personalizadas** (`ApiError`) son esenciales para asociar un `statusCode` al error y enviar respuestas HTTP adecuadas (400, 404, 500).
    
- El _middleware_ de error se usa para **logear** el problema internamente y enviar una **respuesta estandarizada** y segura al cliente.
    

---

# 🚀 Nivel 3 Completado

¡Felicidades! Has completado el **Nivel 3: Express.js y APIs REST**. Ahora puedes crear APIs robustas, modularizadas y con un manejo de errores profesional.

El siguiente gran paso para tu API es la **persistencia de datos**. Una API que no guarda datos no sirve de mucho.

Entramos al **Nivel 4: Bases de datos**. Tu próxima clase te enseñará cómo conectar Node.js a cualquier tipo de base de datos.

Tu próxima clase: **[[17 - Conexión a bases de datos]]**.