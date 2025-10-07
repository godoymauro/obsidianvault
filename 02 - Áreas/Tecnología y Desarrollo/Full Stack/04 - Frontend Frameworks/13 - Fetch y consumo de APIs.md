Los datos son la razón de ser de la mayoría de las aplicaciones. Después de validar los formularios, el paso natural es enviar esos datos o solicitar información para mostrarlos. Vamos a dominar el **Fetching y Consumo de APIs**.

---

Aquí tienes los apuntes de la clase anterior: [[12 - Formularios y validación]]

## 13 - Fetch y Consumo de APIs

### Introducción al tema: Contexto y Utilidad

El **Fetching de Datos** se refiere al proceso de solicitar información de un servicio externo, típicamente una **API REST** o **GraphQL**, para poblar la interfaz de usuario o enviar datos (como un formulario).

Esta tarea es un **Efecto Secundario** por excelencia, ya que no forma parte del renderizado puro. Debe ser gestionada dentro del **Ciclo de Vida** del componente para asegurar que:

1. Se ejecuta **solo al montar** o cuando las dependencias cambian.
    
2. Maneja los estados de la petición: **Cargando (`loading`)**, **Éxito (`data`)** y **Error (`error`)**.
    
3. Se **limpia** si el componente se desmonta antes de que la petición termine (prevención de _memory leaks_).
    

La utilidad es clara: conectar nuestro Frontend (la vista) con el Backend (los datos).

### Prerrequisitos

- Dominio del Ciclo de Vida de Componentes (`useEffect`, `onMounted`, etc.) (ver [[08 - Ciclo de vida de componentes]]).
    
- Conocimiento de la gestión de Estado local (ver [[05 - Estado en componentes]]).
    
- Comprensión de las promesas de JavaScript (`async/await`, `then/catch`).
    

---

### Explicación Completa y Extendida

#### 1. Herramientas para el Fetching

Aunque la API nativa **`fetch`** es la base, las librerías son populares por añadir características clave:

|Herramienta|Mecanismo|Ventajas Clave|
|---|---|---|
|**`fetch` (Nativo)**|Promesa global del navegador.|No requiere instalación. Es ligero y moderno.|
|**Axios**|Librería popular basada en promesas.|Manejo automático de la transformación JSON, cancelación de peticiones, interceptores (para _tokens_).|
|**React Query / SWR**|Librerías de **Caching de Estado del Servidor**.|Gestión automática de _loading_, _caching_, re-fetching en el fondo, manejo de _stale data_. **Recomendado para React/Vue avanzado.**|

Exportar a Hojas de cálculo

#### 2. Implementación Manual con `fetch` (Base)

La implementación manual es el punto de partida, usando el _hook_ de ciclo de vida para controlar cuándo se ejecuta la petición.

##### React (`useEffect` para Fetching)

Se utiliza `useEffect` para iniciar la petición. Es crucial usar una **función de limpieza** para evitar errores.

JavaScript

```
import React, { useState, useEffect } from 'react';

function ListaProductos() {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    // 1. Uso del patrón AbortController para limpieza
    const controller = new AbortController(); 
    const signal = controller.signal;

    const fetchData = async () => {
      setLoading(true);
      try {
        const response = await fetch('/api/productos', { signal }); // Pasar la señal
        if (!response.ok) {
          throw new Error('Error al cargar los datos.');
        }
        const result = await response.json();
        setData(result);
        setError(null);
      } catch (err) {
        // Ignorar el error si es una cancelación (desmontaje)
        if (err.name !== 'AbortError') { 
          setError(err.message);
        }
      } finally {
        setLoading(false);
      }
    };

    fetchData();

    // 2. Función de limpieza: abortar la petición si el componente se desmonta
    return () => {
      controller.abort();
    };

  }, []); // Array vacío: se ejecuta solo al montar.
  
  // ... Renderizar loading, error o data
}
```

##### Vue (`onMounted` y `ref`)

En Vue, usamos `onMounted` para la inicialización y `ref` para el estado.

HTML

```
<script setup>
  import { ref, onMounted } from 'vue';

  const data = ref(null);
  const loading = ref(true);
  const error = ref(null);

  onMounted(async () => {
    loading.value = true;
    try {
      const response = await fetch('/api/productos');
      if (!response.ok) {
        throw new Error('Error de red');
      }
      data.value = await response.json();
    } catch (err) {
      error.value = err.message;
    } finally {
      loading.value = false;
    }
  });
</script>
<template>
  <div v-if="loading">Cargando Vue...</div>
  </template>
```

##### Svelte (`onMount` y promesas)

Svelte también usa `onMount` y variables `let` para el estado. Un patrón común es usar **bloques de promesas** (`{#await ...}`).

HTML

```
<script>
  import { onMount } from 'svelte';
  
  let promise = fetch('/api/productos').then(res => res.json());

  // También se puede gestionar con onMount para más control
  // let data = null;
  // onMount(async () => { data = await fetch... });
</script>

{#await promise}
  <p>Cargando Svelte...</p>
{:then productos}
  <ul>
    {#each productos as producto}
      <li>{producto.nombre}</li>
    {/each}
  </ul>
{:catch error}
  <p style="color: red;">Error: {error.message}</p>
{/await}
```

#### 3. El Enfoque Avanzado: Caching de Estado del Servidor

En aplicaciones grandes, el principal problema es la **gestión de la caché** y la sincronización con el servidor. **React Query** (o TanStack Query) y **SWR** (en React) o **Vue Query** (en Vue) automatizan el 90% del trabajo:

1. **Caché:** Guardan los resultados de las peticiones para que la siguiente vez que se solicite la misma _key_, se muestre la caché inmediatamente y se haga una petición en segundo plano (_stale-while-revalidate_).
    
