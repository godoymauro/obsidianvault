¡Magnífico! Has recorrido un largo camino. Hemos cubierto los fundamentos, la asincronía, las APIs REST, la seguridad, la observabilidad y la escalabilidad. Es hora de dejar los ejemplos aislados y construir algo real.

Entramos al **Nivel 7: Proyecto final**. Esta clase te guiará en la construcción de una API RESTful completa, integrando los conocimientos clave que has adquirido.

Aquí tienes la nota para la clase anterior: **[[31 - Escalabilidad y performance]]**.

---

# 32 - Proyecto guiado: API REST Completa

## 📚 Introducción: Uniendo los Conceptos

El objetivo de este proyecto es consolidar los módulos de los Niveles 1 al 5 creando una API RESTful funcional, robusta y con prácticas de producción.

Utilizaremos la arquitectura **M-C-S (Model-Controller-Service)**, donde:

- **Model:** Se encarga de la estructura de datos (ORM/ODM) y la interacción directa con la DB.
    
- **Service (Servicio):** Contiene la lógica de negocio pura (validaciones, cálculos).
    
- **Controller (Controlador):** La capa que interactúa con Express, manejando `req`, `res` y `next`.
    

### Herramientas a Integrar

|Concepto|Herramienta|Clase de Referencia|
|---|---|---|
|**Framework**|Express.js|[[12 - Introducción a Express]]|
|**DB / ORM**|Mongoose (MongoDB)|[[18 - ORM y ODM]]|
|**Asincronía**|`async/await`|[[08 - Asincronía en Node.js]]|
|**Seguridad**|Bcrypt y JWT|[[21 - Autenticación]]|
|**Middleware**|Validación, Autenticación, Manejo de Errores|[[14 - Middlewares personalizados]], [[16 - Manejo de errores]]|

---

## 🏗 Estructura del Proyecto

Organiza tu proyecto de Node.js con la siguiente estructura:

```
/mi-api-proyecto
├── node_modules
├── package.json
├── .env
├── .gitignore
├── server.js               # Punto de entrada principal
└── /src/
    ├── /config/            # Configuración de DB, JWT
    ├── /db/                # Archivo de conexión a la BD
    ├── /middlewares/       # Logger, Auth, Manejo de errores
    ├── /models/            # Esquemas de Mongoose
    ├── /routes/            # Definición de rutas (Router)
    ├── /services/          # Lógica de negocio (Capa Service)
    └── /utils/             # Errores personalizados (ApiError)
```

---

## 1. Capa de Configuración y Conexión (`.env`, `/config`, `/db`)

**A. Variables de Entorno (`.env`):**

Fragmento de código

```
NODE_ENV=development
PORT=3000

# MongoDB
MONGO_URI="mongodb://localhost:27017/mi_proyecto_db"

# Seguridad
JWT_SECRET="mi_clave_secreta_super_segura_debes_cambiarla_ya"
JWT_EXPIRATION="1d"
```

**B. Conexión a Mongoose (`src/db/mongoose.js`):**

JavaScript

```
// src/db/mongoose.js
const mongoose = require('mongoose');

const connectDB = async () => {
    try {
        await mongoose.connect(process.env.MONGO_URI);
        console.log('✅ MongoDB conectado con éxito.');
    } catch (error) {
        console.error('❌ Error de conexión a MongoDB:', error.message);
        process.exit(1); // Detiene el proceso si falla la conexión
    }
};

module.exports = connectDB;
```

---

## 2. Modelos y Seguridad (`/models`, `/services`)

**A. Modelo de Usuario (Ejemplo con Hashing):**

JavaScript

```
// src/models/User.js
const mongoose = require('mongoose');
const bcrypt = require('bcryptjs');

const userSchema = new mongoose.Schema({
    email: { type: String, required: true, unique: true },
    password: { type: String, required: true },
    role: { type: String, default: 'user' }
});

// Middleware PRE-SAVE de Mongoose para hashear la contraseña
userSchema.pre('save', async function (next) {
    if (!this.isModified('password')) {
        return next();
    }
    const salt = await bcrypt.genSalt(10);
    this.password = await bcrypt.hash(this.password, salt);
    next();
});

// Método del modelo para comparar contraseñas
userSchema.methods.comparePassword = function (candidatePassword) {
    return bcrypt.compare(candidatePassword, this.password);
};

module.exports = mongoose.model('User', userSchema);
```

---

## 3. Lógica de Autenticación (`/services`, `/routes`)

