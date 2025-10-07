Con nuestro entorno listo, es hora de sumergirnos en el lenguaje que usamos para describir las interfaces. Esta es la primera bifurcación importante entre los frameworks: **JSX vs. Templates**.

---

Aquí tienes los apuntes de la clase anterior: [[02 - Configuración del entorno con frameworks]]

## 03 - JSX y Templates

### Introducción al tema: Contexto y Utilidad

Cuando trabajamos con frameworks, ya no escribimos HTML directamente en un archivo `.html`. Necesitamos una manera de incrustar **lógica de JavaScript** (variables, bucles, condiciones) dentro de nuestra **estructura de interfaz** (el HTML).

Aquí es donde entran **JSX** (en React y SolidJS) y los **Templates** (en Vue y Svelte). Ambos cumplen el mismo propósito: describir la interfaz de forma declarativa, pero lo hacen con filosofías de sintaxis muy diferentes. Entender esto es clave para cambiar de framework sin esfuerzo.

### Prerrequisitos

- Entorno de desarrollo configurado con Vite (ver [[02 - Configuración del entorno]]).
    
- Comprensión del concepto de componente y declaratividad (ver [[01 - Introducción a Frontend Frameworks]]).
    
- Dominio de las funciones flecha y la sintaxis básica de ES6+.
    

### Explicación Completa y Extendida

#### 1. JSX: JavaScript XML (React y SolidJS)

**JSX** es una extensión de sintaxis para JavaScript que permite escribir estructuras de árbol (que parecen HTML) dentro de los archivos JS. No es HTML, ni es una cadena de texto; es una forma declarativa de llamar a funciones de JavaScript.

**Filosofía:** Integrar la lógica y la interfaz en un solo archivo (el componente). Si sabes JavaScript, sabes JSX.

**Características Clave de JSX:**

1. **Es JavaScript:** El código se ejecuta dentro del motor JS. Cuando se compila, JSX se transforma en llamadas a funciones (ej: `React.createElement()`).
    
2. **Incrustación de Lógica:** Para insertar variables o cualquier expresión JS, se usan **llaves `{}`**. Dentro de ellas puedes escribir cualquier JS válido.
    
3. **Restricciones de HTML:** Como JSX es JS, algunas palabras reservadas de HTML deben cambiarse:
    
    - `class` se convierte en `className`.
        
    - `for` (en etiquetas `<label>`) se convierte en `htmlFor`.
        
    - Los estilos _inline_ se escriben como objetos JS: `<div style={{ backgroundColor: 'red', fontSize: '14px' }}>`.
        
4. **Retorno Único:** Un componente React (una función) debe devolver un **único elemento raíz** (o usar un `<Fragment>` o `</>`).
    

**Ejemplo en React (JSX):**

JavaScript

```
import React from 'react';

function SaludoDinamico({ usuario, esAdmin }) { // 'esAdmin' viene de las props

  const mensaje = "¡Bienvenido a la aplicación!";
  
  return (
    // Se requiere un solo elemento raíz (el <div>)
    <div>
      {/* 1. Insertar variables JS usando {} */}
      <h1>Hola, {usuario || "Invitado"}</h1> 
      
      {/* 2. Lógica condicional (operador ternario dentro de {}) */}
      {esAdmin ? (
        <p className="admin-msg">Acceso de Administrador.</p> // className, no class
      ) : (
        <p>Usuario estándar.</p>
      )}

      {/* 3. Comentarios JS dentro de JSX */}
      {/* 4. Llamada a una función de JS */}
      <p>{mensaje.toUpperCase()}</p>
    </div>
  );
}
```

---

#### 2. Templates: HTML Extendido (Vue y Svelte)

Los **Templates** toman HTML estándar y le añaden directivas o sintaxis especiales que extienden sus capacidades para la reactividad y la lógica.

**Filosofía:** Separación de preocupaciones. HTML para la estructura, JS para la lógica, y CSS para el estilo, todo en un archivo. La lógica de la interfaz se "injerta" en el HTML usando atributos especiales.

##### **Vue (Templates)**

Vue utiliza un _Template Syntax_ que se parece mucho a HTML puro, pero con atributos especiales que empiezan por `v-` (directivas).

**Características Clave de Vue Templates:**

1. **Interpolación:** Se usan **dobles llaves `{{ }}`** para insertar variables de JavaScript.
    
2. **Directivas:** Usan atributos especiales para controlar la lógica:
    
    - `v-if`: Condicionales.
        
    - `v-for`: Iteración de listas.
        
    - `v-bind` o `:` (atajo): enlazar atributos HTML a variables JS.
        
    - `v-on` o `@` (atajo): manejar eventos.
        

**Ejemplo en Vue (Template):**

HTML

```
<template>
  <div>
    <h1>Hola, {{ usuario || 'Invitado' }}</h1> 
    
    <p v-if="esAdmin" class="admin-msg">Acceso de Administrador.</p>
    <p v-else>Usuario estándar.</p>
    
    <p>{{ mensaje.toUpperCase() }}</p>
  </div>
</template>

<script setup>
// Lógica de JS
const usuario = 'Alice';
const esAdmin = true;
const mensaje = "¡Bienvenido a la aplicación!";
// Vue maneja la reactividad de estas variables
</script>
```

##### **Svelte (Componentes de un solo archivo .svelte)**

Svelte usa un enfoque similar a los templates de Vue, pero es mucho más conciso y utiliza una sintaxis de etiquetas (`{}` y `#`, `:` para lógica).

**Características Clave de Svelte:**

1. **Lógica en `{}`:** Usa llaves `{}` para interpolación, al igual que JSX, pero **sin necesidad de `return`**.
    
