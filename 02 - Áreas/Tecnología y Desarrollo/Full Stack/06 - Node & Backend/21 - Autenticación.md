¡Claro! Hemos asegurado la persistencia de datos, y ahora es momento de asegurar el acceso. **La seguridad, comenzando por la autenticación, es fundamental** para cualquier API que maneje información privada.

Entramos al **Nivel 5: Autenticación, seguridad y despliegue**.

Aquí tienes la nota para la clase anterior: **[[20 - Persistencia y migraciones]]**.

---

# 21 - Autenticación

## 📚 Introducción: Autenticación vs. Autorización

Antes de sumergirnos en los métodos, es crucial diferenciar dos términos que a menudo se confunden:

1. **Autenticación (Authentication - ¿Quién eres?):** El proceso de verificar la identidad de un usuario. Típicamente se hace con credenciales (usuario y contraseña).
    
2. **Autorización (Authorization - ¿Qué puedes hacer?):** El proceso de verificar si un usuario autenticado tiene permiso para acceder a un recurso o realizar una acción (ej: solo un administrador puede eliminar un usuario).
    

La **autenticación** es el primer paso. En el backend moderno con Express, el método más popular es **JWT**.

---

## 🔐 1. Almacenamiento Seguro de Contraseñas

Nunca, bajo ninguna circunstancia, debes almacenar contraseñas en texto plano en tu base de datos. Si la BD es comprometida, todos los usuarios quedan expuestos.

El estándar es usar **Hashing** con una función unidireccional (irrevocable), siendo **Bcrypt** la librería más utilizada en Node.js.

### ¿Cómo Funciona el Hashing con Bcrypt?

1. El usuario se registra y envía la contraseña en texto plano.
    
2. Antes de guardar, usas Bcrypt para aplicar una función hash a la contraseña. El resultado es un _string_ único e irreversible (el hash).
    
3. Bcrypt añade un **Salt** (una cadena aleatoria) al hash, lo que garantiza que dos usuarios con la misma contraseña tengan hashes diferentes, previniendo ataques de "tablas arcoíris".
    

**Ejemplo Práctico (Bcrypt):**

JavaScript

```
// Necesitarás instalar bcryptjs (npm install bcryptjs)
const bcrypt = require('bcryptjs');

// 1. Hashing (en el POST /registro)
async function hashPassword(plainPassword) {
    // Generamos un 'salt' (10 rondas es el estándar)
    const salt = await bcrypt.genSalt(10); 
    
    // Hash final que se guarda en la BD
    const hash = await bcrypt.hash(plainPassword, salt);
    console.log(`Contraseña hasheada: ${hash}`);
    return hash;
}

// 2. Verificación (en el POST /login)
async function comparePassword(plainPassword, userHash) {
    // Bcrypt automáticamente extrae el salt del hash y lo compara
    const isMatch = await bcrypt.compare(plainPassword, userHash);
    return isMatch; 
}
```

---

## 🔑 2. JSON Web Tokens (JWT)

Una vez que un usuario se autentica (envía usuario/contraseña y Bcrypt verifica que coinciden), la API debe darle una "llave" para las peticiones futuras. Esta llave es el **JSON Web Token (JWT)**.

### Características de JWT

1. **Sin Estado (_Stateless_):** El servidor no necesita guardar registros de la sesión (cumpliendo el principio REST de [[15 - APIs RESTful]]).
    
2. **Compacto y Autocontenido:** El _token_ lleva la información de identidad del usuario (el _payload_).
    
3. **Verificable:** Está firmado digitalmente por la API, por lo que el servidor puede verificar su autenticidad y que no ha sido alterado.
    

### Estructura del JWT

Un JWT es una cadena larga dividida en tres partes, separadas por puntos (`.`):

![](data:,)

1. **Header:** Información sobre el tipo de token (JWT) y el algoritmo de firma (ej: HMAC SHA256).
    
2. **Payload (Cuerpo):** Contiene las **Claims** (declaraciones), que son datos sobre el usuario (ej: `userId`, `username`, fecha de expiración `exp`).
    
3. **Signature (Firma):** Se genera hasheando el Header, el Payload y un **Secreto** (una cadena muy larga que solo conoce el servidor). **Esta firma es la que garantiza la autenticidad.**
    

### Flujo de Autenticación con JWT en Express

1. **Login (`POST /login`):**
    
    - Cliente envía credenciales.
        
    - Servidor verifica credenciales con Bcrypt.
        
    - Servidor usa la librería **`jsonwebtoken`** para crear y firmar un nuevo JWT.
        
    - Servidor responde con el código **200 OK** y el JWT.
        
