Terminamos los fundamentos (Estado y Ciclo de Vida) y ahora entramos al nivel avanzado. La abstracción de lógica compleja se logra con **Hooks** en React y **Composables** en Vue, conceptos que elevan tu código a otro nivel.

---

Aquí tienes los apuntes de la clase anterior: [[08 - Ciclo de vida de componentes]]

## 09 - Hooks y Composables

### Introducción al tema: Contexto y Utilidad

Los **Hooks (React)** y los **Composables (Vue)** son funciones especiales que permiten encapsular y reutilizar la lógica de los componentes de manera limpia y modular, sin necesidad de usar clases.

Antes de los Hooks, React usaba **Componentes de Clase** con métodos de ciclo de vida complejos. Ahora, estas funciones nos permiten:

1. **Reutilización de Lógica:** Compartir lógica de estado o de ciclo de vida entre varios componentes sin el patrón de _render props_ o HOCs (High-Order Components).
    
2. **Claridad:** Organizar la lógica dentro de un componente por **preocupaciones** (ej: lógica de _fetch_, lógica de formularios) en lugar de por métodos de ciclo de vida.
    
3. **Simplicidad:** Mantener el código más plano y fácil de leer.
    

### Prerrequisitos

- Entendimiento del `useState` y `useEffect` en React (ver [[05 - Estado en componentes]] y [[08 - Ciclo de vida de componentes]]).
    
- Conocimiento de las Props y la comunicación entre componentes (ver [[06 - Props y comunicación entre componentes]]).
    

---

### Explicación Completa y Extendida

#### 1. Hooks en React: La Reglas Fundamentales

Un **Hook** es una función que empieza con el prefijo `use` (ej: `useState`, `useEffect`). Los Hooks incorporan el estado y el ciclo de vida de React dentro de componentes funcionales.

**Hooks Fundamentales:**

|Hook|Propósito|Visto en|
|---|---|---|
|`useState`|Añadir estado reactivo local.|[[05 - Estado en componentes]]|
|`useEffect`|Ejecutar efectos secundarios y limpieza.|[[08 - Ciclo de vida de componentes]]|
|`useContext`|Suscribirse al contexto de React (estado global).|[[11 - State management global]]|

Exportar a Hojas de cálculo

**Hooks de Rendimiento/Optimización:**

|Hook|Propósito|
|---|---|
|`useMemo`|**Memorizar** el resultado de una función costosa. Solo recalcula si sus dependencias cambian.|
|`useCallback`|**Memorizar** una función (_callback_). Previene que las funciones pasadas como _props_ cambien en cada render.|
|`useRef`|Crear una referencia mutable que **no provoca re-renderizado** al cambiar (útil para referenciar elementos del DOM o guardar valores).|

Exportar a Hojas de cálculo

##### La Regla de Oro de los Hooks (Muy Importante)

1. **Solo Llamar en la Cima:** Solo se llaman Hooks en el nivel superior de un componente funcional o de otro Hook personalizado. **Nunca** dentro de bucles, condicionales, o funciones anidadas.
    
2. **Solo Llamar desde Componentes Funcionales (o Custom Hooks):** Los Hooks no deben llamarse desde funciones regulares de JavaScript.
    

#### 💡 Hooks Personalizados (Custom Hooks)

El verdadero poder de los Hooks reside en la capacidad de crear nuestros propios Hooks (**Custom Hooks**) para reutilizar lógica. Un Custom Hook es simplemente una función de JavaScript que usa otros Hooks y cuyo nombre empieza con `use`.

**Ejemplo de Custom Hook en React: `useFetch`**

Este hook encapsula la lógica de fetching de datos, _loading_ y errores.

JavaScript

```
// useFetch.js
import { useState, useEffect } from 'react';

// Se llama use... y usa Hooks internos
function useFetch(url) { 
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await fetch(url);
        const json = await response.json();
        setData(json);
      } catch (e) {
        setError(e);
      } finally {
        setLoading(false);
      }
    };
    fetchData();
  }, [url]); // Se re-ejecuta si la URL cambia

  return { data, loading, error };
}

// Uso en un componente React
function Perfil() {
  const { data, loading, error } = useFetch('/api/perfil/1');

  if (loading) return <div>Cargando...</div>;
  if (error) return <div>Error: {error.message}</div>;

  return <h1>Hola, {data.nombre}</h1>;
}
```

---

#### 2. Composables en Vue (Composition API)

En Vue 3, el equivalente a los Custom Hooks son los **Composables**. Son funciones de JavaScript que aprovechan las APIs de composición de Vue (`ref`, `reactive`, `onMounted`, `watch`, etc.) para encapsular lógica reactiva.

**Características de los Composables:**

- **Convención:** Aunque no es una regla estricta, la mayoría de los composables también empiezan con `use` (ej: `useMousePosition`, `useFetch`).
    
- **Simplificación:** No hay reglas de posición estricta como en React; se llaman simplemente como funciones.
    
- **Retorno:** Un _composable_ a menudo devuelve un objeto o un _array_ de variables reactivas (`ref`) y funciones.
    

**Ejemplo de Composable en Vue: `useFetch`**

JavaScript

