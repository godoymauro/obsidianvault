¡Continuemos con la arquitectura de nuestras aplicaciones! Si el _routing_ nos permite saltar entre "páginas", la **Gestión de Estado Global** nos permite compartir datos y lógica entre componentes que no tienen una relación directa (padre-hijo).

---

Aquí tienes los apuntes de la clase anterior: [[10 - Routing]]

## 11 - State management Global

### Introducción al tema: Contexto y Utilidad

En aplicaciones pequeñas, el estado local (`useState`, `ref`) y las _props_ son suficientes. Sin embargo, en aplicaciones grandes, se presenta el problema del **"Prop Drilling"**: la necesidad de pasar _props_ a través de múltiples niveles de componentes intermedios que no las necesitan.

El **State Management Global** resuelve esto. Es una solución que centraliza el estado de la aplicación en un lugar accesible para **cualquier componente**, sin importar su posición en el árbol.

La utilidad es:

1. **Eliminar Prop Drilling:** Los componentes acceden directamente a la data que necesitan.
    
2. **Centralización:** Simplifica la depuración y mantenimiento, ya que el estado vive en un único "lugar de verdad".
    
3. **Escalabilidad:** Permite que aplicaciones crezcan sin que la comunicación se vuelva inmanejable.
    

### Prerrequisitos

- Entendimiento de Props y el Flujo de Datos Unidireccional (ver [[06 - Props y comunicación entre componentes]]).
    
- Conocimiento de la composición de lógica con Hooks/Composables (ver [[09 - Hooks y composables]]).
    

---

### Explicación Completa y Extendida

#### El Patrón Central: Flux/Redux

Gran parte de la gestión de estado global se basa en el patrón **Flux**, popularizado por **Redux**. Este patrón establece un flujo estricto y unidireccional para gestionar los cambios de estado:

1. **Componente:** El usuario interactúa.
    
2. **Action (Acción):** Se despacha un objeto que describe lo que sucedió (ej: `LOGIN_USER`).
    
3. **Dispatcher (Redux/Store):** La acción se envía al Store.
    
4. **Reducer (Vuex/Pinia/Redux):** Una función pura que toma el **Estado Anterior** y la **Acción** para devolver un **Nuevo Estado**. (El estado nunca se muta directamente).
    
5. **Store (Fuente de Verdad):** Almacena el estado global.
    
6. **Componente:** Se suscribe a los cambios del Store y se re-renderiza.
    

---

#### 1. React: Context API y Librerías Ligeras (Zustand, Redux Toolkit)

Históricamente, React usaba Redux, una librería potente pero con mucha "boilerplate" (código repetitivo). Hoy, las soluciones ligeras dominan, aunque el concepto de Redux sigue siendo vital.

##### A. Context API (Solución Nativa para Estado Simple)

El **Context API** es la solución nativa de React. Permite "inyectar" un valor (el estado y las funciones de actualización) en el árbol de componentes, saltándose las _props_ intermedias. Es ideal para datos que cambian poco (ej: tema de la interfaz, información del usuario logeado).

- **Proveedor (`<Context.Provider>`):** Componente que envuelve el árbol y provee el valor.
    
- **Consumidor (`useContext`):** Hook para acceder al valor del contexto en cualquier componente descendiente.
    

**Limitación:** El Context API re-renderiza **todos** los componentes que lo consumen cuando el valor del contexto cambia, incluso si solo necesitan una pequeña porción del estado.

##### B. Zustand (La Opción Moderna, Simple y Escalable)

**Zustand** es una de las librerías más populares de 2025. Es simple, minimalista y usa una sintaxis basada en _hooks_. Resuelve la limitación de Context al usar un sistema de suscripción optimizado.

- **Concepto:** Crea _stores_ con una sintaxis concisa.
    
- **Uso:** Los componentes solo se re-renderizan si el **pedazo de estado específico** que están usando cambia.
    

**Ejemplo Básico Zustand (Store):**

JavaScript