2. **Acceso a Ruta Protegida:**
    
    - Cliente envía peticiones futuras incluyendo el JWT en el encabezado `Authorization: Bearer <token>`.
        
    - Servidor usa un Middleware de Autenticación para:
        
        a. Extraer el token del encabezado.
        
        b. Verificar la Firma del token (si el token es alterado, la verificación falla).
        
        c. Extraer el Payload (ej: userId) y adjuntarlo al objeto req (ej: req.user = { userId: 123 }).
        
        d. Llamar a next().
        

### Ejemplo de Middleware de Verificación

JavaScript

```
// middlewares/auth.js (Necesita npm install jsonwebtoken)
const jwt = require('jsonwebtoken');
const ApiError = require('../utils/ApiError');

const SECRET_KEY = process.env.JWT_SECRET || 'mi_secreto_muy_seguro';

const verificarToken = (req, res, next) => {
    // 1. Extraer el token del encabezado (Authorization: Bearer <token>)
    const authHeader = req.headers.authorization;

    if (!authHeader || !authHeader.startsWith('Bearer ')) {
        return next(ApiError.unauthorized('Token no proporcionado o formato inválido.'));
    }

    const token = authHeader.split(' ')[1]; // Obtener solo el <token>

    try {
        // 2. Verificar el token usando el secreto del servidor
        const decoded = jwt.verify(token, SECRET_KEY);
        
        // 3. Adjuntar la información del usuario al objeto req
        req.user = decoded; 
        
        // 4. Continuar con el handler de la ruta
        next(); 
    } catch (err) {
        // Error de firma inválida o expiración
        return next(ApiError.unauthorized('Token inválido o expirado.'));
    }
};

module.exports = verificarToken;
```

**Uso en una Ruta Protegida:**

JavaScript

```
const verificarToken = require('./middlewares/auth');

// Solo los usuarios autenticados pueden acceder a sus propios perfiles
app.get('/perfil', verificarToken, (req, res) => {
    // req.user está disponible gracias al middleware
    res.json({ mensaje: `Acceso concedido para el usuario ID: ${req.user.userId}` });
});
```

---

## 🍪 3. Autenticación Basada en Sesiones y Cookies (Alternativa)

Históricamente, la autenticación se hacía con **Sesiones**. En este modelo:

1. El servidor guarda la sesión del usuario en una base de datos (ej: Redis o BD).
    
2. El servidor envía un **ID de Sesión** al cliente, típicamente en una **Cookie**.
    
3. En cada petición, el cliente envía la Cookie. El servidor busca el ID en su BD de sesiones.
    

**Desventaja:** No es _Stateless_, requiere mantener un almacenamiento de sesiones del lado del servidor, lo que complica la escalabilidad (se debe replicar el estado en múltiples servidores).

**Ventaja:** Permite revocar sesiones fácilmente (simplemente eliminando la sesión de la BD).

---

## ✅ Buenas Prácticas y Errores Comunes

|Categoría|Buena Práctica|Error Común|
|---|---|---|
|**Contraseñas**|Usar **Bcrypt** (o Argon2) con un _salt_ adecuado para el hashing.|Usar hashing simple (como SHA256) sin salt, o almacenar contraseñas en texto plano.|
|**JWT Secreto**|Almacenar la **Clave Secreta de JWT** en una variable de entorno (`.env`).|Codificar el secreto directamente en el código fuente.|
|**Expiración**|Usar la propiedad `exp` de JWT para establecer un tiempo de vida corto (ej: 15 minutos). Usar _Refresh Tokens_ para obtener uno nuevo.|Crear tokens que no expiran, dejando una "llave" permanentemente válida si es robada.|
|**Autorización**|Después de autenticar (`req.user`), usar middlewares adicionales para verificar los **Roles** o permisos del usuario.|Asumir que un usuario autenticado puede hacer cualquier cosa sin verificar permisos.|

---

## 🔑 Resumen de Puntos Clave

- **Autenticación** es verificar la identidad; **Autorización** es verificar los permisos.
    
- Las contraseñas deben ser hasheadas de forma irreversible y segura con **Bcrypt**.
    
- **JWT** es el estándar moderno en APIs REST: es _Stateless_, se firma con un **Secreto** y se verifica con un **Middleware**.
    
- El cliente debe enviar el JWT en el encabezado **`Authorization: Bearer <token>`** para cada petición protegida.
    
- El **Middleware de Autenticación** verifica la firma del token y adjunta los datos del usuario (`req.user`) antes de llamar a `next()`.
    

La autenticación es solo una parte. Ahora debemos proteger nuestra API de ataques externos y configurarla para un entorno productivo.

Tu próxima clase se centrará en la **Seguridad** más allá de la autenticación: **[[22 - Seguridad en Node.js]]**.