Hemos cubierto todos los cimientos teóricos y de optimización. Es hora de ponerlo todo en práctica. Iniciaremos un proyecto desde cero usando el enfoque moderno con **Vite** para construir una Single Page Application (SPA).

---

# 19 - Proyecto guiado: SPA moderna

## 📝 Introducción

En esta clase, crearemos un proyecto completo de SPA (Single Page Application) utilizando **Vite** como _Build Tool_ principal y **React** como _framework_. Aplicaremos los conocimientos de _Scripts npm_, _Bundling_, _Lazy Loading_ y optimización.

**Objetivos del Proyecto:**

1. Inicializar el proyecto con Vite y un gestor de paquetes (usaremos pnpm por su eficiencia).
    
2. Definir scripts de desarrollo y _build_.
    
3. Implementar **Code Splitting** y **Lazy Loading** para mejorar el rendimiento.
    
4. Configurar la salida para **Producción** (minificación y _hashing_).
    

## 🧠 Teoría: El Flujo de Trabajo Moderno

En la práctica, un proyecto moderno sigue este ciclo:

1. **Setup:** `$ pnpm create vite` (Instalación y Configuración).
    
2. **Desarrollo:** `$ pnpm dev` (Servidor No-Bundle + HMR ultra-rápido).
    
3. **Build:** `$ pnpm build` (Bundling optimizado con Rollup, Tree Shaking, Minificación).
    
4. **Optimización:** Usar `import()` dinámico para dividir el código.
    

---

## 🛠️ Ejemplos Prácticos: Implementación

### Paso 1: Inicialización con pnpm y Vite

Usaremos `pnpm` y elegiremos el _template_ de React con TypeScript para un entorno robusto.

Bash

```
# 1. Creamos el proyecto (usando el pnpm equivalente a npx create-vite)
$ pnpm create vite mi-spa-moderna --template react-ts

# 2. Navegamos e instalamos dependencias
$ cd mi-spa-moderna
$ pnpm install 
# Esto crea node_modules, package.json y pnpm-lock.yaml.
```

### Paso 2: Revisión de Scripts y Dependencias

El `package.json` de Vite es muy limpio:

**`package.json`**

JSON

```
{
  "name": "mi-spa-moderna",
  "version": "0.0.0",
  "scripts": {
    "dev": "vite",        // Modo Desarrollo (No-Bundle, HMR)
    "build": "tsc && vite build", // 👈 Primero Type Check, luego Build (Rollup)
    "lint": "eslint . --ext ts,tsx --report-unused-disable-directives --max-warnings 0",
    "preview": "vite preview" // Sirve el build de producción localmente
  },
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-router-dom": "^6.20.0" // 👈 Añadido para el router
  },
  "devDependencies": {
    "vite": "^5.0.0",
    // ... typescript, eslint, etc.
  }
}
```

> **Comando de Build:** El script `build` usa el operador `&&` (secuencial, ver [[05 - Scripts npm]]) para asegurar que el _Type Check_ (`tsc`) se ejecute y tenga éxito **antes** de iniciar el _build_ final con `vite build`.

### Paso 3: Implementación de Lazy Loading y Code Splitting

Instalaremos **React Router** para simular un _split point_ entre la página de inicio y la de administración (que es pesada y rara vez visitada).

**Estructura de Componentes:**

- `src/pages/Home.tsx` (Componente pequeño, cargado de inmediato).
    
- `src/pages/Admin.tsx` (Componente pesado, cargado con Lazy Loading).
    

**`src/App.tsx` (El Punto de División)**

TypeScript

```
import React, { lazy, Suspense } from 'react';
import { BrowserRouter, Routes, Route } from 'react-router-dom';

// 1. Home Page: Carga estática (incluida en el bundle principal)
import { Home } from './pages/Home';

// 2. Admin Page: Carga perezosa (Code Splitting)
// El bundler (Rollup/Vite) ve la importación dinámica y crea un 'chunk' separado.
const AdminPage = lazy(() => 
    import(/* webpackChunkName: "admin-chunk" */ './pages/Admin') 
);

export function App() {
  return (
    <BrowserRouter>
      {/* 3. Suspense: Muestra el fallback mientras el chunk 'admin-chunk' se descarga */}
      <Suspense fallback={<div>Cargando Admin...</div>}>
        <Routes>
          <Route path="/" element={<Home />} />
          {/* El código de AdminPage solo se solicita cuando se accede a /admin */}
          <Route path="/admin" element={<AdminPage />} /> 
        </Routes>
      </Suspense>
    </BrowserRouter>
  );
}
```