```
// useFetch.js
import { ref, onMounted } from 'vue';

// No lleva prefijo 'use' en la importación, pero sigue la convención en el nombre
export function useFetch(url) {
  const data = ref(null);
  const loading = ref(true);
  const error = ref(null);

  const fetchData = async () => {
    try {
      const response = await fetch(url);
      const json = await response.json();
      data.value = json; // Usar .value
    } catch (e) {
      error.value = e;
    } finally {
      loading.value = false;
    }
  };

  onMounted(() => {
    fetchData(); // Usar el hook de ciclo de vida
  });

  // Retornar los refs y otras funciones/estados
  return { data, loading, error, fetchData }; 
}

// Uso en un componente Vue (.vue)
<script setup>
  import { useFetch } from './useFetch';
  const { data, loading, error } = useFetch('/api/perfil/1');
</script>

<template>
  <div v-if="loading">Cargando...</div>
  <div v-else-if="error">Error: {{ error.message }}</div>
  <h1 v-else>Hola, {{ data.nombre }}</h1>
</template>
```

---

#### 3. Svelte: Módulos y Stores (Patrón Ligeramente Diferente)

Svelte, al ser un compilador, no necesita Hooks ni Composables de la misma manera. La lógica reutilizable a menudo se abstrae en:

1. **Módulos de JavaScript simples:** Funciones utilitarias.
    
2. **Svelte Stores:** Objetos reactivos fuera del componente que permiten a los componentes suscribirse a sus cambios (Ver [[11 - State management global]]).
    

Si quieres abstraer la lógica reactiva en Svelte, lo harías creando una **Store Personalizada**.

### Comparación de Conceptos

|Característica|React (Hooks Personalizados)|Vue (Composables)|
|---|---|---|
|**Definición**|Función que usa Hooks React (empieza con `use`).|Función que usa APIs de composición (`ref`, `onMounted`, etc.).|
|**Reglas**|Reglas estrictas: solo en la cima del componente o de otro Hook.|Sin reglas de posición estrictas; se llaman como funciones normales.|
|**Valor de Retorno**|El estado y los _setters_ (`{ data, setData }`).|Variables reactivas (`ref`) y funciones.|
|**Propósito**|Reutilizar la lógica de estado y efectos secundarios.|Reutilizar lógica reactiva, de ciclo de vida y estado.|

Exportar a Hojas de cálculo

### Ejercicios Prácticos Paso a Paso: `useToggle`

**Objetivo:** Crear un Hook/Composable que maneje un estado booleano simple (`true/false`) y una función para cambiarlo.

1. **Definición:** Crea una función llamada `useToggle`.
    
2. **Lógica:** Usará estado (`useState` en React, `ref` en Vue) y una función `toggle` que invierte el valor.
    
3. **Retorno:** Devuelve el valor actual y la función `toggle`.
    

**Implementación en React (Hook):**

JavaScript

```
// useToggle.js
import { useState } from 'react';

export function useToggle(initialValue = false) {
  const [value, setValue] = useState(initialValue);
  
  const toggle = () => {
    setValue(prev => !prev); // Invertir el valor
  };
  
  return [value, toggle]; // Devolver como array (convención de React)
}
// Uso: const [abierto, setAbierto] = useToggle(false);
```

**Implementación en Vue (Composable):**

JavaScript

```
// useToggle.js
import { ref } from 'vue';

export function useToggle(initialValue = false) {
  const value = ref(initialValue);
  
  const toggle = () => {
    value.value = !value.value; // Invertir el valor usando .value
  };
  
  return { value, toggle }; // Devolver como objeto
}
// Uso: const { value, toggle } = useToggle(false);
```

### Errores Comunes y Troubleshooting

- **Violación de Reglas (React):** Intentar llamar un Hook dentro de un `if` o un `for`. Esto rompe el orden de los Hooks y causa errores difíciles de depurar.
    
- **Vue: Olvidar Desestructurar o Usar `.value`:** Si un _composable_ devuelve un `ref`, debes acceder a él con `.value` en el `<script>` y desestructurarlo correctamente si no está ya desenvuelto.
    
- **Sobrecarga de Hooks/Composables:** No usar la abstracción por el simple hecho de usarla. Si la lógica es única para un componente y simple, déjala en el componente. Solo abstraer cuando hay potencial de **reutilización**.
    

### Resumen de Puntos Clave

- Los **Hooks (React)** y **Composables (Vue)** son funciones para reutilizar la lógica de estado y ciclo de vida entre componentes.
    
- Los **Custom Hooks** en React deben seguir dos reglas estrictas de posición (solo en la cima).
    
- Los **Composables** en Vue usan las APIs de composición (`ref`, `onMounted`) y son más flexibles en su llamado.
    
- **`useMemo`** y **`useCallback`** son cruciales en React para optimizar el rendimiento y evitar re-renders innecesarios al pasar _props_ o realizar cálculos costosos.
    
- La abstracción de lógica compleja, como el _fetching_ de datos o la lógica de _toggle_, es el caso de uso principal.
    

### Enlaces Internos

👉 Un caso de uso avanzado de Hooks/Composables es el manejo de datos asíncronos, que cubriremos en [[13 - Fetch y consumo de APIs]].

👉 Cuando el estado se vuelve demasiado complejo para gestionarlo localmente, los Hooks/Composables nos llevan a la gestión de estado global: [[11 - State management global]].

---

**Siguiente paso:** La gestión de lógica compleja está clara. Ahora, vamos a la navegación entre "páginas" de nuestra SPA. Pasemos a [[10 - Routing]].