Hemos llegado al final de nuestro segundo nivel, cubriendo la gestión avanzada de paquetes. Ahora, es el momento de aprender cómo contribuir a la comunidad, publicando nuestro propio código.

---

Aquí tienes los apuntes de la clase anterior: [[07 - Resolución de conflictos y troubleshooting]]

# 08 - Publicación de paquetes

## 📝 Introducción

Publicar un paquete en el **npm Registry** transforma tu proyecto de ser solo una aplicación a ser una **librería** o **módulo** que otros desarrolladores pueden instalar y reutilizar. Esto es fundamental si trabajas en equipos con _monorepos_ (múltiples proyectos en un solo repositorio) o si deseas compartir tu código con el mundo _open source_.

## 🧠 Teoría: Requisitos y Proceso de Publicación

### 1. Requisitos Clave en `package.json`

Antes de publicar, tu manifiesto (`package.json`) debe estar impecable:

|Campo|Propósito|Ejemplo|
|---|---|---|
|**`name`**|**Nombre único** del paquete en el Registry.|`"@miusuario/utils-lib"`|
|**`version`**|La versión actual del paquete, siguiendo **SemVer**.|`"1.0.0"`|
|**`main`**|**Punto de entrada principal** del paquete (lo que se importa al usar `require()` o `import`).|`"dist/index.js"`|
|**`files`**|**Arreglo de archivos/carpetas** que se incluirán en el paquete subido. Es crucial para mantener el paquete pequeño.|`["dist", "README.md"]`|
|**`type` (Opcional)**|Define el sistema de módulos, si usas `ESM` (módulos de ES6) o `CommonJS`.|`"module"` o `"commonjs"`|

Exportar a Hojas de cálculo

### 2. El Campo `files` y Exclusiones

Cuando ejecutas `$ npm publish`, el gestor crea un archivo `.tgz` con el contenido del paquete. Por defecto, incluye casi todo, pero **excluye** automáticamente:

- `node_modules`
    
- Archivos de control de versiones (`.git`, `.svn`)
    
- Archivos de caché (`.npmrc`, `*.log`, etc.)
    
- Los _lock files_ (`package-lock.json`, etc.)
    

Sin embargo, para tener control, siempre se recomienda usar el campo **`files`** para solo incluir la carpeta de código **construido** (`dist` o `lib`) y la documentación.

### 3. El Flujo de Trabajo

El flujo de trabajo estándar para publicar es:

1. **Limpiar y Construir:** Ejecutar tu script de _build_ (`npm run build`) para generar el código de producción (minificado, transpiled) en la carpeta `dist`.
    
2. **Iniciar Sesión:** Autenticarte en el Registry.
    
3. **Incrementar Versión:** Usar `$ npm version` para actualizar la versión de forma segura (SemVer).
    
4. **Publicar:** Usar `$ npm publish`.
    

---

## 🛠️ Ejemplos: Versionado y Publicación

### Paso 1: Configurar el Entorno

Primero, asumiendo que tu código de desarrollo está en `src/` y el script de build lo pasa a `dist/index.js`.

**`package.json` (Ejemplo de Configuración)**

JSON

```
{
  "name": "mi-proyecto-moderno-lib",
  "version": "1.0.0",
  "description": "Librería de utilidades para un build limpio.",
  "main": "dist/index.js", // 👈 El punto de entrada es el archivo construido
  "scripts": {
    "build": "esbuild src/index.js --bundle --minify --outfile=dist/index.js",
    "prepublishOnly": "npm run build" // 👈 Hook: Asegura que el build se ejecute antes de publicar
  },
  "files": [
    "dist" // 👈 Solo sube la carpeta de código construido
  ],
  "license": "MIT"
}
```

### Paso 2: Autenticación

