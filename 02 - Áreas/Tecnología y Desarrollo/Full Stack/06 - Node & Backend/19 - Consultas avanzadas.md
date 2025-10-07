¡Continuemos con fuerza! Ya sabes cómo conectar y abstraer tus datos con ORM/ODM ( **[[18 - ORM y ODM]]**). El siguiente desafío es interactuar con esos datos para obtener información significativa. Una API real no solo hace CRUD simple; necesita manejar relaciones y extraer datos complejos.

Esta clase se enfocará en las técnicas para realizar consultas que van más allá de un simple `findAll()`.

Aquí tienes la nota para la clase anterior: **[[18 - ORM y ODM]]**.

---

# 19 - Consultas avanzadas

## 📚 Introducción: El Poder del Filtrado y la Relación

En el contexto de una API RESTful ( **[[15 - APIs RESTful]]**), las consultas avanzadas son esenciales para manejar peticiones comunes de clientes, tales como:

1. **Filtrado/Búsqueda:** Encontrar recursos que coincidan con ciertos criterios (ej: productos en stock).
    
2. **Paginación:** Devolver los resultados en lotes pequeños para no saturar al cliente ni al servidor.
    
3. **Relaciones (Joins):** Obtener datos de recursos relacionados en una sola consulta (ej: un usuario y todos sus pedidos).
    

Usaremos un enfoque genérico, ya que todos los ORM/ODM modernos (Prisma, Sequelize, Mongoose) tienen métodos equivalentes para estas operaciones.

---

## 🔍 1. Filtrado, Ordenamiento y Limitación

Estas operaciones se realizan típicamente en la capa de la base de datos para optimizar el rendimiento, ya que la BD es mucho más eficiente que Node.js para estas tareas.

### A. Filtrado Condicional (`WHERE`)

Permite especificar condiciones de búsqueda. Se basa en el objeto `req.query` ( **[[13 - Rutas avanzadas y parámetros]]**).

|Operación|SQL (Ejemplo)|ORM (Mongoose/Prisma equivalente)|
|---|---|---|
|**AND**|`WHERE status='active' AND rol='user'`|`{ status: 'active', rol: 'user' }`|
|**OR**|`WHERE qty < 10 OR precio > 50`|`{ $or: [{ qty: { $lt: 10 } }, { precio: { $gt: 50 } }] }`|
|**LIKE** (Búsqueda)|`WHERE nombre LIKE '%camisa%'`|`{ nombre: { $regex: 'camisa', $options: 'i' } }` (Mongoose)|

### B. Ordenamiento (`ORDER BY`) y Paginación

La paginación es clave para la escalabilidad. Si tienes un millón de usuarios, no quieres enviarlos todos de golpe.

|Concepto|Método|Ejemplo (Prisma/Sequelize)|
|---|---|---|
|**Ordenamiento**|`ORDER BY`|`orderBy: { creadoEn: 'desc' }`|
|**Límite**|`LIMIT`|`take: 10` (Prisma) o `limit: 10` (Sequelize)|
|**Salto/Offset**|`OFFSET`|`skip: 20` (Prisma) o `offset: 20` (Sequelize)|

**Cálculo de Paginación:**

Si el cliente pide pagina=3 y limite=10:

![](data:,)![](data:,)

La consulta saltará los primeros 20 resultados y tomará los siguientes 10.

---

## 🤝 2. Relaciones (Joins)

En una aplicación real, los datos están interconectados. Un **Usuario** tiene muchos **Pedidos**.

- **SQL (Relacional):** La relación se define mediante claves foráneas y se consulta con operaciones **JOIN**.
    
- **NoSQL (Documentos):** La relación se define mediante la **referencia** (guardar el ID de un documento en otro) o la **incrustación** (guardar un documento dentro de otro).
    

### A. Join/Inclusión (Eager Loading)

La técnica _Eager Loading_ permite cargar los datos relacionados en la misma consulta, evitando múltiples viajes a la base de datos (N+1 Query Problem).

**Escenario:** Obtener todos los pedidos y los datos del usuario que los creó.

|Tipo de BD|Consulta|ORM (Mongoose/Prisma equivalente)|
|---|---|---|
|**SQL**|`SELECT * FROM pedidos JOIN usuarios ON ...`|`include: { usuario: true }` (Prisma/Sequelize)|
|**MongoDB**|Usa **`populate`** (Mongoose)|`populate: 'usuario'`|

