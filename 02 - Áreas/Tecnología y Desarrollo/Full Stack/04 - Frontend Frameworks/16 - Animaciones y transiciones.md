Con la funcionalidad y la confiabilidad aseguradas, es hora de embellecer nuestra aplicación. El siguiente tema se centra en la experiencia visual: **Animaciones y Transiciones**, lo que da vida a la interfaz.

---

Aquí tienes los apuntes de la clase anterior: [[15 - Testing]]

## 16 - Animaciones y Transiciones

### Introducción al tema: Contexto y Utilidad

Las **Animaciones y Transiciones** son esenciales para una **Experiencia de Usuario (UX)** moderna. No solo son estéticas, sino que también cumplen funciones prácticas, como:

1. **Feedback Visual:** Indican al usuario que una acción ha tenido lugar (ej: un botón se presiona, un ítem se añade).
    
2. **Continuidad:** Ayudan al usuario a seguir el flujo de la aplicación durante los cambios de estado o la navegación, evitando saltos bruscos.
    
3. **Percepción de Velocidad:** Ocultan latencia percibida (ej: _spinners_ o animaciones de carga fluidas).
    

Aunque las animaciones simples se manejan con CSS puro, las interacciones complejas, el movimiento coordinado y la gestión del estado de la animación requieren la ayuda de librerías o características del _framework_.

### Prerrequisitos

- Conocimiento básico de CSS, incluyendo `transition` y `keyframes`.
    
- Dominio del manejo del Estado para disparar animaciones (ver [[05 - Estado en componentes]]).
    
- Entendimiento del Ciclo de Vida para limpiar animaciones (ver [[08 - Ciclo de vida de componentes]]).
    

---

### Explicación Completa y Extendida

La animación en Frontend se divide en tres enfoques, usados por todos los _frameworks_:

1. **Animación basada en CSS:** La más eficiente para transiciones simples.
    
2. **Animación basada en JS:** Permite control de fotograma a fotograma y lógicas complejas.
    
3. **Librerías Híbridas (Recomendadas):** Usan la capacidad de renderizado del _framework_ para manipular el CSS/DOM de manera optimizada (ej: Framer Motion).
    

#### 1. Animaciones en React: Framer Motion (La Opción Estándar)

React se beneficia enormemente de librerías de terceros que abstractizan la compleja manipulación del DOM y el manejo de eventos de animación. **Framer Motion** es la líder indiscutible, ofreciendo una API declarativa basada en componentes.

**Conceptos Clave de Framer Motion:**

- **Componentes `motion`:** Cualquier elemento HTML o componente React puede convertirse en un componente `motion` (ej: `<motion.div>`, `<motion.button>`).
    
- **Propiedades de Animación:** Se definen los estados de la animación como _props_ especiales:
    
    - `initial`: Estado inicial (antes de montar).
        
    - `animate`: Estado final (después de montar o al cambiar el estado).
        
    - `exit`: Estado al desmontar (requiere el componente `AnimatePresence`).
        
    - `whileHover`, `whileTap`: Estados disparados por interacción.
        

**Ejemplo React con Framer Motion (Transición de Montaje/Desmontaje):**

JavaScript

```
import { motion, AnimatePresence } from 'framer-motion';

function AlertaAnimada({ isVisible }) {
  return (
    // 1. AnimatePresence gestiona el desmontaje de componentes con 'exit'
    <AnimatePresence> 
      {isVisible && ( // 2. Renderizado condicional
        <motion.div
          initial={{ opacity: 0, y: -50 }} // 3. Estado inicial
          animate={{ opacity: 1, y: 0 }}   // 4. Estado de montaje (animación principal)
          exit={{ opacity: 0, scale: 0.8 }} // 5. Estado de salida (al desmontar)
          transition={{ duration: 0.5 }}    // 6. Configuración de la transición
          style={{ padding: '15px', background: 'red', color: 'white' }}
        >
          ¡Aviso Importante!
        </motion.div>
      )}
    </AnimatePresence>
  );
}
```

**Nota:** Para usar `exit` en React, el componente a animar (ej: `<motion.div>`) debe ser un hijo directo de `<AnimatePresence>` y debe usar el renderizado condicional.

#### 2. Animaciones en Vue: `<Transition>` y `<TransitionGroup>`

Vue ofrece componentes de alto nivel para gestionar las transiciones de CSS, lo que simplifica enormemente la animación de montaje/desmontaje.

- **`<Transition>`:** Envuelve un único elemento que entra y sale de la vista (para renderizado condicional: `v-if` o `v-show`).
    
- **`<TransitionGroup>`:** Envuelve una lista de elementos (para bucles: `v-for`) que entran, salen o cambian de posición (ideal para animaciones de listas).
    

**Mecanismo (Transición basada en CSS):**

Cuando el estado de un componente envuelto por `<Transition>` cambia, Vue añade y remueve automáticamente clases CSS en seis etapas:

|Clase|Descripción|
|---|---|
|`v-enter-from`|Estado inicial de entrada.|
|`v-enter-active`|Clase activa durante toda la transición de entrada (define `transition: all X.Xs`).|
|`v-enter-to`|Estado final de entrada.|
|`v-leave-from`|Estado inicial de salida.|
|`v-leave-active`|Clase activa durante toda la transición de salida.|
|`v-leave-to`|Estado final de salida.|

Exportar a Hojas de cálculo

**Ejemplo Vue (`<Transition>`):**

HTML

