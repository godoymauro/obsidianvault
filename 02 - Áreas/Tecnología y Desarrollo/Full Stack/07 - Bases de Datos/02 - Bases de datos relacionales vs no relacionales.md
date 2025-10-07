¡Perfecto! Hemos sentado las bases. La siguiente clase es crucial para entender el panorama moderno de las bases de datos.

---

# 02 - Bases de datos relacionales vs no relacionales

## ⚔️ La Gran División: SQL vs NoSQL

Después de entender qué son las bases de datos y el rol del SGBD, el siguiente paso es comprender la principal división del mercado: las **Bases de Datos Relacionales (SQL)** y las **Bases de Datos No Relacionales (NoSQL)**.

La elección entre una y otra es la decisión de diseño más importante que tomarás, ya que afecta la escalabilidad, la flexibilidad y la integridad de tu aplicación.

---

## 🏛️ Bases de Datos Relacionales (SQL)

Las bases de datos relacionales, que usan **SQL** (_Structured Query Language_), se basan en un modelo matemático establecido por E.F. Codd en los años 70.

### Conceptos Clave

1. **Esquema Rígido (Schema-on-Write):** Debes definir la estructura de las tablas (_columnas y tipos de datos_) **antes** de insertar cualquier dato. Cambiar la estructura es costoso.
    
2. **Tablas y Relaciones:** Los datos se almacenan en **tablas** (o **relaciones**), y las conexiones entre ellas se definen explícitamente usando claves primarias y foráneas.
    
3. **Integridad ACID:** Su característica definitoria. Garantiza que las transacciones sean confiables.
    

### 🛡️ ACID (Atomicidad, Consistencia, Aislamiento, Durabilidad)

Este es el estándar de oro para la confiabilidad de datos:

