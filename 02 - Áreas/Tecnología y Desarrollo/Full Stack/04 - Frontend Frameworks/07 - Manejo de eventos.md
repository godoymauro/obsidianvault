La interacción es el siguiente paso lógico. Ya sabemos pasar datos (Props) y gestionar la memoria (Estado). Ahora, usemos el **Manejo de Eventos** para que el usuario pueda interactuar con esos datos.

---

Aquí tienes los apuntes de la clase anterior: [[06 - Props y comunicación entre componentes]]

## 07 - Manejo de Eventos

### Introducción al tema: Contexto y Utilidad

El **Manejo de Eventos** es el proceso de capturar y responder a las interacciones del usuario con la interfaz (clics, entradas de teclado, envíos de formularios, etc.). Los frameworks Frontend abstraen la compleja API de eventos nativa del navegador para proporcionar una sintaxis limpia y consistente.

La utilidad es doble:

1. **Interactividad:** Permite que la aplicación cambie su estado o ejecute lógica en respuesta a las acciones del usuario.
    
2. **Abstracción:** Nos libera de tener que usar `addEventListener` y `removeEventListener` manualmente, ya que el framework se encarga de optimizar estos procesos por nosotros, lo que a menudo implica el uso de un **Sistema de Eventos Sintéticos** (como en React).
    

### Prerrequisitos

- Entendimiento de cómo modificar el estado (`useState`, `ref`, `let`) (ver [[05 - Estado en componentes]]).
    
- Conocimiento de la sintaxis básica de los componentes (ver [[04 - Componentes básicos]]).
    

---

### Explicación Completa y Extendida

#### 1. Manejo de Eventos en React (Eventos Sintéticos)

React no usa los eventos nativos del DOM directamente. Implementa un **Sistema de Eventos Sintéticos** que envuelve los eventos nativos del navegador para asegurar:

1. **Consistencia:** Los eventos funcionan igual en todos los navegadores.
    
2. **Rendimiento:** Implementa la **Delegación de Eventos** (todos los _listeners_ se adjuntan al documento o al elemento raíz, y el evento se propaga de vuelta).
    

**Sintaxis Clave:**

- **Nomenclatura:** Los nombres de eventos son **camelCase** (ej: `onClick`, `onChange`, `onSubmit`), a diferencia del HTML puro (`onclick`).
    
- **Manejadores (Handlers):** Se pasa la **referencia a una función de JavaScript** entre llaves `{}`.
    

JavaScript

```
function FormularioLogin() {
  const [usuario, setUsuario] = useState('');

  // El manejador recibe el objeto de Evento Sintético
  const manejarCambio = (e) => {
    // 1. Acceder al valor: e.target.value
    setUsuario(e.target.value);
  };

  const manejarEnvio = (e) => {
    // 2. Prevenir la recarga de página por defecto
    e.preventDefault(); 
    console.log(`Usuario a enviar: ${usuario}`);
  };

  return (
    // 3. Pasar la referencia de la función al evento
    <form onSubmit={manejarEnvio}> 
      <input type="text" onChange={manejarCambio} value={usuario} />
      <button type="submit">Enviar</button>
    </form>
  );
}
```

---

#### 2. Manejo de Eventos en Vue (Directiva `v-on` o `@`)

Vue utiliza el sistema de eventos nativo del navegador, mejorado con la directiva **`v-on`** o su atajo **`@`**. Su característica más útil son los **Modificadores de Eventos**.

**Sintaxis Clave:**

- **Directiva:** Se usa `@nombreEvento` (ej: `@click`, `@input`).
    
- **Modificadores:** Se añaden sufijos para simplificar la lógica (ej: `@click.once`, `@submit.prevent`).
    

HTML

```
<template>
  <form @submit.prevent="manejarEnvio"> 
    <input type="text" v-model="usuario" /> 
    <button type="submit">Enviar</button>
  </form>
  <button @click="alerta('Hola!')">Alerta</button>
</template>

<script setup>
  import { ref } from 'vue';
  const usuario = ref('');
  
  const manejarEnvio = () => {
    // El modificador .prevent ya evitó la recarga
    console.log(`Usuario a enviar: ${usuario.value}`);
  };

  const alerta = (msg) => {
    alert(msg);
  };
</script>
```

|Modificador Común (Vue)|Utilidad|Equivalente en JS/React|
|---|---|---|
|**`.prevent`**|Llama a `event.preventDefault()`|`e.preventDefault()` dentro del handler|
|**`.stop`**|Llama a `event.stopPropagation()`|`e.stopPropagation()` dentro del handler|
|**`.once`**|El handler se dispara solo una vez|Lógica manual en el handler para deshabilitarse|
|**`@keyup.enter`**|Solo se dispara si se presiona la tecla Enter|Lógica manual: `if (e.key === 'Enter')`|

Exportar a Hojas de cálculo

---

#### 3. Manejo de Eventos en Svelte

Svelte usa la sintaxis de eventos nativos con el prefijo **`on:`**. Al igual que Vue, Svelte también soporta **Modificadores de Eventos** para simplificar tareas comunes.

**Sintaxis Clave:**

- **Directiva:** Se usa `on:nombreEvento` (ej: `on:click`, `on:input`).
    
- **Modificadores:** Se añaden sufijos con el símbolo `|` (pipe) (ej: `on:click|once`, `on:submit|preventDefault`).
    

HTML

