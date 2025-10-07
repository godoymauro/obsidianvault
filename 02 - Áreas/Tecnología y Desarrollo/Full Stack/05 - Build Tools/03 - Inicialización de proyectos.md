Con Node.js y nuestros gestores instalados, es hora de dar el primer paso crucial: **inicializar un proyecto** y entender su corazón, el archivo `package.json`.

---

Aquí tienes los apuntes de la clase anterior: [[02 - Instalación y configuración básica]]

# 03 - Inicialización de proyectos

## 📝 Introducción

Todo proyecto de JavaScript moderno comienza con la **inicialización**. Este proceso crea la estructura básica de archivos y, lo más importante, el archivo **`package.json`**. Este archivo es el **manifiesto** de tu proyecto; define metadatos, enumera todas tus dependencias y contiene los _scripts_ que usaremos para construir (build), probar (test) y desarrollar (dev) nuestra aplicación.

## 🧠 Teoría: El Corazón del Proyecto (`package.json`)

El `package.json` es un archivo fundamental. Es un estándar de la industria y la clave para que cualquier persona o máquina (incluidos los servicios de CI/CD) pueda entender y replicar tu entorno de desarrollo.

### 1. Inicialización

Los tres gestores de paquetes ofrecen comandos similares para iniciar un proyecto. Lo que hacen es hacerte algunas preguntas y luego generar el `package.json`.

|Gestor|Comando de Inicialización|
|---|---|
|**npm**|`$ npm init`|
|**Yarn**|`$ yarn init`|
|**pnpm**|`$ pnpm init`|

Exportar a Hojas de cálculo

### 2. Estructura Básica de `package.json`

Cuando abres un `package.json` recién generado, verás campos esenciales:

JSON

```
{
  "name": "mi-proyecto-moderno", // 👈 Nombre único, en minúsculas y sin espacios.
  "version": "1.0.0",            // 👈 Versión actual del proyecto (veremos semver en [[04 - Conceptos clave]])
  "description": "Mi primera SPA moderna con Build Tools.",
  "main": "index.js",            // 👈 Punto de entrada principal para Node o Bundlers.
  "scripts": {                   // 👈 Sección crucial para automatizar tareas (ver [[05 - Scripts npm]])
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {},            // 👈 Dependencias de producción
  "devDependencies": {}           // 👈 Dependencias de desarrollo
}
```

- **`name`** y **`version`**: Son los identificadores únicos. Son obligatorios si planeas publicar este proyecto como un paquete en el Registro de npm.
    
- **`main`**: Indica qué archivo debe cargarse si tu proyecto es requerido como un módulo por otro (importante para librerías).
    
- **`scripts`**: **La sección más importante** para el día a día. Aquí definimos comandos cortos para ejecutar tareas largas (Ej: `npm run build` en lugar de `webpack --config...`).
    
- **`dependencies`** / **`devDependencies`**: Las listas de paquetes de terceros necesarios. Lo cubriremos a fondo en la próxima clase [[04 - Conceptos claves]].
    

---

## 🛠️ Ejemplos: Inicialización y Scripts Iniciales

### Ejemplo 1: Inicialización Interactiva

Cuando ejecutas `$ npm init`, se te preguntará por cada campo:

Bash

```
$ npm init

package name: (mi-proyecto) mi-primera-app
version: (1.0.0) 
description: Una app simple de React
entry point: (index.js) src/main.js # 👈 Puedes cambiar el punto de entrada
test command: 
git repository: 
keywords: react, vite, build-tools
author: Profesor Build Tools
license: (ISC) MIT
Is this OK? (yes) yes
```

El archivo `package.json` se crea con la información proporcionada.

### Ejemplo 2: Inicialización Rápida y Silenciosa

Si quieres crear el archivo rápidamente con todos los valores por defecto, usa el _flag_ `-y` (o `--yes`):

Bash

```
# Inicializa rápidamente un proyecto con valores por defecto
$ npm init -y 

# Equivalente en Yarn
$ yarn init -y

# Equivalente en pnpm
$ pnpm init -y
```

Esto es común cuando estás creando un proyecto _boilerplate_ o probando algo rápido.

### Ejemplo 3: El Primer Script Útil

Una vez creado el `package.json`, vamos a añadir un primer script que nos ayude a verificar que Node.js está funcionando correctamente y que nuestros scripts se ejecutan.

1. Abre `package.json` y modifica la sección `scripts`:
    
    JSON
    
    ```
    "scripts": {
      "start": "echo 'Iniciando mi primera aplicación...' && node index.js",
      "test": "echo \"Error: no test specified\" && exit 1"
    }
    ```
    
2. Crea el archivo `index.js` en la raíz del proyecto:
    
    JavaScript
    
    ```
    // index.js
    console.log("¡El Build Tool está listo para trabajar!");
    ```
    
3. Ejecuta el script:
    
    Bash
    
    ```
    # Ejecutamos el script 'start'
    $ npm run start 
    
    # Salida:
    # > mi-primera-app@1.0.0 start
    # > echo 'Iniciando mi primera aplicación...' && node index.js
    
    # Iniciando mi primera aplicación...
    # ¡El Build Tool está listo para trabajar!
    ```
    

> **NOTA DE COMANDOS:** Para ejecutar un script definido en `package.json`, siempre se usa la sintaxis `$ npm run <nombre-del-script>`. La única excepción es para los scripts reservados (`test`, `start`, `install`) que puedes ejecutar directamente como `$ npm start`.

---

## 🛑 Errores Comunes

|Error|Descripción|Solución|
|---|---|---|
|**"Cannot read property 'scripts' of undefined"**|Común si el archivo `package.json` tiene un error de sintaxis JSON (falta una coma, llave mal cerrada).|Usa un validador JSON en línea o revisa cuidadosamente el formato.|
|**"npm run start: El sistema no puede encontrar la ruta especificada."**|Esto significa que el comando interno del script (Ej: `node index.js`) no encuentra el archivo `index.js` o el binario `node`.|Verifica que el archivo exista en la ruta correcta y que Node.js esté en tu `PATH`.|
|**"The project name must be lowercase..."**|Intentaste poner mayúsculas, espacios o caracteres especiales en el campo `name` durante `npm init`.|El nombre debe ser un _slug_ (ej: `mi-proyecto-web`).|

Exportar a Hojas de cálculo

## 📚 Resumen de Puntos Clave

- El archivo **`package.json`** es el manifiesto central de tu proyecto, que define metadatos y dependencias.
    
- Los comandos `$ npm init`, `$ yarn init`, y `$ pnpm init` lo crean. El _flag_ `-y` lo hace rápidamente.
    
- La sección **`scripts`** es vital para automatizar comandos de desarrollo y _build_.
    
- Siempre ejecuta los scripts con `$ npm run <nombre-script>` (o la excepción `$ npm start`).
    

---

En nuestra siguiente clase, profundizaremos en el concepto más crítico: cómo definimos y gestionamos las librerías externas que hacen funcionar nuestra aplicación.

[[04 - Conceptos claves]]