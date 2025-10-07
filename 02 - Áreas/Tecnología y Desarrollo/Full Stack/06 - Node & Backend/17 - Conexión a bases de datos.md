¡Claro! Nuestra API RESTful ya es profesional, pero necesita **memoria a largo plazo**. Una API que solo guarda datos en un _array_ (como hicimos) olvida todo al reiniciarse.

Entramos al **Nivel 4: Bases de datos**. El primer paso es entender cómo Node.js se conecta al vasto mundo de la persistencia de datos.

Aquí tienes la nota para la clase anterior: **[[16 - Manejo de errores]]**.

---

# 17 - Conexión a bases de datos

## 📚 Introducción: Tipos de Persistencia

En el desarrollo backend, la Base de Datos (BD) es la capa de persistencia donde se almacena la información de manera duradera. Node.js, gracias a NPM, puede conectarse a prácticamente cualquier BD.

Antes de la conexión, es fundamental entender la diferencia entre los dos grandes paradigmas: **SQL (Relacionales)** y **NoSQL (No Relacionales)**.

---

## 💾 1. Bases de Datos Relacionales (SQL)

Las bases de datos SQL (Structured Query Language) son el modelo tradicional, basadas en el modelo relacional: datos organizados en **tablas** con **esquemas fijos** y **relaciones** estrictas.

|Característica|Descripción|Bases de Datos Comunes|
|---|---|---|
|**Estructura**|Esquema estricto (filas y columnas).|**PostgreSQL**, **MySQL**, SQL Server, SQLite.|
|**Consistencia**|Alta (Principio ACID). Priorizan la integridad de los datos.|PostgreSQL es el favorito en el ecosistema Node moderno por su robustez.|
|**Drivers de Node**|Se conectan típicamente con librerías como `pg` (para PostgreSQL) o `mysql2`.|Necesitarás el **driver** para hablar el lenguaje de la BD.|
|**Ventaja**|Ideal para datos complejos e interconectados (ej: contabilidad, sistemas bancarios).||

### Ejemplo de Conexión (PostgreSQL nativo con `pg`)

JavaScript

```
// db/postgres.js
const { Pool } = require('pg');

// 💡 Configuración de conexión usando variables de entorno (BUENA PRÁCTICA)
const pool = new Pool({
  user: process.env.DB_USER || 'postgres',
  host: process.env.DB_HOST || 'localhost',
  database: process.env.DB_NAME || 'mi_api_db',
  password: process.env.DB_PASSWORD || 'secret',
  port: process.env.DB_PORT || 5432,
});

async function query(text, params) {
  try {
    const res = await pool.query(text, params);
    return res;
  } catch (error) {
    console.error('Error en la consulta a PostgreSQL:', error.message);
    throw error; // Propagar el error para el manejador de Express (next(error))
  }
}

// Ejemplo de uso:
// const resultados = await query('SELECT * FROM usuarios WHERE id = $1', [1]);
// console.log(resultados.rows);

module.exports = {
  query,
  pool,
};
```

---

## 🌿 2. Bases de Datos No Relacionales (NoSQL)

Las bases de datos NoSQL ofrecen una flexibilidad de esquema y están optimizadas para la escalabilidad horizontal y el manejo de grandes volúmenes de datos no estructurados.

|Característica|Descripción|Bases de Datos Comunes|
|---|---|---|
|**Estructura**|Esquema dinámico (orientada a documentos, clave-valor, grafos).|**MongoDB** (Documentos JSON), Redis (Clave-Valor), Neo4j (Grafos).|
|**Escalabilidad**|Alta. Fáciles de distribuir entre muchos servidores (_sharding_).|MongoDB es la más popular en el ecosistema Node.js (MEAN/MERN stack).|
|**Drivers de Node**|Se conectan con librerías como `mongodb` (el driver oficial) o **Mongoose** (un ODM).|El driver devuelve objetos JSON/JavaScript.|
|**Ventaja**|Ideal para desarrollo rápido, datos que cambian frecuentemente (ej: logs, perfiles de usuario, catálogos).||