```
<script>
  let usuario = '';

  const manejarEnvio = () => {
    // El modificador |preventDefault ya evitó la recarga
    console.log(`Usuario a enviar: ${usuario}`);
  };

  const alerta = (msg) => {
    alert(msg);
  };
</script>

<div>
  <form on:submit|preventDefault={manejarEnvio}>
    <input type="text" bind:value={usuario} />
    <button type="submit">Enviar</button>
  </form>

  <button on:click={() => alerta('Hola!')}>Alerta</button> 
</div>
```

|Modificador Común (Svelte)|Utilidad|Equivalente en JS/React|
|---|---|---|
|**`|preventDefault`**|Llama a `event.preventDefault()`|
|**`|stopPropagation`**|Llama a `event.stopPropagation()`|
|**`|once`**|El handler se dispara solo una vez|
|**`|self`**|Solo se dispara si el evento viene del propio elemento, no de hijos|

Exportar a Hojas de cálculo

---

### El Objeto Evento (`e` o `event`)

En todos los frameworks, el manejador de eventos recibe un objeto de evento. Este objeto contiene información crucial sobre la interacción:

- **`e.target`:** El elemento del DOM que disparó el evento (ej: el input).
    
- **`e.currentTarget`:** El elemento al que se adjuntó el _listener_ (ej: el botón).
    
- **`e.preventDefault()`:** Detiene la acción por defecto del navegador (ej: el envío de un formulario).
    
- **`e.stopPropagation()`:** Evita que el evento burbujee a los elementos padres.
    

### Ejercicios Prácticos Paso a Paso

**Objetivo:** Crear un botón que cambie un estado _booleano_ (un _toggle_) y que, al hacer click, detenga la propagación del evento a un _div_ padre.

1. **Crea el Estado:** Define un estado booleano `activo` (inicialmente `false`).
    
2. **Crea el _Handler_ Padre:** Una función que imprima "Div clickeado" y cambie el estado `activo` a `false`.
    
3. **Crea el _Handler_ Hijo:** Una función que cambie el estado `activo` a `true` y detenga la propagación.
    

**Implementación en React:**

JavaScript

```
// React (se usa e.stopPropagation() manualmente)
function ToggleReact() {
  const [activo, setActivo] = useState(false);
  
  const handleDivClick = () => {
    console.log('Div clickeado - Desactivando');
    setActivo(false);
  };
  
  const handleButtonClick = (e) => {
    e.stopPropagation(); // Detener la propagación
    console.log('Botón clickeado - Activando');
    setActivo(true);
  };

  return (
    <div onClick={handleDivClick} style={{ padding: '20px', border: '1px solid black' }}>
      <p>Estado: {activo ? 'Activo' : 'Inactivo'}</p>
      <button onClick={handleButtonClick}>Activar y Detener Burbujeo</button>
    </div>
  );
}
```

### Errores Comunes y Troubleshooting

- **React: Llamar a la función en lugar de pasar la referencia:** `onClick={handleClick()}` (Malo) vs `onClick={handleClick}` (Bueno). El primero ejecuta la función inmediatamente durante el renderizado, el segundo pasa la referencia para que React la ejecute en el evento.
    
- **Vue/Svelte: Olvidar los Modificadores:** Intentar prevenir el envío de un formulario con JS manual en lugar de usar el modificador (`@submit.prevent` o `on:submit|preventDefault`), que es mucho más legible.
    
- **Perder el Contexto `this` (Solo en clases de React o JS antiguo):** Si usas clases o funciones tradicionales de JS, a menudo pierdes el contexto `this`. **Solución:** Usa funciones flecha para los _handlers_ (el estándar en React Funcional, Vue Composition API y Svelte).
    
- **Eventos de Elementos Customizados:** Si un componente hijo emite un evento customizado, React lo escucha con `onNombreEvento` y Vue/Svelte lo escuchan con `@nombre-evento` o `on:nombre-evento` (ver [[06 - Props y comunicación entre componentes]] sobre `emit`).
    

### Resumen de Puntos Clave

- El **Manejo de Eventos** permite a la aplicación responder a las interacciones del usuario.
    
- **React** utiliza **Eventos Sintéticos** (`onClick`, `onChange` en _camelCase_) y requiere manejar `e.preventDefault()` y `e.stopPropagation()` manualmente.
    
- **Vue** utiliza la directiva **`@`** (o `v-on`) y simplifica la lógica con **Modificadores de Eventos** (ej: `.prevent`, `.stop`).
    
- **Svelte** utiliza la sintaxis **`on:`** y también soporta **Modificadores de Eventos** (ej: `|preventDefault`, `|stopPropagation`).
    
- Es crucial pasar la **referencia a la función** como _handler_, no el resultado de su ejecución.
    
- El objeto de evento proporciona información clave como `e.target` y métodos de control como `e.preventDefault()`.
    

### Enlaces Internos

👉 La lógica que se ejecuta en estos _handlers_ a menudo involucra efectos secundarios (llamadas a APIs, interacciones con el DOM externo), lo cual nos lleva al [[08 - Ciclo de vida de componentes]].

👉 La vinculación bidireccional de formularios simplifica aún más la respuesta a eventos de _input_: [[12 - Formularios y validación]].

---

**Siguiente paso:** Ya podemos responder a los eventos. El siguiente paso es entender cuándo se ejecuta el código que depende de ese estado o evento: el **Ciclo de Vida de Componentes**. Pasemos a [[08 - Ciclo de vida de componentes]].