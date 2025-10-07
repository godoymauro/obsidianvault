Hemos llegado a la fase de aplicación práctica. Con todos los conceptos teóricos y avanzados cubiertos, es momento de poner todo junto y construir una aplicación real. Empezaremos con la estructura de un proyecto típico de **SPA (Single Page Application)**.

---

Aquí tienes los apuntes de la clase anterior: [[18 - Buenas prácticas y patrones de diseño]]

## 19 - Proyecto Guiado: Dashboard SPA

### Introducción al tema: Contexto y Utilidad 🏗️

En este módulo, aplicaremos la arquitectura y los patrones que hemos aprendido para construir una **Dashboard SPA** básica. Un _dashboard_ es un excelente proyecto inicial porque combina varios elementos esenciales del frontend moderno: **routing**, **gestión de estado global** (usuario autenticado), **consumo de APIs** y **componentes reutilizables**.

El objetivo es pasar de la teoría a la práctica, viendo cómo los conceptos de las clases anteriores (como `useFetch`, `useContext`, Patrón Contenedor/Presentacional) se traducen en una estructura de carpetas funcional.

### Prerrequisitos

- Dominio del **Routing** (React Router, Vue Router, SvelteKit) (ver [[10 - Routing]]).
    
- Entendimiento de la **Gestión de Estado Global** (Context, Zustand, Pinia, Stores) (ver [[11 - State management global]]).
    
- Conocimiento del **Fetching de APIs** (ver [[13 - Fetch y consumo de APIs]]).
    
- Aplicación de **Patrones de Diseño** (Contenedor/Presentacional) (ver [[18 - Buenas prácticas y patrones de diseño]]).
    

---

### Explicación Completa y Extendida

#### 1. Estructura de Carpetas (Clean Architecture Ligera)

Adoptaremos una estructura modular, basada en las buenas prácticas y el concepto de **organización por preocupación** (dominio) en lugar de por tipo de archivo.

|Carpeta|Contenido|Patrón Aplicado|
|---|---|---|
|**`src/`**|Raíz del código fuente.||
|**`assets/`**|Imágenes, fuentes, archivos de estilo globales.||
|**`components/`**|Componentes de presentación reutilizables (Átomos/Moléculas/Organismos). **No tienen lógica de _fetch_ o _store_**.|Atomic Design (Átomos/Moléculas).|
|**`hooks/` o `composables/`**|Lógica reutilizable y aislada (ej: `useToggle`, `useFetch`).|DRY, Reutilización de Lógica.|
|**`services/` o `api/`**|Funciones que gestionan la interacción con APIs externas (Axios/fetch).|Aislamiento de I/O.|
|**`stores/` o `context/`**|Gestión de estado global (Pinia, Zustand, React Context).|State Management Global.|
|**`views/` o `pages/`**|Componentes que representan rutas o páginas completas. **Suelen ser Contenedores.**|Routing, Patrón Contenedor.|
|**`layouts/`**|Componentes de estructura de página (ej: `Sidebar`, `Header`, `Footer`).|Templates.|

Exportar a Hojas de cálculo

#### 2. Implementación de un Módulo Clave: Autenticación (React Ejemplo)

Usaremos el módulo de Autenticación para ejemplificar el flujo de trabajo completo:

##### A. Estado Global (Context/Zustand)

Definimos un Store para guardar el estado de autenticación (`isLoggedIn`, `user`) y las funciones (`login`, `logout`).

JavaScript

```
// src/stores/useAuthStore.js (Zustand/React)
import { create } from 'zustand';

export const useAuthStore = create((set) => ({
  isLoggedIn: false,
  user: null,
  login: (userData) => {
    // Lógica para guardar token en localStorage, etc.
    set({ isLoggedIn: true, user: userData });
  },
  logout: () => {
    // Lógica para limpiar localStorage, etc.
    set({ isLoggedIn: false, user: null });
  },
}));
```

##### B. Componentes de Layout y Protección de Rutas

El _layout_ principal utiliza el _store_ de autenticación.

JavaScript

```
// src/layouts/MainLayout.jsx (El Contenedor Principal)
import { Outlet, Navigate } from 'react-router-dom';
import { useAuthStore } from '../stores/useAuthStore';
import Header from './Header'; // Componente Presentacional

export default function MainLayout() {
  const isLoggedIn = useAuthStore(state => state.isLoggedIn);

  // 1. Protección de Rutas (Si no está logeado, redirigir)
  if (!isLoggedIn) {
    return <Navigate to="/login" replace />; 
  }

  return (
    <div className="layout">
      <Header /> 
      <main>
        {/* 2. <Outlet> es donde React Router renderiza la vista actual */}
        <Outlet /> 
      </main>
    </div>
  );
}
```

##### C. Componentes de Vista (Contenedores)

La página de _Dashboard_ es la vista principal.

JavaScript

```
// src/views/DashboardView.jsx (La Vista/Página)
import ListaEstadisticasContainer from '../components/estadisticas/ListaEstadisticasContainer';
import { useAuthStore } from '../stores/useAuthStore';

export default function DashboardView() {
  const user = useAuthStore(state => state.user);
  
  return (
    <section>
      {/* 3. Composición con datos globales */}
      <h1>Bienvenido, {user?.nombre}!</h1> 
      
      {/* 4. Composición con un Contenedor de datos de API */}
      <ListaEstadisticasContainer /> 
      
      <p>Este es tu resumen de actividad.</p>
    </section>
  );
}
```

#### 3. Flujo de Datos API y Presentación (Contenedor/Presentacional)

El componente `ListaEstadisticasContainer` es la mejor muestra del patrón Contenedor/Presentacional:

