La optimización de producción es excelente, pero no es la única configuración que necesitamos. Un proyecto profesional debe poder ajustarse a múltiples entornos: desde el desarrollo local hasta el despliegue final.

---

# 18 - Configuración de entornos

## 📝 Introducción

En un proyecto real, la aplicación se comporta de manera diferente en distintos **entornos**:

1. **Development (Dev):** Enfocado en la velocidad, _debugging_ y _Hot Module Replacement_ (HMR).
    
2. **Staging (Pre-producción):** Un espejo de Producción, usado para pruebas finales de control de calidad (QA).
    
3. **Production (Prod):** El entorno final, enfocado en la optimización, _caching_ y seguridad.
    

La **Configuración Múltiple** y el uso de **Variables de Entorno** son las técnicas que nos permiten gestionar estas diferencias de forma segura y eficiente.

## 🧠 Teoría: Variables y Configuración Condicional

### 1. Variables de Entorno (Environment Variables)

Las variables de entorno son la forma estándar de inyectar valores de configuración externos a nuestro código, sin _hardcodearlos_.

- **Uso Principal:** Almacenar claves secretas (API Keys), URLs de servicios (URL de la API de desarrollo vs. producción) o banderas de configuración.
    
- **Acceso:** En Node.js, se accede a ellas a través de `process.env.<NOMBRE>`. En el código _frontend_ (navegador), deben ser _inyectadas_ por el _bundler_.
    
- **El Problema de la Exposición:** Nunca debes inyectar secretos (_API Keys_) que puedan exponerse en el código de producción. Por convención, las variables que se inyectan en el _bundle_ (y que, por lo tanto, son visibles en el navegador) suelen prefijarse (ej: `VITE_`, `NEXT_PUBLIC_`, `REACT_APP_`).
    

### 2. Gestión de Entornos Específicos

Los _build tools_ manejan la configuración condicional de dos maneras:

|Técnica|Descripción|Herramientas Típicas|
|---|---|---|
|**Configuraciones Múltiples**|Usar diferentes archivos de configuración (`webpack.dev.js`, `webpack.prod.js`) o lógica condicional dentro del archivo de configuración.|Webpack, Rollup|
|**Archivos `.env`**|Usar archivos de texto simple (`.env`, `.env.production`) para definir pares clave-valor. El _bundler_ o una librería (como `dotenv`) los carga automáticamente.|Vite, Create React App, Next.js|

Exportar a Hojas de cálculo

---

## 🛠️ Ejemplos Prácticos

### Ejemplo 1: Variables de Entorno en Vite

Vite utiliza archivos `.env` y expone las variables con el prefijo `VITE_`.

1. **Archivos `.env`:**
    
    Bash
    
    ```
    # .env.development 👈 Usado solo por 'npm run dev'
    VITE_API_URL=http://localhost:3000/api
    VITE_DEBUG_MODE=true
    
    # .env.production 👈 Usado solo por 'npm run build'
    VITE_API_URL=https://api.myapp.com/api
    VITE_DEBUG_MODE=false
    ```
    
2. **Acceso en el código JS/TS:**
    
    JavaScript
    
    ```
    // src/main.js
    console.log(`Modo Debug: ${import.meta.env.VITE_DEBUG_MODE}`);
    
    // fetch(import.meta.env.VITE_API_URL + '/users');
    
    // Nota: import.meta.env.MODE siempre es 'development' o 'production'
    if (import.meta.env.MODE === 'development') {
        // Ejecuta código específico de desarrollo (ej: logging verboso)
    }
    ```
    

### Ejemplo 2: Configuración Múltiple en Webpack

En Webpack, a menudo se usa un archivo base y se fusiona con archivos específicos para cada modo.

1. **Estructura de Archivos:**
    
    - `webpack.common.js` (Reglas compartidas: Loaders, Babel)
        
    - `webpack.dev.js` (Reglas de desarrollo: `devServer`, _sourcemaps_ rápidos)
        
    - `webpack.prod.js` (Reglas de producción: Minificación, _hashing_, _sourcemaps_ optimizados)
        