< O�>Vmati���>V0s 1 run�q��>Vppea���>V bace 0%3�>VepeaP$�>Vdingbox �� �>V, 0)���>Vx no8, 2`��>Vuto;���>V; cllor:�4�>V 29)`��>Vuto;ne; �� �>Vone;`��>Vrmalto; @� �>V 0px&��>Virecr; display: block; fill: rgb(0, 0, 0); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 16px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(27, 28, 29) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: "Google Sans Text", sans-serif !important; line-height: 1.15 !important;">

## V�������� Z�>V,ސ>V�������� Z�>Vx5ސ>V�������� Z�>V�>ސ>V�������� Z�>VXHސ>V�������� Z�>V�Qސ>V�������� Z�>V8[ސ>V�������� Z�>V�dސ>V�������� Z�>Vnސ>Ve: rgb(27, 28, 29) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: "Google Sans Text", sans-serif !important; line-height: 1.15 !important;">

|Principio|Significado|Ejemplo|
|---|---|---|
|**A**tomicity (Atomicidad)|Una transacción debe completarse **por completo** o fallar **por completo**.|En una transferencia bancaria, el dinero sale de la Cuenta A **y** entra a la Cuenta B. Si falla un paso, todo se revierte.|
|**C**onsistency (Consistencia)|Una transacción debe mover la BD de un estado válido a otro.|Las reglas del esquema (ej. la edad debe ser un número positivo) no se violan.|
|**I**solation (Aislamiento)|Las transacciones concurrentes no deben interferir entre sí.|Dos usuarios actualizando el mismo registro al mismo tiempo no se ven afectados mutuamente.|
|**D**urability (Durabilidad)|Una vez que una transacción ha sido confirmada (`COMMIT`), sus cambios son permanentes, incluso si hay un fallo del sistema.|Una vez confirmado el pago, el dato no se pierde aunque se caiga el servidor.|

### Ejemplos de SGBD Relacionales

- **PostgreSQL** (Potente, escalable, con muchas características avanzadas)
    
- **MySQL** (Rápido, popular, estándar de la industria)
    
- SQLite (Ligero, ideal para aplicaciones móviles o embebidas)
    
- Oracle, SQL Server (Soluciones empresariales)
    

---

## 🚀 Bases de Datos No Relacionales (NoSQL)

El término **NoSQL** abarca una familia de bases de datos que surgieron para resolver las limitaciones de escalabilidad y flexibilidad del modelo relacional ante el auge de Internet y el _Big Data_.

### Conceptos Clave

1. **Esquema Flexible (Schema-on-Read):** No se requiere una estructura predefinida. Puedes agregar nuevos campos a los documentos/datos sin afectar los existentes.
    
2. **Variedad de Modelos:** No se limitan a tablas. Los principales modelos son:
    
    - **Clave-Valor:** Almacenan datos simples como un diccionario (ej: _clave = "nombre_usuario", valor = "Carlos"_). Ideal para _caching_.
        
    - **Documental:** Almacenan datos semiestructurados en formatos como JSON/BSON. Ej: Perfiles de usuario, catálogos.
        
    - **Grafo:** Optimizadas para representar y consultar relaciones complejas. Ej: Redes sociales.
        
    - **Columnar:** Optimizadas para leer grandes volúmenes de datos para analítica.
        
3. **Énfasis en CAP:** Priorizan la **Disponibilidad** y la **Tolerancia a Particiones** sobre la Consistencia inmediata (ver [[26 - Bases de datos distribuidas|Teorema CAP]]).
    

### Ejemplos de SGBD NoSQL

- **MongoDB** (Documental)
    
- **Redis** (Clave-Valor y _Caché_)
    
- Cassandra (Columnar)
    
- Neo4j (Grafo)
    

---

## 📊 Tabla Comparativa: SQL vs NoSQL

|Característica|Relacionales (SQL)|No Relacionales (NoSQL)|
|---|---|---|
|**Modelo de Datos**|Tablas, filas, columnas. Estructurado.|Varios: Documentos, Clave-Valor, Grafo, Columnar. Flexible.|
|**Esquema**|Rígido (Schema-on-Write). Definido de antemano.|Dinámico (Schema-on-Read). Se adapta sobre la marcha.|
|**Escalabilidad**|Principalmente **Vertical** (mejorar el hardware).|Principalmente **Horizontal** (agregar más servidores).|
|**Transacciones**|**ACID** (Máxima integridad).|Generalmente **BASE** (Consistencia final).|
|**Mejor para...**|Sistemas transaccionales (Banca, inventarios, contabilidad).|Grandes volúmenes, datos cambiantes, _caching_, contenido web.|
|**Relaciones**|Excelentes: Usa `JOINS` y claves foráneas.|Limitadas: Las relaciones se manejan anidando datos (Documentos) o por referencias.|

---

## 🤔 ¿Cuándo Usar Cuál?

### Elige SQL (Relacional) si...

1. **La Integridad es Crítica:** Estás manejando dinero, inventario o datos legales. Necesitas la garantía **ACID**.
    
2. **La Estructura es Constante:** Sabes que tus datos no cambiarán mucho con el tiempo.
    
3. **Necesitas Relaciones Complejas:** La mayoría de tus consultas implican unir (hacer _JOINS_) muchas tablas (ej: _encuentra todos los pedidos hechos por clientes de cierta región con productos de un proveedor específico_).
    

### Elige NoSQL (No Relacional) si...

1. **Necesitas Escalabilidad Extrema:** Estás construyendo un sistema que puede crecer a millones de usuarios o peticiones por segundo.
    
2. **Necesitas Flexibilidad de Esquema:** Tus datos evolucionan rápidamente (ej: datos de _IoT_, perfiles de usuario con campos opcionales).
    
3. **Los Datos son Simples o Desacoplados:** Estás guardando datos para _caching_ (Redis), logs, o documentos individuales que no necesitan unirse a otras estructuras.
    

---

## 📝 Error Común: "Una es Mejor que la Otra"

**Falso.** En las arquitecturas modernas, lo más común es el **Modelo Híbrido** (o **Políglota**):

- Usar **PostgreSQL** (SQL) para manejar usuarios y pedidos (donde ACID es vital).
    
- Usar **MongoDB** (NoSQL Documental) para guardar los logs de actividad o el contenido del carrito de compra (donde la flexibilidad es útil).
    
- Usar **Redis** (NoSQL Clave-Valor) para la sesión de usuario y _caching_ (donde la velocidad es vital).
    

Te convertirás en un experto cuando sepas combinar estas herramientas.

---

## ✅ Cierre: Puntos Clave

- **SQL** es sinónimo de **Estructura Rígida**, **Integridad (ACID)** y excelentes **Relaciones**. Ideal para transacciones.
    
- **NoSQL** es sinónimo de **Esquema Flexible**, **Escalabilidad Horizontal** y variedad de **Modelos** (Documentos, Clave-Valor). Ideal para grandes volúmenes y agilidad.
    
- La arquitectura moderna usa ambas (`SQL + NoSQL`) para aprovechar lo mejor de cada mundo.
    

---

**Próximo Paso:** Antes de escribir una sola línea de código, debemos dominar la fase de diseño. Empezaremos a modelar la estructura de nuestros datos con el **Modelo Entidad-Relación**.

[[03 - Modelo entidad-relación]]