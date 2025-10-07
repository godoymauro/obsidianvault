¡Claro! Hemos dominado los scripts; ahora, la clase se centrará en la gestión avanzada de los paquetes con los tres titanes: **npm**, **Yarn**, y **pnpm**.

---

Aquí tienes los apuntes de la clase anterior: [[05 - Scripts npm]]

# 06 - Gestión de paquetes

## 📝 Introducción

Los gestores de paquetes (Package Managers) son más que simples herramientas de instalación. Son los guardianes de la consistencia de tu proyecto. En esta clase, compararemos los comandos equivalentes de **npm**, **Yarn** y **pnpm** y, lo más importante, entenderemos las diferencias estructurales clave que hacen a **pnpm** tan atractivo para el desarrollo moderno.

## 🧠 Teoría: El Problema de `node_modules`

Tradicionalmente, la carpeta **`node_modules`** ha sido notoria por dos problemas principales:

### 1. El Problema del Espacio (npm / Yarn V1)

Cada proyecto en tu máquina (`proyecto-A`, `proyecto-B`, `proyecto-C`) instala su propia copia de todas sus dependencias. Si los tres proyectos usan React, tendrás **tres copias completas** de React en tu disco duro. Esto consume gigabytes de espacio innecesariamente.

### 2. El Problema de las Dependencias Fantasma (npm / Yarn V1)

Ambos gestores usan una estructura de instalación **plana** (_flat hierarchy_). Para evitar la duplicación de código, `npm` y `Yarn` suben las sub-dependencias al nivel superior de `node_modules`.

- **Problema:** Tu código podría estar usando un paquete (ej: `lodash`) que **no declaraste** en tu `package.json`, pero que fue instalado en el nivel superior porque era una sub-dependencia de otra cosa. Esto se llama **Dependencia Fantasma** (_Phantom Dependency_). Si esa dependencia superior deja de necesitar `lodash`, tu proyecto se romperá en la siguiente instalación.
    

### La Solución de pnpm: Enlaces Simbólicos (Symlinks)

**pnpm (Performant npm)** resuelve ambos problemas utilizando un enfoque basado en un **almacén de contenido** (_Content-Addressable Store_) y **enlaces simbólicos** (_Symlinks_):

1. **Ahorro de Espacio:** Cada paquete se instala **una sola vez** en un almacén global en tu disco.
    
2. **Instalación Rápida:** Si otro proyecto necesita la misma versión, pnpm simplemente crea un enlace simbólico, evitando la descarga y la instalación.
    
3. **Prevención de Fantasmas:** La carpeta `node_modules` de tu proyecto solo contendrá **enlaces simbólicos** a los paquetes que declaraste **explícitamente** en `package.json`. No más dependencias fantasma.
    

---

## 🛠️ Ejemplos: Comandos Equivalentes

Aquí tienes una tabla de referencia rápida para las operaciones más comunes:

|Operación|npm|Yarn (Classic)|pnpm|
|---|---|---|---|
|**Instalar Dependencias**|`$ npm install`|`$ yarn` o `$ yarn install`|`$ pnpm install` o `$ pnpm i`|
|**Instalar Producción**|`$ npm i axios`|`$ yarn add axios`|`$ pnpm add axios`|
|**Instalar Desarrollo**|`$ npm i -D webpack`|`$ yarn add -D webpack`|`$ pnpm add -D webpack`|
|**Instalar Global**|`$ npm i -g nodemon`|`$ yarn global add nodemon`|`$ pnpm add -g nodemon`|
|**Actualizar a la última versión**|`$ npm update <paquete>`|`$ yarn upgrade <paquete>`|`$ pnpm update <paquete>`|
|**Eliminar Dependencia**|`$ npm uninstall <paquete>`|`$ yarn remove <paquete>`|`$ pnpm remove <paquete>`|
|**Ejecutar un Script**|`$ npm run dev`|`$ yarn dev`|`$ pnpm dev`|
|**Ejecutar Binario Local**|`$ npx eslint`|`$ yarn dlx eslint`|`$ pnpm dlx eslint`|

