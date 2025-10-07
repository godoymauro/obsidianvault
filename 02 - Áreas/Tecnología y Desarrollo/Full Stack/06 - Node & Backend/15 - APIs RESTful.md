¡Perfecto! Hemos cubierto todos los cimientos: sabes cómo funciona Node.js, cómo usar Express, y cómo controlar el flujo con _middlewares_. Ahora, vamos a aplicar todo ese conocimiento para construir el producto más común en el desarrollo backend: una **API RESTful**.

Esta clase te enseñará a estructurar los _endpoints_ siguiendo los principios REST, completando el ciclo CRUD.

Aquí tienes la nota para la clase anterior: **[[14 - Middlewares personalizados]]**.

---

# 15 - APIs RESTful

## 📚 Introducción: ¿Qué es REST?

**REST (Representational State Transfer)** no es un protocolo ni una tecnología; es un **estilo arquitectónico** para el diseño de APIs web. Su objetivo principal es hacer que los sistemas distribuidos (cliente y servidor) sean más escalables, desacoplados y fáciles de entender.

La clave de REST es usar los **recursos** (sustantivos) y las **operaciones HTTP** (verbos) de manera coherente.

### Principios Fundamentales

|Principio|Descripción|Implicación en Express|
|---|---|---|
|**Recursos**|La información se expone como recursos (ej: usuarios, productos), identificados por una URI (Uniform Resource Identifier).|Las URLs usan **sustantivos en plural** (ej: `/usuarios`, `/productos`).|
|**Cliente-Servidor**|El cliente y el servidor están separados.|Express se encarga del servidor; el cliente (web/móvil) se encarga de la UI.|
|**Sin Estado (_Stateless_)**|Cada petición del cliente al servidor debe contener toda la información necesaria para entender la solicitud. El servidor **no guarda información de sesión** del cliente entre peticiones.|Usamos **Tokens** (ej: JWT, que veremos en [[21 - Autenticación]]) en lugar de sesiones de servidor.|
|**Interfaz Uniforme**|Uso consistente de los métodos HTTP.|Usar `GET` para obtener, `POST` para crear, etc.|

---

## 🏗 El Ciclo CRUD con Express

**CRUD** son las cuatro operaciones básicas que realizas sobre un recurso: **C**reate, **R**ead, **U**pdate, **D**elete. En una API RESTful, estas operaciones se mapean directamente a los métodos HTTP.

|Operación CRUD|Método HTTP|URL (Recurso)|Código de Respuesta|Propósito|
|---|---|---|---|---|
|**READ (All)**|`GET`|`/recursos`|200 OK|Obtener lista de recursos.|
|**READ (One)**|`GET`|`/recursos/:id`|200 OK / 404 Not Found|Obtener un recurso específico.|
|**CREATE**|`POST`|`/recursos`|201 Created|Crear un nuevo recurso.|
|**UPDATE**|`PUT` / `PATCH`|`/recursos/:id`|200 OK / 404 Not Found|Actualizar un recurso existente.|
|**DELETE**|`DELETE`|`/recursos/:id`|204 No Content|Eliminar un recurso.|

---

## 💻 Ejemplo Práctico: API de Tareas (TODOs)

Vamos a simular una API CRUD completa para el recurso **`tareas`**. Usaremos un array simple para simular la base de datos (BD).

### Setup del Archivo `tareas-api.js`

JavaScript

