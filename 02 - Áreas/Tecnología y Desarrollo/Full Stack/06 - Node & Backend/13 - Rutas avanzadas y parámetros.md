¡Continuamos! Ya tienes un servidor Express básico funcionando. Sin embargo, una API no es útil si solo responde a rutas estáticas como `/saludo`. Necesitas que tu servidor sea capaz de recibir datos del cliente a través de la URL o en el cuerpo de la petición.

Esta clase te enseñará a manejar los distintos tipos de datos de entrada en Express.

Aquí tienes la nota para la clase anterior: **[[12 - Introducción a Express]]**.

---

# 13 - Rutas avanzadas y parámetros

## 📚 Introducción: El Flujo de Datos del Cliente

Una de las principales tareas de un desarrollador de APIs es extraer, validar y procesar la información enviada por el cliente. En Express, estos datos se encuentran convenientemente organizados en tres lugares dentro del objeto de **Petición (`req`)**:

1. **`req.params`**: Parámetros dentro de la ruta (URL) para identificar un recurso.
    
2. **`req.query`**: Parámetros de consulta (filtros, ordenamiento).
    
3. **`req.body`**: El cuerpo de la petición (JSON o formularios) para crear o actualizar datos.
    

---

## 1. `req.params`: Parámetros de Ruta

Los **Parámetros de Ruta** se usan para identificar un recurso específico. Se definen en la ruta usando dos puntos (`:nombreParametro`).

**Ejemplo de Uso:** Obtener un usuario por su ID.

- **Ruta:** `/usuarios/123`
    
- **Endpoint en Express:** `/usuarios/:id`
    

### Ejemplo Práctico

JavaScript

```
// app.js
const app = require('express')();

// La ruta dinámica :id
app.get('/productos/:id', (req, res) => {
    // 💡 req.params es un objeto donde las claves son los nombres definidos (:id)
    const productoId = req.params.id;
    
    // Simulación: Buscar el producto en la "base de datos"
    if (productoId === '42') {
        res.json({ id: 42, nombre: 'Libro Guía del Hitchhiker', precio: 42.00 });
    } else {
        // Usamos el código de estado 404 (Not Found)
        res.status(404).json({ error: `Producto con ID ${productoId} no encontrado.` });
    }
});

// Ruta con múltiples parámetros
app.get('/usuarios/:userId/comentarios/:commentId', (req, res) => {
    // req.params tendrá { userId: 'valor', commentId: 'valor' }
    const { userId, commentId } = req.params;

    res.json({
        mensaje: `Buscando comentario ${commentId} del usuario ${userId}.`,
        params: req.params
    });
});

// ... app.listen(...)
```

---

## 2. `req.query`: Parámetros de Consulta (Query Strings)

Los **Parámetros de Consulta** se usan principalmente para filtros, paginación, u opciones opcionales. Aparecen después del signo de interrogación (`?`) en la URL y se separan por `&`.

**Ejemplo de Uso:** Listar usuarios activos en la página 2.

- **URL:** `/usuarios?activo=true&pagina=2&limite=10`
    

### Ejemplo Práctico

JavaScript

```
// En el mismo app.js
app.get('/usuarios', (req, res) => {
    // 💡 req.query es un objeto con las claves después del "?"
    const { activo, pagina, limite, orden } = req.query;

    let mensaje = 'Lista de usuarios';
    
    // Lógica condicional basada en los queries
    if (activo === 'true') {
        mensaje += ' (filtrados por activos)';
    }

    console.log(`Paginación: Página ${pagina || 1}, Límite ${limite || 20}`);
    
    res.json({
        mensaje: mensaje,
        filtros_recibidos: req.query,
        resultado_simulado: []
    });
});

// ... app.listen(...)
```

### Tabla Comparativa de Parámetros

|Tipo|Ubicación en URL|Propósito|¿Obligatorio/Opcional?|
|---|---|---|---|
|**`req.params`**|Parte de la ruta (`/recurso/:id`)|Identificación de recursos.|**Obligatorio** para la ruta.|
|**`req.query`**|Después de `?` (`/recurso?key=val`)|Filtrado, paginación, ordenamiento.|**Opcional**.|

---

## 3. `req.body`: Lectura del Cuerpo de la Petición