2. **Re-Fetching:** Vuelven a solicitar datos automáticamente cuando la ventana del navegador se enfoca o la red se reconecta.
    
3. **Estado Global:** Los datos se vuelven **Estado del Servidor** (un tipo de estado global) que es accesible por cualquier componente.
    

**Ejemplo React con React Query (Hook):**

JavaScript

```
import { useQuery } from '@tanstack/react-query'; // La librería 'React Query'

// 1. Crear una función de fetch simple
const fetchProductos = async () => {
  const res = await fetch('/api/productos');
  return res.json();
};

function ListaProductosQuery() {
  // 2. Usar el hook useQuery
  const { data, isLoading, isError, error } = useQuery({
    queryKey: ['productos'], // La clave única para la caché
    queryFn: fetchProductos,
  });

  if (isLoading) return <div>Cargando Query...</div>;
  if (isError) return <div>Error: {error.message}</div>;

  return (
    <ul>
      {/* 3. Renderizar data (ya gestionada por la caché) */}
      {data.map(p => <li key={p.id}>{p.nombre}</li>)}
    </ul>
  );
}
```

#### 4. Envío de Datos (POST/PUT/DELETE)

Para enviar datos (ej: el resultado de un formulario), la estructura es similar, pero el uso de **Axios** es más práctico por su soporte para _interceptors_ y envío de JSON.

**Ejemplo Envío (Axios y React Query):**

React Query también provee `useMutation` para enviar datos y gestionar los estados de envío (`isPending`, `isSuccess`, `isError`).

JavaScript

```
import { useMutation, useQueryClient } from '@tanstack/react-query';
import axios from 'axios';

const crearProducto = async (nuevoProducto) => {
  const { data } = await axios.post('/api/productos', nuevoProducto);
  return data;
};

function FormularioCrear() {
  const queryClient = useQueryClient();
  
  // 1. Inicializar la mutación
  const mutation = useMutation({
    mutationFn: crearProducto,
    // 2. Invalidar la caché al tener éxito (refrescar la lista de productos)
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['productos'] }); 
    },
  });

  const handleSubmit = (formData) => {
    mutation.mutate(formData); // 3. Disparar la mutación
  };

  return (
    <form onSubmit={handleSubmit}>
      {/* ... inputs ... */}
      <button type="submit" disabled={mutation.isPending}>
        {mutation.isPending ? 'Enviando...' : 'Crear Producto'}
      </button>
      {mutation.isError && <p>Error al crear.</p>}
    </form>
  );
}
```

---

### Ejercicios Prácticos Paso a Paso

**Objetivo:** Implementar un botón "Recargar" que dispare la petición de datos nuevamente.

1. **Estado de Dependencia:** En la implementación manual (React `useEffect`), introduce un estado llamado `reFetchKey` (`useState(0)`).
    
2. **Dependencia:** Agrega `reFetchKey` al array de dependencias de `useEffect`.
    
3. **Botón:** Crea un botón que llame a `setReFetchKey(prev => prev + 1)` para forzar la re-ejecución del efecto.
    

**(En React Query, esto es trivial: simplemente llamar a la función `refetch()` que devuelve `useQuery`.)**

### Errores Comunes y Troubleshooting

- **Fugas de Memoria (Leaks):** Olvidar la limpieza (el `return () => controller.abort()` en `useEffect`) al usar `fetch` o `axios` en componentes con ciclo de vida.
    
- **Dependencias Faltantes (React):** Olvidar incluir una prop o estado en el array de dependencias de `useEffect` (ver [[08 - Ciclo de vida de componentes]]), lo que causa que la petición no se re-ejecute cuando debería.
    
- **Data Stale (Data Obsoleta):** Ver datos viejos después de una actualización (POST/PUT).
    
    - **Solución:** Usar librerías de _caching_ (React Query/SWR) y la función `invalidateQueries` para forzar la actualización después de una mutación.
        
- **Error de JSON:** Intentar llamar a `response.json()` cuando la respuesta del servidor es una cadena de texto (ej: un mensaje de error). Siempre verifica si `response.ok` es `true` antes de intentar parsear el JSON.
    

### Resumen de Puntos Clave

- El **Fetching de Datos** es un **Efecto Secundario** que debe gestionar los estados de **Loading**, **Data** y **Error**.
    
- **Implementación Manual:** Se logra usando `fetch` y el Hook de ciclo de vida (`useEffect` en React, `onMounted` en Vue/Svelte), requiriendo **limpieza** (ej: `AbortController`) para evitar _memory leaks_.
    
- **React Query/SWR/Vue Query:** Son la opción moderna. Abstraen el estado, manejan automáticamente el **caching**, el re-fetching y la sincronización con el estado del servidor.
    
- **Envío de Datos:** Se recomienda **Axios** para peticiones POST/PUT/DELETE por sus características avanzadas.
    
- El patrón **`useMutation`** de React Query es la forma más efectiva de manejar el envío de formularios y la invalidación de la caché.
    
- **Svelte** simplifica el estado de _loading_ y _error_ con el bloque nativo **`{#await promise}`**.
    

### Enlaces Internos

👉 El uso de `useQuery` o `useMutation` es un ejemplo avanzado de Hooks/Composables que vimos en [[09 - Hooks y composables]].

👉 La gestión de datos es más robusta si se centraliza el _loading_ y el _error_ en un Store global: [[11 - State management global]].

---

**Siguiente paso:** Ya tenemos la funcionalidad completa. El enfoque se mueve a la calidad: **Optimización y Buenas Prácticas**. Comenzaremos con el rendimiento. Pasemos a [[14 - Performance y lazy loading]].