### Paso 4: Configuración de Producción (Vite.config.ts)

Vite ya configura Rollup con minificación y _hashing_ por defecto, pero podemos afinar la salida.

**`vite.config.ts` (Añadiendo el Bundle Analyzer)**

Para verificar que el _Code Splitting_ funciona y que el _bundle_ de administración es grande (separado), añadiremos el `rollup-plugin-visualizer` (ver [[17 - Optimización de producción]]).

1. **Instalar plugin:** `$ pnpm add -D rollup-plugin-visualizer`
    
2. **Configuración:**
    
    TypeScript
    
    ```
    import { defineConfig } from 'vite';
    import react from '@vitejs/plugin-react';
    import { visualizer } from 'rollup-plugin-visualizer'; // 👈 Importamos
    
    export default defineConfig({
      plugins: [
        react(), 
        visualizer({ 
          filename: 'dist/bundle-report.html', // Nombre del reporte
          open: true, // Abrir el reporte en el navegador después del build
        })
      ],
      build: {
        // Hashing de contenido para Caching (Rollup lo hace por defecto, pero lo confirmamos)
        outDir: 'dist',
        rollupOptions: {
            output: {
                // Aseguramos que los chunks tengan un nombre descriptivo
                chunkFileNames: 'assets/[name]-[hash].js', 
            }
        }
      }
    });
    ```
    

### Paso 5: Ejecución y Verificación

1. **Ejecutar Build:**
    
    Bash
    
    ```
    $ pnpm run build
    ```
    
2. **Resultado en `dist/`:** Deberías ver múltiples archivos JavaScript en `dist/assets/`:
    
    - `index-[hash].js`: El _bundle_ principal, que es **pequeño** y contiene el _router_ y la `HomePage`.
        
    - `admin-chunk-[hash].js`: El _chunk_ separado que contiene solo el código de `AdminPage` (será más grande).
        
3. **Análisis:** Se abrirá `dist/bundle-report.html`, confirmando que el _Code Splitting_ ha dividido `AdminPage` del _bundle_ principal.
    

---

## 🛑 Errores Comunes en el Proyecto Guía

|Error|Descripción|Solución|
|---|---|---|
|**"Cannot read property 'then' of undefined"**|Usaste `import` estático (`import {Admin} from '...'`) en lugar de `import()` dinámico.|Asegúrate de usar la función que devuelve la Promesa: `lazy(() => import('./Admin'))`.|
|**`pnpm run build` es muy lento**|La comprobación de tipos (`tsc`) está tardando demasiado o el _build_ de Rollup es pesado.|**Opción A:** En `vite.config.ts`, usa `vite-plugin-checker` para ejecutar `tsc` en un proceso worker (ver [[16 - Plugins y ecosistema]]). **Opción B:** Revisa el Bundle Analyzer para podar dependencias pesadas.|
|**Fallo en el _Tree Shaking_**|El Bundle Analyzer muestra que toda la librería React se está incluyendo en un _chunk_.|Asegúrate de que todas las librerías se importen usando Módulos ES6 (`import`).|

Exportar a Hojas de cálculo

## 📚 Resumen de Puntos Clave

- Un proyecto moderno se inicializa rápidamente con **Vite** y usa scripts claros para `dev` y `build`.
    
- El script de `build` profesional siempre debe incluir una **verificación de calidad/tipos** antes del _bundling_ (Ej: `tsc && vite build`).
    
- El **Lazy Loading** en React/Vue se implementa con **Importaciones Dinámicas** (`import()`) y la utilidad **`<Suspense>`** para crear **Chunks** separados.
    
- El **Bundle Analyzer** es la herramienta clave para verificar que el **Code Splitting** y la **Optimización** están funcionando correctamente en el _bundle_ de producción.
    

---

¡Felicidades por tu primera SPA moderna! Hemos aplicado todas las técnicas cruciales. La última clase será un resumen avanzado sobre cómo llevar este _build_ a la etapa de despliegue y CI/CD.

[[20 - Proyecto final avanzado - Build Tools]]