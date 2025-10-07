¡Continuemos! Has llegado a un punto donde tu API funciona y se conecta a la base de datos ( **[[19 - Consultas avanzadas]]**). Pero, ¿qué pasa cuando, después de meses en producción, necesitas añadir una nueva columna a tu tabla de usuarios o cambiar el nombre de una colección?

Simplemente modificar el código del modelo no es suficiente. Necesitas un proceso controlado para aplicar cambios al esquema de la base de datos sin perder los datos existentes. Esto se llama **Migraciones**.

Esta clase te enseñará la estrategia profesional para gestionar la evolución de tu base de datos.

Aquí tienes la nota para la clase anterior: **[[19 - Consultas avanzadas]]**.

---

# 20 - Persistencia y migraciones

## 📚 Introducción: El Desafío del Esquema

En desarrollo de software, la base de datos es la parte más sensible. Un cambio incorrecto puede llevar a la pérdida de datos o al tiempo de inactividad de la aplicación.

- **Migración:** Es un archivo de código que describe los cambios que deben aplicarse al esquema de la base de datos de manera incremental y controlada. Cada migración es una versión de tu base de datos.
    
- **Seeding (Siembra):** Es el proceso de insertar datos iniciales, de prueba o de configuración (ej: roles de administrador, datos de prueba) en la base de datos.
    

Utilizaremos **Sequelize** y **Mongoose** (con un enfoque de _schema evolution_) como ejemplos, ya que son los más comunes para ORM y ODM, respectivamente.

---

## 🛠 1. Migraciones con ORM (Bases de Datos SQL)

Para las bases de datos SQL (donde el esquema es estricto), las migraciones son obligatorias. La mayoría de los ORM (Sequelize, Prisma, TypeORM) tienen sus propias herramientas CLI para generarlas y ejecutarlas.

### El Proceso de Migración (Sequelize CLI)

El proceso sigue tres pasos clave:

1. **Generar (Up):** Crear un nuevo archivo de migración con la lógica de cómo aplicar el cambio (ej: `addColumn`).
    
2. **Rollback (Down):** Añadir la lógica de cómo **deshacer** ese cambio (ej: `removeColumn`).
    
3. **Ejecutar:** Aplicar las migraciones a la BD con un comando CLI.
    

### Estructura de un Archivo de Migración

Una migración es un archivo de código que exporta dos funciones, `up` y `down`.

JavaScript

```
// 20250101-add-rol-to-usuarios.js (Ejemplo con Sequelize)

module.exports = {
  // ⬆️ Función UP: Aplica el cambio (el avance)
  async up(queryInterface, Sequelize) {
    // 💡 queryInterface es la abstracción que te permite hablar con la BD
    await queryInterface.addColumn('Usuarios', 'rol', {
      type: Sequelize.STRING,
      allowNull: false,
      defaultValue: 'usuario_estandar',
    });
  },

  // ⬇️ Función DOWN: Deshace el cambio (el rollback)
  async down(queryInterface, Sequelize) {
    await queryInterface.removeColumn('Usuarios', 'rol');
  }
};
```

### Comandos Clave (Ejemplo Sequelize CLI)

|Comando|Propósito|
|---|---|
|`npx sequelize-cli db:migrate`|Ejecuta todas las migraciones pendientes (llama a la función `up` en cada archivo).|
|`npx sequelize-cli db:migrate:undo`|Deshace la **última** migración ejecutada (llama a la función `down`).|
|`npx sequelize-cli db:seed:all`|Ejecuta todos los archivos de _seeding_ (ver más abajo).|

---

## 🍃 2. Seeding (Datos Iniciales)

El _Seeding_ es crucial para inicializar la base de datos con datos que no deberían faltar.

**Ejemplo de Seeding (Sequelize):**

JavaScript

