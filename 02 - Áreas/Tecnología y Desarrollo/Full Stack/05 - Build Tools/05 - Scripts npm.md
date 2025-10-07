Ya tienes el entorno base y sabes gestionar las dependencias. Ahora, aprenderemos a automatizar la ejecución de tareas repetitivas utilizando el músculo de los gestores de paquetes: los **Scripts npm**.

---

Aquí tienes los apuntes de la clase anterior: [[04 - Conceptos claves]]

# 05 - Scripts npm

## 📝 Introducción

En el desarrollo profesional, rara vez ejecutas comandos de _build_ o _test_ largos y complejos directamente en la terminal. En su lugar, defines **Scripts npm** en tu `package.json`. Los scripts son **atajos** y una capa de abstracción que te permiten encapsular comandos complejos, asegurar la portabilidad y mejorar la legibilidad de las tareas de tu proyecto.

## 🧠 Teoría: Anatomía de un Script

Un Script npm es un par clave-valor dentro del objeto `"scripts"` de tu `package.json`. La **clave** es el nombre corto que usarás para ejecutarlo, y el **valor** es el comando real que se ejecuta en el shell de tu sistema operativo.

### 1. El Entorno de Ejecución

Cuando ejecutas `$ npm run <script>`, `npm` hace dos cosas clave:

1. **Ejecuta en el Shell:** El comando se ejecuta como si lo hubieras escrito directamente en tu terminal (CMD, Bash, Zsh).
    
2. **Añade `node_modules/.bin` al $PATH:** Esto es vital. Significa que **puedes llamar a ejecutables** instalados como dependencias de desarrollo (como `webpack`, `vite`, `jest`, `eslint`) **sin necesidad de instalarlos globalmente** (`-g`).
    

### 2. Comandos Reservados

Hay algunos nombres de scripts que tienen un comportamiento especial y se pueden ejecutar sin la palabra `run`:

|Script|Propósito Común|Ejecución|
|---|---|---|
|**`start`**|Iniciar la aplicación en modo desarrollo.|`$ npm start`|
|**`test`**|Ejecutar la suite de pruebas.|`$ npm test` o `$ npm t`|
|**`install`**|Ejecutar después de que `npm install` complete.|`$ npm install`|
|**`pre*` y `post*`**|Ganchos que se ejecutan **antes** o **después** de un script nombrado (ej: `prebuild`, `postbuild`).|Se ejecutan automáticamente al correr `npm run build`|

Exportar a Hojas de cálculo

---

## 🛠️ Ejemplos y Comandos Prácticos

### Ejemplo 1: El Script Básico

Supongamos que instalas un transpilador simple llamado `esbuild` como `devDependency`.

**Instalación:**

Bash

```
$ npm i -D esbuild
```

**`package.json`**

JSON

```
{
  // ...
  "scripts": {
    "build": "esbuild src/index.js --bundle --minify --outfile=dist/bundle.js",
    "dev": "node index.js"
  },
  // ...
}
```

**Ejecución:**

Bash

```
# 1. Ejecutando la tarea de 'build' compleja:
$ npm run build
# El sistema encuentra el ejecutable 'esbuild' en node_modules/.bin y lo ejecuta.

# 2. Ejecutando el script 'dev' (que es un comando reservado):
$ npm start
# O
$ npm run dev
```

### Ejemplo 2: Combinación y Encadenamiento de Scripts

Puedes encadenar scripts usando operadores estándar del shell:

|Operador|Significado|Uso|
|---|---|---|
|**`&&`**|**Y lógico (Secuencial)**: Ejecuta el siguiente comando **solo si el anterior tuvo éxito** (código de salida 0).|Limpiar **y luego** construir.|
|**`&`**|**Paralelo**: Ejecuta los comandos al mismo tiempo (útil para varios _watchers_).|Iniciar servidor API **y** el servidor frontend.|

Exportar a Hojas de cálculo

JSON

```
// package.json (Encadenamiento)
{
  "scripts": {
    "clean": "rm -rf dist", // Borra la carpeta de destino (usa 'rimraf dist' para mejor compatibilidad en Windows)
    "build:js": "esbuild src/main.js --bundle --outfile=dist/main.js",
    "build:css": "node-sass src/styles.scss -o dist/styles.css",
    
    // Script que limpia, luego ejecuta JS, y luego ejecuta CSS.
    "build:all": "npm run clean && npm run build:js && npm run build:css", 
    
    // Ejecuta el build y luego, si fue exitoso, notifica.
    "postbuild:all": "echo '✅ Build completado exitosamente.'", 

    // Scripts en paralelo (requiere un paquete como concurrently para manejo robusto)
    "dev": "npm run start:frontend & npm run start:backend" 
  }
}
```

### Ejemplo 3: Usando Variables de Entorno

Puedes pasar información de configuración (como el entorno) a los comandos usando variables de entorno.

**Compatibilidad:**

- **macOS/Linux:** Usa la sintaxis estándar `VAR=valor comando`.
    
- **Windows:** Usa `set VAR=valor && comando` (o mejor, usa librerías como `cross-env` para compatibilidad cruzada).
    

**Usando `cross-env` (Recomendado para la portabilidad):**

1. Instala: `$ npm i -D cross-env`
    

**`package.json`**

JSON

```
{
  "scripts": {
    // La variable NODE_ENV se establece como 'production' sin importar el SO.
    "build": "cross-env NODE_ENV=production webpack --config webpack.config.js",
    
    // La variable NODE_ENV se establece como 'development'.
    "dev": "cross-env NODE_ENV=development vite dev",
    
    // El comando de Vite o Webpack puede leer esta variable y ajustar su comportamiento.
    "test:coverage": "cross-env COVERAGE=true jest"
  }
}
```

---

## 🛑 Errores Comunes

|Error|Descripción|Solución|
|---|---|---|
|**"command not found" (en un script)**|Estás intentando ejecutar un binario (como `jest` o `webpack`) que está instalado localmente, pero no estás usando `npm run` o estás fuera del contexto de `package.json`.|Siempre ejecuta con `$ npm run <script>` para asegurar que `node_modules/.bin` esté en el $PATH.|
|**El comando no se ejecuta en Windows**|Estás usando comandos de shell de Linux (ej: `rm -rf`) directamente en un `scripts` y tu máquina es Windows.|Usa librerías _cross-platform_ como `cross-env` para variables o `rimraf` para comandos de archivos, o usa el shell de git bash si está disponible.|
|**El script se detiene a mitad de camino**|Usaste el operador `&` (paralelo) donde debiste usar `&&` (secuencial).|Revisa si la segunda tarea depende de que la primera termine exitosamente. Si es así, usa `&&`.|

Exportar a Hojas de cálculo

## 📚 Resumen de Puntos Clave

- Los **Scripts npm** (o Scripts Yarn/pnpm) son la forma estándar de automatizar tareas en un proyecto JS.
    
- `npm` automáticamente añade los binarios de las dependencias locales (`node_modules/.bin`) al `PATH` durante la ejecución del script.
    
- Los scripts se pueden **encadenar secuencialmente** con `&&` o **en paralelo** con `&`.
    
- Utiliza librerías como **`cross-env`** para gestionar variables de entorno de forma compatible entre diferentes sistemas operativos (macOS, Linux, Windows).
    

---

Ahora que sabemos cómo definir y automatizar tareas, la siguiente clase se centrará en el uso avanzado de los gestores: cómo instalar, actualizar y migrar entre ellos.

[[06 - Gestión de paquetes]]