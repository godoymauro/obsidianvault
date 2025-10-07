# 00 - MOC (Map of Content)

Este es el índice maestro del módulo **Build Tools**. Úsalo para navegar a cualquier clase, o simplemente sigue el orden secuencial.

## Nivel 1: Introducción y conceptos básicos

|Clase|Título|Descripción|
|---|---|---|
|[[01 - Introducción a Build Tools]]|Introducción a Build Tools|Qué son, para qué sirven, diferencias (moderno vs. legacy), ventajas de usar un bundler.|
|[[02 - Instalación y configuración básica]]|Instalación y configuración básica|Node.js, npm, yarn, pnpm. Configuración de entornos.|
|[[03 - Inicialización de proyectos]]|Inicialización de proyectos|`npm init`, `yarn init`, `pnpm init`, estructura de `package.json`, scripts iniciales.|
|[[04 - Conceptos claves]]|Conceptos clave|Dependencias (`dependencies`, `devDependencies`, `peerDependencies`), versionado semántico, lock files.|
## Nivel 2: npm / Yarn / pnpm

|Clase|Título|Descripción|
|---|---|---|
|[[05 - Scripts npm]]|Scripts npm|Crear scripts, combinarlos, usar variables de entorno.|
|[[06 - Gestión de paquetes]]|Gestión de paquetes|Instalar, actualizar, eliminar. Diferencias prácticas entre npm, yarn y pnpm.|
|[[07 - Resolución de conflictos y troubleshooting]]|Resolución de conflictos y troubleshooting|Errores comunes (`npm ERR!`, lockfile desincronizado, cache corrupto).|
|[[08 - Publicación de paquetes]]|Publicación de paquetes|Crear y publicar paquetes en npm registry. Versionado y buenas prácticas.|
## Nivel 3: Bundlers modernos

| Clase                            | Título                  | Descripción                                                                                |
| -------------------------------- | ----------------------- | ------------------------------------------------------------------------------------------ |
| [[09 - Introducción a bundlers]] | Introducción a bundlers | Qué hace un bundler. Diferencias entre Vite, Webpack, esbuild, Parcel y Snowpack.          |
| [[10 - Vite]]                    | Vite                    | Instalación, configuración, plugins, desarrollo rápido, HMR.                               |
| [[11 - Webpack]]                 | Webpack                 | Conceptos clave: `entry`, `output`, `loaders`, `plugins`. Configuración básica y avanzada. |
| [[12 - Parcel]]                  | Parcel                  | Configuración zero-config, ventajas y limitaciones.                                        |
| [[13 - esbuild y Snowpack]]      | esbuild y Snowpack      | Velocidad, bundling y build scripts, comparación con Vite y Webpack.                       |
## Nivel 4: Configuración avanzada y optimización

|Clase|Título|Descripción|
|---|---|---|
|[[14 - Code splitting y lazy loading]]|Code splitting y lazy loading|Cómo mejorar performance en proyectos grandes. Ejemplos en frameworks.|
|[[15 - Transpiladores y compatibilidad]]|Transpiladores y compatibilidad|Babel, TypeScript, ESNext, polyfills.|
|[[16 - Plugins y ecosistema]]|Plugins y ecosistema|Plugins de Webpack y Vite, integración con linters, formateadores y testing.|
|[[17 - Optimización de producción]]|Optimización de producción|Minificación, tree shaking, caching, bundle analyzer.|
|[[18 - Configuración de entornos]]|Configuración de entornos|Dev, staging, production; variables de entorno y configuración múltiple.|
## Nivel 5: Proyecto guiado

| Clase                                          | Título                       | Descripción                                                                                                   |
| ---------------------------------------------- | ---------------------------- | ------------------------------------------------------------------------------------------------------------- |
| [[19 - Proyecto guiado SPA moderna]]           | Proyecto guiado: SPA moderna | Crear un proyecto React/Vue/Svelte con build tools, scripts, bundling y lazy loading.                         |
| [[20 - Proyecto final avanzado - Build Tools]] | Proyecto final avanzado      | Build completo, despliegue a producción, configuración de CI/CD, testing de bundles, análisis de performance. |

---
