Hemos cubierto el flujo de datos completo, desde el componente hasta la API. Es hora de enfocarnos en la calidad: hacer que nuestras aplicaciones sean **rápidas** y eficientes. Pasamos al Nivel 4, iniciando con **Optimización y Performance**.

---

Aquí tienes los apuntes de la clase anterior: [[13 - Fetch y consumo de APIs]]

## 14 - Performance y Lazy Loading

### Introducción al tema: Contexto y Utilidad

La **Performance** en el Frontend no es solo una característica; es una necesidad. Una aplicación lenta afecta la **Experiencia del Usuario (UX)**, reduce la conversión de negocio y perjudica el posicionamiento SEO.

Las principales métricas de rendimiento (Core Web Vitals) se centran en la velocidad de carga. La mayor causa de lentitud en SPAs grandes es el tamaño excesivo de los archivos JavaScript que el navegador debe descargar, parsear y ejecutar al inicio.

Aquí es donde el **Lazy Loading (Carga Perezosa)** y la **Optimización** entran en juego.

1. **Lazy Loading:** Consiste en cargar partes de la aplicación (componentes, rutas) solo cuando son necesarias, no al inicio.
    
2. **Code Splitting:** Es la técnica de dividir el _bundle_ (paquete grande) de JavaScript en paquetes más pequeños.
    

### Prerrequisitos

- Entendimiento del _Routing_ (ver [[10 - Routing]]), ya que el _lazy loading_ se aplica a menudo a nivel de ruta.
    
- Dominio de la composición de componentes (ver [[04 - Componentes básicos]]).
    

---

### Explicación Completa y Extendida

#### 1. Code Splitting y Lazy Loading (Enfoque General)

Cuando usamos _bundlers_ modernos como **Vite** (que usa Rollup por debajo), el _Code Splitting_ se produce automáticamente en gran medida al usar el concepto de **importación dinámica**.

Una importación normal es estática y se incluye en el paquete inicial:

JavaScript

```
import ComponenteA from './ComponenteA'; // Carga inmediata
```

Una importación dinámica es una función de JavaScript que devuelve una Promesa y solo se carga cuando se llama:

JavaScript

```
const C = import('./ComponenteB'); // Carga a demanda (Lazy Loading)
```

Al hacer esto, el _bundler_ crea un archivo JavaScript separado (_chunk_) para `ComponenteB`, y el navegador solo lo descarga cuando se accede a él.

#### 2. Lazy Loading en React: `lazy` y `Suspense`

React proporciona dos herramientas nativas para implementar el _Code Splitting_ a nivel de componente: `React.lazy` y `<Suspense>`.

1. **`React.lazy`:** Envuelve la importación dinámica del componente.
    
2. **`<Suspense>`:** Es un componente que permite "esperar" mientras el código del componente dinámico se está descargando. Requiere una prop `fallback` para mostrar contenido mientras se espera (ej: un _spinner_).
    

**Ejemplo React (Lazy Loading de una Ruta):**

JavaScript

```
import React, { Suspense, lazy } from 'react';
import { BrowserRouter, Routes, Route } from 'react-router-dom';

// 1. Aplicar React.lazy a la importación dinámica
const Dashboard = lazy(() => import('./views/Dashboard'));
const Configuracion = lazy(() => import('./views/Configuracion'));

function AppRoutes() {
  return (
    <BrowserRouter>
      {/* 2. Envolver la sección de rutas con Suspense */}
      <Suspense fallback={<div>Cargando la sección...</div>}>
        <Routes>
          {/* 3. Usar el componente lazy en las rutas */}
          <Route path="/dashboard" element={<Dashboard />} />
          <Route path="/config" element={<Configuracion />} />
        </Routes>
      </Suspense>
    </BrowserRouter>
  );
}
```

#### 3. Lazy Loading en Vue: `defineAsyncComponent`

Vue maneja las cargas perezosas de manera similar a React, utilizando una función especial para envolver la importación dinámica, y el manejo de _loading_ y _error_ se puede hacer con la propia función o con un `v-if`.

**Ejemplo Vue (Lazy Loading de Componentes):**

JavaScript

```
// router/index.js o en la definición de rutas
import { createRouter, createWebHistory } from 'vue-router';
import Home from '@/views/Home.vue'; 

const router = createRouter({
  history: createWebHistory(),
  routes: [
    {
      path: '/dashboard',
      name: 'Dashboard',
      // 1. Usar la importación dinámica directamente en la definición de la ruta
      component: () => import('@/views/Dashboard.vue') 
    },
    // Otras rutas...
  ]
});
```

Para control más fino sobre _loading_ y errores, se usa `defineAsyncComponent`:

JavaScript

```
import { defineAsyncComponent } from 'vue';

const AsyncComponent = defineAsyncComponent({
  // 1. El cargador de componentes (la importación dinámica)
  loader: () => import('./views/Pesado.vue'), 
  // 2. Componente a mostrar mientras se carga
  loadingComponent: LoadingSpinner, 
  // 3. Componente a mostrar si hay un error
  errorComponent: ErrorMessage, 
  // 4. Retardo antes de mostrar el componente de carga (ms)
  delay: 200, 
});
```

#### 4. Lazy Loading en Svelte: Integrado en SvelteKit