```
// useAuthStore.js
import { create } from 'zustand';

export const useAuthStore = create((set) => ({
  isLoggedIn: false,
  user: null,
  
  // Función para actualizar el estado (como un Reducer simple)
  login: (userData) => set({ 
    isLoggedIn: true, 
    user: userData 
  }),
  logout: () => set({ 
    isLoggedIn: false, 
    user: null 
  }),
}));
```

**Uso en un Componente React:**

JavaScript

```
import { useAuthStore } from './useAuthStore';

function Header() {
  // Solo suscripción al valor 'isLoggedIn'
  const isLoggedIn = useAuthStore(state => state.isLoggedIn); 
  const logout = useAuthStore(state => state.logout); // Suscripción a la función

  return (
    <nav>
      {isLoggedIn ? (
        <button onClick={logout}>Cerrar Sesión</button>
      ) : (
        <span>Invitado</span>
      )}
    </nav>
  );
}
```

---

#### 2. Vue: Pinia (El Estándar Oficial)

**Pinia** reemplazó a Vuex (el router de Redux para Vue) como la librería de gestión de estado oficial de Vue 3. Es moderno, modular y usa una sintaxis basada en la Composition API que se siente muy natural para los desarrolladores de Vue.

- **Concepto:** Mantiene la idea de _stores_, pero con sintaxis limpia y fuertemente tipada.
    
- **Elementos Clave:**
    
    - `state`: El estado central, definido con funciones.
        
    - `actions`: Funciones para cambiar el estado (equivalente a _setters_ o _dispatchers_). Pueden ser asíncronas.
        
    - `getters`: Propiedades computadas para derivar estado (como filtros).
        

**Ejemplo Básico Pinia (Store):**

JavaScript

```
// stores/auth.js
import { defineStore } from 'pinia';

export const useAuthStore = defineStore('auth', {
  // 1. Estado (como data() en Vue)
  state: () => ({
    isLoggedIn: false,
    user: null,
  }),
  // 2. Getters (como computed properties en Vue)
  getters: {
    bienvenida: (state) => state.user ? `Bienvenido, ${state.user.nombre}` : 'Inicia sesión',
  },
  // 3. Acciones (como methods o mutations)
  actions: {
    login(userData) {
      this.isLoggedIn = true; // Mutación directa (Pinia lo permite)
      this.user = userData;
    },
    logout() {
      this.isLoggedIn = false;
      this.user = null;
    }
  }
});
```

**Uso en un Componente Vue:**

HTML

```
<script setup>
  import { useAuthStore } from '../stores/auth';
  const authStore = useAuthStore(); // 1. Obtener la store

  // 2. Acceder a un getter/estado: Se usa como una ref (sin .value, Pinia lo desenvuelve)
  // 3. Acceder a la función: Directamente
  const handleLogout = () => {
    authStore.logout(); 
  };
</script>

<template>
  <nav>
    <span>{{ authStore.bienvenida }}</span>
    <button v-if="authStore.isLoggedIn" @click="handleLogout">
      Cerrar Sesión
    </button>
  </nav>
</template>
```

---

#### 3. Svelte: Stores Reactivas

Svelte tiene un sistema de gestión de estado global integrado llamado **Stores**. Son simplemente objetos con los métodos `subscribe`, `set` y `update`.

- **Stores Mínimas:** Simples objetos para estados básicos (`writable`).
    
- **Reactividad Mágica:** Los componentes de Svelte pueden suscribirse a una _store_ usando el prefijo **`$`**. El compilador gestiona la suscripción y desuscripción.
    

**Ejemplo Básico Svelte (Store):**

JavaScript

```
// stores/auth.js
import { writable } from 'svelte/store';

// writable crea una store con set, update y subscribe
export const isLoggedIn = writable(false);
export const user = writable(null);

export function loginUser(userData) {
  isLoggedIn.set(true); // Método set para cambiar el valor
  user.set(userData);
}
export function logoutUser() {
  isLoggedIn.set(false);
  user.set(null);
}
```

**Uso en un Componente Svelte (Uso del `$`):**

HTML

