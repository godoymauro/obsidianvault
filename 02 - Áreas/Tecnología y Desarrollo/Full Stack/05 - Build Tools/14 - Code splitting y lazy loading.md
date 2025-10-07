Hemos cubierto el **quién** (gestores de paquetes) y el **qué** (bundlers). Ahora pasamos al Nivel 4, que se centra en el **cómo** optimizar y hacer que nuestras aplicaciones sean rápidas y eficientes.

Comenzamos con la técnica más importante para el rendimiento en proyectos grandes: **Code Splitting y Lazy Loading**.

---

# 14 - Code splitting y lazy loading

## 📝 Introducción

En un proyecto pequeño, servir toda la aplicación en un único `bundle.js` está bien. Sin embargo, si tu aplicación crece a 500KB o 1MB, estás obligando al usuario a descargar todo, incluso la parte de la administración o el perfil que quizás nunca visiten.

El **Code Splitting** (división de código) es una técnica que resuelve este problema dividiendo el _bundle_ monolítico en fragmentos más pequeños que se pueden cargar **a demanda** (Lazy Loading). Esto reduce drásticamente el tiempo de carga inicial y mejora la experiencia del usuario.

## 🧠 Teoría: Dividir y Conquistar

### 1. Code Splitting: El Proceso

El Code Splitting es el proceso que realiza el _bundler_ (Webpack, Rollup/Vite, Parcel) para generar múltiples archivos JavaScript a partir de uno solo. Esto se logra identificando los **puntos de división** (_split points_).

- **Chunk/Fragmento:** Un archivo de JavaScript que resulta del Code Splitting (ej: `0.bundle.js`, `home.chunk.js`).
    
- **Bundle Principal:** El archivo inicial y más pequeño que contiene la lógica básica y el código para cargar los _chunks_ bajo demanda.
    

### 2. Lazy Loading (Carga Perezosa): La Implementación

El **Lazy Loading** es el patrón de diseño que utilizamos en nuestro código para indicar dónde deben ocurrir los _split points_. Usamos una sintaxis especial de JavaScript: el **`import()` dinámico**.

|Sintaxis|Uso|Propósito|
|---|---|---|
|**`import * as module from './file.js'`**|Importación Estática (Síncrona)|Carga el código **inmediatamente** y lo incluye en el _bundle_ principal.|
|**`import('./file.js')`**|**Importación Dinámica (Asíncrona)**|Le dice al _bundler_: "No incluyas este código en el _bundle_ principal. Créale un _chunk_ separado y cárgalo **solo cuando esta línea de código se ejecute**."|

Exportar a Hojas de cálculo

La importación dinámica devuelve una **Promesa** que se resuelve cuando el _chunk_ se ha descargado y el módulo está disponible.

### 3. El Beneficio: Primer Contenido (First Contentful Paint)

Al reducir el tamaño del _bundle_ inicial, se acelera el momento en que el navegador puede renderizar contenido útil. Esto mejora el puntaje de **Core Web Vitals** y la percepción de velocidad por parte del usuario.

---

## 🛠️ Ejemplos en Frameworks Modernos

Los _bundlers_ modernos se integran perfectamente con las librerías frontend para facilitar el Lazy Loading.

### Ejemplo 1: Lazy Loading en React (React.lazy y Suspense)

En React, combinamos el `import()` dinámico con las utilidades de React:

JavaScript

```
// 1. Define el componente que se cargará de forma perezosa
import React, { lazy, Suspense } from 'react';

// El bundler ve esta sintaxis y crea un chunk separado para './About.jsx'
const AboutPage = lazy(() => import('./About.jsx')); 
// 👆 Este es el punto de división

function App() {
  return (
    <div>
      {/* 2. Suspense muestra un fallback (ej: un loader) mientras se descarga el chunk */}
      <Suspense fallback={<div>Cargando página...</div>}> 
        <Router>
          <Route path="/" component={Home} />
          {/* La página AboutPage solo se intenta cargar cuando se visita esta ruta */}
          <Route path="/about" component={AboutPage} />
        </Router>
      </Suspense>
    </div>
  );
}

export default App;
```