```
// seeders/20250101-initial-roles.js
module.exports = {
  // ⬆️ Función UP (Seeding)
  async up(queryInterface, Sequelize) {
    await queryInterface.bulkInsert('Roles', [
      { nombre: 'Administrador', creadoEn: new Date(), actualizadoEn: new Date() },
      { nombre: 'Editor', creadoEn: new Date(), actualizadoEn: new Date() },
      { nombre: 'Usuario', creadoEn: new Date(), actualizadoEn: new Date() }
    ], {});
  },

  // ⬇️ Función DOWN (Rollback del Seeding)
  async down(queryInterface, Sequelize) {
    await queryInterface.bulkDelete('Roles', null, {});
  }
};
```

---

## 🧊 3. Evolución de Esquema en NoSQL (MongoDB/Mongoose)

Las bases de datos NoSQL, como MongoDB, no tienen esquemas estrictos. Esto es una ventaja, pero también requiere una estrategia diferente.

- **No hay migraciones de esquema:** La BD acepta cualquier documento. Sin embargo, tu **código (el Schema de Mongoose)** sí tiene un esquema fijo.
    
- **Estrategia de Evolución:** Cuando cambias el esquema de Mongoose, no necesitas un archivo de migración para la BD. Simplemente:
    
    1. Despliegas el nuevo código de Node.js con el esquema modificado.
        
    2. Si necesitas **transformar** los datos antiguos (ej: renombrar un campo), escribes un **script de una sola ejecución** que recorre los documentos antiguos y los actualiza, ejecutándolo _antes_ de desplegar la nueva versión.
        

**Ejemplo de Script de Transformación (Mongoose):**

JavaScript

```
// scripts/transform-rename-username.js
const mongoose = require('mongoose');
const User = require('../models/User'); 

async function renameField() {
    await mongoose.connect(process.env.MONGO_URI);
    
    // Encuentra todos los usuarios que aún tienen el campo 'username'
    const { modifiedCount } = await User.updateMany(
        { username: { $exists: true } }, 
        { $rename: { 'username': 'alias' } } // 💡 Operación atómica de renombrar en Mongo
    );

    console.log(`✅ ${modifiedCount} documentos actualizados.`);
    await mongoose.disconnect();
}

renameField();
// Este script se ejecuta manualmente/por CI/CD una sola vez.
```

---

## ✅ Buenas Prácticas y Errores Comunes

|Categoría|Buena Práctica|Error Común|
|---|---|---|
|**Transacciones**|Usar **transacciones** en migraciones complejas (SQL). Si falla una parte, todo el cambio se revierte automáticamente.|No usar transacciones, dejando la BD en un estado inconsistente si la migración falla a mitad de camino.|
|**Rollback**|**Siempre** escribir y probar la función `down` (`rollback`) de cada migración.|No escribir la función `down`, lo que impide retroceder a una versión estable en caso de un error catastrófico.|
|**Seeding**|Separar el _seeding_ de los datos de **desarrollo** (falsos) de los datos de **producción** (configuración esencial).|Usar _seeding_ para datos de prueba que deberían estar en archivos JSON o fixtures separados.|
|**ID en SQL**|Nunca modificar migraciones ya ejecutadas. Si una migración está mal, crea una **nueva** para corregir la anterior.|Editar una migración antigua, lo que rompe el historial de migraciones en otros entornos.|

---

## 🔑 Resumen de Puntos Clave

- Las **Migraciones** son scripts con lógica **`up`** (aplicar) y **`down`** (revertir) que controlan la evolución del esquema en bases de datos SQL.
    
- **Seeding** es la inserción de datos iniciales o de prueba.
    
- En Node.js, las herramientas CLI de los ORM (ej: Sequelize CLI) son la forma estándar de gestionar migraciones.
    
- Las operaciones de migración son **asíncronas** y deben ejecutarse en un entorno de **transacción** para mayor seguridad.
    
- En NoSQL (MongoDB), la evolución del esquema se maneja con la **actualización atómica** de los documentos existentes a través de scripts de una sola ejecución, en lugar de migraciones formales de esquema.
    

---

# 🚀 Nivel 4 Completado

¡Felicidades! Has completado el **Nivel 4: Bases de datos**. Ahora puedes conectar, consultar y mantener la estructura de tus datos como un profesional.

Es hora de proteger lo que has construido. Entramos al **Nivel 5: Autenticación, seguridad y despliegue**.

Tu próxima clase: **[[21 - Autenticación]]**.