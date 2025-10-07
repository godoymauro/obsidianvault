Hemos dominado la abstracción de lógica con Hooks y Composables. Ahora, enfrentamos el desafío de la navegación. En una **Aplicación de Una Sola Página (SPA)**, el usuario tiene la ilusión de navegar entre diferentes páginas, aunque el navegador nunca recarga completamente. Esto lo gestiona el **Routing**.

---

Aquí tienes los apuntes de la clase anterior: [[09 - Hooks y composables]]

## 10 - Routing

### Introducción al tema: Contexto y Utilidad

El **Routing** es el mecanismo que mapea una URL específica del navegador (ej: `/perfil/1` o `/productos`) a un componente específico de la interfaz. Los routers de Frontend permiten construir SPAs complejas y navegables.

La utilidad clave del routing es:

1. **Navegación sin Recarga:** Actualiza la vista del usuario sin recargar la página completa (usando la History API de HTML5: `pushState`).
    
2. **Rutas Dinámicas:** Permite crear rutas que aceptan parámetros (ej: un ID de usuario) para mostrar contenido específico.
    
3. **Rutas Anidadas (Nested Routing):** Permite cargar componentes dentro de otros componentes según la ruta actual (útil para layouts complejos).
    

### Prerrequisitos

- Conocimiento del concepto de SPA (ver [[01 - Introducción a Frontend Frameworks]]).
    
- Dominio de la composición de componentes (ver [[04 - Componentes básicos]]).
    

---

### Explicación Completa y Extendida

En el frontend, el router se encarga de escuchar los cambios en la URL y decidir qué componente debe renderizarse. Los frameworks más populares tienen soluciones robustas para esto.

#### 1. Routing en React: React Router (v6+)

**React Router** es la librería estándar de facto para el routing en React. Utiliza componentes para definir la configuración de la ruta de forma declarativa dentro del JSX.

**Componentes Clave:**

|Componente|Utilidad|
|---|---|
|`<BrowserRouter>`|Envuelve la aplicación; usa la History API del navegador.|
|`<Routes>`|Define el espacio donde se encuentran todas las rutas.|
|`<Route>`|Mapea una URL (`path`) a un componente (`element`).|
|`<Link>`|Navega a una URL sin recargar la página (reemplaza a `<a>`).|
|`useParams()`|Hook para leer parámetros de rutas dinámicas (ej: `:id`).|
|`useNavigate()`|Hook para navegar programáticamente (ej: después de un login).|

Exportar a Hojas de cálculo

**Ejemplo Básico en React Router:**

JavaScript

```
// App.jsx
import { BrowserRouter, Routes, Route, Link, useParams, useNavigate } from 'react-router-dom';

function Home() { return <h1>Inicio</h1>; }
function DetalleProducto() { 
  const { id } = useParams(); // Hook para leer parámetros
  return <h2>Detalle Producto: {id}</h2>; 
}

function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Inicio</Link> | 
        <Link to="/producto/123">Producto 123</Link>
      </nav>
      
      {/* 1. Definición del espacio de las rutas */}
      <Routes> 
        {/* 2. Rutas estáticas y dinámicas */}
        <Route path="/" element={<Home />} />
        <Route path="/producto/:id" element={<DetalleProducto />} />
        <Route path="*" element={<h1>404 - No Encontrado</h1>} /> {/* 3. Ruta de fallback */}
      </Routes>
    </BrowserRouter>
  );
}
```

---

#### 2. Routing en Vue: Vue Router (v4+)

**Vue Router** es el router oficial y se integra muy bien con el ecosistema de Vue. Su configuración es centralizada en un archivo de configuración de JavaScript, a diferencia del enfoque basado en componentes de React.

**Componentes/Funciones Clave:**

|Componente/Función|Utilidad|
|---|---|
|`createRouter`|Crea la instancia del router con la configuración de rutas.|
|`<router-view>`|Componente donde se renderiza el componente de la ruta actual.|
|`<router-link>`|Navega sin recargar la página (reemplaza a `<a>`).|
|`useRoute()`|Composables para acceder a información de la ruta (parámetros, _query_).|
|`useRouter()`|Composables para navegación programática.|

Exportar a Hojas de cálculo

**Ejemplo Básico en Vue Router (Configuración):**

JavaScript

```
// router/index.js (Archivo de configuración)
import { createRouter, createWebHistory } from 'vue-router';
import Home from '../views/Home.vue';
import DetalleProducto from '../views/DetalleProducto.vue';

// 1. Array de rutas con sus componentes
const routes = [
  { path: '/', component: Home },
  { path: '/producto/:id', component: DetalleProducto, props: true }, // props: true para pasar :id como prop
  { path: '/:pathMatch(.*)*', redirect: '/404' } // Fallback
];

// 2. Crear instancia del router
const router = createRouter({
  history: createWebHistory(), // Usa la History API
  routes,
});

export default router;

// main.js (Uso en la app principal)
// import { createApp } from 'vue';
// import router from './router';
// createApp(App).use(router).mount('#app');
```

**Uso en el Componente Vue:**

HTML

```
<template>
  <nav>
    <router-link to="/">Inicio</router-link> | 
    <router-link :to="{ path: '/producto/123' }">Producto 123</router-link>
  </nav>
  <router-view></router-view> 
</template>

<script setup>
  // En DetalleProducto.vue
  // import { useRoute } from 'vue-router';
  // const route = useRoute();
  // console.log(route.params.id); // Acceder a los parámetros
</script>
```

---

#### 3. Routing en Svelte: SvelteKit (El Metodología de Archivos)