JavaScript

```
// src/components/estadisticas/ListaEstadisticasContainer.jsx (El Contenedor)
import { useFetch } from '../../hooks/useFetch'; // Hook de fetch custom
import ListaEstadisticas from './ListaEstadisticas'; // Componente Presentacional

export default function ListaEstadisticasContainer() {
  // Lógica de fetch
  const { data, loading, error } = useFetch('/api/stats'); 

  // Manejo de Early Returns
  if (loading) return <div>Cargando estadísticas...</div>;
  if (error) return <div>Error: No se pudieron cargar las estadísticas.</div>;
  
  // Pasar solo los datos limpios y callbacks al Presentacional
  return <ListaEstadisticas data={data} />; 
}

// src/components/estadisticas/ListaEstadisticas.jsx (El Presentacional)
// Recibe props de su padre (el Contenedor)
function ListaEstadisticas({ data }) {
  return (
    <div className="stats-grid">
      {data.map(stat => (
        // Uso de Átomos/Moléculas más pequeños
        <TarjetaStat key={stat.id} titulo={stat.titulo} valor={stat.valor} /> 
      ))}
    </div>
  );
}
```

#### 4. Integración de Routing con Lazy Loading

Para asegurar el rendimiento (ver [[14 - Performance y lazy loading]]), aplicamos _Lazy Loading_ a las rutas que no son críticas en el inicio.

JavaScript

```
// App.jsx (Integración final)
import React, { Suspense, lazy } from 'react';
import { Routes, Route } from 'react-router-dom';
import MainLayout from './layouts/MainLayout';

// Carga perezosa de vistas
const DashboardView = lazy(() => import('./views/DashboardView'));
const LoginView = lazy(() => import('./views/LoginView'));
const AjustesView = lazy(() => import('./views/AjustesView'));

export default function App() {
  return (
    // Suspense para manejar el loading de las vistas
    <Suspense fallback={<div>Cargando módulo...</div>}>
      <Routes>
        {/* Ruta pública */}
        <Route path="/login" element={<LoginView />} /> 

        {/* Ruta Protegida: usa el MainLayout como Contenedor de Ruta */}
        <Route element={<MainLayout />}>
          <Route path="/" element={<DashboardView />} /> 
          <Route path="/ajustes" element={<AjustesView />} />
        </Route>
        
        {/* Fallback 404 */}
        <Route path="*" element={<div>404 No Encontrado</div>} />
      </Routes>
    </Suspense>
  );
}
```

---

### Ejercicios Prácticos Paso a Paso (Conceptualización)

**Objetivo:** Diseñar la estructura de un componente de tabla para mostrar datos de la API.

1. **Capa de Servicios (`api/`):** Define una función `getTransacciones()` que use `fetch` o `axios` y retorne una promesa con los datos.
    
2. **Capa Lógica (Contenedor):** Crea `TablaTransaccionesContainer.jsx`. Usa tu `useFetch` custom con `getTransacciones()` como la función de _fetch_. Gestiona los estados de `loading/error`.
    
3. **Capa Presentacional (View):** Crea `TablaTransacciones.jsx`. Recibe las transacciones como _prop_ y un _callback_ `onRowClick`. Renderiza la tabla (usando, idealmente, un componente de librería como MUI DataTable).
    
4. **Capa UI (Átomos):** Si no usas librería, crea el `<FilaTabla>` (Molécula) que recibe los datos de una transacción.
    

### Errores Comunes y Troubleshooting

- **Rompimiento de Abstracción:** Un componente Presentacional (ej: `TarjetaStat`) llama a `useAuthStore` directamente.
    
    - **Solución:** Los componentes Presentacionales deben ser **puros**; la data del _store_ debe ser pasada como _prop_ por su componente Contenedor padre.
        
- **Problemas de Ciclo de Vida en la Ruta:** Olvidar usar un `useEffect` con limpieza (`AbortController`) en el `useFetch` cuando la ruta cambia, lo que causa _warnings_ o _memory leaks_.
    
- **Organización Confusa:** Poner un componente `LoginButton` que tiene lógica de `logout` en la carpeta `views/`.
    
    - **Solución:** Utilizar la estructura modular. El botón es un componente pequeño y reutilizable, debe ir en `components/`.
        

### Resumen de Puntos Clave

- La **Dashboard SPA** se construye aplicando los conceptos aprendidos en un solo proyecto cohesivo.
    
- La **Estructura Modular** organiza el código por preocupación (`views`, `components`, `stores`, `hooks`) para facilitar la escalabilidad.
    
- El módulo de **Autenticación** se gestiona con el **Estado Global** (Zustand/Pinia) y el **Routing Protegido** (`<Navigate>` o _guards_).
    
- El patrón **Contenedor/Presentacional** se usa para aislar la lógica de _fetch_ y _store_ de la UI pura.
    
- El **Lazy Loading** con **`<Suspense>`** (React) o importaciones dinámicas (Vue/SvelteKit) se aplica a las vistas de ruta para optimizar la carga inicial.
    
- Las vistas (`views/`) actúan típicamente como componentes **Contenedores**, orquestando datos y componentes más pequeños.
    

### Enlaces Internos

👉 Este proyecto final integra conceptos clave: [[10 - Routing]], [[11 - State management global]], [[13 - Fetch y consumo de APIs]] y [[18 - Buenas prácticas y patrones de diseño]].

---

**Siguiente paso:** Hemos construido la estructura. Ahora, el paso final: el proyecto sin restricciones, incorporando testing y despliegue. Pasemos a [[20 - Proyecto final avanzado]].