```
<script>
  import { isLoggedIn, logoutUser } from '../stores/auth';
  
  // No hay necesidad de suscribirse manualmente. 
  // El compilador lo hace con el prefijo $.
  const handleLogout = () => {
    logoutUser();
  };
</script>

<nav>
  {#if $isLoggedIn}
    <button on:click={handleLogout}>Cerrar Sesión</button>
  {:else}
    <span>Invitado</span>
  {/if}
</nav>
```

---

### Tablas Comparativas de Soluciones Globales

|Framework|Solución Recomendada|Mecanismo de Consumo|Filosofía Central|
|---|---|---|---|
|**React**|**Zustand** (o Context para simple)|Hooks Personalizados (`useMiStore(select)`)|Minimalista, basado en _hooks_ de estado.|
|**Vue**|**Pinia**|Composables (`useMiStore()`) y acceso directo al estado.|Modular, tipado, basado en la Composition API.|
|**Svelte**|**Stores** (`writable`, `readable`)|Prefijo mágico `$` en el _markup_ y `<script>`.|Integrado en el lenguaje, máxima concisión.|

Exportar a Hojas de cálculo

### Ejercicios Prácticos Paso a Paso

**Objetivo:** Crear una funcionalidad de "Modo Oscuro" global utilizando la herramienta preferida de cada _framework_.

1. **Define el Estado:** Una variable booleana `isDark` (inicialmente `false`) y una función `toggleDark()`.
    
2. **Define la Suscripción:** Un componente `Header` que lea `isDark` para cambiar un ícono.
    
3. **Define el Controlador:** Un componente `BotonToggle` que llame a `toggleDark()`.
    

_(Los ejemplos anteriores de Zustand, Pinia y Svelte Stores muestran cómo crear este tipo de estado centralizado, usando `isLoggedIn` como `isDark`.)_

### Errores Comunes y Troubleshooting

- **Context API (React): Re-renderizado Excesivo:** Si usas Context para un estado complejo que cambia mucho, tendrás problemas de rendimiento.
    
    - **Solución:** Usar **Zustand** o **Redux Toolkit** para gestionar la selección de estado y evitar re-renders.
        
- **Mutación de Estado (Patrón Flux/Redux):** En los sistemas Flux puros (como Redux), el estado es inmutable. Intentar cambiarlo directamente es un error grave.
    
    - **Nota:** **Pinia** y **Zustand** relajan esta regla al permitir la mutación directa en sus _actions_, simplificando el código.
        
- **Fugas de Memoria (Stores de Svelte):** Si te suscribes manualmente a una _store_ en Svelte sin el prefijo `$` (ej: `store.subscribe(...)`), debes llamar a la función de desuscripción al desmontar el componente (`onDestroy`). El uso del `$` gestiona esto automáticamente.
    

### Resumen de Puntos Clave

- La **Gestión de Estado Global** resuelve el **Prop Drilling** y centraliza la **Fuente de Verdad** de la aplicación.
    
- **React** se mueve hacia soluciones ligeras como **Zustand** y la **Context API** para estado simple.
    
- **Zustand** permite suscripciones **selectivas** para optimizar el re-renderizado.
    
- **Vue** utiliza **Pinia** como su estándar oficial, el cual ofrece una sintaxis simple con `state`, `getters` y `actions`.
    
- **Svelte** utiliza **Stores Reactivas** integradas. El prefijo mágico **`$`** en el _markup_ maneja la suscripción y desuscripción.
    
- El patrón subyacente (Flux/Redux) promueve el flujo de datos unidireccional y la inmutabilidad (aunque Pinia/Zustand la relajan).
    

### Enlaces Internos

👉 La gestión de estado global es clave para manejar la data que viene de APIs (el siguiente paso lógico): [[13 - Fetch y consumo de APIs]].

👉 Otro tipo de datos que requiere estado global son los formularios complejos: [[12 - Formularios y validación]].

---

**Siguiente paso:** Hemos cubierto el corazón de la arquitectura. Ahora, abordaremos un tema práctico y fundamental para cualquier aplicación: la gestión de formularios. Pasemos a [[12 - Formularios y validación]].