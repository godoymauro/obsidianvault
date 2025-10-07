Ahora que nuestros componentes tienen **estado** (memoria interna), el siguiente desafío es la **comunicación**. ¿Cómo pasan datos los componentes padres a sus hijos, y cómo notifican los hijos a sus padres de un evento?

---

Aquí tienes los apuntes de la clase anterior: [[05 - Estado en componentes]]

## 06 - Props y Comunicación entre Componentes

### Introducción al tema: Contexto y Utilidad

La comunicación entre componentes es fundamental para construir cualquier interfaz que no sea trivial. En el desarrollo moderno, se rige por el principio del **Flujo de Datos Unidireccional (One-way Data Flow)**.

1. **Datos de Padre a Hijo (Props):** Es el método principal. El padre pasa datos y configuraciones al hijo a través de las **Propiedades (Props)**.
    
2. **Comunicación de Hijo a Padre (Eventos/Callbacks):** El hijo **nunca** modifica las _props_ directamente. En su lugar, el hijo llama a una función que el padre le pasó a través de una _prop_, o **emite un evento**, para notificar al padre sobre una acción.
    

La utilidad es la **previsibilidad** y **simplicidad** del flujo de datos. Si un dato cambia, sabemos exactamente dónde buscar la fuente del cambio: en el componente padre o en el estado propio del componente.

### Prerrequisitos

- Entendimiento de los Componentes Básicos (ver [[04 - Componentes básicos]]).
    
- Dominio del manejo del Estado local (`useState`, `ref`, `let`) (ver [[05 - Estado en componentes]]).
    

### Explicación Completa y Extendida

#### 1. De Padre a Hijo: Las Props

Las _props_ (abreviatura de propiedades) son objetos de solo lectura que un componente recibe de su componente padre. Son inmutables dentro del componente hijo.

**La Regla de Oro:** **Las props son de solo lectura (read-only) para el hijo.** Si el hijo necesita cambiar ese dato, debe notificar al padre.

##### React (Props como argumentos de función)

En React, las _props_ se reciben como el primer argumento de la función del componente.

JavaScript

```
// Componente Padre: App.jsx
function App() {
  const [mensaje, setMensaje] = useState("¡Hola desde el Padre!");

  // 1. Pasar el valor del estado como una prop
  return <ComponenteHijo texto={mensaje} />;
}

// Componente Hijo: ComponenteHijo.jsx
// 2. Recibir la prop a través de la desestructuración de argumentos
function ComponenteHijo({ texto }) { 
  return <h1>Mensaje recibido: {texto}</h1>;
  // SIEMPRE es inmutable: ¡NO HACER! texto = "Cambio";
}
```

##### Vue (Props con `defineProps`)

En Vue 3 con la Composition API, las _props_ se definen explícitamente usando `defineProps` en el `<script setup>`.

HTML

```
<template>
  <h1>Mensaje recibido: {{ texto }}</h1>
</template>

<script setup>
  import { defineProps } from 'vue';

  // 1. Definir las props que se esperan, incluyendo tipo (opcional pero recomendado)
  const props = defineProps({
    texto: {
      type: String,
      required: true,
      default: "Valor por defecto"
    }
  });

  // 2. Acceder a la prop como props.texto
</script>
```

##### Svelte (Props con `export let`)

Svelte utiliza una sintaxis muy concisa para definir _props_ y hacerlas accesibles en el _markup_: se usa `export let`.

HTML

```
<script>
  // 1. Declarar la variable con 'export let' la convierte en una prop
  export let texto; 
  // También puedes definir un valor por defecto: export let texto = "Default";
</script>

<h1>Mensaje recibido: {texto}</h1>
```

---

#### 2. De Hijo a Padre: Eventos y Callbacks

Dado que las _props_ son unidireccionales, si el hijo necesita modificar datos del padre (ej: actualizar el estado del contador del padre tras un click), debe invocar una función que el padre le proporcionó.

##### React (Callbacks como Props)

El padre pasa una función (_callback_) como una _prop_. El hijo simplemente la llama cuando ocurre el evento.

JavaScript

```
// Componente Padre: App.jsx
function App() {
  const [contador, setContador] = useState(0);

  // Función que el padre quiere que se ejecute
  const incrementarContador = () => {
    setContador(prev => prev + 1);
  };

  return (
    <div>
      <h2>Contador Padre: {contador}</h2>
      {/* 1. Pasar la función como prop al hijo */}
      <BotonIncremento alClick={incrementarContador} /> 
    </div>
  );
}

// Componente Hijo: BotonIncremento.jsx
function BotonIncremento({ alClick }) { // 2. Recibir el callback
  return (
    // 3. Llamar a la función cuando ocurre el evento
    <button onClick={alClick}>Incrementar al Padre</button> 
  );
}
```

##### Vue (Eventos Emmitidos con `defineEmits`)

Vue tiene un sistema de eventos nativo. El hijo **emite un evento personalizado** (o "emite") que el padre "escucha" con la directiva `@`.

HTML

```
<template>
  <button @click="emit('incremento')">Incrementar al Padre</button>
</template>

<script setup>
  import { defineEmits } from 'vue';

  // 1. Definir los eventos que este componente puede emitir
  const emit = defineEmits(['incremento']); 

  // La función es llamada por el @click del template.
  // El padre escuchará: <BotonIncremento @incremento="funcionPadre" />
</script>

<template>
  <h2>Contador Padre: {{ contador }}</h2>
  <BotonIncremento @incremento="incrementarContador" /> 
</template>
<script setup>
  import { ref } from 'vue';
  import BotonIncremento from './BotonIncremento.vue';
  const contador = ref(0);
  const incrementarContador = () => {
    contador.value++;
  };
</script>
```

