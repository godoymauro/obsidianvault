Después de la introducción a los _bundlers_, es hora de conocer a la herramienta que ha transformado la velocidad del desarrollo frontend: **Vite**.

---

Aquí tienes los apuntes de la clase anterior: [[09 - Introducción a bundlers]]

# 10 - Vite

## 📝 Introducción

**Vite** (pronunciado /vit/ en francés, que significa "rápido") es un **Dev Server** y un **Build Tool** creado por Evan You (también creador de Vue.js). Su principal promesa es un desarrollo frontend **extremadamente rápido** y sin la complejidad de configuración de herramientas como Webpack.

Vite logra esta velocidad revolucionando el proceso de desarrollo al aprovechar los **Módulos Nativos de ES** (ESM) que ya están disponibles en los navegadores modernos.

## 🧠 Teoría: La Arquitectura de Velocidad de Vite

### 1. Desarrollo: No-Bundle con ESM Nativo

En modo desarrollo (`npm run dev`), Vite no hace un _bundle_ completo de tu aplicación. En su lugar:

- **Sirve el código fuente directamente:** El navegador utiliza `<script type="module">` para solicitar cada archivo JavaScript (`.js`, `.jsx`, `.ts`, etc.) individualmente.
    
- **Divide las dependencias:** Vite distingue entre tu código fuente (que cambia a menudo) y las dependencias de terceros (`node_modules`, que son estáticas). Las dependencias se pre-empaquetan usando **esbuild** (extremadamente rápido) y se sirven con encabezados de caché fuertes.
    
- **Hot Module Replacement (HMR) Instantáneo:** Cuando cambias un archivo, Vite solo necesita invalidar y servir ese módulo específico (y sus dependencias directas) al navegador, sin recargar la página completa. Esto es lo que lo hace **casi instantáneo**.
    

### 2. Producción: Bundling con Rollup

Para el build de producción (`npm run build`), Vite utiliza **Rollup**, un _bundler_ conocido por su eficiencia en la generación de librerías y su excelente **Tree Shaking** (eliminación de código muerto). Vite actúa como una capa de configuración sobre Rollup, lo que te permite aprovechar la optimización de Rollup sin tener que configurar Rollup directamente.

|Característica|Webpack (Tradicional)|Vite|
|---|---|---|
|**Inicio del Servidor**|Lento (espera el _bundling_ inicial)|**Instantáneo** (servidor sin _bundling_)|
|**HMR**|Lento/Moderado (necesita _rebuilds_ parciales)|**Ultra Rápido** (solo inyecta el módulo cambiado)|
|**Bundler de Producción**|Webpack|**Rollup** (configurado por Vite)|
|**Transpilación (Dev)**|Babel Loader (JS/TS)|**esbuild** (muy rápido)|

Exportar a Hojas de cálculo

---

## 🛠️ Ejemplos y Comandos Prácticos

### Paso 1: Instalación y Configuración de Proyectos

Vite ofrece una forma sencilla de iniciar un proyecto con _templates_ preconfigurados para los frameworks más populares.

Bash

```
# 1. Comando de inicialización (usa npm, yarn, o pnpm)
$ npm create vite@latest 

# 2. El comando te hará preguntas:
# Project name: my-vite-app
# Select a framework: (eliges react, vue, svelte, vanilla, etc.)
# Select a variant: (eliges JavaScript, TypeScript, etc.)
```

**Estructura de `package.json` (Ejemplo con Vite)**

Vite genera scripts estándar que son muy fáciles de entender:

JSON

```
{
  // ...
  "scripts": {
    "dev": "vite",        // Inicia el servidor de desarrollo ultrarrápido (No-Bundle)
    "build": "vite build",// Genera los bundles optimizados con Rollup
    "preview": "vite preview" // Sirve la versión de producción localmente para pruebas
  },
  "devDependencies": {
    "vite": "^5.0.0"
  }
}
```

### Paso 2: Desarrollo Rápido (HMR)

1. **Ejecutar el servidor de desarrollo:**
    
    Bash
    
    ```
    $ npm run dev 
    # Servidor iniciado en 50ms, gracias a esbuild y ESM nativo.
    ```
    
2. Mientras está corriendo, cambia un archivo (ej: `src/App.jsx`). Verás la actualización en el navegador **inmediatamente** sin recargar la página.
    

### Paso 3: Configuración Básica (`vite.config.js`)

Vite funciona _zero-config_ para la mayoría de los casos. Si necesitas personalizarlo, usas `vite.config.js` (o `.ts`).

JavaScript

```
// vite.config.js

import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react'; // Importamos un plugin para React

export default defineConfig({
  plugins: [react()], // Lo activamos
  server: {
    port: 3001, // Cambiar el puerto predeterminado (3000)
    open: true, // Abre automáticamente el navegador
  },
  build: {
    outDir: 'mi-dist-personalizado', // Cambia el nombre de la carpeta de salida (por defecto 'dist')
    sourcemap: true, // Genera sourcemaps para facilitar el debugging en producción
  }
});
```

### El Ecosistema de Plugins

El poder de Vite reside en su arquitectura de _plugins_ que extiende su funcionalidad (similar a Webpack _Loaders_ y _Plugins_). Los plugins usan la interfaz Rollup, lo que hace que sea retrocompatible con muchos _plugins_ de Rollup.

|Plugin Común|Función|
|---|---|
|**`@vitejs/plugin-react`**|Soporte para JSX y HMR para React.|
|**`@vitejs/plugin-vue`**|Soporte para Single File Components (`.vue`).|
|**`vite-plugin-pwa`**|Genera manifestos y service workers para PWA.|

Exportar a Hojas de cálculo

---

## 🛑 Errores Comunes

|Error|Descripción|Solución|
|---|---|---|
|**"Failed to resolve import..."**|El navegador (y Vite) no pueden encontrar el archivo importado. Ocurre si la ruta de importación es incorrecta o si olvidaste el sufijo `.js` en módulos nativos.|Verifica las rutas de `import` (deben ser relativas o absolutas, no solo el nombre del archivo si es local).|
|**HMR no funciona**|El código no se actualiza instantáneamente. Suele ser causado por un error de sintaxis en el código, o un plugin incompatible.|Revisa la consola del navegador para ver errores de sintaxis o desactiva _plugins_ para diagnosticar.|
|**Vite y Node.js Versiones**|Usas una versión de Node.js demasiado antigua (ej: Node 14 o anterior).|Asegúrate de usar una versión reciente de Node.js (actualmente LTS, ver [[02 - Instalación y configuración básica]] y usa NVM).|

Exportar a Hojas de cálculo

## 📚 Resumen de Puntos Clave

- **Vite** es un _bundler_ moderno que prioriza la velocidad de desarrollo.
    
- En **Desarrollo**, usa la arquitectura **No-Bundle** con **ESM Nativo** del navegador para un inicio de servidor casi instantáneo.
    
- Su **HMR** es extremadamente rápido porque solo procesa el módulo cambiado.
    
- En **Producción**, usa **Rollup** para el _bundling_ final optimizado.
    
- La configuración es mínima y se basa en el archivo **`vite.config.js`** y plugins para funcionalidad extendida.
    

---

Vite es la opción predilecta hoy para la mayoría de proyectos nuevos. Sin embargo, **Webpack** sigue siendo el estándar para configuraciones muy complejas y proyectos heredados. En la siguiente clase, desglosaremos a Webpack.

[[11 - Webpack]]