¡Perfecto! Hemos cubierto el diseño teórico (MER) y la lógica matemática (Álgebra Relacional). Ahora, toca pasar de la teoría a la práctica: necesitamos el _hardware_ para nuestro entrenamiento.

---

# 05 - Instalación de motores

## ⚙️ Preparando el Entorno: El Laboratorio del Experto

En esta clase, no solo instalaremos software, sino que configuraremos un entorno de trabajo que nos permitirá practicar con los principales paradigmas: el relacional robusto (**PostgreSQL**), el relacional popular (**MySQL** o su derivado **MariaDB**), el ligero (**SQLite**) y el documental (**MongoDB**).

> 📌 **Objetivo:** Tener al menos un motor de cada tipo funcionando y accesible.

---

## 1. El Gigante de la Integridad: PostgreSQL (Relacional)

**PostgreSQL** es conocido por su cumplimiento estricto del estándar SQL, su robustez y sus características avanzadas, lo que lo convierte en la opción favorita de muchos desarrolladores y empresas de tecnología.

### Instalación (Generalidades)

1. **Descarga:** Visita el sitio oficial de PostgreSQL.
    
2. **Instalador Gráfico (Recomendado):** El instalador para Windows, macOS y Linux incluye el servidor, el _pgAdmin_ (una herramienta gráfica de administración) y las utilidades de línea de comandos.
    
3. **Configuración Inicial:** Durante la instalación, se te pedirá:
    
    - **Puerto:** Por defecto, `5432`.
        
    - **Contraseña del _superuser_** (`postgres`): **¡Recuerda esta contraseña!**
        

### Conexión y Verificación

Una vez instalado, el cliente gráfico **pgAdmin** será nuestra herramienta principal.

|Paso|Acción|Propósito|
|---|---|---|
|1.|Abrir pgAdmin.|Acceder al entorno de gestión.|
|2.|Conectar al servidor `localhost:5432` usando la contraseña que definiste.|Establecer la conexión inicial.|
|3.|Crear una nueva base de datos llamada `curso_db`.|Espacio de trabajo para nuestras prácticas.|

---

## 2. El Motor Popular: MySQL / MariaDB (Relacional)

**MySQL** (y su bifurcación de código abierto, **MariaDB**) es el SGBD relacional más utilizado en aplicaciones web (el componente 'M' del _stack_ LAMP/LEMP).

### Instalación (Generalidades)

1. **Opción Todo en Uno (Recomendada):** Instala **XAMPP** o **WAMP/MAMP**. Estos paquetes instalan Apache (servidor web), PHP/Python y MySQL/MariaDB al mismo tiempo. Es la forma más rápida de tenerlo funcional.
    
2. **Instalador de MySQL:** Si solo quieres el motor, el instalador oficial de MySQL Workbench te permite instalar el servidor y la herramienta gráfica.
    

### Conexión y Verificación

Una vez instalado, usaremos **MySQL Workbench** (si usaste el instalador oficial) o **phpMyAdmin** (si usaste XAMPP/WAMP/MAMP).

- **Puerto por defecto:** `3306`.
    
- **Usuario por defecto:** `root`.
    
- **Verificación:** Abre la herramienta gráfica, conecta y crea una base de datos llamada `curso_mysql`.
    

---

## 3. El Ligero y Embebido: SQLite (Relacional, sin Servidor)

**SQLite** es único: no es un sistema cliente-servidor como los anteriores. La base de datos es un **archivo único** en el disco. Es perfecto para aplicaciones pequeñas, bases de datos locales o pruebas.

### Instalación

1. **¡No necesita instalación!** Simplemente descarga el binario (`sqlite3.exe` o `sqlite3` en Linux/Mac).
    
2. **Recomendación:** Descarga **DB Browser for SQLite**, una excelente herramienta visual para crear y gestionar archivos SQLite.
    

### Operación

Para crear o abrir una BD:

Bash

```
# En la línea de comandos
sqlite3 curso_sqlite.db

# Una vez dentro, verifica la versión
.version
```

---

## 4. El Documental Flexible: MongoDB (No Relacional)

**MongoDB** es el SGBD documental más popular. Almacena datos en formato **BSON** (binario similar a JSON), ofreciendo flexibilidad de esquema.

### Instalación (Generalidades)

1. **Descarga:** Obtén el **MongoDB Community Server**.
    
2. **Servicio:** Se instala como un servicio que se ejecuta en segundo plano.
    
3. **Herramienta Gráfica (Recomendada):** Instala **MongoDB Compass**. Es la interfaz oficial y la más cómoda para interactuar con colecciones y documentos.
    

### Conexión y Verificación

- **Puerto por defecto:** `27017`.
    
- **Verificación (Vía Compass):** Conéctate a `mongodb://localhost:27017`. Crea una nueva base de datos, por ejemplo, `curso_documentos`.
    

---

## 🛠️ Resumen y Puertos Clave

Mantener un registro de los puertos por defecto es crucial para la administración y las futuras conexiones desde código.

|SGBD|Tipo|Herramienta de Administración Común|Puerto por Defecto|
|---|---|---|---|
|**PostgreSQL**|Relacional|pgAdmin|`5432`|
|**MySQL/MariaDB**|Relacional|MySQL Workbench / phpMyAdmin|`3306`|
|**SQLite**|Relacional/Embebido|DB Browser for SQLite|N/A (basado en archivos)|
|**MongoDB**|No Relacional (Documental)|MongoDB Compass|`27017`|

---

## ✅ Cierre: Puntos Clave

- Hemos configurado nuestro laboratorio para usar los motores más relevantes: **PostgreSQL** y **MySQL** para SQL, y **MongoDB** para NoSQL.
    
- Los SGBD relacionales (`PostgreSQL`, `MySQL`) funcionan como un **servicio cliente-servidor** con puertos específicos.
    
- **SQLite** es un motor _serverless_ (sin servidor), basado en un **archivo único**.
    
- Las herramientas gráficas (`pgAdmin`, `MySQL Workbench`, `MongoDB Compass`) son esenciales para la gestión y visualización de datos.
    

---

**Próximo Paso:** Con nuestros motores funcionando, es hora de hablar el lenguaje de las bases de datos relacionales. Empezaremos con el componente fundamental: el **Structured Query Language**.

[[06 - Sintaxis SQL]]