### Ejemplo de Conexión (MongoDB con el Driver Nativo)

JavaScript

```
// db/mongodb.js
const { MongoClient } = require('mongodb');

// URL de conexión (puede estar en .env)
const url = process.env.MONGO_URI || 'mongodb://localhost:27017';
const client = new MongoClient(url);
const dbName = process.env.DB_NAME || 'mi_api_mongo';

async function connectToMongo() {
  try {
    // 💡 Esta llamada es ASÍNCRONA y puede fallar (Event Loop en acción)
    await client.connect(); 
    console.log('✅ Conexión exitosa a MongoDB.');
    const db = client.db(dbName);
    return db;
  } catch (error) {
    console.error('❌ Error al conectar a MongoDB:', error.message);
    // En un servidor real, deberías salir del proceso aquí: process.exit(1);
    throw error;
  }
}

// La clase principal se exporta para ser reutilizada
module.exports = {
  connectToMongo,
  client,
};
```

---

## 🧱 Abstracción: ORM y ODM (El Próximo Paso)

Conectarse con los _drivers_ nativos (como en los ejemplos anteriores) es la forma de más bajo nivel. Requiere escribir SQL o consultas específicas de MongoDB como cadenas o JSONs.

Para evitar esto, y simplificar el trabajo, usamos capas de abstracción:

|Abstracción|Base de Datos|Librerías Populares|Propósito|
|---|---|---|---|
|**ORM** (Object-Relational Mapper)|SQL|**Sequelize**, **Prisma** (moderno), TypeORM.|Mapea filas de la BD a **Objetos JavaScript** (ej: `Usuario.findAll()`).|
|**ODM** (Object-Document Mapper)|NoSQL (MongoDB)|**Mongoose**.|Añade validación de esquema y lógica de negocio a los documentos de MongoDB.|

**La Mayoría de las veces, usarás un ORM/ODM** para tu lógica de negocio. Lo veremos en detalle en **[[18 - ORM y ODM]]**.

---

## ✅ Buenas Prácticas y Errores Comunes

|Categoría|Buena Práctica|Error Común|
|---|---|---|
|**Seguridad**|**NUNCA** codificar las credenciales de la BD directamente en el código. Usar siempre **Variables de Entorno** (`.env`).|Guardar _user_ y _password_ de la BD como cadenas fijas en `db.js`.|
|**Asincronía**|Las funciones de conexión y consulta son inherentemente asíncronas. Usar siempre **`async/await`** y manejadores de errores.|Olvidar `await` en la conexión, creyendo que la BD está lista antes de que realmente lo esté.|
|**Pool de Conexiones**|Usar un **Pool de Conexiones** (ej: `pg.Pool` o un cliente MongoDB) y reutilizar la misma conexión en toda la aplicación.|Crear una nueva conexión a la BD en cada petición HTTP, lo cual es ineficiente y puede saturar la BD.|
|**Tipos**|Elegir la BD correcta para el trabajo: SQL para relaciones complejas, NoSQL para flexibilidad y escalabilidad.|Usar MongoDB para un sistema de contabilidad estricto, o PostgreSQL para almacenar logs simples.|

---

## 🔑 Resumen de Puntos Clave

- **SQL** (PostgreSQL, MySQL) usa un esquema estricto (tablas) y es ideal para datos relacionales.
    
- **NoSQL** (MongoDB, Redis) usa un esquema flexible (documentos) y es ideal para escalabilidad y rapidez.
    
- Las conexiones a la BD son **operaciones I/O** (Input/Output), por lo que siempre deben ser **asíncronas** (`async/await`).
    
- Un **Pool de Conexiones** es la forma eficiente de gestionar la conexión en un servidor Express.
    
- Para el desarrollo moderno, la abstracción con **ORM/ODM** (Sequelize, Mongoose) es la práctica estándar.
    

Con la conexión clara, ahora podemos enfocarnos en cómo interactuar con los datos de manera limpia, sin escribir SQL/consultas nativas.

Tu próxima clase se enfocará en las herramientas de abstracción: **[[18 - ORM y ODM]]**.