Svelte no tiene un router de librería separado, sino que su framework de desarrollo, **SvelteKit**, incluye el routing integrado. SvelteKit utiliza un enfoque de **routing basado en archivos (File-based Routing)**.

**Metodología Clave:**

- La estructura de carpetas dentro de la carpeta `src/routes` define las rutas de la aplicación.
    
- Un archivo `+page.svelte` dentro de una carpeta se convierte en la página de esa ruta.
    
- Los parámetros de ruta se definen usando nombres de carpeta entre corchetes `[...]`.
    

|Ruta en el Navegador|Estructura de Archivos|Componente Renderizado|
|---|---|---|
|`/`|`src/routes/+page.svelte`|`+page.svelte`|
|`/perfil/alice`|`src/routes/perfil/[slug]/+page.svelte`|`+page.svelte`|

Exportar a Hojas de cálculo

**Navegación y Lectura de Parámetros:**

- **Navegación:** Se usa la etiqueta `<a>` nativa. SvelteKit la intercepta automáticamente para evitar recargas.
    
- **Parámetros:** Se accede a ellos mediante el módulo `$page.params` (una store de Svelte) o cargándolos en un archivo `+page.js` o `+page.server.js`.
    

**Ejemplo de Lectura de Parámetros en SvelteKit:**

HTML

```
<script>
  import { page } from '$app/stores'; // Importar la store de la página

  // Acceder a los parámetros de la ruta
  // $page es una store reactiva, por lo que se usa $ para acceder a su valor
  let idProducto = $page.params.id; 
</script>

<h1>Detalle Producto Svelte: {idProducto}</h1>
<p>La navegación fue sin recarga, usando un enlace <a> nativo.</p>
```

---

### Rutas Anidadas (Nested Routing)

Las rutas anidadas permiten que un componente padre (_layout_) permanezca visible mientras solo cambia una sección de su contenido (_outlet_).

|Framework|Mecanismo|
|---|---|
|**React Router**|Se definen rutas hijas dentro del JSX de la ruta padre. El componente padre usa un `<Outlet />` para renderizar el componente hijo.|
|**Vue Router**|Se definen rutas anidadas en la configuración `routes` (usando la propiedad `children`). El componente padre usa un `<router-view>` dentro de su template.|
|**SvelteKit**|Se usan archivos `+layout.svelte`. Un layout envuelve todas las rutas en la misma carpeta y sus subcarpetas.|

Exportar a Hojas de cálculo

### Ejercicios Prácticos Paso a Paso

**Objetivo:** Implementar la navegación programática (redirección) después de un login simulado.

1. **Crea el escenario:** Un componente `Login` y un componente `Dashboard`.
    
2. **Lógica:** Al hacer click en "Login", usamos la función de navegación del router para ir a `/dashboard`.
    

**Implementación en React (useNavigate):**

JavaScript

```
import { useNavigate } from 'react-router-dom';

function Login() {
  const navigate = useNavigate(); // 1. Obtener la función de navegación

  const handleLogin = () => {
    // Lógica de autenticación...
    
    // 2. Navegar a la nueva ruta
    navigate('/dashboard'); 
  };

  return <button onClick={handleLogin}>Login</button>;
}
```

**Implementación en Vue (useRouter):**

HTML

```
<script setup>
  import { useRouter } from 'vue-router';
  const router = useRouter(); // 1. Obtener la instancia del router

  const handleLogin = () => {
    // Lógica de autenticación...
    
    // 2. Navegar usando el método .push()
    router.push('/dashboard'); 
  };
</script>
<template>
  <button @click="handleLogin">Login</button>
</template>
```

### Errores Comunes y Troubleshooting

- **React Router: Olvidar el Contexto:** Intentar usar `useNavigate` o `useParams` fuera de un componente que está envuelto por el `<BrowserRouter>`. El router necesita contexto.
    
- **Vue Router: Problemas de _Hash Mode_:** Usar el modo _hash_ (rutas con `#` como `midominio.com/#/acerca`) en producción cuando se quería el modo History. Asegúrate de configurar bien `createWebHistory` o `createWebHashHistory`.
    
- **SvelteKit: Errores de Archivo:** Nombrar mal el archivo de página (ej: `page.svelte` en lugar de `+page.svelte`) o la carpeta de parámetros (ej: `[id].svelte` en lugar de `[id]/+page.svelte`).
    

### Resumen de Puntos Clave

- **Routing** permite la navegación en SPAs sin recarga total de la página.
    
- **React** usa **React Router** y define las rutas de forma declarativa con componentes JSX como `<Route>` y `<Routes>`.
    
- **Vue** usa **Vue Router** y define las rutas de forma centralizada en un array de configuración JavaScript.
    
- **Svelte** (a través de **SvelteKit**) usa **Routing Basado en Archivos**, donde la estructura de carpetas define las rutas.
    
- El componente **`<Link>`** (React) o **`<router-link>`** (Vue) se usa para la navegación sin recarga.
    
- La navegación programática se logra con el hook **`useNavigate`** (React) o el composable **`useRouter().push()`** (Vue).
    
- Las **Rutas Dinámicas** permiten capturar parámetros de la URL para cargar contenido específico (ej: `/producto/:id`).
    

### Enlaces Internos

👉 La siguiente gran decisión arquitectónica es cómo manejar el estado compartido entre estas diferentes rutas: [[11 - State management global]].

👉 Las rutas suelen requerir datos antes de cargar el componente, un caso de uso avanzado de _fetching_ que se ve en [[13 - Fetch y consumo de APIs]].

---

**Siguiente paso:** La navegación está cubierta. El siguiente tema es esencial para aplicaciones grandes: **Gestión de Estado Global**. Pasemos a [[11 - State management global]].