##### Svelte (Eventos con `createEventDispatcher`)

Svelte usa un _dispatcher_ para crear y enviar eventos personalizados que el padre escucha de forma similar a Vue.

HTML

```
<script>
  import { createEventDispatcher } from 'svelte';

  // 1. Crear el dispatcher
  const dispatch = createEventDispatcher(); 

  const handleClick = () => {
    // 2. Disparar un evento llamado 'incremento'
    dispatch('incremento'); 
  };
</script>

<button on:click={handleClick}>Incrementar al Padre</button>

<script>
  import BotonIncremento from './BotonIncremento.svelte';
  let contador = 0;
  const incrementarContador = () => {
    contador++;
  };
</script>

<div>
  <h2>Contador Padre: {contador}</h2>
  <BotonIncremento on:incremento={incrementarContador} /> 
</div>
```

---

### Comunicación Bidireccional: El Caso Especial de los Formularios

La comunicación "normal" es unidireccional (Prop-Down, Event-Up). Sin embargo, en formularios, es muy común querer un enlace bidireccional (como el `v-model` de Vue o `bind:value` de Svelte).

**¿Cómo lo logra React?** React no tiene `v-model`. Simula el enlace bidireccional forzando al hijo a usar **dos _props_**: una para el valor (`value`) y otra para el evento de cambio (`onChange`).

**Ejemplo React (Input Controlado):**

JavaScript

```
// El Input es el "Hijo"
<input 
  value={valor} // Prop: el valor actual
  onChange={manejarCambio} // Prop: la función para actualizar el estado del padre
/>
// El padre gestiona el estado y se lo pasa al input, y el input llama a la función del padre.
```

**Vue y Svelte** abstraen esto en `v-model` y `bind:value`, haciéndolo más limpio:

HTML

```
<input v-model="valor" /> 

<input bind:value={valor} />
```

### Ejercicios Prácticos Paso a Paso

**Objetivo:** Crear un componente `Alerta` (Hijo) que se cierre cuando el usuario haga click, pero que el estado de si está visible lo maneje el Padre.

1. **Componente Padre (`App`):** Define un estado `isVisible` (`useState(true)` o `ref(true)`).
    
2. **`Prop-Down`:** Pasa `isVisible` y una función `cerrarAlerta` al hijo.
    
3. **Componente Hijo (`Alerta`):**
    
    - Recibe la prop `isVisible` y solo se renderiza si es `true`.
        
    - Recibe la prop `cerrarAlerta` (React) o define el evento `cerrar` (Vue/Svelte).
        
    - Llama al _callback_ o emite el evento al hacer click en el botón de cerrar.
        

### Errores Comunes y Troubleshooting

- **Mutación de Props (el error fundamental):** Intentar cambiar una _prop_ dentro del componente hijo. Esto resultará en un _warning_ en React y Vue, y en Svelte simplemente fallará al reaccionar.
    
    - **Solución:** Si necesitas modificar la _prop_, cópiala a un estado local (`useState` o `ref`) en el hijo.
        
- **Prop Drilling:** Pasar una _prop_ a través de muchos niveles de componentes intermedios que no la necesitan. Esto hace el código difícil de mantener.
    
    - **Solución:** Utilizar Context (React) o Inyectar/Proveir (Vue) o Stores (Svelte) para gestión de estado global (ver [[11 - State management global]]).
        
- **Renderizado Innecesario (React):** Pasar un nuevo _callback_ (`() => funcionPadre()`) directamente en la prop del JSX en cada render. Esto hace que React re-renderice el hijo innecesariamente, ya que el _callback_ es una "nueva" función cada vez.
    
    - **Solución:** En React, usa el Hook `useCallback` para memorizar la función _callback_ (ver [[09 - Hooks y composables]]).
        

### Resumen de Puntos Clave

- La comunicación se basa en el **Flujo de Datos Unidireccional** (de Padre a Hijo).
    
- **De Padre a Hijo:** Se usa el mecanismo de **Props** (propiedades de solo lectura).
    
- **De Hijo a Padre:** Se usan **Callbacks** (React) o **Eventos Personalizados** (`emit` en Vue, `dispatch` en Svelte).
    
- En **React**, los _callbacks_ se pasan como _props_, y el hijo los invoca en sus manejadores de eventos.
    
- En **Vue** y **Svelte**, el hijo **emite un evento** personalizado, y el padre lo "escucha" con una directiva (`@evento` o `on:evento`).
    
- El enlace bidireccional en formularios se simula en React con `value` y `onChange`, y se abstrae con **`v-model`** (Vue) y **`bind:value`** (Svelte).
    

### Enlaces Internos

👉 El paso lógico para entender cómo reaccionar a estos eventos del usuario es [[07 - Manejo de eventos]].

👉 El _Prop Drilling_ se resuelve con la gestión de estado avanzada en [[11 - State management global]].

---

**Siguiente paso:** Ya sabemos pasar datos. Ahora, enfoquémonos en la interacción directa con el usuario: cómo gestionar clics, entradas de texto y envíos de formularios. Pasemos a [[07 - Manejo de eventos]].