Hemos dominado la gestión de paquetes. Ahora, entramos en el Nivel 3: **Bundlers modernos**, la maquinaria pesada que transforma tu código de desarrollo en un producto final optimizado.

---

Aquí tienes los apuntes de la clase anterior: [[08 - Publicación de paquetes]]

# 09 - Introducción a bundlers

## 📝 Introducción

Si el gestor de paquetes (`npm`, `yarn`) es el administrador de recursos, el **Bundler** (Empaquetador) es la **fábrica**. Un Bundler es una herramienta de _build_ esencial que toma todos los archivos de tu proyecto (JavaScript, CSS, imágenes, etc.), determina cómo se relacionan entre sí (el **Grafo de Dependencias**) y los combina de forma eficiente en paquetes listos para ser desplegados en producción.

## 🧠 Teoría: ¿Qué hace exactamente un Bundler?

El principal objetivo de un bundler es optimizar la entrega del código al navegador, superando las limitaciones del desarrollo en módulos.

### 1. El Grafo de Dependencias (Dependency Graph)

Cuando escribes código moderno, usas la sintaxis `import ... from './file.js'`. El bundler empieza en un punto de entrada (`entry point`, ej: `main.js`) y sigue recursivamente todos los `import` y `require` para construir un mapa mental de **todos** los archivos que componen tu aplicación. Este mapa es el **Grafo de Dependencias**.

### 2. Las 4 Funciones Clave del Bundling

1. **Resolución de Módulos (Module Resolution):** Encuentra todos los archivos importados (incluyendo paquetes de `node_modules`).
    
2. **Transpilación:** Convierte el código moderno (ej: TypeScript, JSX, ESNext) en un JavaScript compatible con el _target_ (navegadores viejos o específicos) usando herramientas como **Babel** o **esbuild** internamente.
    
3. **Bundling:** Combina todos los módulos resueltos en uno o varios archivos de salida optimizados (**bundles**).
    
4. **Optimización:** Aplica _minificación_ (eliminar espacios y comentarios), _tree shaking_ (eliminar código no utilizado) y _code splitting_ (dividir el código para carga perezosa).
    

---

### Diferencias entre Bundlers: La Evolución

La principal diferencia entre los bundlers modernos y los _legacy_ (como Webpack o Parcel en sus inicios) es la velocidad de desarrollo.

#### Webpack (Legacy/Tradicional)

- **Enfoque:** **Bundle-First.** Siempre hace el _bundling_ de todo el código (incluidos los módulos de `node_modules`) **antes** de servirlo en el servidor de desarrollo.
    
- **Ventaja:** Máximo control sobre la salida final y gran ecosistema de _loaders_ y _plugins_.
    
- **Desventaja:** Los tiempos de inicio del servidor y las recargas (_rebuilds_) pueden ser **lentos** en proyectos grandes, ya que tiene que procesar miles de módulos.
    

#### Vite, esbuild, Snowpack, Parcel (Moderno)

- **Enfoque:** **No-Bundle / ESM Nativo.**
    
    - **En Desarrollo:** Usan el sistema nativo de módulos de JavaScript del navegador (`<script type="module">`). Solo transpilan lo necesario y sirven el código directamente. **No tienen que bundear toda la aplicación**. Esto hace que el inicio del servidor sea **casi instantáneo**.
        
    - **En Producción:** Utilizan herramientas ultra-rápidas (como **esbuild** o **Rollup** con plugins) para el _bundling_ final.
        

|Herramienta|Tipo de Arquitectura|Velocidad de Desarrollo|Configuración Típica|
|---|---|---|---|
|**Webpack**|Bundle-First (Transpilación de módulos)|Lenta a Moderada|Alta (muchos _loaders_ y _plugins_)|
|**Vite**|**No-Bundle** (ESM Nativo)|Muy Rápida|Baja a Moderada (basado en plugins)|
|**esbuild**|Solo Bundler (sin dev server)|Extremadamente Rápido|Baja (optimizado para velocidad)|
|**Parcel**|Zero-Config|Rápida|Mínima (cero configuración)|

Exportar a Hojas de cálculo

---

## 🛠️ Conceptos Clave del Bundling

Para entender cualquier bundler (especialmente Webpack), debes dominar estos términos:

1. **Entry Point:** El(los) archivo(s) donde el bundler debe empezar a construir el grafo de dependencias.
    
    - _Ejemplo:_ `main.js`
        
2. **Output:** Dónde y cómo se debe guardar el _bundle_ de código final.
    
    - _Ejemplo:_ Carpeta `dist/bundle.js`
        
3. **Loaders (Webpack):** Módulos que le enseñan al bundler a procesar tipos de archivos que no son JavaScript (ej: un loader para convertir archivos CSS o SASS a JS/CSS).
    
4. **Plugins (Webpack/Vite):** Tareas más amplias que actúan sobre el _bundle_ en general o en el proceso de compilación (ej: minificación, inyección de HTML, manejo de variables de entorno).
    

### Ejemplo Conceptual: Transpilación

Imagina que escribes en TypeScript (`.ts`):

TypeScript

```
// src/app.ts
const message: string = "Hello World";
console.log(message);
```

Un bundler usará un **Transpilador** (como Babel o el motor de esbuild) para convertirlo en JavaScript compatible, y luego lo bundeará:

JavaScript

```
// dist/bundle.js (después de transpilar y bundear)
var message = "Hello World"; // Transpilación de 'const' a 'var' si es necesario.
console.log(message);
```

---

## 🛑 Errores Comunes

|Error|Descripción|Solución|
|---|---|---|
|**"You may need an appropriate loader..."**|El bundler (típicamente Webpack) encontró un tipo de archivo (ej: `.css` o `.svg`) pero no sabe cómo interpretarlo y convertirlo.|Instala e incluye el _Loader_ o _Plugin_ necesario en la configuración del bundler.|
|**"Hot Module Replacement (HMR) failed"**|El código fue actualizado, pero la recarga rápida no funcionó.|Puede ser un problema con la configuración del servidor de desarrollo o que el código tiene un error de sintaxis que rompe la conexión.|
|**Tamaño excesivo del Bundle**|El archivo final de JavaScript es demasiado grande (cientos de KB o MB).|El **Tree Shaking** no está funcionando, o necesitas implementar **Code Splitting** (ver [[14 - Code splitting y lazy loading]]).|

Exportar a Hojas de cálculo

## 📚 Resumen de Puntos Clave

- El **Bundler** crea el **Grafo de Dependencias** y procesa todo el código de tu aplicación.
    
- Sus funciones clave son la **Resolución**, **Transpilación**, **Bundling** y **Optimización**.
    
- La gran división es: **Webpack** (Bundle-First, lento en desarrollo) vs. **Vite/esbuild** (No-Bundle/ESM Nativo, muy rápido en desarrollo).
    
- **Loaders/Plugins** son los módulos de Webpack que procesan y optimizan los activos.
    

---

Ahora que entendemos la teoría del _bundling_, empezaremos con la herramienta que ha revolucionado la velocidad de desarrollo: **Vite**.

[[10 - Vite]]