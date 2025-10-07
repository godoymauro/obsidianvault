¡Continuemos! Has implementado la **autenticación** con JWT, lo cual es excelente. Sin embargo, una API expuesta en internet es un blanco constante para ataques automatizados. La seguridad en Node.js va mucho más allá de solo saber quién es el usuario.

Esta clase se centrará en las mejores prácticas y _middlewares_ esenciales para proteger tu API de los ataques web más comunes.

Aquí tienes la nota para la clase anterior: **[[21 - Autenticación]]**.

---

# 22 - Seguridad en Node.js

## 📚 Introducción: Seguridad como Prioridad

La seguridad no es una característica opcional, es una responsabilidad. Al ser Node.js un entorno de ejecución, es vulnerable a los mismos ataques web que cualquier otro backend (SQL Injection, XSS, CSRF). Afortunadamente, Express facilita la implementación de defensas con una serie de _middlewares_ robustos.

---

## 🛡 1. Helmet: Blindaje Básico

**Helmet** es un _middleware_ simple pero esencial que configura encabezados HTTP relacionados con la seguridad, ayudando a prevenir vulnerabilidades web comunes. Debería ser uno de los primeros _middlewares_ que apliques en tu aplicación Express.

### ¿Qué hace Helmet?

|Encabezado de Seguridad|Descripción|Ataque que Previene|
|---|---|---|
|**X-Content-Type-Options**|Evita que el navegador intente "adivinar" el tipo de contenido (MIME Sniffing).|MIME Sniffing|
|**X-Frame-Options**|Evita que tu sitio se muestre en un `<frame>` o `<iframe>`.|Clickjacking|
|**X-Powered-By (Desactivación)**|Elimina el encabezado que dice que usas Express/Node.js.|_Fingerprinting_ (Ocultar versión de software)|
|**Content-Security-Policy (CSP)**|El más complejo. Restringe las fuentes de contenido que el navegador puede cargar (scripts, estilos, imágenes).|Cross-Site Scripting (XSS)|

### Uso Práctico de Helmet

JavaScript

```
// Necesitarás instalar helmet (npm install helmet)
const helmet = require('helmet');
const express = require('express');
const app = express();

// 💡 Aplicar Helmet de forma global. 
// Por defecto, habilita la mayoría de los 11 middlewares de seguridad.
app.use(helmet()); 

// Si quieres personalizar o deshabilitar alguno:
/*
app.use(helmet.contentSecurityPolicy({
    directives: {
        defaultSrc: ["'self'"], // Permite solo recursos del propio dominio
        scriptSrc: ["'self'", "'unsafe-inline'"],
    }
}));
*/

// Las defensas están listas antes de las rutas.
// app.use(...)
```

---

## 🌐 2. CORS (Cross-Origin Resource Sharing)

Por defecto, los navegadores imponen la **Política del Mismo Origen (_Same-Origin Policy_)**. Esto significa que el código JavaScript de un dominio (`https://mi-frontend.com`) no puede hacer peticiones a otro dominio (`https://mi-api-backend.com`).

**CORS** es el mecanismo que te permite **relajar** esta restricción de forma controlada, especificando qué orígenes (dominios) tienen permiso para acceder a tu API.

### Uso Práctico de CORS

JavaScript

```
// Necesitarás instalar cors (npm install cors)
const cors = require('cors');
const express = require('express');
const app = express();

// 1. CORS Global (PELIGROSO: Solo para APIs públicas)
// app.use(cors()); // Permite peticiones desde *cualquier* dominio (Origin: *)

// 2. CORS Configurado (RECOMENDADO)
const corsOptions = {
    // 💡 Lista de dominios permitidos
    origin: ['https://tu-frontend-principal.com', 'https://tu-dominio-secundario.net'], 
    methods: 'GET,HEAD,PUT,PATCH,POST,DELETE', // Métodos permitidos
    credentials: true, // Permite que se envíen cookies y headers de autorización
};

app.use(cors(corsOptions)); 
```

Si un dominio no listado intenta acceder a tu API, Express lo bloqueará inmediatamente con el _middleware_ CORS.

---

