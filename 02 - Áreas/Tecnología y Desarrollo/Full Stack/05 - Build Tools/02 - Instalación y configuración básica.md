El cimiento de cualquier proyecto frontend moderno son **Node.js** y el **Gestor de Paquetes**.

Pasamos a nuestra segunda clase.

---

Aquí tienes los apuntes de la clase anterior: [[01 - Introducción a Build Tools]]

# 02 - Instalación y configuración básica

## 📝 Introducción

Antes de poder usar cualquier herramienta de _build_ (como Vite, Webpack, o incluso React), necesitamos tener un entorno donde ejecutar las herramientas de JavaScript fuera del navegador. Este entorno es **Node.js**. Una vez que tenemos Node.js, automáticamente tendremos acceso a **npm**, el gestor de paquetes por defecto, lo que nos abre la puerta a otros gestores como **Yarn** y **pnpm**.

## 🧠 Teoría: Node.js y Gestores de Paquetes

### 1. Node.js: El Motor

**Node.js** es un entorno de ejecución de JavaScript **del lado del servidor (fuera del navegador)**.

- **¿Por qué lo necesitamos?** Nuestros _Build Tools_ (bundlers, transpiladores) están escritos en JavaScript. Node.js les da el motor (el V8 de Chrome) para que puedan ejecutarse en tu máquina local.
    
- **Concepto Clave:** Al instalar Node.js, también se instala automáticamente **npm** (Node Package Manager).
    

### 2. Gestores de Paquetes: Los Administradores de Dependencias

Un gestor de paquetes es una herramienta que se comunica con el **Registro de npm (npm Registry)**, una base de datos pública de librerías de código abierto (paquetes).

|Gestor|Origen|Diferencia Clave|
|---|---|---|
|**npm**|Original de Node.js|Por defecto, tradicional, fiable. Instala dependencias de forma _plana_ (desde la versión 3).|
|**Yarn**|Creado por Meta (Facebook)|Buscaba mejorar la velocidad y la fiabilidad del _lock file_. Usa una estructura _plana_.|
|**pnpm**|Más moderno|Utiliza un **almacén de contenido (Content-Addressable Store)** para ahorrar espacio en disco. Usa enlaces simbólicos (_symlinks_).|

Exportar a Hojas de cálculo

**La estructura plana de instalación (flat hierarchy):** Los gestores intentan _aplanar_ la carpeta `node_modules` para evitar duplicidades innecesarias, pero esto a veces genera el fenómeno llamado **"Phantom Dependencies"** (dependencias fantasma), donde puedes usar un paquete que no instalaste explícitamente porque fue subido por una de tus dependencias. **pnpm** resuelve esto de forma elegante usando _symlinks_.

---

## 🛠️ Ejemplos y Comandos: Instalación

### Paso 1: Instalación de Node.js (y npm)

La forma más sencilla es usar el instalador oficial para tu sistema operativo (Windows, macOS).

- **Recomendación:** Siempre descarga la versión **LTS (Long Term Support)**, ya que es la más estable para entornos de producción.
    

**Verificación:** Abre tu terminal (Command Prompt, PowerShell, Terminal) y ejecuta:

|Comando|Propósito|Salida de Ejemplo|
|---|---|---|
|`$ node -v`|Verifica la versión de Node.js|`v20.9.0`|
|`$ npm -v`|Verifica la versión de npm|`10.1.0`|

Exportar a Hojas de cálculo

### Paso 2: Instalación de Gestores Alternativos (Yarn y pnpm)

Aunque ya tienes `npm`, es muy común instalar `yarn` o `pnpm` para aprovechar sus mejoras.

**Instalación global con npm:**

Bash

```
# Instala Yarn globalmente (opción tradicional, aunque Yarn ya tiene instaladores propios)
$ npm install -g yarn

# Instala pnpm globalmente. El más recomendado por su eficiencia en disco.
$ npm install -g pnpm 
```

**Verificación de gestores alternativos:**

Bash

```
# Verifica Yarn
$ yarn -v 

# Verifica pnpm
$ pnpm -v
```

### Paso 3: Configuración de Entornos (Opcional pero Útil)

#### A. Cambiar el Registro

Si trabajas en un entorno corporativo o con un _mirror_ local (artefactos), puedes cambiar el registro de dónde `npm` obtiene los paquetes:

Bash

```
# Muestra el registro actual (debería ser https://registry.npmjs.org/)
$ npm config get registry

# Establece un registro nuevo (ej: un registro privado)
$ npm config set registry https://my-private-registry.com/
```

#### B. Gestión de Versiones de Node.js (NVM)

En proyectos reales, a veces necesitas cambiar rápidamente entre diferentes versiones de Node.js (ej: un proyecto usa v16 y otro usa v20). Para esto usamos un **gestor de versiones de Node.js (NVM)**.

- **Para Linux/macOS:** Se recomienda usar **nvm (Node Version Manager)**.
    
- **Para Windows:** Se recomienda usar **nvm-windows**.
    

**Comandos útiles de NVM (una vez instalado NVM):**

Bash

```
# Instala una versión específica de Node.js
$ nvm install 18

# Cambia a la versión 18
$ nvm use 18

# Muestra todas las versiones que tienes instaladas
$ nvm ls
```

---

## 🛑 Errores Comunes

|Error|Descripción|Solución|
|---|---|---|
|**"npm: command not found"**|Node.js no está instalado o no está en la variable de entorno `PATH`.|Reinstala Node.js y asegúrate de reiniciar la terminal para que se actualice el `PATH`.|
|**"EACCES: permission denied..."**|Estás intentando instalar un paquete **global** (`-g`) y el sistema operativo no te da permisos (común en Linux/macOS).|Vuelve a ejecutar el comando usando `sudo` (ej: `sudo npm install -g yarn`) o ajusta los permisos de tu carpeta `npm`.|
|**"npm WARN deprecated"**|Una dependencia que estás instalando es antigua o descontinuada.|No es un error crítico, pero es una advertencia. Intenta buscar una alternativa más moderna para esa dependencia si es posible.|

Exportar a Hojas de cálculo

## 📚 Resumen de Puntos Clave

- **Node.js** es el entorno de ejecución que nos permite correr herramientas de JS fuera del navegador.
    
- **npm** se instala automáticamente con Node.js y es el gestor de paquetes estándar.
    
- **Yarn** y **pnpm** son alternativas populares: **pnpm** es la más eficiente en espacio gracias a su uso de _symlinks_.
    
- Es fundamental usar **NVM** para gestionar distintas versiones de Node.js en diferentes proyectos.
    

---

Ahora que tenemos nuestro entorno base configurado, el siguiente paso es usar estas herramientas para iniciar un proyecto.

[[03 - Inicialización de proyectos]]