Necesitas tener una cuenta en [https://npmjs.com](https://npmjs.com) y estar autenticado en tu terminal.

Bash

```
# Inicia sesión en el npm Registry (te pedirá usuario, contraseña y 2FA)
$ npm login 

# Si quieres verificar que iniciaste sesión
$ npm whoami 
# Salida: tu_nombre_de_usuario
```

### Paso 3: Versionado Semántico y Publicación

La mejor práctica es usar el comando `npm version` para cambiar la versión; esto actualiza `package.json` y crea automáticamente un **commit y un tag de Git**.

|Comando|Acción|Ejemplo (`1.0.0` a...)|
|---|---|---|
|`$ npm version patch`|Incrementa el PARCHE (Fix de Bug).|`1.0.1`|
|`$ npm version minor`|Incrementa el MENOR (Nueva Feature Compatible).|`1.1.0`|
|`$ npm version major`|Incrementa el MAYOR (Cambio que Rompe).|`2.0.0`|

Exportar a Hojas de cálculo

**Secuencia de Publicación:**

1. **Incrementar la versión (ej. un fix):**
    
    Bash
    
    ```
    $ npm version patch 
    # Actualiza package.json a 1.0.1, crea commit y tag v1.0.1.
    ```
    
2. **Publicar:**
    
    Bash
    
    ```
    $ npm publish 
    # El hook 'prepublishOnly' se ejecuta automáticamente aquí (npm run build).
    # Luego, el contenido del paquete (solo la carpeta 'dist') se sube al Registry.
    ```
    

### Paso 4: Publicación de Paquetes Privados o con Ámbito (Scoped Packages)

Para evitar colisiones de nombres o si trabajas con paquetes internos, puedes usar **ámbitos** (_scopes_).

- **Nombre:** Se usa el formato `@nombre-usuario/nombre-paquete`.
    
- **Publicación:** Si usas un ámbito, por defecto es privado (pagado en npm). Para publicar tu código _open source_ con ámbito:
    

Bash

```
# Publica un paquete con ámbito como público (gratuito)
$ npm publish --access public
```

---

## 🛑 Errores Comunes

|Error|Descripción|Solución|
|---|---|---|
|**`403 Forbidden - You cannot publish over the previously published version`**|Intentas publicar un paquete que ya tiene esa versión (ej: `1.0.0` ya existe).|Incrementa la versión usando `$ npm version <patch/minor/major>`.|
|**`403 Forbidden - You must sign up for a private package`**|Intentas publicar un paquete con ámbito (`@tuuser/paquete`) sin el _flag_ `--access public`.|Si es _open source_, usa `$ npm publish --access public`. Si es privado, configura un registro privado.|
|**`'dist/index.js' not found`**|Tu script de _build_ falló o no generó el archivo de entrada (`main`).|Verifica el script `build` y la ruta en el campo `main` de `package.json`. Asegúrate de que `prepublishOnly` se haya ejecutado.|
|**Subiste el paquete con `node_modules`**|El paquete es enorme porque incluiste accidentalmente dependencias.|**Solución:** Agrega la carpeta `node_modules` a un archivo `.npmignore` (similar a `.gitignore`) o usa el campo **`files`** para restringir explícitamente el contenido.|

Exportar a Hojas de cálculo

## 📚 Resumen de Puntos Clave

- El **`package.json`** requiere **`name`**, **`version`**, **`main`** y el campo **`files`** (para restringir el contenido) antes de la publicación.
    
- Usa el script **`prepublishOnly`** para asegurarte de que el código de producción se haya generado con **`npm run build`** justo antes de subirlo.
    
- Siempre usa **`$ npm version patch/minor/major`** para incrementar la versión de forma segura, respetando SemVer y creando tags de Git.
    
- Los paquetes con ámbito (`@user/pkg`) son privados por defecto; usa **`--access public`** para liberarlos.
    

---

¡Felicidades! Has completado el Nivel 2. Hemos cubierto todo lo esencial sobre **Gestores de Paquetes**. Ahora, nos adentraremos en el corazón de la modernidad frontend: los **Bundlers modernos**.

[[09 - Introducción a bundlers]]