Svelte, a través de SvelteKit, maneja el _Code Splitting_ de forma automática y predictiva. SvelteKit utiliza el _routing_ basado en archivos para generar _chunks_ separados para cada ruta.

- **Pre-fetching Predictivo:** SvelteKit observa los enlaces (`<a>`) en la vista y precarga el JavaScript de la ruta de destino justo antes de que el usuario haga click (al hacer _hover_ o _touchstart_), haciendo que la transición se sienta instantánea.
    
- **No Requiere Sintaxis Especial:** No necesitas `lazy` o `Suspense`; simplemente defines las rutas con la estructura de archivos y SvelteKit se encarga de la optimización del _bundle_.
    

---

### Técnicas Adicionales de Optimización (React Focus)

Además del _Code Splitting_, la optimización se centra en **reducir los re-renders innecesarios**.

#### A. Memorización de Componentes (`memo`)

En React, si un componente padre se re-renderiza, todos sus hijos se re-renderizan por defecto. **`React.memo`** es una función que envuelve un componente funcional y le dice a React que **solo se re-renderice si sus props han cambiado**.

JavaScript

```
// Componente de presentación puro y pesado
const TarjetaProducto = React.memo(({ nombre, precio }) => {
  console.log("TarjetaProducto renderizada"); // Solo se ejecuta si nombre o precio cambian
  return <div>{nombre} - ${precio}</div>;
});
```

#### B. Memorización de Valores y Callbacks (`useMemo` y `useCallback`)

- **`useMemo`:** Evita la re-ejecución de cálculos costosos en cada render. Solo se recalcula si las dependencias cambian.
    
    JavaScript
    
    ```
    const totalIVA = useMemo(() => calcularIVA(productos), [productos]);
    ```
    
- **`useCallback`:** Asegura que una función pasada como _prop_ a un componente memorizado (`React.memo`) **no sea recreada** en cada render.
    
    JavaScript
    
    ```
    // La función 'handleClick' será la misma en cada render a menos que 'id' cambie
    const handleClick = useCallback((id) => {
      console.log(`Click en ID: ${id}`);
    }, []); 
    ```
    

**¡Advertencia!** Usar `memo`, `useMemo` o `useCallback` en exceso puede ser contraproducente. La verificación de si las _props_ o dependencias han cambiado también tiene un costo. **Úsalos solo cuando hayas identificado un cuello de botella de rendimiento.**

---

### Ejercicios Prácticos Paso a Paso

**Objetivo:** Crear un componente `ContadorPesado` que solo se re-renderice si su prop cambia.

1. **Componente Padre (`App`):** Define dos estados: `contadorA` y `contadorB`.
    
2. **Componente Hijo (`MensajePesado`):** Recibe una prop `mensaje` y simula una operación pesada (`console.log` + un bucle largo).
    
3. **Optimización:** Envuelve `MensajePesado` con `React.memo`.
    
4. **Prueba:** Muestra `MensajePesado` usando `contadorA` como prop. Haz click en un botón que solo cambie `contadorB`. **Observa** que el componente memoizado no se re-renderiza.
    

### Errores Comunes y Troubleshooting

- **React.memo Roto:** Usar `React.memo` en un componente que recibe funciones o arrays/objetos **no memorizados** como _props_. En cada render, la prop de la función/objeto es un nuevo objeto en memoria, lo que rompe la memorización.
    
    - **Solución:** Si pasas una función a un componente memoizado, **envuélvela en `useCallback`**.
        
- **Lazy Loading en el Lugar Incorrecto:** Intentar aplicar `lazy` a un componente que siempre se renderiza (ej: un `Header` o un `Footer`). Esto solo añade complejidad sin beneficio de carga.
    
- **Dependencias de `useMemo`/`useCallback` Incorrectas:** Olvidar incluir una variable externa en el array de dependencias. El _hook_ usará un valor obsoleto de un render anterior.
    

### Resumen de Puntos Clave

- La **Optimización de Performance** se centra en reducir el tamaño del _bundle_ JS y evitar re-renders innecesarios.
    
- **Code Splitting** y **Lazy Loading** son esenciales para cargar código solo cuando se necesita, típicamente a nivel de ruta.
    
- **React** usa **`React.lazy`** para la carga dinámica y **`<Suspense>`** para mostrar el estado de _loading_.
    
- **Vue** usa la importación dinámica en la configuración del _router_ o **`defineAsyncComponent`** para control de _loading_.
    
- **SvelteKit** automatiza el _Code Splitting_ y usa **Pre-fetching Predictivo** para una UX rápida.
    
- **React.memo** memoriza componentes funcionales para evitar re-renders a menos que sus _props_ cambien.
    
- **`useMemo`** memoriza valores calculados y **`useCallback`** memoriza funciones.
    

### Enlaces Internos

👉 El rendimiento es vital para las pruebas. Pasemos a cómo asegurarnos de que el código optimizado siga funcionando: [[15 - Testing]].

👉 El uso correcto de `useMemo` y `useCallback` depende del entendimiento profundo de los Hooks: [[09 - Hooks y composables]].

---

**Siguiente paso:** La optimización es la primera parte de la calidad. La siguiente es la seguridad y confiabilidad: **Testing**. Pasemos a [[15 - Testing]].