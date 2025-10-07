La estructura estática es la base, pero para crear aplicaciones interactivas, nuestros componentes necesitan **memoria**. Esa "memoria" se llama **Estado**.

---

Aquí tienes los apuntes de la clase anterior: [[04 - Componentes básicos]]

## 05 - Estado en Componentes

### Introducción al tema: Contexto y Utilidad

El **Estado (State)** es el corazón dinámico de cualquier aplicación frontend. Es un conjunto de datos que puede cambiar con el tiempo y que, cuando lo hace, provoca que la interfaz de usuario se **re-renderice** (actualice) automáticamente.

La utilidad del Estado es doble:

1. **Reactividad:** Garantiza que la interfaz siempre refleje el valor actual de los datos sin que tengamos que manipular el DOM manualmente.
    
2. **Interactividad:** Permite que los componentes respondan a las acciones del usuario (un click, una entrada de texto) cambiando su apariencia o comportamiento.
    

Si el componente es la pieza de LEGO, el Estado es el mecanismo que hace que esa pieza pueda moverse o cambiar de color.

### Prerrequisitos

- Dominio de la creación de Componentes Básicos (ver [[04 - Componentes básicos]]).
    
- Entendimiento de las funciones de JavaScript (para React Hooks).
    

### Explicación Completa y Extendida

#### El Paradigma del Flujo de Datos Unidireccional

En los frameworks modernos (especialmente React), el flujo de datos es **unidireccional** (de arriba abajo, del padre al hijo, o del estado a la interfaz).

1. **Estado Cambia:** El componente usa una función especial del framework para actualizar un valor del estado.
    
2. **Re-renderizado:** El framework detecta el cambio.
    
3. **Actualización del DOM:** El framework (usando VDOM o compilación) actualiza la interfaz solo en las partes necesarias.
    

La clave es que **nunca se debe modificar el estado directamente**. Siempre debemos usar las funciones de actualización proporcionadas por el framework para que este pueda rastrear el cambio y activar el re-renderizado.

---

#### 1. Estado en React: El Hook `useState`

React, al ser una librería basada en funciones, maneja el estado a través de funciones especiales llamadas **Hooks**. El principal para el estado local es `useState`.

**Características de `useState`:**

- Solo se puede llamar **dentro de un Componente Funcional** (o dentro de otro Hook personalizado).
    
- Devuelve un _array_ con **exactamente dos elementos**: el valor actual del estado y una función para actualizar ese valor.
    
- Al llamar a la función de actualización, React activa un **re-renderizado** del componente.
    

**Sintaxis de `useState`:**

JavaScript

```
import React, { useState } from 'react';

function Contador() {
  // 1. Desestructuración del array que devuelve useState
  //    'contador': valor actual del estado (ej: 0)
  //    'setContador': función que USAMOS para cambiar 'contador'
  const [contador, setContador] = useState(0); // 0 es el valor inicial
  
  const incrementar = () => {
    // 2. ¡NO modificar directamente! (Mal: contador = contador + 1)
    // 3. Usar la función de actualización.
    setContador(contador + 1); 
    
    // NOTA AVANZADA: Para actualizaciones que dependen del valor anterior, 
    // es más seguro usar una función de callback:
    // setContador(prevContador => prevContador + 1);
  };

  return (
    <div>
      <h1>Valor: {contador}</h1>
      <button onClick={incrementar}>Incrementar</button>
    </div>
  );
}
```

---

#### 2. Estado en Vue: La función `ref` (Composition API)

En Vue 3, la forma moderna de manejar el estado reactivo dentro del `<script setup>` es usando `ref` para valores simples (primitivos) o `reactive` para objetos/arrays.

**Características de `ref`:**

- Convierte un valor simple (número, cadena) en un **objeto reactivo** con una propiedad `.value`.
    
- Para leer o escribir su valor dentro del `<script>`, **siempre** hay que usar la propiedad `.value`.
    
- Dentro del `<template>`, Vue lo desenvuelve automáticamente, por lo que solo usamos el nombre de la variable.
    

**Ejemplo: Componente `Contador` en Vue**

HTML

```
<template>
  <div>
    <h1>Valor: {{ contador }}</h1>
    <button @click="incrementar">Incrementar</button>
  </div>
</template>

<script setup>
  import { ref } from 'vue';

  // 1. Declarar el estado reactivo usando ref
  const contador = ref(0); // El valor inicial está dentro del ref

  const incrementar = () => {
    // 2. Para modificar el valor en el script, DEBEMOS usar .value
    contador.value = contador.value + 1; 
    
    // Vue detecta este cambio y actualiza el template automáticamente.
  };
</script>
```

---

#### 3. Estado en Svelte: Variables Declaradas (Reactividad Integrada)

Svelte es el más simple. No necesita Hooks (`useState`) ni funciones como `ref`. Simplemente declaramos variables con `let` dentro de la etiqueta `<script>`. La reactividad está integrada en el compilador.

**Características de Svelte Reactivity:**

- Cualquier variable declarada con `let` en el `<script>` del componente es reactiva por defecto.
    
- El compilador rastrea las asignaciones (`=`) a esa variable.
    