2. **Etiquetas de Bloque:** Usa `#` para iniciar bloques de lógica, `:` para _else_, y `/` para cerrarlos.
    
    - `{#if ...}`: Condicionales.
        
    - `{#each ...}`: Iteración de listas.
        
3. **CSS Local por Defecto:** Los estilos dentro de la etiqueta `<style>` están automáticamente _scoped_ (solo afectan a ese componente).
    

**Ejemplo en Svelte:**

HTML

```
<script>
  // Lógica de JS
  let usuario = 'Alice';
  let esAdmin = true;
  let mensaje = "¡Bienvenido a la aplicación!";
</script>

<div>
  <h1>Hola, {usuario || 'Invitado'}</h1>
  
  {#if esAdmin}
    <p class="admin-msg">Acceso de Administrador.</p>
  {:else}
    <p>Usuario estándar.</p>
  {/if}
  
  <p>{mensaje.toUpperCase()}</p>
</div>
```

---

### Comparación de Sintaxis

|Característica|React (JSX)|Vue (Templates)|Svelte (Componentes)|
|---|---|---|---|
|**Incrustación de JS**|Llaves `{}`|Dobles llaves `{{ }}`|Llaves `{}`|
|**Condicionales**|Operador Ternario o Cortocircuito JS dentro de `{}`|Directiva `v-if`|Etiquetas de bloque `{#if}`|
|**Iteraciones (Loops)**|Función `map()` de JS dentro de `{}`|Directiva `v-for`|Etiqueta de bloque `{#each}`|
|**Clases CSS**|`className`|`class` normal (o `v-bind:class`)|`class` normal|
|**Filosofía**|JS es primario, el HTML es una abstracción de JS.|HTML es primario, la lógica de JS se inyecta con directivas.|Lógica de JS y HTML se mezclan de forma nativa.|

Exportar a Hojas de cálculo

### Ejercicios Prácticos Paso a Paso

**Objetivo:** Implementar un renderizado condicional en los tres estilos de sintaxis.

1. **Crea el escenario:** Queremos mostrar un botón "Eliminar" solo si el usuario que ve la página es el mismo que creó el contenido (`currentUser === creatorId`).
    
2. **React (JSX):** Usa el operador de cortocircuito (`&&`) dentro de las llaves.
    
    JavaScript
    
    ```
    // React JSX
    const currentUser = 1;
    const creatorId = 1;
    
    // Lógica dentro del return
    return (
      <div>
        {/* Si la condición es TRUE, renderiza el <button> */}
        {currentUser === creatorId && (
          <button onClick={handleDelete}>Eliminar</button>
        )}
      </div>
    );
    ```
    
3. **Vue (Template):** Usa la directiva `v-if`.
    
    HTML
    
    ```
    <template>
      <button v-if="currentUser === creatorId" @click="handleDelete">
        Eliminar
      </button>
    </template>
    
    <script setup>
    const currentUser = 1;
    const creatorId = 1;
    function handleDelete() { /* ... */ }
    </script>
    ```
    
4. **Svelte (Componente):** Usa la etiqueta de bloque `#if`.
    
    HTML
    
    ```
    <script>
      let currentUser = 1;
      let creatorId = 1;
      function handleDelete() { /* ... */ }
    </script>
    
    {#if currentUser === creatorId}
      <button on:click={handleDelete}>Eliminar</button>
    {/if}
    ```
    

### Errores Comunes y Troubleshooting

- **Error de JSX: Usar `class` en React:** Siempre usa `className`. Si usas `class`, React lo ignorará o te dará un _warning_.
    
- **Error de Templates: Poner lógica compleja:** Las plantillas de Vue/Svelte están diseñadas para lógica simple (condicionales, bucles). Si necesitas hacer transformaciones complejas de datos, hazlo **dentro de la etiqueta `<script>`** o en la función del componente de React, y luego pasa el resultado a la interfaz.
    
    - _Mala Práctica:_ `{{ calcularTotal(items) / descuento }}`
        
    - _Buena Práctica:_ Calcula el valor (`const totalFinal = calcularTotal(items) / descuento;`) y luego usa `{{ totalFinal }}`.
        
- **Error de React: Renderizar `undefined`:** Si una variable que insertas con `{}` es `undefined`, React no renderiza nada (lo cual suele estar bien). Pero si intentas renderizar `null`, `false` o `true` directamente, puede llevar a comportamientos inesperados (excepto `false` en renderizado condicional, que es común).
    

### Resumen de Puntos Clave

- **JSX** (React) es **JavaScript** con azúcar sintáctico; integra la lógica de interfaz directamente en el código JS mediante llaves `{}`.
    
- **Templates** (Vue/Svelte) extienden el HTML con **directivas** (`v-if`, `v-for`) o **etiquetas de bloque** (`{#if}`), manteniendo la lógica de JS separada en su bloque `<script>`.
    
- En JSX, la estructura debe retornar un **elemento raíz único** (o un Fragment).
    
- En JSX, los atributos de HTML deben usar **camelCase** (ej: `className`).
    
- La elección entre JSX y Templates es una cuestión de filosofía: ¿Prefieres que la interfaz sea **JS** (React) o que la lógica de JS se inyecte en el **HTML** (Vue/Svelte)?
    

### Enlaces Internos

👉 La base de la reutilización con estas sintaxis se explica en [[04 - Componentes básicos]].

👉 El manejo de la información que pasa entre estas sintaxis se ve en [[05 - Estado en componentes]] y [[06 - Props y comunicación entre componentes]].

---

**Siguiente paso:** Ya que comprendemos las sintaxis, es hora de ponerlas en práctica para crear la unidad fundamental de cualquier aplicación moderna: el **Componente Básico**. Pasemos a [[04 - Componentes básicos]].