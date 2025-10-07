Ya que sabemos gestionar e instalar paquetes, es hora de enfrentar la realidad: los problemas surgen. Esta clase te equipará con las herramientas y el conocimiento para diagnosticar y solucionar los fallos más comunes en la gestión de paquetes.

---

Aquí tienes los apuntes de la clase anterior: [[06 - Gestión de paquetes]]

# 07 - Resolución de conflictos y troubleshooting

## 📝 Introducción

En cualquier proyecto de gran tamaño, experimentarás fallos en la instalación, conflictos de versiones o builds inconsistentes. El _troubleshooting_ (resolución de problemas) eficaz es una habilidad de _Build Tools_ de nivel experto. Nos centraremos en tres áreas clave: errores de instalación (**npm ERR!**), conflictos de versiones (`lock file` desincronizado) y problemas de caché.

## 🧠 Teoría: Causas Raíz de los Fallos

### 1. El Famoso Error `npm ERR!`

La mayoría de los errores de instalación se resumen en tres categorías:

- **Permisos (`EACCES`, `EPERM`):** El gestor de paquetes no tiene permiso para escribir en ciertas carpetas (generalmente al instalar globalmente o en Windows por bloqueo de proceso).
    
- **Archivos Nativos Fallidos (`node-gyp`):** Algunas dependencias (como `node-sass`, `sqlite3` o ciertas herramientas de _cifrado_) contienen código C++ o nativo que necesita ser compilado en tu máquina. Si te faltan las herramientas de compilación (`C++ Build Tools`), la instalación fallará.
    
- **Red / Cache Corrupto:** Fallos temporales en la conexión al registro de npm o datos corruptos almacenados localmente.
    

### 2. Lock Files Desincronizados

Este es un problema común en equipos. Ocurre cuando:

- Un desarrollador actualiza una dependencia y sube solo el `package.json` **sin el `lock file`** (`package-lock.json`, `yarn.lock`, o `pnpm-lock.yaml`).
    
- Otro desarrollador ejecuta `install`, y el gestor de paquetes (siguiendo los rangos de SemVer en `package.json`) instala una **versión diferente** de la sub-dependencia, rompiendo el _build_.
    

**Síntoma:** "Funciona en mi máquina, pero no en el CI/CD."

---

## 🛠️ Ejemplos y Técnicas de Troubleshooting

### Técnica 1: La Solución Nuclear 💥

Cuando el `node_modules` y el _lock file_ están irremediablemente corruptos o el build es inconsistente, aplica esta secuencia de comandos. Es el _reset_ más efectivo y garantiza una instalación limpia.

Bash

```
# 1. Borra la carpeta de dependencias
$ rm -rf node_modules 
# TIP: En Windows, si rm -rf falla, bórrala manualmente o usa 'rimraf node_modules' (si lo tienes instalado)

# 2. Borra el lock file (elige el que corresponda a tu proyecto)
$ rm package-lock.json # o yarn.lock, o pnpm-lock.yaml

# 3. Limpia la caché global del gestor
$ npm cache clean --force # Para npm
# $ yarn cache clean       # Para yarn (en V1)
# $ pnpm store prune      # Para pnpm (limpia paquetes no referenciados)

# 4. Vuelve a instalar todo desde cero
$ npm install # o yarn install, o pnpm install
```

### Técnica 2: Resolver Errores de Permisos (`EACCES`)

Si el error `EACCES` ocurre al instalar algo **globalmente** (`-g`):

Bash

```
# Usa sudo para la instalación global (solo en sistemas Unix/Linux/macOS)
$ sudo npm install -g <paquete> 
```

> **Nota:** La mejor práctica es **no** usar instalaciones globales siempre que sea posible, confiando en los binarios locales de `$ npm run` (ver [[05 - Scripts npm]]).

### Técnica 3: Diagnosticar Conflictos de Versión

Si una dependencia está causando problemas de compatibilidad y necesitas saber **qué paquete la está requiriendo**, usa el comando de auditoría o `ls`.

|Gestor|Comando para Auditoría y Búsqueda|
|---|---|
|**npm**|`$ npm why <paquete-problemático>`|
|**Yarn**|`$ yarn why <paquete-problemático>`|
|**pnpm**|`$ pnpm why <paquete-problemático>`|

Exportar a Hojas de cálculo

**Ejemplo:** Sabes que `lodash@3.0.0` está causando un error, pero tú nunca la instalaste directamente.

Bash

```
$ npm why lodash
# ... npm te mostrará que 'tu-dependencia-principal@1.5.0' requiere 'lodash@3.0.0'.
# Solución: Actualiza o reemplaza 'tu-dependencia-principal' o usa 'resolutions' (ver más abajo).
```

### Técnica 4: Forzar una Versión con "Resolutions"

Cuando no puedes actualizar la dependencia principal, pero sabes que la sub-dependencia tiene una versión segura (ej: `lodash@4.17.21`), puedes obligar al gestor a usar esa versión.

- Esta técnica anula el comportamiento normal del gestor.
    

**`package.json`**

JSON

```
{
  // ...
  "resolutions": {
    "lodash": "4.17.21" 
    // O "**/lodash": "4.17.21" para Yarn y pnpm, forzando la resolución en cualquier nivel
  }
}
```

> **Nota:** Este campo se llama `overrides` en npm v8+ y `resolutions` en Yarn y pnpm. En la práctica, obliga al gestor a usar la versión especificada.

---

## 🛑 Errores Comunes y Soluciones

|Error o Síntoma|Causa Probable|Solución|
|---|---|---|
|**`npm ERR! code 1` o `node-gyp error`**|Fallo al compilar código nativo (falta C++ Build Tools).|**Windows:** Instala Visual Studio Build Tools. **Linux/macOS:** Asegúrate de tener GCC/Clang y Python.|
|**`peer dependency violation`**|Tu librería tiene un `peerDependency` (ej: React v18), pero el proyecto principal usa una versión incompatible (ej: React v16).|Actualiza la versión de la dependencia principal en el proyecto o usa la _flag_ `--legacy-peer-deps` (temporalmente peligroso).|
|**Versiones inconsistentes en el log**|El `lock file` no está en el repositorio o está desactualizado.|Asegúrate de que el _lock file_ se incluya en git. Ejecuta la **Solución Nuclear** y sube el nuevo _lock file_.|
|**Cache corrupto**|npm o Yarn están usando datos desactualizados o incompletos del disco.|Ejecuta el comando de limpieza de caché (`$ npm cache clean --force`).|

Exportar a Hojas de cálculo

## 📚 Resumen de Puntos Clave

- La mayoría de los fallos de instalación se deben a **permisos**, falta de **herramientas de compilación nativas** o **cache corrupto**.
    
- La **Solución Nuclear** (`rm -rf node_modules`, borrar `lock file`, limpiar caché e instalar) resuelve la mayoría de los problemas de inconsistencia.
    
- Usa `$ npm why` (o equivalente) para diagnosticar qué dependencia está requiriendo un paquete problemático.
    
- El campo `resolutions`/`overrides` permite **forzar una versión específica** de una sub-dependencia para mitigar conflictos complejos.
    

---

Con estas técnicas de _troubleshooting_ en mano, hemos cubierto la gestión de paquetes a fondo. La próxima clase es el paso final para cualquier desarrollador que quiera contribuir al ecosistema: publicar sus propios paquetes.

[[08 - Publicación de paquetes]]