## ⏱ 3. Rate Limiting (Limitación de Tasa)

El **Rate Limiting** previene ataques de fuerza bruta, ataques de denegación de servicio (DoS) y el uso abusivo de tu API, al limitar el número de peticiones que un solo usuario (o dirección IP) puede hacer en un período de tiempo.

### Uso Práctico de Express-Rate-Limit

JavaScript

```
// Necesitarás instalar express-rate-limit (npm install express-rate-limit)
const rateLimit = require('express-rate-limit');
const app = require('express')();

// 1. Configuración de Rate Limiter (General)
const limiter = rateLimit({
    windowMs: 15 * 60 * 1000, // 15 minutos (ventana de tiempo)
    max: 100, // Límite de 100 peticiones por IP por ventana
    standardHeaders: true, // Agrega headers de límite de tasa
    legacyHeaders: false, // Deshabilita headers antiguos
    message: 'Demasiadas peticiones desde esta IP, por favor intente de nuevo más tarde.',
});

// 2. Aplicar a todas las peticiones
app.use(limiter);

// 3. Puedes tener un límite más estricto para rutas críticas (ej: login)
const loginLimiter = rateLimit({
    windowMs: 5 * 60 * 1000, // 5 minutos
    max: 5, // 5 intentos de login fallidos
});

app.post('/api/login', loginLimiter, (req, res) => {
    // Lógica de login
});
```

---

## 💉 4. Sanitización y Validación

Aunque no son _middlewares_ específicos de Express, son esenciales para la seguridad:

|Técnica|Descripción|Riesgo que Previene|
|---|---|---|
|**Sanitización**|Limpiar los datos, eliminando caracteres peligrosos (ej: tags `<script>`).|Cross-Site Scripting (XSS) y SQL Injection|
|**Validación**|Asegurar que los datos cumplen las reglas de negocio (ej: el email es un email, el campo no está vacío).|Inserción de datos corruptos/maliciosos|
|**SQL Injection**|**Usar siempre ORM/ODM** (`Sequelize`, `Prisma`, `Mongoose`). Estos escapan automáticamente los datos de entrada, previniendo la inyección.|SQL Injection|

Librerías como **`express-validator`** (construida sobre `validator.js`) son el estándar para estas tareas, combinando sanitización y validación en un _middleware_ elegante.

---

## ✅ Buenas Prácticas y Errores Comunes

|Categoría|Buena Práctica|Error Común|
|---|---|---|
|**Dependencias**|Mantener todas las dependencias (`package.json`) actualizadas y escanearlas regularmente en busca de vulnerabilidades conocidas.|Usar dependencias antiguas que tienen vulnerabilidades conocidas (ej: `npm audit`).|
|**Input**|**Nunca confiar en la entrada del cliente.** Siempre asumir que los datos son maliciosos y deben ser validados y sanitizados.|Insertar `req.query` o `req.body` directamente en una consulta sin validación.|
|**Errores**|No devolver información detallada de errores (ej: `stack traces`, nombres de tablas) al cliente en producción (ver [[16 - Manejo de errores]]).|Exponer rutas internas, nombres de variables o detalles del sistema operativo.|
|**Secretos**|Guardar claves privadas, JWT secretos, y credenciales de BD **solamente** en variables de entorno (ej: `.env`, vaults).|Subir secretos o `.env` al repositorio de Git.|

---

## 🔑 Resumen de Puntos Clave

- **Helmet** es un _middleware_ de seguridad fundamental que añade encabezados de protección.
    
- **CORS** se usa para especificar qué dominios tienen permitido acceder a tu API.
    
- **Rate Limiting** previene el abuso y los ataques DoS al limitar el número de peticiones por IP/ventana de tiempo.
    
- **Sanitización y Validación** son procesos esenciales para limpiar y verificar la entrada del usuario antes de que se use.
    
- **Usar ORM/ODM** es la defensa más efectiva contra la **Inyección SQL**.
    

Hemos asegurado la API. Ahora, necesitamos verificar que todo funciona como se espera antes del despliegue: **el testing**.

Tu próxima clase se centrará en la validación de tu código: **[[23 - Testing]]**.