```
<template>
  <button @click="show = !show">Toggle</button>
  <Transition name="slide-fade">
    <div v-if="show" class="box">Contenido</div>
  </Transition>
</template>

<script setup>
  import { ref } from 'vue';
  const show = ref(true);
</script>

<style>
/* 2. CSS que define la transición y los estados */
.slide-fade-enter-active, 
.slide-fade-leave-active {
  transition: all 0.5s ease-out; /* Define la duración y curva */
}

/* Estado inicial de entrada (oculto y movido) */
.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: translateY(-20px);
  opacity: 0;
}
/* La transición animará de -from a -to (o viceversa) usando el transition definido en -active */
</style>
```

#### 3. Animaciones en Svelte: `transition:` y `in:`/`out:`

Svelte tiene el soporte de animación más conciso, ya que está integrado directamente en la sintaxis de la plantilla (similares a directivas). Usa el concepto de **acciones de transición** que se aplican a elementos.

**Mecanismo:** Svelte proporciona la palabra clave `transition:` o las variantes `in:` (entrada) y `out:` (salida), que pueden usar animaciones predefinidas o funciones personalizadas.

**Funciones de Transición Integradas:**

- `fade`
    
- `blur`
    
- `slide`
    
- `scale`
    
- `fly`
    

**Ejemplo Svelte (`in:` y `out:`):**

HTML

```
<script>
  import { slide } from 'svelte/transition'; // 1. Importar la función de transición
  let show = true;
</script>

<button on:click={() => show = !show}>Toggle</button>

{#if show}
  <div 
    in:slide="{{ duration: 400 }}" 
    out:slide="{{ duration: 300 }}" 
    class="box"
  >
    Contenido Svelte
  </div>
{/if}
```

**Animación de Listas:** Para animar la reordenación de listas, Svelte utiliza la directiva `animate:flip`, que aprovecha la técnica **FLIP (First, Last, Invert, Play)** para calcular el movimiento.

---

### Tablas Comparativas de Animación

|Framework|Animación de Montaje/Desmontaje|Animación de Interacción|Librería Estándar|
|---|---|---|---|
|**React**|`<Suspense>` + `React.lazy` (Funcional)|`whileHover`, `whileTap` (Framer Motion)|**Framer Motion**|
|**Vue**|`<Transition>`, `<TransitionGroup>` (Clases CSS)|Directivas de Vue (ej: `v-bind:style` dinámico)|GSAP / VueUse (Composables)|
|**Svelte**|`transition:`, `in:`, `out:` (Funciones integradas)|`tweened` stores (para valores animados)|Svelte FLIP (Integrado)|

Exportar a Hojas de cálculo

### Ejercicios Prácticos Paso a Paso

**Objetivo:** Implementar una transición simple de opacidad en una lista de elementos que aparecen secuencialmente.

1. **Crea el escenario:** Un _array_ de 3 elementos.
    
2. **Vue (`<TransitionGroup>`):** Envuelve el `v-for` en un `<TransitionGroup>`. Usa una clase de transición para la opacidad. Vue gestiona automáticamente el escalonamiento de la entrada.
    
3. **React (Framer Motion):** Define una variante en Framer Motion donde el `initial` y `animate` incluyen un `staggerChildren` para crear un retardo entre los elementos de la lista.
    

### Errores Comunes y Troubleshooting

- **Animación Rota por Renderizado Condicional:** En React, el error más común es no envolver el componente con **`<AnimatePresence>`** cuando se usa la prop `exit` de Framer Motion. Sin ella, el componente desaparece antes de que la animación `exit` pueda ejecutarse.
    
- **Vue: Clases CSS No Encontradas:** Olvidar añadir la prop `name="mi-nombre"` al componente `<Transition>` y luego no usar el prefijo correcto en el CSS (ej: usar `.v-enter-from` en lugar de `.mi-nombre-enter-from`).
    
- **Svelte: Animación de Posición:** Intentar animar la posición de una lista con `in:slide`. **Solución:** Usa `animate:flip` para animaciones de reordenamiento de listas, ya que está optimizado para ese propósito.
    
- **Animación Lenta (Jank):** Animar propiedades que fuerzan al navegador a redibujar todo (ej: `width`, `height`, `left`, `top`).
    
    - **Buena Práctica:** Siempre anima propiedades que son baratas: `transform` (para posición/escala) y `opacity`. Esto aprovecha la **aceleración por hardware**.
        

### Resumen de Puntos Clave

- Las animaciones mejoran la **UX** y la **percepción de velocidad**.
    
- **React** utiliza librerías de componentes declarativos como **Framer Motion**, que gestiona el montaje y desmontaje con **`<AnimatePresence>`**.
    
- **Vue** tiene componentes nativos **`<Transition>`** y **`<TransitionGroup>`** que gestionan la adición y eliminación automática de **clases CSS** para disparar transiciones.
    
- **Svelte** utiliza directivas de plantilla como **`transition:`**, **`in:`** y **`out:`**, que pueden usar funciones integradas como `slide` y `fade`.
    
- El mejor rendimiento se obtiene animando `transform` y `opacity`.
    
- Es crucial gestionar la animación de salida (_exit_) correctamente, asegurando que el componente permanezca en el DOM el tiempo suficiente para completar su animación.
    

### Enlaces Internos

👉 Las animaciones a menudo son parte de bibliotecas de componentes más grandes que veremos a continuación: [[17 - Component libraries y ecosistema]].

👉 El rendimiento es clave incluso en las animaciones; revisa las prácticas de optimización en [[14 - Performance y lazy loading]].

---

**Siguiente paso:** Hemos dotado a nuestra aplicación de belleza y fluidez. Ahora, hablaremos de cómo acelerar el desarrollo utilizando herramientas y ecosistemas preexistentes. Pasemos a [[17 - Component libraries y ecosistema]].