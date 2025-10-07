Hemos cubierto el estado y los eventos. Ahora, daremos un paso atrás para entender el _tempo_ de nuestra aplicación: el **Ciclo de Vida** del componente. Saber **cuándo** se ejecuta el código es tan importante como saber **qué** código escribir.

---

Aquí tienes los apuntes de la clase anterior: [[07 - Manejo de eventos]]

## 08 - Ciclo de Vida de Componentes

### Introducción al tema: Contexto y Utilidad

El **Ciclo de Vida de un Componente** se refiere a las diferentes fases que atraviesa un componente desde que nace hasta que muere. Estas fases nos permiten enganchar lógica en momentos específicos, lo que es esencial para manejar tareas fuera de la interfaz (llamadas a APIs, configuración de _timers_, manipulación manual del DOM, etc.). Estas tareas se conocen como **Efectos Secundarios (Side Effects)**.

Los frameworks proveen _hooks_ o métodos para ejecutar código en estos puntos clave:

1. **Montaje (Mounting):** El componente se crea y se inserta por primera vez en el DOM.
    
2. **Actualización (Updating):** El componente se re-renderiza debido a un cambio en su estado o en sus _props_.
    
3. **Desmontaje (Unmounting):** El componente se retira del DOM.
    

### Prerrequisitos

- Dominio del manejo del Estado (`useState`, `ref`, `let`) (ver [[05 - Estado en componentes]]).
    
- Comprensión del manejo de Eventos (ver [[07 - Manejo de eventos]]).
    

---

### Explicación Completa y Extendida

#### El Concepto de Efectos Secundarios

Un **Efecto Secundario** es cualquier operación que afecte el mundo fuera de la función de renderizado del componente. Ejemplos comunes:

- **Fetching de Datos:** Hacer una llamada `fetch` o `axios` a una API.
    
- **Suscripciones:** Configurar _listeners_ de eventos globales, _timers_ (`setTimeout`, `setInterval`).
    
- **Manipulación Directa del DOM:** Cambiar el título del documento o interactuar con librerías de terceros (ej: mapas de Google).
    

El ciclo de vida nos da la herramienta para manejar estos efectos de forma segura, asegurando que se ejecuten cuando el componente está presente y se limpien cuando desaparece.

---

#### 1. Ciclo de Vida en React: El Hook `useEffect`

React unificó todas las fases del ciclo de vida (montaje, actualización, desmontaje) en un solo Hook: **`useEffect`**. Su nombre es claro: úsalo para manejar efectos secundarios.

**Sintaxis de `useEffect`:**

JavaScript

```
useEffect(callback, [dependencias]); 
// 1. callback: La función que contiene la lógica del efecto secundario.
// 2. [dependencias]: Un array opcional de valores (props o estado) de los que depende el efecto.
```

|Fase de Ciclo de Vida|Sintaxis `useEffect`|Descripción|
|---|---|---|
|**Montaje** (`ComponentDidMount`)|`useEffect(() => { ... }, [])`|El _callback_ se ejecuta **solo una vez** después del primer render. El array vacío `[]` indica que no depende de nada.|
|**Actualización** (`ComponentDidUpdate`)|`useEffect(() => { ... }, [prop1, state2])`|El _callback_ se ejecuta después del render **solo si** cualquiera de los valores en el array de dependencias ha cambiado.|
|**Desmontaje** (`ComponentWillUnmount`)|`useEffect(() => { return () => { ... limpieza ... }; }, [])`|La función de **limpieza** (el `return` del _callback_) se ejecuta justo antes de que el componente sea retirado del DOM.|
|**Cada Render** (Evitar, si es posible)|`useEffect(() => { ... })`|Se ejecuta después de **cada** render (no tiene array de dependencias).|

Exportar a Hojas de cálculo

**Ejemplo React (`useEffect`): Fetching de Datos y Limpieza**

JavaScript