**A. Servicio de Auth (`src/services/authService.js`):** Encargado de la lógica de negocio (login, registro, generación de JWT).

**B. Controlador de Auth (`src/controllers/authController.js`):**

JavaScript

```
// src/controllers/authController.js (Ejemplo de Login)
const User = require('../models/User');
const jwt = require('jsonwebtoken');
const ApiError = require('../utils/ApiError'); // Tu error personalizado

const login = async (req, res, next) => {
    const { email, password } = req.body;
    
    // 1. Validar existencia del usuario
    const user = await User.findOne({ email });
    if (!user) {
        return next(ApiError.unauthorized('Credenciales inválidas.'));
    }

    // 2. Comparar contraseña (Método de User.js)
    const isMatch = await user.comparePassword(password);
    if (!isMatch) {
        return next(ApiError.unauthorized('Credenciales inválidas.'));
    }

    // 3. Generar JWT (Payload: user ID, Secret: de .env)
    const token = jwt.sign(
        { userId: user._id, role: user.role }, 
        process.env.JWT_SECRET, 
        { expiresIn: process.env.JWT_EXPIRATION }
    );

    // 4. Respuesta exitosa
    res.json({ token });
};

module.exports = { login, /* ... register, etc. */ };
```

---

## 4. Middleware de Autenticación y Rutas Protegidas

Implementarás el _middleware_ `verificarToken` (como se vio en **[[21 - Autenticación]]**).

JavaScript

```
// src/routes/userRoutes.js
const express = require('express');
const router = express.Router();
const verificarToken = require('../middlewares/authMiddleware'); // Tu JWT checker
const userController = require('../controllers/userController');

// Ruta protegida: requiere un token JWT válido
router.get('/me', verificarToken, userController.getProfile); 

// Ruta protegida que requiere el rol de 'admin'
const checkAdmin = (req, res, next) => {
    if (req.user.role !== 'admin') {
        return next(ApiError.forbidden('Acceso denegado: Se requiere rol de administrador.'));
    }
    next();
};

router.delete('/:id', verificarToken, checkAdmin, userController.deleteUser); 

module.exports = router;
```

---

## 5. El Punto de Entrada Final (`server.js`)

Aquí integras todo, especialmente el **Manejo de Errores** al final.

JavaScript

```
// server.js
require('dotenv').config(); // Cargar variables de entorno
const express = require('express');
const connectDB = require('./src/db/mongoose');
const manejadorErrores = require('./src/middlewares/errorMiddleware'); // El de 4 argumentos
const requestLogger = require('./src/middlewares/loggerMiddleware');
const userRoutes = require('./src/routes/userRoutes');
const authRoutes = require('./src/routes/authRoutes');
const ApiError = require('./src/utils/ApiError');

// Conexión a la BD
connectDB(); 

const app = express();
app.use(express.json()); // Body Parser
app.use(requestLogger); // Logger (antes de las rutas)

// Rutas
app.use('/api/v1/auth', authRoutes);
app.use('/api/v1/users', userRoutes);

// Middleware 404 - Si ninguna ruta coincidió (DEBE IR ANTES DEL ERROR HANDLER)
app.use((req, res, next) => {
    next(ApiError.notFound(`Ruta ${req.originalUrl} no encontrada.`));
});

// Middleware de Manejo de Errores (¡EL ÚLTIMO!)
app.use(manejadorErrores);

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => console.log(`🚀 Servidor corriendo en puerto ${PORT}`));
```

---

## 🔑 Puntos Clave del Proyecto

- **Organización (M-C-S):** Se separó la lógica de la ruta (Controller), del negocio (Service) y de la DB (Model).
    
- **Seguridad:** Uso de `.env`, Bcrypt para contraseñas y JWT para el control de acceso (_Stateless_).
    
- **Middlewares en Cadena:** Se usaron _middlewares_ para Logging, Autenticación (`verificarToken`) y Autorización (`checkAdmin`).
    
- **Manejo de Errores:** Centralizado con `ApiError` y el _middleware_ de 4 argumentos, asegurando respuestas HTTP consistentes (401, 403, 404).
    

---

# 🚀 Nivel 7: Proyecto Final (Parte 1)

Este proyecto te da la base de una API robusta. El siguiente paso en tu desarrollo avanzado es aplicar las arquitecturas modernas.

Tu última clase en este curso se centrará en integrar arquitecturas avanzadas para un proyecto de nivel experto: **[[33 - Proyecto avanzado]]**.