¡Claro que sí! Ya hemos establecido la conexión a los diferentes tipos de bases de datos ( **[[17 - Conexión a bases de datos]]**). Sin embargo, escribir consultas SQL o JSON nativas puede ser propenso a errores y poco mantenible.

Es hora de usar **ORM** y **ODM**, las herramientas que te permiten interactuar con tu base de datos usando **Objetos JavaScript** puros. Esto es un pilar del desarrollo backend moderno.

Aquí tienes la nota para la clase anterior: **[[17 - Conexión a bases de datos]]**.

---

# 18 - ORM y ODM

## 📚 Introducción: El Desafío de Impedancia

El **desafío de impedancia objeto-relacional** es el conflicto entre la forma en que un lenguaje orientado a objetos (como JavaScript) modela los datos (en objetos y clases) y la forma en que las bases de datos relacionales los almacenan (en tablas y filas).

- **Node.js/JS:** Usa objetos (`{ nombre: 'Alice' }`).
    
- **PostgreSQL:** Usa tablas (`SELECT nombre FROM usuarios`).
    

Los **ORM (Object-Relational Mapper)** y **ODM (Object-Document Mapper)** resuelven este problema al actuar como un traductor entre tu código JavaScript y la base de datos.

---

## 🛠 1. ORM (Object-Relational Mapper)

Los ORM se usan con bases de datos **SQL** (PostgreSQL, MySQL, SQLite).

|Característica|Descripción|
|---|---|
|**Definición**|Mapea filas y tablas SQL a **clases y objetos JavaScript**.|
|**Operación**|Permite realizar CRUD sin escribir SQL. El ORM **genera la consulta SQL** por ti.|
|**Ventaja**|Portabilidad entre bases de datos SQL, seguridad (prevención de Inyección SQL), y código más legible.|
|**Ejemplos**|**Prisma** (moderno, _query builder_), **Sequelize** (tradicional, basado en Promesas), TypeORM.|

### Ejemplo Práctico: Sequelize (o similar)

Imagina que quieres obtener todos los usuarios de la tabla `usuarios`.

|Uso de Driver Nativo|Uso de ORM (Sequelize)|
|---|---|
|`javascript \ const res = await pool.query('SELECT * FROM usuarios WHERE activo = true'); \ return res.rows.map(row => new Usuario(row)); \`|```javascript|
|// Definimos un "Modelo" (una clase JS que representa la tabla)||
|const { Usuario } = require('./models');||

// La consulta se hace con métodos de objeto

const usuariosActivos = await Usuario.findAll({

where: { activo: true }

});

return usuariosActivos; // Retorna una lista de objetos JS

Fragmento de código

````

## 📦 2. ODM (Object-Document Mapper)

Los ODM se usan con bases de datos **NoSQL** orientadas a documentos, siendo **MongoDB** el ejemplo principal.

| Característica | Descripción |
| :--- | :--- |
| **Definición** | Mapea documentos de MongoDB a **objetos JavaScript** con un **esquema definido**. |
| **Operación** | Añade validación, *middlewares* de pre-guardado y métodos de consulta complejos a los documentos. |
| **Ventaja** | Introduce un **esquema** y **validación** a MongoDB, que por naturaleza no tiene esquema. |
| **Ejemplo** | **Mongoose** (el estándar de facto para Node.js y MongoDB). |

### Ejemplo Práctico: Mongoose

Mongoose requiere definir un **Schema** para garantizar que los documentos de MongoDB tengan la estructura que esperas.

```javascript
// models/Usuario.js
const mongoose = require('mongoose');

// 1. Definición del Esquema (Estructura y Validación)
const UsuarioSchema = new mongoose.Schema({
    nombre: { 
        type: String, 
        required: true 
    },
    email: { 
        type: String, 
        required: true, 
        unique: true 
    },
    creadoEn: { 
        type: Date, 
        default: Date.now 
    }
});

// 2. Creación del Modelo (Clase con la que interactuamos)
const Usuario = mongoose.model('Usuario', UsuarioSchema);
module.exports = Usuario;
````

**Uso del Modelo en un _Handler_ de Express:**

JavaScript

```
// routes/usuario.js
const Usuario = require('../models/Usuario');

// Handler para crear un nuevo usuario (POST /usuarios)
router.post('/', async (req, res, next) => {
    try {
        // 1. Crear la instancia de objeto JS
        const nuevoUsuario = new Usuario(req.body);
        
        // 2. Llamar a save() para que Mongoose valide y guarde en MongoDB
        await nuevoUsuario.save(); 
        
        res.status(201).json(nuevoUsuario);
    } catch (error) {
        // Mongoose captura automáticamente errores de validación/conexión
        next(error); 
    }
});
```

---

## 🚀 Prisma: El ORM Moderno (Enfoque Híbrido)

**Prisma** es la opción moderna que está ganando tracción. Técnicamente no es un ORM tradicional, sino un **generador de _clients_ de base de datos** que se basa en un esquema externo.

- **Modelo de Datos Único:** Defines tus tablas/colecciones en un archivo de esquema (`schema.prisma`).
    
- **Tipado:** Genera un cliente tipado, lo que mejora la seguridad y la experiencia de desarrollo (DX) si usas TypeScript (muy común en Node.js moderno).
    

**Ejemplo de Consulta con Prisma Client:**

JavaScript

```
const { PrismaClient } = require('@prisma/client');
const prisma = new PrismaClient();

async function obtenerUsuarios() {
  // Prisma genera la consulta SQL o de MongoDB por ti
  const usuarios = await prisma.usuario.findMany({
    where: {
      rol: 'admin',
    },
    select: {
      nombre: true,
      email: true
    }
  });
  return usuarios;
}
```

---

## ✅ Buenas Prácticas

|Categoría|Buena Práctica|Error Común|
|---|---|---|
|**Modularidad**|Crear una carpeta **`/models`** o **`/schemas`** para definir y exportar todos tus modelos de ORM/ODM.|Definir modelos directamente en los archivos de ruta o en la capa de negocio.|
|**Abstracción**|**Delegar la lógica de consulta al ORM/ODM**. Evitar escribir SQL nativo o consultas nativas de MongoDB si el ORM/ODM puede hacerlo.|Usar el driver nativo de la BD para la mayoría de las operaciones CRUD simples.|
|**Promesas**|Recordar que todas las operaciones del ORM/ODM son **asíncronas** y devuelven Promesas. Usar **`async/await`** es obligatorio.|Olvidar `await` y tratar de acceder a los datos antes de que la consulta haya terminado.|
|**Elegir la herramienta**|Usar ORM (Sequelize/Prisma) para SQL y ODM (Mongoose) para MongoDB.|Tratar de usar Mongoose con PostgreSQL o viceversa.|

---

## 🔑 Resumen de Puntos Clave

- **ORM** (para SQL) y **ODM** (para NoSQL/MongoDB) resuelven la desconexión entre el código JS y la base de datos.
    
- **ORM** te permite manipular datos como objetos JavaScript (`Usuario.create(...)`).
    
- **Mongoose** (el principal ODM) es esencial para añadir **esquema, validación y lógica de negocio** a MongoDB.
    
- **Prisma** es una alternativa moderna que ofrece un enfoque tipado y limpio.
    
- Todas las operaciones de ORM/ODM son **asíncronas** y deben ser manejadas con **`async/await`**.
    

Ahora que sabes cómo estructurar tus datos en JavaScript, el siguiente paso es la complejidad real: manejar las relaciones y consultas avanzadas en la base de datos.

Tu próxima clase se enfocará en la manipulación de datos: **[[19 - Consultas avanzadas]]**.