```
import React, { useState, useEffect } from 'react';

function PerfilUsuario({ idUsuario }) {
  const [datos, setDatos] = useState(null);

  useEffect(() => {
    // 1. Lógica del Efecto (Montaje y Actualización por idUsuario)
    console.log(`Buscando usuario ${idUsuario}...`);
    
    // Simulación de llamada a API
    fetch(`/api/users/${idUsuario}`)
      .then(res => res.json())
      .then(data => setDatos(data));

    // 2. Función de Limpieza (Desmontaje)
    // Se ejecuta al desmontar O antes de cada re-ejecución del efecto.
    return () => {
      console.log(`Limpando el efecto antes de cambiar de usuario o desmontar.`);
      // Aquí cancelaríamos la petición fetch o limpiaríamos timers.
    };

  }, [idUsuario]); // 3. Array de Dependencias: Solo se re-ejecuta si idUsuario cambia.

  return (
    // ... renderizar datos ...
  );
}
```

---

#### 2. Ciclo de Vida en Vue: Hooks de Ciclo de Vida (Composition API)

Vue 3 con la Composition API usa funciones que comienzan con `on...` para engancharse a los momentos específicos del ciclo de vida. Son más explícitos que `useEffect`.

**Sintaxis Clave:**

- `onMounted(callback)`
    
- `onUpdated(callback)`
    
- `onUnmounted(callback)`
    

**Ejemplo Vue:**

HTML

```
<script setup>
  import { ref, onMounted, onUnmounted, watch } from 'vue';

  const contador = ref(0);

  // Montaje: El componente acaba de ser insertado en el DOM
  onMounted(() => {
    console.log('Componente Vue ha sido montado. Iniciando data fetching...');
    // Iniciar timer, llamada a API, etc.
  });

  // Desmontaje: El componente está a punto de ser eliminado
  onUnmounted(() => {
    console.log('Componente Vue a punto de desmontarse. Limpiando recursos...');
    // Limpiar timer, remover listeners, etc.
  });

  // Observación (Equivalente a useEffect para una sola dependencia)
  // watch() permite reaccionar a cambios específicos sin re-renderizar todo
  watch(contador, (nuevoValor, viejoValor) => {
      console.log(`El contador cambió de ${viejoValor} a ${nuevoValor}`);
  });
</script>

<template>
  <button @click="contador++">{{ contador }}</button>
</template>
```

---

#### 3. Ciclo de Vida en Svelte: Funciones de Ciclo de Vida

Svelte, al igual que Vue, tiene funciones explícitas para el ciclo de vida, siendo estas importables desde el paquete `svelte`.

**Sintaxis Clave:**

- `onMount(callback)`
    
- `afterUpdate(callback)` (Se ejecuta después de que el DOM ha sido actualizado).
    
- `onDestroy(callback)`
    

**Ejemplo Svelte:**

HTML

```
<script>
  import { onMount, onDestroy } from 'svelte';

  let contador = 0;
  let intervalo;

  // Montaje: Se ejecuta después del primer render (onMount)
  onMount(() => {
    console.log('Svelte Componente montado. Iniciando intervalo...');
    
    // Iniciar timer
    intervalo = setInterval(() => {
      contador += 1; // La reactividad de Svelte actualiza el DOM
    }, 1000);
    
    // La función que retornamos aquí es la de 'limpieza' (onDestroy)
    return () => {
      console.log('Limpiando intervalo de Svelte.');
      clearInterval(intervalo);
    };
  });

  // También se puede usar onDestroy explícitamente si la limpieza es más compleja:
  onDestroy(() => {
    console.log('Svelte: ¡Adiós!');
  });
</script>

<h1>Contador: {contador}</h1>
```

---

### Tablas Comparativas de Ganchos de Ciclo de Vida

