La velocidad que ganamos con el _Code Splitting_ debe ir acompañada de compatibilidad. Esta clase trata sobre cómo los **Transpiladores** y **Polyfills** nos permiten escribir código JavaScript moderno sin sacrificar a los usuarios de navegadores más antiguos.

---

# 15 - Transpiladores y compatibilidad

## 📝 Introducción

Escribimos código usando las últimas y mejores características de JavaScript (ESNext, TypeScript) porque son más productivas y elegantes. Sin embargo, no todos los navegadores entienden este código. Un **Transpilador** es la herramienta de _Build_ que traduce (compila) el código fuente de un lenguaje a otro del mismo nivel de abstracción. En frontend, esto significa traducir **JS Moderno** a **JS Antiguo (ES5)**.

El Transpilador más famoso es **Babel**, aunque los _bundlers_ modernos como Vite/esbuild a menudo usan sus propios motores más rápidos.

## 🧠 Teoría: Babel, ESNext y Polyfills

### 1. Babel: El Transpilador Clave

**Babel** es una herramienta esencial que toma el código de JavaScript de próxima generación (ES2015+, ESNext) y lo convierte en código compatible con versiones anteriores de JavaScript (generalmente ES5).

- **¿Qué Transpila?** Principalmente la **sintaxis**. Por ejemplo:
    
    - `const` y `let` a `var`.
        
    - Funciones de flecha (`=>`) a funciones tradicionales (`function () {}`).
        
    - Clases, _spread operators_ (`...`), _destructuring_, etc.
        
- **Plugins y Presets:** Babel opera mediante _plugins_ (que transpilan una característica específica, ej: `plugin-transform-arrow-functions`). Para no instalar cientos de _plugins_, usamos **Presets**, que son colecciones predefinidas:
    
    - **`@babel/preset-env`:** El _preset_ más importante. Transpila de forma inteligente solo las características necesarias basándose en los **navegadores que quieres soportar** (tu _target_).
        

### 2. Polyfills: Cubriendo la Funcionalidad Faltante

Un transpilador solo maneja la sintaxis. ¿Qué pasa si una funcionalidad de JavaScript simplemente no existe en el navegador antiguo (ej: `Promise`, `fetch`, `Array.prototype.includes`)?

Aquí es donde entran los **Polyfills**:

- **Propósito:** Son trozos de código JavaScript que **simulan o proporcionan** una funcionalidad moderna que falta en el entorno de ejecución antiguo. Piensa en ellos como parches.
    
- **Ejemplo:** Si un navegador no soporta `Promise`, el _polyfill_ es código que define el objeto `Promise` y recrea su comportamiento.
    
- **Herramienta Clave:** El paquete **`core-js`** es la fuente más común de _polyfills_ en el ecosistema.
    

### 3. TypeScript: La Capa de Tipado

**TypeScript (TS)** es un _superset_ de JavaScript que añade tipado estático. Antes de que el código llegue al _bundler_, debe ser transformado a JavaScript puro (eliminando el tipado).

- **Proceso de Transpilación de TS:**
    
    1. El código `.ts` pasa por el compilador de TypeScript (`tsc`) o un transpilador rápido (como **esbuild** o **SWC**).
        
    2. Se convierte en código `.js` limpio (ej: ESNext).
        
    3. Este código `.js` es luego alimentado a Babel o al _bundler_ para su transpilación final (si se necesita compatibilidad con navegadores antiguos).
        

> **Enfoque moderno (Vite):** Vite usa **esbuild** para transpilar TS a JS muy rápidamente y solo usa Babel si se requiere un _preset_ avanzado de Babel (ej: ciertas características de React).

---

## 🛠️ Ejemplos: Configuración de Compatibilidad

### Paso 1: Definir el Target de Navegadores (`browserslist`)

En lugar de decirle a Babel qué características transpolar, le decimos **qué navegadores debe soportar**. Esto se hace en el archivo `browserslist`.

**`.browserslistrc` (Archivo de configuración global)**

Ini, TOML

