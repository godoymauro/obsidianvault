Hemos visto el futuro con Vite, ahora volvemos al estándar de la industria que lo hizo posible: **Webpack**. Entender Webpack es esencial para trabajar en la mayoría de los proyectos empresariales y heredados.

---

Aquí tienes los apuntes de la clase anterior: [[10 - Vite]]

# 11 - Webpack

## 📝 Introducción

**Webpack** es un _bundler_ de módulos estático para aplicaciones JavaScript. Durante muchos años, fue la solución definitiva al problema de la modularización y la optimización en frontend. A diferencia de Vite, Webpack sigue una arquitectura **"Bundle-First"** (primero empaqueta) y requiere una configuración explícita, pero ofrece un control y una flexibilidad sin igual en proyectos complejos.

## 🧠 Teoría: Los 4 Conceptos Clave de Webpack

Webpack no es una herramienta _zero-config_; requiere un archivo de configuración (`webpack.config.js`). Dominar esta configuración se reduce a entender cuatro pilares fundamentales: **Entry, Output, Loaders y Plugins.**

### 1. Entry (Entrada)

- **Propósito:** Le dice a Webpack dónde debe comenzar a construir el **Grafo de Dependencias** de la aplicación.
    
- **Concepto:** Webpack comienza a resolver los módulos a partir de este punto. Puede haber múltiples puntos de entrada (útil para generar _bundles_ separados).
    

### 2. Output (Salida)

- **Propósito:** Le dice a Webpack dónde emitir los _bundles_ y cómo nombrarlos.
    
- **Propiedades Clave:**
    
    - **`path`**: Ruta absoluta donde se guardarán los archivos.
        
    - **`filename`**: Patrón de nombres para los _bundles_ (ej: `bundle.js`, `[name].js`, `[name].[contenthash].js`).
        

### 3. Loaders (Cargadores)

- **Propósito:** Webpack solo entiende JavaScript por defecto. Los **Loaders** (traductores) le enseñan a Webpack a procesar otros tipos de archivos (CSS, imágenes, SASS, TypeScript, JSX) y convertirlos en módulos válidos de JavaScript o activos que puedan ser incluidos en el _bundle_.
    
- **Flujo:** Los _loaders_ se aplican **de derecha a izquierda** o **de abajo hacia arriba** en la configuración.
    
    - _Ejemplo:_ Para un archivo CSS, usarías primero el `css-loader` (para entender los `import 'style.css'`) y luego el `style-loader` (para inyectar ese CSS en el DOM).
        

### 4. Plugins

- **Propósito:** Los _Loaders_ actúan en el **nivel de módulo individual** (archivo por archivo). Los **Plugins** actúan en el **nivel de _bundle_** o en el proceso de compilación completo.
    
- **Ejemplos Típicos:** Minificación de la salida, manejo de variables de entorno, generación de un archivo `index.html` (para inyectar los _bundles_ automáticamente), o análisis de los _bundles_.
    

---

## 🛠️ Ejemplos: Configuración y Comandos

### Instalación Básica

Necesitas el core de Webpack y la herramienta para ejecutarlo desde la línea de comandos.

Bash

```
# Instalación como dependencias de desarrollo
$ npm i -D webpack webpack-cli
```

### Ejemplo de Configuración Esencial (`webpack.config.js`)

Este es un ejemplo que define la entrada, salida y un _loader_ básico para JS moderno (Babel).

JavaScript

```
// webpack.config.js
const path = require('path');

module.exports = {
  // 1. ENTRY: Donde Webpack comienza
  entry: './src/index.js', 
  
  // 2. OUTPUT: Donde Webpack guarda los archivos
  output: {
    path: path.resolve(__dirname, 'dist'), // Ruta absoluta a la carpeta 'dist'
    filename: 'bundle.js',                  // Nombre del archivo de salida
  },
  
  // 3. LOADERS: Reglas para manejar módulos
  module: {
    rules: [
      {
        test: /\.js$/,       // Aplica esta regla a archivos .js
        exclude: /node_modules/, // Ignora la carpeta de dependencias
        use: {
          loader: 'babel-loader', // Usa Babel para transpilación
          options: {
            presets: ['@babel/preset-env', '@babel/preset-react'],
          }
        }
      },
      // { test: /\.css$/, use: ['style-loader', 'css-loader'] } // Ejemplo para CSS
    ]
  },
  
  // Modo de ejecución (para optimizaciones por defecto)
  mode: 'development' // o 'production'
};
```

### Comandos de Ejecución

Usaremos los scripts npm para simplificar la ejecución.

**`package.json`**

JSON

```
{
  "scripts": {
    "build": "webpack --config webpack.config.js",
    "dev": "webpack serve --open" // Requiere webpack-dev-server
  },
  // ...
}
```

**Ejecución:**

Bash

```
# Ejecuta la configuración de build
$ npm run build 

# Inicia el servidor de desarrollo con HMR (si webpack-dev-server está instalado)
$ npm run dev 
```

### Plugins Esenciales (Ejemplo)

Para generar el `index.html` automáticamente e inyectar el `bundle.js`, necesitamos un Plugin.

1. **Instalar Plugin:**
    
    Bash
    
    ```
    $ npm i -D html-webpack-plugin
    ```
    
2. **Usar Plugin en `webpack.config.js`:**
    
    JavaScript
    
    ```
    const HtmlWebpackPlugin = require('html-webpack-plugin');
    
    module.exports = {
      // ... entry, output, module...
    
      // 4. PLUGINS: Funciones que operan en todo el proceso de compilación
      plugins: [
        new HtmlWebpackPlugin({
          title: 'Mi App Webpack',
          template: './src/index.html' // Usa este HTML como plantilla
        })
      ],
      // ...
    };
    ```
    

---

## 🛑 Errores Comunes

|Error|Descripción|Solución|
|---|---|---|
|**`Module not found: Error: Can't resolve...`**|Webpack no pudo encontrar el archivo importado o el _loader_ necesario.|**A:** Revisa las rutas de `import`. **B:** Asegúrate de que tienes un _loader_ configurado para ese tipo de archivo (ej: si importas `.svg`, necesitas un _loader_ de archivos).|
|**`Loading Canceled` o Fallo en HMR**|El `webpack-dev-server` no está configurado correctamente o un error en el _bundle_ detiene el proceso.|Asegúrate de que `mode: 'development'` esté activo. Verifica que el puerto no esté en uso.|
|**`path must be an absolute path`**|El campo `output.path` no está usando `path.resolve(__dirname, 'dist')` o similar para asegurar una ruta absoluta.|Siempre usa el módulo `path` de Node.js para definir rutas absolutas.|

Exportar a Hojas de cálculo

## 📚 Resumen de Puntos Clave

- **Webpack** es un _bundler_ de módulos "Bundle-First" que ofrece un control granular.
    
- Se configura mediante **`webpack.config.js`** y se basa en 4 conceptos:
    
    - **Entry:** El inicio del grafo de dependencias.
        
    - **Output:** Dónde guardar y cómo nombrar los _bundles_.
        
    - **Loaders:** Traducen archivos no-JS (CSS, TS, etc.) a módulos.
        
    - **Plugins:** Realizan tareas complejas en el proceso de compilación o _bundle_ (minificación, inyección HTML, etc.).
        
- Webpack es la opción si necesitas **configuración profunda**, mientras que Vite es la opción por **velocidad de desarrollo**.
    

---

Ya cubrimos los dos gigantes del _bundling_. En la siguiente clase, veremos la configuración _zero-config_ de **Parcel** y la velocidad pura de **esbuild**.

[[12 - Parcel]]