Exportar a Hojas de cálculo

> **NOTA SOBRE `npm run` vs. `yarn/pnpm run`:** Una ventaja de Yarn y pnpm es que a menudo omiten la palabra `run` para los scripts, haciendo comandos como `$ yarn dev` o `$ pnpm build` más concisos.

### Ejemplo de Migración Rápida

Si heredas un proyecto que usa `package-lock.json` (npm) y quieres migrar a `pnpm` por sus beneficios de velocidad y espacio:

1. **Eliminar artefactos antiguos:**
    
    Bash
    
    ```
    $ rm -rf node_modules package-lock.json # Borra los archivos de npm
    ```
    
2. **Instalar con pnpm (genera el nuevo lock file):**
    
    Bash
    
    ```
    $ pnpm install 
    # Esto genera pnpm-lock.yaml y crea la nueva node_modules basada en symlinks.
    ```
    
3. **Eliminar las dependencias fantasma (Auditoría):** pnpm ofrece un comando para escanear si estás usando dependencias que no están en tu `package.json` (fantasmas).
    
    Bash
    
    ```
    $ pnpm why # Te ayuda a entender la estructura de las dependencias.
    ```
    

### Ejemplo de Actualización

Imagina que quieres actualizar una dependencia a la versión **mayor** más reciente, ignorando los rangos (`^` o `~`) en tu `package.json`.

Bash

```
# Actualiza React a la versión más nueva, incluso si es una actualización Mayor (ej: v17 a v18)
$ npm install react@latest

# Equivalente en Yarn
$ yarn add react@latest

# Equivalente en pnpm
$ pnpm add react@latest
```

---

## 🛑 Errores Comunes y Troubleshooting

|Error|Descripción|Solución|
|---|---|---|
|**"ELIFECYCLE" o "Command failed"**|Común al ejecutar `$ npm install`. Generalmente indica que un script de instalación de una dependencia falló (a menudo en paquetes que compilan código nativo, como `node-sass`).|Busca la dependencia que falla en el _log_. Asegúrate de tener las herramientas de compilación nativas instaladas (ej: Visual Studio Build Tools en Windows).|
|**"EEXIST: file already exists..."**|Los _lock files_ o `node_modules` están corruptos o el sistema operativo está confundido (común al migrar gestores).|**Solución Nuclear:** Borra `node_modules` y **todos** los _lock files_ (`package-lock.json`, `yarn.lock`, `pnpm-lock.yaml`) y vuelve a ejecutar `install` con el gestor que desees usar.|
|**`pnpm` falla en un monorepo**|La estricta estructura de `pnpm` (que previene fantasmas) puede romper _builds_ diseñados para la estructura plana.|En el proyecto específico, considera añadir la dependencia faltante a `package.json` o, en casos extremos, usa las _hoist patterns_ de pnpm, aunque esto debilita su beneficio principal.|

Exportar a Hojas de cálculo

## 📚 Resumen de Puntos Clave

- **npm** es el estándar, **Yarn** es más rápido que el `npm` clásico, pero **pnpm** es el más moderno y eficiente.
    
- **pnpm** resuelve los problemas de la carpeta **`node_modules`** mediante un **almacén de contenido** global y **enlaces simbólicos**, ahorrando espacio y previniendo las **Dependencias Fantasma**.
    
- Los comandos para `add`, `remove` y `update` son consistentes entre los tres gestores, aunque la sintaxis de `npm` a menudo requiere _flags_ (`-D`, `-g`).
    
- Si un build falla, la **Solución Nuclear** (borrar `node_modules` y _lock files_) suele ser la forma más rápida de restaurar la coherencia.
    

---

Ahora que sabemos cómo gestionar los paquetes, estamos listos para resolver los inevitables problemas que surgen en proyectos complejos.

[[07 - Resolución de conflictos y troubleshooting]]