```
# Configuración común para un proyecto moderno:
> 0.2% and not dead 
last 2 versions
not ie <= 11 

# Explicación: 
# - Soporta navegadores con > 0.2% de uso global.
# - Excluye navegadores "muertos" (sin soporte o actualización).
# - Incluye las últimas 2 versiones de cada navegador.
# - Excluye explícitamente IE 11 y anteriores. 
```

Tanto `@babel/preset-env` como herramientas de _autoprefixing_ de CSS leen este archivo para optimizar el _build_ final.

### Paso 2: Configuración de Babel

Para usar Babel, primero instalamos los paquetes necesarios:

Bash

```
$ npm i -D @babel/core @babel/preset-env
```

**`.babelrc` o `babel.config.json`**

JSON

```
{
  "presets": [
    [
      "@babel/preset-env",
      {
        // Babel usa automáticamente el target de .browserslistrc, 
        // pero podemos optimizar el uso de polyfills:
        "useBuiltIns": "usage", 
        "corejs": 3 // Especifica que usaremos polyfills de core-js v3
      }
    ],
    // Si usas React, añadirías:
    // "@babel/preset-react" 
  ]
}
```

- **`useBuiltIns: "usage"`**: Esta es la optimización clave. En lugar de incluir **todos** los _polyfills_ posibles (aumentando enormemente el tamaño del _bundle_), Babel solo inyecta los _polyfills_ **necesarios** para el código que estás usando y para el _target_ de navegadores especificado.
    

### Paso 3: Transpilación de TypeScript (Vite/esbuild)

En un entorno **Vite**, no necesitas configurar Babel para TS. Vite usa **esbuild** para la transpilación de TS, que es casi instantánea:

Bash

```
# vite.config.js (Vite usa esbuild por defecto para TS/JSX sin configuración adicional)
import { defineConfig } from 'vite';

export default defineConfig({
  // No se requiere 'babel-loader' ni '@babel/preset-typescript'
  // Esbuild hace esto por nosotros, solo necesitamos el plugin del framework:
  plugins: [/* @vitejs/plugin-react */], 
});
```

---

## 🛑 Errores Comunes

|Error|Descripción|Solución|
|---|---|---|
|**"ReferenceError: Promise is not defined"**|El navegador antiguo no tiene soporte nativo para `Promise` y Babel no inyectó el _polyfill_.|Asegúrate de tener **`core-js`** instalado y que **`useBuiltIns: "usage"`** esté correctamente configurado en `@babel/preset-env`.|
|**Babel Ignora Archivos**|Los archivos de una dependencia de `node_modules` usan sintaxis moderna y rompen el _build_ antiguo, pero Babel solo transpila tu código.|Por defecto, Babel excluye `node_modules`. En Webpack, debes configurar `include` o `exclude` para forzar a Babel a transpolar esa dependencia problemática. En Vite/Rollup, usa `optimizeDeps.include` (para desarrollo).|
|**El bundle es grande por Polyfills**|Se están incluyendo demasiados _polyfills_.|Verifica que no estás usando **`useBuiltIns: "entry"`** (que incluye todo, sin importar el código usado) y que estás usando **`"usage"`**.|

Exportar a Hojas de cálculo

## 📚 Resumen de Puntos Clave

- **Transpiladores** (**Babel**, esbuild) convierten la **sintaxis** moderna (ESNext) en sintaxis compatible (ES5).
    
- **Polyfills** (**core-js**) simulan la **funcionalidad** moderna (`Promise`, `fetch`) que falta en navegadores antiguos.
    
- El archivo **`.browserslistrc`** define el _target_ de navegadores soportados, optimizando la transpilación y los _polyfills_.
    
- La mejor configuración es usar `@babel/preset-env` con **`useBuiltIns: "usage"`** para incluir solo los _polyfills_ que son estrictamente necesarios, reduciendo el tamaño del _bundle_.
    

---

La transpilación nos asegura la compatibilidad. En la próxima clase, exploraremos cómo integrar otras herramientas críticas de desarrollo (linters, formateadores) dentro de nuestros _builds_ usando el ecosistema de _plugins_.

[[16 - Plugins y ecosistema]]