2. **Uso de la Herramienta `webpack-merge`:**
    
    Bash
    
    ```
    $ npm i -D webpack-merge 
    ```
    
3. **`webpack.dev.js` (Ejemplo de Fusión):**
    
    JavaScript
    
    ```
    const { merge } = require('webpack-merge');
    const common = require('./webpack.common.js');
    
    module.exports = merge(common, {
      mode: 'development',
      devtool: 'eval-source-map', // Sourcemap rápido para desarrollo
      devServer: {
        port: 8080,
        hot: true, // Habilitar HMR
      },
      // Plugin para inyectar variables de entorno (simula el comportamiento de Vite)
      plugins: [
        new webpack.DefinePlugin({
          'process.env.API_URL': JSON.stringify('http://localhost:8080/dev-api'),
        }),
      ],
    });
    ```
    
4. **Ejecución con Scripts npm:** Se usa la _flag_ `--config` para apuntar al archivo correcto.
    
    JSON
    
    ```
    // package.json
    "scripts": {
      "start": "webpack serve --config webpack.dev.js",
      "build": "webpack --config webpack.prod.js"
    }
    ```
    

### Ejemplo 3: Uso de `cross-env` para Scripts Simples

Si solo necesitas establecer la bandera `NODE_ENV`, usa `cross-env` (visto en [[05 - Scripts npm]]):

JSON

```
// package.json
"scripts": {
  "build:staging": "cross-env NODE_ENV=staging webpack --config webpack.prod.js",
  "build:prod": "cross-env NODE_ENV=production webpack --config webpack.prod.js"
}
```

- **Nota:** Webpack lee la variable `NODE_ENV` para establecer su modo (`development` o `production`), lo cual activa optimizaciones específicas.
    

---

## 🛑 Errores Comunes

|Error|Descripción|Solución|
|---|---|---|
|**"process is not defined"**|Estás usando `process.env.<VAR>` en frontend sin que el _bundler_ haya inyectado o reemplazado ese valor.|Asegúrate de usar el plugin de inyección de variables (ej: `DefinePlugin` en Webpack o la sintaxis `import.meta.env` en Vite).|
|**Claves Secretas Expuestas**|Una clave de API de producción aparece en el _bundle_ final.|**¡CRÍTICO!** Renombra las variables sensibles para que no tengan el prefijo de exposición (`VITE_`, `REACT_APP_`). Solo usa variables prefijadas para valores que deban ser públicos.|
|**Carga de `.env` incorrecta**|Se carga el archivo `.env.development` al hacer _build_ de producción.|Verifica la configuración de tu _bundler_ o librería `dotenv`. Vite lo maneja automáticamente, pero en Webpack debes asegurarte de que tu `webpack.prod.js` use los valores correctos de producción.|

Exportar a Hojas de cálculo

## 📚 Resumen de Puntos Clave

- Los entornos (`dev`, `staging`, `prod`) requieren configuraciones diferentes para optimizar ya sea la **velocidad** (dev) o el **rendimiento** (prod).
    
- Las **Variables de Entorno** son el mecanismo para inyectar configuraciones (APIs, URLs) de forma segura.
    
- Usa **Variables Prefijadas** (ej: `VITE_`, `REACT_APP_`) para los valores que son seguros inyectar en el código del navegador.
    
- La configuración múltiple se logra mediante:
    
    - **Archivos `.env` específicos** (Vite, CRA).
        
    - **Fusión de configuraciones** (`webpack-merge`) basadas en el modo de ejecución.
        

---

Hemos cubierto todos los aspectos teóricos y de optimización de los _Build Tools_. ¡Felicidades! Ahora estamos listos para aplicar todo este conocimiento en un proyecto real.

[[19 - Proyecto guiado SPA moderna]]