**Ejemplo: Componente `Contador` en Svelte**

HTML

```
<script>
  // 1. Declarar el estado con let. ¡Eso es todo!
  let contador = 0; 

  const incrementar = () => {
    // 2. Modificar el valor directamente con una asignación
    contador = contador + 1; 
    
    // El compilador de Svelte genera el código para actualizar el DOM.
  };
</script>

<div>
  <h1>Valor: {contador}</h1>
  <button on:click={incrementar}>Incrementar</button>
</div>
```

---

### Tablas Comparativas de Sintaxis de Estado

|Framework|Mecanismo|Lectura (en Lógica)|Escritura (en Lógica)|
|---|---|---|---|
|**React**|`useState` Hook|Nombre de la variable (`contador`)|Función `set...` (`setContador(newValue)`)|
|**Vue**|`ref` (Composition API)|Objeto `.value` (`contador.value`)|Objeto `.value = newValue`|
|**Svelte**|Variables `let`|Nombre de la variable (`contador`)|Asignación directa (`contador = newValue`)|

Exportar a Hojas de cálculo

### Ejercicios Prácticos Paso a Paso

**Objetivo:** Crear un campo de texto que refleje el input del usuario en tiempo real.

#### **1. Implementación en React**

JavaScript

```
import { useState } from 'react';

function InputReactivo() {
  const [nombre, setNombre] = useState(''); // Estado inicial: cadena vacía

  const handleInput = (event) => {
    // El valor del input está en event.target.value
    setNombre(event.target.value);
  };

  return (
    <div>
      {/* El atributo 'value' (valor) y 'onChange' (evento) deben estar vinculados al estado */}
      <input type="text" value={nombre} onChange={handleInput} />
      <p>Hola, {nombre || 'desconocido'}!</p>
    </div>
  );
}
```

#### **2. Implementación en Vue (Directiva `v-model`)**

Vue simplifica la tarea anterior de vincular valor y evento usando la directiva **`v-model`** (enlace de doble sentido).

HTML

```
<template>
  <div>
    <input type="text" v-model="nombre" /> 
    <p>Hola, {{ nombre || 'desconocido' }}!</p>
  </div>
</template>

<script setup>
  import { ref } from 'vue';
  const nombre = ref('');
  // ¡No se necesita función handleInput! v-model lo hace por nosotros.
</script>
```

#### **3. Implementación en Svelte (Directiva `bind:value`)**

Svelte usa una sintaxis similar a `v-model` llamada `bind:` para lograr el enlace bidireccional.

HTML

```
<script>
  let nombre = '';
</script>

<div>
  <input type="text" bind:value={nombre} />
  <p>Hola, {nombre || 'desconocido'}!</p>
</div>
```

### Errores Comunes y Troubleshooting

- **Mutación Directa (React/Vue):** El error más grave es intentar cambiar el estado sin la función oficial.
    
    - _Mal React:_ `contador = 5;`
        
    - _Mal Vue:_ `contador.value.push('nuevo');` (Si el array es complejo, Vue puede perder la reactividad, aunque con `ref` en primitivos es difícil).
        
    - **Solución:** Siempre usa `setContador(...)` en React. Siempre reasigna un nuevo objeto/array en Vue si el anidamiento es profundo.
        
- **Estado Asíncrono (React):** La función `setContador` en React a menudo es asíncrona. Si necesitas hacer algo **inmediatamente** después de actualizar el estado, no confíes en el valor de `contador` justo después de `setContador`. En su lugar, usa `useEffect` (ver [[08 - Ciclo de vida de componentes]]).
    
- **Olvidar `.value` (Vue):** Intentar leer o escribir un ref en el `<script>` de Vue sin usar `.value` es un error común que hace que la reactividad se rompa.
    

### Resumen de Puntos Clave

- El **Estado** es la memoria interna y dinámica de un componente, que al cambiar, provoca el re-renderizado automático.
    
- En **React**, se gestiona con el **`useState` Hook**, que devuelve el valor y una función _setter_ para actualizarlo.
    
- En **Vue** (Composition API), se usa la función **`ref`**, y el valor se accede/modifica mediante la propiedad `.value` en la lógica.
    
- En **Svelte**, la reactividad es implícita; basta con declarar una variable con `let` y reasignarla para activar la actualización.
    
- **Nunca se muta el estado directamente**; siempre se usa la función _setter_ (React) o una reasignación (`=` en Svelte y a menudo en Vue).
    
- **Enlaces bidireccionales** como `v-model` (Vue) y `bind:value` (Svelte) simplifican la gestión de formularios.
    

### Enlaces Internos

👉 Ahora que el componente tiene estado local, el siguiente paso es cómo comparte esa información con su padre y sus hermanos: [[06 - Props y comunicación entre componentes]].

👉 La gestión de efectos secundarios y la inicialización del estado después de que el componente se monta se cubren en [[08 - Ciclo de vida de componentes]].

---

**Siguiente paso:** Entendemos el estado local. Pasemos a la comunicación: cómo fluye el Estado y las propiedades a través de la jerarquía de componentes. Vamos a [[06 - Props y comunicación entre componentes]].