**Ejemplo con Mongoose `populate`:**

JavaScript

```
// models/Pedido.js tiene un campo: usuarioId: { type: mongoose.Schema.Types.ObjectId, ref: 'Usuario' }

router.get('/pedidos', async (req, res, next) => {
    try {
        // Busca todos los pedidos y "rellena" el campo 'usuario' con los datos del documento de Usuario
        const pedidos = await Pedido.find({})
            .populate('usuario', 'nombre email') // Especificamos qué campos queremos del usuario
            .sort({ fecha: 'desc' });
        
        res.json(pedidos);
    } catch (error) {
        next(error);
    }
});
```

El resultado será una lista de objetos `Pedido` donde la propiedad `usuario` es el objeto real del usuario, no solo su ID.

---

## 📈 3. Agregaciones (Aggregate)

La agregación es la operación de procesar registros y devolver un único resultado calculado. Esto incluye funciones matemáticas (suma, promedio, conteo) y agrupación de datos.

**Escenario:** Calcular el total de ventas por mes o el promedio de edad de los usuarios.

### Ejemplo de Conteo y Agrupación (Mongoose)

JavaScript

```
router.get('/reporte/ventas-por-producto', async (req, res, next) => {
    try {
        const resultado = await Pedido.aggregate([
            // 1. $match: Filtrar (opcional)
            { $match: { estado: 'completado' } }, 
            
            // 2. $group: Agrupar los resultados
            { $group: {
                // _id: La clave por la que agrupar (ej: ID del producto)
                _id: '$productoId', 
                // totalVendido: El campo calculado (suma de la cantidad de productos)
                totalVendido: { $sum: '$cantidad' },
                // cantidadPedidos: El conteo de documentos en el grupo
                cantidadPedidos: { $sum: 1 } 
            }},
            
            // 3. $sort: Ordenar el resultado final
            { $sort: { totalVendido: -1 } }
        ]);

        res.json(resultado);
    } catch (error) {
        next(error);
    }
});
```

Aunque la sintaxis de Mongoose (`$match`, `$group`) se parece mucho a MongoDB Query Language, la mayoría de los ORM (incluido Prisma) ofrecen una interfaz más amigable para estas funciones (`groupBy`, `_sum`, `_avg`).

---

## ✅ Buenas Prácticas y Errores Comunes

|Categoría|Buena Práctica|Error Común|
|---|---|---|
|**N+1 Problem**|Usar técnicas de **_Eager Loading_** (`populate` o `include`) para cargar todas las relaciones necesarias en una sola consulta.|Usar el método _Lazy Loading_ (cargar relaciones separadamente) y hacer N+1 consultas a la BD (una para el recurso principal y N para sus relaciones).|
|**Paginación**|Siempre implementar paginación en _endpoints_ que puedan devolver muchos resultados (`GET /recursos`).|Devolver todos los resultados por defecto, sobrecargando la red y la memoria.|
|**Proyección**|Usar la opción `select:` (Mongoose/Sequelize) o `select:` (Prisma) para **obtener solo los campos que necesitas** de la BD.|Devolver `SELECT *` y enviar campos sensibles o innecesarios al cliente.|
|**Seguridad**|Sanitizar y validar los _queries_ de entrada (`req.query`) antes de pasarlos al ORM para evitar Inyección SQL o fallos lógicos.|Dejar al cliente manipular consultas sin validación.|

---

## 🔑 Resumen de Puntos Clave

- Las consultas avanzadas se basan en **filtrado**, **ordenamiento**, **paginación** y **relaciones**.
    
- El **_Eager Loading_** (`populate` o `include`) es la técnica estándar para resolver el **N+1 Query Problem** al cargar relaciones.
    
- Las **Agregaciones** (`aggregate` o métodos ORM equivalentes) se usan para calcular métricas (sumas, promedios, conteos) y agrupar datos.
    
- **La paginación** (usando `skip`/`offset` y `take`/`limit`) es crucial para la escalabilidad.
    
- Debes delegar la complejidad de estas consultas a tu ORM/ODM, manteniendo tu código JavaScript limpio.
    

Hemos dominado la consulta de datos. El último paso de esta capa es crucial: ¿qué pasa cuando la estructura de tus datos debe cambiar en producción?

Tu próxima clase te enseñará a gestionar la evolución de tu base de datos: **[[20 - Persistencia y migraciones]]**.