|Fase|React (Hooks)|Vue (Composition API)|Svelte (Funciones)|Propósito Común|
|---|---|---|---|---|
|**Montaje**|`useEffect(..., [])`|`onMounted()`|`onMount()`|Inicializar data (fetch), configurar listeners.|
|**Actualización**|`useEffect(..., [deps])`|`onUpdated()` o `watch()`|`afterUpdate()`|Sincronizar con APIs externas, reaccionar a cambios específicos.|
|**Desmontaje**|`return () => {}` dentro de `useEffect`|`onUnmounted()`|`onDestroy()`|Limpiar suscripciones, `clearInterval()`, remover _listeners_.|

Exportar a Hojas de cálculo

### Ejercicios Prácticos Paso a Paso

**Objetivo:** Implementar un efecto de título de documento en React y su limpieza.

1. **Componente React `TituloPagina`:**
    
2. **Efecto:** Usa `useEffect` para cambiar `document.title` al valor de una _prop_ `titulo`.
    
3. **Dependencia:** El efecto debe re-ejecutarse solo cuando cambie la prop `titulo`.
    
4. **Limpieza:** La función de limpieza debe restaurar el título a un valor por defecto ("App Frontend").
    

JavaScript

```
// React: useEffect con Dependencia y Limpieza
function TituloPagina({ titulo }) {
  useEffect(() => {
    const tituloAnterior = document.title; // Guardar el valor original
    document.title = titulo; // Configurar el nuevo título

    // Función de Limpieza
    return () => {
      document.title = tituloAnterior; // Restaurar el título al desmontar
    };
  }, [titulo]); // Se ejecuta si 'titulo' cambia

  return <h1>{titulo}</h1>;
}
```

### Errores Comunes y Troubleshooting

- **Missing Dependency (React):** El error más común. Si olvidas incluir una variable de estado o _prop_ dentro del array de dependencias (`[]`), tu `useEffect` usará un valor **obsoleto** de un render anterior. React ESLint avisa sobre esto.
    
    - **Solución:** Siempre incluye todas las variables usadas dentro del _callback_ en el array de dependencias.
        
- **Bucle Infinito (React):** Un `useEffect` que se ejecuta después del render y que, a su vez, actualiza el estado, provocando un nuevo render, lo que ejecuta el `useEffect` de nuevo...
    
    - **Solución:** Si el estado actualizado no depende del estado anterior, usa un array de dependencias vacío `[]`. Si depende del estado anterior, usa la forma de _setter_ con _callback_: `setContador(prev => prev + 1)`.
        
- **Olvidar la Limpieza:** No retornar la función de limpieza al configurar suscripciones o _timers_. Esto causa **fugas de memoria** (memory leaks) porque el _timer_ sigue ejecutándose o la suscripción sigue activa, aunque el componente ya no esté en el DOM.
    

### Resumen de Puntos Clave

- El **Ciclo de Vida** define cuándo se ejecuta la lógica del componente (Montaje, Actualización, Desmontaje).
    
- Los **Efectos Secundarios** son tareas fuera del renderizado (fetch de data, timers, manipulación del DOM) que deben manejarse con el ciclo de vida.
    
- **React** usa el **`useEffect` Hook** para todas las fases, controlando la re-ejecución con el **Array de Dependencias** (`[]`).
    
- **Vue** usa ganchos de función explícitos como **`onMounted`** y **`onUnmounted`**.
    
- **Svelte** usa funciones explícitas como **`onMount`** y **`onDestroy`**.
    
- La **Función de Limpieza** (el `return` en `useEffect` o `onUnmounted/onDestroy`) es crucial para prevenir **fugas de memoria**.
    

### Enlaces Internos

👉 El poder de `useEffect` en React se complementa con otros Hooks avanzados que veremos en [[09 - Hooks y composables]].

👉 El _fetching_ de datos (la aplicación más común del Montaje) se tratará en detalle en [[13 - Fetch y consumo de APIs]].

---

**Siguiente paso:** Hemos terminado los fundamentos de estado y ciclo de vida. Ahora entramos al Nivel 3: **Avanzado y Prácticas Modernas**. El próximo módulo profundiza en el poder de los Hooks y las funciones de composición. Pasemos a [[09 - Hooks y composables]].