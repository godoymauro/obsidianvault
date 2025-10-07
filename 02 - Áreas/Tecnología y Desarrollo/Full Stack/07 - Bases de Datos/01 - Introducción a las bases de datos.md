## ¡Empecemos con la primera clase!

Comenzamos con la introducción esencial.

# 01 - Introducción a las bases de datos

## 💡 ¿Qué es una Base de Datos (BD)?

Imagina una base de datos (BD) como un **archivador digital súper organizado** y persistente. No es solo un montón de archivos sueltos; es un sistema estructurado y diseñado para almacenar, gestionar y recuperar grandes cantidades de información de manera eficiente y segura.

Técnicamente, una Base de Datos es una **colección organizada de datos interrelacionados** que se gestionan mediante un _software_ llamado **Sistema de Gestión de Bases de Datos (SGBD)**.

### El rol del SGBD

El **SGBD** (como MySQL, PostgreSQL, o MongoDB) es el _motor_ que hace que la base de datos funcione. Actúa como intermediario entre la base de datos física (los archivos en disco) y la aplicación o el usuario que solicita la información.

|Función del SGBD|Descripción|Importancia|
|---|---|---|
|**Definición de Datos**|Permite crear la estructura (esquema, tablas, campos).|Establece las reglas y el formato de los datos.|
|**Manipulación de Datos**|Permite insertar, consultar, modificar y eliminar datos (CRUD).|Es el medio para interactuar y operar con la información.|
|**Seguridad**|Gestiona [[21 - Usuarios y permisos\|usuarios y privilegios]] para el acceso.|Protege la información de accesos no autorizados.|
|**Integridad**|Aplica reglas para asegurar que los datos sean correctos y consistentes.|Es vital, por ejemplo, en transacciones bancarias.|
|**Control de Concurrencia**|Permite que múltiples usuarios trabajen a la vez sin interferencias.|Evita que dos personas modifiquen el mismo dato al mismo tiempo.|

Exportar a Hojas de cálculo

---

## 📜 Un Breve Vistazo Histórico

El concepto de BD evolucionó por la necesidad de manejar volúmenes crecientes de información y, crucialmente, la necesidad de establecer **relaciones** entre esos datos.

|Época|Modelo Dominante|Concepto Clave|SGBD Representativo|
|---|---|---|---|
|1960s|**Jerárquico**|Estructura de árbol (un padre, muchos hijos).|IBM IMS|
|**1970s - Actualidad**|**Relacional (SQL)**|**Tablas** (relaciones) y filas; el modelo matemático de Codd.|Oracle, MySQL, PostgreSQL|
|**2000s - Actualidad**|**No Relacional (NoSQL)**|Modelos flexibles para escalar, grandes volúmenes y agilidad.|MongoDB, Redis, Cassandra|

Exportar a Hojas de cálculo

---

## 🏗️ Tipos Fundamentales de Bases de Datos

Hoy en día, la distinción principal se centra en el modelo de datos:

### 1. Bases de Datos Relacionales (SQL)

- **Estructura:** Rígida, basada en el álgebra relacional. Los datos se organizan en **tablas** con filas y columnas bien definidas.
    
- **Lenguaje:** **SQL** (_Structured Query Language_), el estándar para definir y manipular los datos.
    
- **Prioridad:** **Integridad y consistencia** de los datos (el principio **ACID**).
    
- **Ejemplos:** **PostgreSQL**, **MySQL**, Oracle, SQL Server.
    

### 2. Bases de Datos No Relacionales (NoSQL)

- **Estructura:** Flexible, sin un esquema fijo (Schema-less). Se adaptan mejor a datos que cambian constantemente o que no encajan en tablas.
    
- **Tipos principales:**
    
    - **Documentales:** Almacenan datos en documentos (JSON o BSON). Ej: **MongoDB**.
        
    - **Clave-Valor:** Ideal para _caching_. Ej: **Redis**.
        
    - **Grafo:** Optimizadas para relaciones complejas. Ej: Neo4j.
        
    - **Columnares:** Optimizadas para Big Data y analítica. Ej: Cassandra.
        
- **Prioridad:** **Escalabilidad horizontal** (manejar mucho tráfico) y **Disponibilidad** (teorema [[26 - Bases de datos distribuidas|CAP]]).
    
- **Ejemplos:** **MongoDB**, **Redis**, Cassandra, DynamoDB.
    

---

## 🌎 Casos de Uso Reales

Las bases de datos son el esqueleto de casi cualquier aplicación moderna.

|Caso de Uso|Tipo de BD Recomendado|Justificación|
|---|---|---|
|**Banca, E-commerce (órdenes)**|**Relacional (SQL)**|Se requiere **ACID** (Transacciones seguras) para asegurar que una orden no se pierda o duplique.|
|**Sistema de _Caché_ (Datos temporales)**|**NoSQL (Clave-Valor - Redis)**|Necesidad de acceso a datos extremadamente rápido (_in-memory_).|
|**Redes Sociales (perfiles, _feeds_)**|**NoSQL (Documental - MongoDB)**|Alta flexibilidad para perfiles cambiantes y necesidad de escalar a millones de usuarios fácilmente.|
|**Rutas y Conexiones (Amigos)**|**NoSQL (Grafo - Neo4j)**|Optimizado para almacenar y consultar las relaciones complejas de manera eficiente.|

Exportar a Hojas de cálculo

---

## 🛠️ Buenas Prácticas y Errores Comunes (Desde el Inicio)

Adoptar buenas prácticas desde el diseño inicial te ahorrará horas de trabajo y frustración.

|Categoría|Buena Práctica|Error Común|
|---|---|---|
|**Diseño**|Invertir tiempo en el [[03 - Modelo entidad-relación\|Modelo ER]] antes de crear la primera tabla.|Empezar a codificar (`CREATE TABLE`) inmediatamente sin un plano.|
|**Nomenclatura**|Usar nombres descriptivos, en **singular** para tablas (ej: `producto`, `usuario`) y **minúsculas** o _snake_case_.|Usar plurales, mayúsculas y espacios (ej: `Productos`, `Tabla 1`, `Usuarios Finales`).|
|**Consistencia**|Definir claramente el **tipo de dato** para cada campo (entero, texto, fecha, booleano).|Usar el tipo de dato más genérico (`VARCHAR` o `TEXT`) para todo, dificultando la búsqueda y la integridad.|

Exportar a Hojas de cálculo

---

## ✅ Cierre: Puntos Clave

- Una **Base de Datos** es una colección de datos organizada, y el **SGBD** es el _software_ que la administra (el motor).
    
- Las **BD Relacionales (SQL)** se basan en **tablas** y priorizan la **Integridad (ACID)**, ideales para transacciones.
    
- Las **BD No Relacionales (NoSQL)** tienen estructuras flexibles (documentos, clave-valor) y priorizan la **Escalabilidad** y la **Disponibilidad**.
    
- El buen diseño (Modelado ER) es la base de todo proyecto de BD exitoso.
    

---

**Próximo Paso:** La distinción entre SQL y NoSQL es crucial. La siguiente clase la dedicaremos por completo a compararlas en detalle para que sepas cuándo usar cada una.

[[02 - Bases de datos relacionales vs no relacionales]]