### Ejemplo 2: Lazy Loading en Vue (Router)

Vue Router soporta la importación dinámica directamente en la definición de rutas:

JavaScript

```
// router/index.js

const routes = [
  {
    path: '/',
    name: 'Home',
    component: () => import('../views/Home.vue') // Chunk separado para Home
  },
  {
    path: '/admin',
    name: 'Admin',
    // Chunk separado para la página de Administración (solo cargado al acceder)
    component: () => import('../views/Admin.vue') 
  }
];
```

### Ejemplo 3: Code Splitting en Librerías Comunes

También se puede hacer Lazy Loading para librerías que no son componentes de UI, como un editor de texto pesado:

JavaScript

```
document.getElementById('editor-button').addEventListener('click', () => {
  // Solo descarga y carga la librería 'tinymce' cuando el usuario hace clic
  import('tinymce') 
    .then(tinymce => {
      tinymce.default.init({ selector: '#my-editor' });
    })
    .catch(error => {
      console.error('Fallo al cargar el editor:', error);
    });
});
```

---

## 🛠️ Naming Chunks (Nombres de Fragmentos)

Por defecto, los _chunks_ generados por el _bundler_ tienen nombres numéricos (ej: `0.js`, `1.js`). Para un mejor _debugging_ y _caching_ explícito, es mejor darles un nombre descriptivo.

Esto se hace con un comentario especial de Webpack/Rollup llamado **Magic Comment**:

JavaScript

```
// Usando Magic Comment para nombrar el chunk 'about-chunk'
const AboutPage = lazy(() => 
  import(/* webpackChunkName: "about-chunk" */ './About.jsx')
); 
```

Al hacer esto, Webpack/Rollup generará un archivo llamado algo como `about-chunk.js` o `about-chunk.f12a4b.js`.

## 🛑 Errores Comunes

|Error|Descripción|Solución|
|---|---|---|
|**"ChunkLoadError: Loading chunk X failed"**|El navegador no pudo descargar el _chunk_. Común si la ruta del _bundle_ es incorrecta (Ej: CDN fallida) o si se desplegó una versión nueva sin limpiar la vieja.|Asegúrate de que el `output.publicPath` en tu configuración de Webpack/Vite sea correcto. Limpia el cache del navegador y del _bundler_.|
|**El bundle sigue siendo grande**|Aplicaste `lazy` pero el código no se dividió.|Verifica que estás usando la sintaxis **`import('./file.js')` dinámica** y no la estática (`import ... from '...'`). El _bundler_ es el que debe interpretar la promesa.|
|**Fallo en Renderizado (React)**|Intentaste usar `React.lazy` sin envolverlo en el componente **`<Suspense>`**.|Siempre usa el `Suspense` con su propiedad `fallback` alrededor de los componentes de carga perezosa para manejar el estado de "cargando".|

Exportar a Hojas de cálculo

## 📚 Resumen de Puntos Clave

- **Code Splitting** es la división del _bundle_ monolítico en fragmentos (**chunks**).
    
- **Lazy Loading** es el patrón que utiliza la **importación dinámica (`import()`)** para cargar esos _chunks_ bajo demanda.
    
- Esto reduce el tiempo de carga inicial y el tamaño del _bundle_ principal.
    
- Los frameworks como React y Vue tienen utilidades (`React.lazy`, Vue Router) que facilitan la implementación del Lazy Loading.
    
- Se pueden usar **Magic Comments** (`/* webpackChunkName: "..." */`) para dar nombres legibles a los _chunks_ generados.
    

---

La división de código es inútil si el código final es ilegible o incompatible. En la siguiente clase, veremos las herramientas para la **Transpilación** y cómo asegurar la compatibilidad con navegadores antiguos.

[[15 - Transpiladores y compatibilidad]]