```
// tareas-api.js
const express = require('express');
const router = express.Router(); // 💡 express.Router() para modularizar
const app = express();
app.use(express.json()); // Middleware esencial para POST/PUT

// Simulación de la "Base de Datos" (Memoria del servidor)
let tareas = [
    { id: 1, descripcion: 'Estudiar Express.js', completada: false },
    { id: 2, descripcion: 'Configurar base de datos', completada: true }
];
let nextId = 3;

// ----------------------------------------------------
// 1. READ (GET /api/v1/tareas) - Obtener todas las tareas
// ----------------------------------------------------
router.get('/', (req, res) => {
    // Podemos incluir lógica de filtro aquí usando req.query
    const { completada } = req.query;
    if (completada === 'true') {
        return res.json(tareas.filter(t => t.completada));
    }
    res.json(tareas); // 200 OK implícito
});

// ----------------------------------------------------
// 2. READ (GET /api/v1/tareas/:id) - Obtener tarea por ID
// ----------------------------------------------------
router.get('/:id', (req, res) => {
    const id = parseInt(req.params.id);
    const tarea = tareas.find(t => t.id === id);

    if (tarea) {
        res.json(tarea);
    } else {
        // 404 Not Found
        res.status(404).json({ error: 'Tarea no encontrada.' });
    }
});

// ----------------------------------------------------
// 3. CREATE (POST /api/v1/tareas) - Crear nueva tarea
// ----------------------------------------------------
router.post('/', (req, res) => {
    const { descripcion } = req.body;
    
    if (!descripcion) {
        // 400 Bad Request
        return res.status(400).json({ error: 'La descripción es obligatoria.' });
    }

    const nuevaTarea = {
        id: nextId++,
        descripcion,
        completada: false
    };

    tareas.push(nuevaTarea);
    
    // 201 Created es el Status Code correcto
    res.status(201).json(nuevaTarea); 
});

// ----------------------------------------------------
// 4. UPDATE (PUT /api/v1/tareas/:id) - Actualizar completamente una tarea
// ----------------------------------------------------
router.put('/:id', (req, res) => {
    const id = parseInt(req.params.id);
    const index = tareas.findIndex(t => t.id === id);

    if (index === -1) {
        return res.status(404).json({ error: 'Tarea no encontrada para actualizar.' });
    }

    const { descripcion, completada } = req.body;

    // Actualizamos el objeto en el array (simulando la BD)
    tareas[index] = {
        ...tareas[index],
        descripcion: descripcion || tareas[index].descripcion,
        completada: completada !== undefined ? completada : tareas[index].completada
    };

    res.json(tareas[index]); // 200 OK
});

// ----------------------------------------------------
// 5. DELETE (DELETE /api/v1/tareas/:id) - Eliminar tarea
// ----------------------------------------------------
router.delete('/:id', (req, res) => {
    const id = parseInt(req.params.id);
    const longitudInicial = tareas.length;

    // Filtramos para crear un nuevo array sin la tarea a eliminar
    tareas = tareas.filter(t => t.id !== id);

    if (tareas.length === longitudInicial) {
         // Si la longitud no cambió, la tarea no existía
        return res.status(404).json({ error: 'Tarea no encontrada para eliminar.' });
    }
    
    // 204 No Content: éxito, pero no hay cuerpo de respuesta que enviar
    res.status(204).end(); 
});

// 6. Conectar el Router a la aplicación principal
app.use('/api/v1/tareas', router);

app.listen(3000, () => {
    console.log('🚀 API de Tareas RESTful corriendo en http://localhost:3000/api/v1/tareas');
});
```

---

## 💡 `express.Router()`: Modularización de Rutas

El objeto **`express.Router()`** es esencial para la organización.

- **Ventaja:** Permite agrupar rutas relacionadas y _middlewares_ en archivos separados.
    
- **Uso:** En el archivo de la ruta (ej: `tareas.js`), defines las rutas con `router.get(...)`. En el archivo principal (`app.js`), usas `app.use('/base-path', router)` para "montar" el router.
    

**En el ejemplo anterior:** Definimos las rutas sin el prefijo `/api/v1/tareas`, pero al usar `app.use('/api/v1/tareas', router)`, todas las rutas dentro de `router` quedan bajo ese prefijo.

---

## ✅ Buenas Prácticas y Consejos REST

|Principio REST|Buena Práctica|Error Común|
|---|---|---|
|**Nombres**|Usar **sustantivos en plural** para los recursos (ej: `/clientes`).|Usar verbos en la URL (ej: `/obtenerTodosLosClientes`).|
|**Status Codes**|Usar códigos de respuesta HTTP correctos (201 para POST, 204 para DELETE, 400/404 para errores).|Usar siempre 200 OK, incluso si algo falló o se creó.|
|**Idempotencia**|Entender que `GET`, `PUT` y `DELETE` deben ser **idempotentes** (repetirlos no causa efectos diferentes a la primera vez). `POST` no es idempotente.|Usar `GET` para cambiar datos o `POST` para actualizar un recurso existente.|
|**Manejo de Errores**|Devolver cuerpos de error consistentes (JSON con un campo `error` o `mensaje`) y códigos 4xx/5xx.|Devolver errores en texto plano o con estructuras inconsistentes.|

---

## 🔑 Resumen de Puntos Clave

- **REST** es un estilo arquitectónico que usa **recursos** (sustantivos) y **métodos HTTP** (verbos) de forma coherente.
    
- El **ciclo CRUD** se mapea a `POST` (Create), `GET` (Read), `PUT/PATCH` (Update), `DELETE` (Delete).
    
- **`express.Router()`** es la forma correcta de modularizar y organizar los _endpoints_ en una API grande.
    
- Los **Status Codes** son vitales: 201 (`POST` exitoso), 204 (`DELETE` exitoso), 404 (recurso no existe), 400 (petición inválida).
    

Hemos creado una API funcional. Sin embargo, en nuestro código actual, el manejo de errores es muy manual (verificamos `if (index === -1)` en cada ruta). Hay una forma mucho más limpia de centralizar esto.

Tu próxima clase te enseñará el manejo de errores centralizado en Express: **[[16 - Manejo de errores]]**.