El **Cuerpo de la Petición** contiene los datos que un cliente envía para crear o actualizar un recurso (típicamente con métodos **POST** o **PUT/PATCH**).

**IMPORTANTE:** Express **NO** parsea el cuerpo de la petición por defecto. Necesitas un _middleware_ específico.

### Body Parsing Middlewares

Para que `req.body` contenga el objeto JSON o los datos de formulario, debes usar los _middlewares_ nativos de Express:

|Middleware|Propósito|¿Cuándo usar?|
|---|---|---|
|**`express.json()`**|Parsea peticiones con encabezado `Content-Type: application/json`.|**El estándar** para APIs REST.|
|**`express.urlencoded()`**|Parsea peticiones con encabezado `Content-Type: application/x-www-form-urlencoded` (formularios web tradicionales).|Cuando trabajas con formularios HTML simples.|

### Ejemplo Práctico de Body Parsing

JavaScript

```
// app.js

const express = require('express');
const app = express();

// 💡 1. Middleware para JSON (CRUCIAL para APIs)
app.use(express.json());

// 💡 2. Middleware para formularios URL-encoded (Opcional, si lo necesitas)
app.use(express.urlencoded({ extended: true }));


app.post('/usuarios', (req, res) => {
    // req.body ahora contiene el objeto JSON enviado por el cliente
    const nuevoUsuario = req.body; 

    // Verificación básica
    if (!nuevoUsuario || !nuevoUsuario.email || !nuevoUsuario.password) {
        return res.status(400).json({ error: 'Faltan campos obligatorios (email, password).' });
    }

    // Simulación: Guardar en la "base de datos"
    console.log('Usuario recibido y "guardado":', nuevoUsuario);

    // 201 Created es el Status Code correcto para una creación exitosa
    res.status(201).json({ 
        mensaje: 'Usuario creado con éxito',
        id_asignado: Math.floor(Math.random() * 1000) + 1,
        datos_recibidos: nuevoUsuario
    });
});
```

**Prueba con cURL (o Postman):**

Bash

```
# Ejecuta este comando en la terminal con tu servidor Express corriendo
curl -X POST http://localhost:3000/usuarios \
     -H "Content-Type: application/json" \
     -d '{"email": "test@mail.com", "password": "securepassword123", "nombre": "Alice"}'
```

---

## ✅ Buenas Prácticas y Errores Comunes

|Categoría|Buena Práctica|Error Común|
|---|---|---|
|**Parsing**|**Incluir `app.use(express.json())` al inicio de tu `app.js`** (antes de definir las rutas).|Olvidar `express.json()`, resultando en un `req.body` indefinido (`{}`) en las peticiones POST.|
|**Tipos de Datos**|Validar siempre los tipos y la presencia de datos en `req.body` antes de usarlos.|Confiar ciegamente en que el cliente enviará datos válidos (¡nunca confíes en el cliente!).|
|**Semántica**|Usar **`req.params`** para la identificación (ej. `GET /usuarios/5`) y **`req.query`** para el filtrado (ej. `GET /usuarios?status=activo`).|Mezclar propósitos (ej. poner el ID en el `query string`).|
|**Status Codes**|Usar **201 (Created)** para las peticiones POST exitosas, no 200 (OK).|Usar solo `res.send()` sin establecer un código de estado explícito con `res.status(code)`.|

---

## 🔑 Resumen de Puntos Clave

- Los datos del cliente se encuentran en **`req.params`**, **`req.query`** y **`req.body`**.
    
- **`req.params`** se usa para identificar recursos y se define con `:nombre` en la ruta.
    
- **`req.query`** se usa para filtros y opciones, y es opcional.
    
- Para usar **`req.body`** con JSON (el estándar de API), debes incluir el _middleware_ **`app.use(express.json())`**.
    
- Usa códigos de estado HTTP correctos, como **404** para recursos no encontrados o **201** para creación exitosa.
    

Ahora que sabes cómo recibir datos, el siguiente paso es crucial para la seguridad, el logging y la reusabilidad del código: dominar la creación de tus propios _middlewares_.

Tu próxima clase te enseñará a construir funciones intermedias poderosas: **[[14 - Middlewares personalizados]]**.