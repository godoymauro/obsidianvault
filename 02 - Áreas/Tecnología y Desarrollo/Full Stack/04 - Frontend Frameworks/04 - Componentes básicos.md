Hemos configurado el entorno y aprendido las sintaxis. Ahora, uniremos esos conocimientos para construir la pieza fundamental de cualquier aplicación moderna: el **Componente Básico**.

---

Aquí tienes los apuntes de la clase anterior: [[03 - JSX y templates]]

## 04 - Componentes Básicos

### Introducción al tema: Contexto y Utilidad

Los **Componentes** son la unidad atómica de cualquier aplicación de Frontend basada en _frameworks_. Piensa en ellos como las piezas de LEGO de tu interfaz. Un componente es una pieza de código independiente y reutilizable que encapsula su propia lógica, estructura (HTML/JSX) y, a veces, estilo (CSS).

La utilidad principal de los componentes es:

1. **Reutilización:** Escribes el código una vez y lo usas en múltiples lugares (ej: un botón o una tarjeta de producto).
    
2. **Mantenibilidad:** Los errores o cambios se aíslan dentro de ese componente, sin afectar el resto de la aplicación.
    
3. **Abstracción:** Puedes construir componentes complejos a partir de componentes más simples, creando una jerarquía clara.
    

### Prerrequisitos

- Entender la sintaxis de JSX y Templates (ver [[03 - JSX y templates]]).
    
- Saber cómo arrancar un proyecto con Vite (ver [[02 - Configuración del entorno]]).
    

### Explicación Completa y Extendida

#### 1. Tipos de Componentes y su Creación

Aunque los nombres varían ligeramente, todos los _frameworks_ distinguen entre dos tipos principales de componentes, basados en cómo se definen:

|Framework|Tipo de Componente|Definición|
|---|---|---|
|**React**|Componentes de Función (Recomendado)|Una función de JavaScript que devuelve JSX.|
|**Vue (3)**|Componentes con API de Composición (`<script setup>`)|Bloque `<script setup>` que define la lógica y retorna un template.|
|**Svelte**|Componentes de Archivo Único (`.svelte`)|Un archivo que contiene `<script>`, _markup_ y `<style>`.|

Exportar a Hojas de cálculo

#### 2. Componentes en React (Funcionales)

En React moderno, usamos **Componentes Funcionales**. Son simplemente funciones que, por convención, inician con mayúscula y devuelven una estructura JSX.

**Convención:** Los nombres de los componentes **deben** empezar con mayúscula (ej: `Boton`, `TarjetaUsuario`). Esto le indica a React que debe tratarlo como un componente personalizado y no como una etiqueta HTML nativa (`div`, `span`).

**Ejemplo: Componente `Boton` en React**

JavaScript

```
// src/components/Boton.jsx

// Componente de Función: recibe 'props' (propiedades) como un objeto.
export function Boton(props) { 
  // 1. Acceder a las propiedades (ej: el texto del botón)
  const texto = props.texto || "Click Aquí";
  
  // 2. Retornar la estructura JSX
  return (
    // 'className' en lugar de 'class'
    <button className="boton-primario" onClick={props.alHacerClick}>
      {texto} 
    </button>
  );
}

// Uso del componente en otro archivo (App.jsx)
// import { Boton } from './components/Boton';
// function App() {
//   return <Boton texto="Guardar Cambios" alHacerClick={() => alert('Guardado!')} />;
// }
```

#### 3. Componentes en Vue (Composition API)

Vue utiliza el **Single-File Component (SFC)**, donde la lógica, el _template_ y el estilo conviven en un archivo `.vue`. La forma moderna es usar el `<script setup>`.

**Ejemplo: Componente `Boton` en Vue**

HTML

```
<template>
  <button :class="tipoClase" @click="emitirClick">
    {{ texto || 'Click Aquí' }}
  </button>
</template>

<script setup>
  import { defineProps, defineEmits, computed } from 'vue';

  // 1. Definir las propiedades esperadas (Props)
  const props = defineProps({
    texto: String, 
    tipo: { // Ejemplo de prop con tipo y valor por defecto
      type: String, 
      default: 'primario'
    }
  });

  // 2. Definir eventos que este componente puede emitir
  const emit = defineEmits(['alHacerClick']);
  
  // Lógica de Vue: Clase dinámica basada en la prop 'tipo'
  const tipoClase = computed(() => `boton-${props.tipo}`);

  // Función para manejar el click y emitir el evento
  const emitirClick = () => {
    emit('alHacerClick');
  };
</script>

<style scoped>
.boton-primario { background-color: blue; color: white; }
/* ...otros estilos */
</style>
```

#### 4. Componentes en Svelte (SFC)

Svelte también usa el formato de Componente de Archivo Único (`.svelte`), pero es el más simple de los tres, ya que la reactividad y las _props_ son casi indistinguibles del JavaScript estándar.

**Ejemplo: Componente `Boton` en Svelte**

HTML

```
<script>
  // 1. Definir propiedades (Props): Se exportan. Svelte lo maneja.
  export let texto = "Click Aquí";
  export let tipo = "primario";

  // Función para manejar el click. No hay necesidad de 'emit' explícito 
  // para eventos nativos, se usa on:click.
  const handleClick = () => {
    // Para eventos personalizados, usaremos 'createEventDispatcher' (ver más adelante)
  };
</script>

<button class="boton-{tipo}" on:click={handleClick}>
  {texto}
</button>

<style>
/* El CSS es local por defecto, sin necesidad de 'scoped' */
.boton-primario { 
  background-color: blue; 
  color: white; 
}
</style>
```

---

### Componentes Anidados y Composición

El poder de los _frameworks_ viene de la **Composición de Componentes**, que es el acto de usar un componente dentro de otro. Esto crea una **jerarquía de componentes** (un árbol).

**Ejemplo de Composición (React):**

JavaScript

```
// Componente hijo: Titulo.jsx
function Titulo({ children }) {
  // 'children' es una prop especial de React que contiene el contenido 
  // que se anida entre las etiquetas del componente.
  return <h2>{children}</h2>;
}

// Componente padre: Tarjeta.jsx
import { Boton } from './Boton';
import { Titulo } from './Titulo';

function TarjetaUsuario({ nombre, cargo }) {
  return (
    <div className="tarjeta">
      {/* Usando el componente Titulo y pasando contenido como children */}
      <Titulo>{nombre}</Titulo> 
      <p>{cargo}</p>
      {/* Usando el componente Boton */}
      <Boton texto="Ver Perfil" alHacerClick={() => console.log('Navegando...')} />
    </div>
  );
}
```

### Buenas Prácticas: Principio de Responsabilidad Única

Un buen componente debe seguir el **Principio de Responsabilidad Única (SRP)**:

- **Componentes Puros (Presentacionales):** Se enfocan únicamente en la **apariencia** y no gestionan su propio _estado_ ni hacen llamadas a APIs. Simplemente reciben datos a través de _props_ y renderizan. (Ej: `Boton`, `Titulo`).
    
- **Componentes Contenedores (Inteligentes):** Se enfocan en la **lógica**, la gestión del _estado_, la conexión a APIs y la manipulación de datos. Renderizan a los componentes puros. (Ej: `TarjetaUsuarioContainer`).
    

**La regla de oro:** Cuanto más pequeño y especializado sea un componente, más fácil es de reutilizar y testear.

### Ejercicios Prácticos Paso a Paso

**Objetivo:** Crear una estructura de componente anidado.

1. **Crea el componente `Avatar` (Hijo):** Un componente que solo reciba una `url` y un `alt` por _prop_ y renderice un `<img>`.
    
    - _En React:_ `function Avatar({ url, alt }) { return <img src={url} alt={alt} />; }`
        
2. **Crea el componente `CabeceraPerfil` (Padre):** Este componente recibirá `nombre`, `urlAvatar` y `cargo`.
    
3. **Anidación:** Usa el componente `Avatar` dentro de `CabeceraPerfil`.
    

**Ejemplo (React):**

JavaScript

```
// 1. Componente Hijo (Avatar)
function Avatar({ url, alt }) {
  return <img src={url} alt={alt} style={{ width: 50, borderRadius: '50%' }} />;
}

// 2. Componente Padre (CabeceraPerfil)
function CabeceraPerfil({ nombre, urlAvatar, cargo }) {
  return (
    <header>
      {/* 3. Anidación y paso de props */}
      <Avatar url={urlAvatar} alt={`Avatar de ${nombre}`} /> 
      <div>
        <h1>{nombre}</h1>
        <p>{cargo}</p>
      </div>
    </header>
  );
}

// Uso:
// <CabeceraPerfil 
//   nombre="Alice Johnson" 
//   urlAvatar="https://..." 
//   cargo="Desarrolladora Senior" 
// />
```

### Errores Comunes y Troubleshooting

- **Olvidar la Mayúscula Inicial:** Si llamas a un componente React como `<tarjeta-usuario>` en lugar de `<TarjetaUsuario>`, React lo tratará como una etiqueta HTML simple y fallará al no encontrarla. **Siempre usa PascalCase para componentes.**
    
- **Problemas de _Scoping_ en CSS (Vue/Svelte):** Olvidar la etiqueta `<style scoped>` en Vue (o simplemente no usarla) hará que los estilos de ese componente se "escapen" e impacten a otros, rompiendo el aislamiento. En Svelte, por defecto, los estilos están _scoped_.
    
- **No Pasar Props:** Intentar acceder a una variable que debería venir del componente padre a través de _props_ (ej: `props.texto`) pero olvidar pasarla al usar el componente (`<Boton />` en lugar de `<Boton texto="Aceptar" />`).
    

### Resumen de Puntos Clave

- Los **Componentes** son la base de la reutilización y la mantenibilidad en Frontend.
    
- En **React**, los componentes son **Funciones** que devuelven JSX y usan `props` como argumento.
    
- En **Vue** y **Svelte**, se usa el formato de **Componente de Archivo Único** (SFC), que combina `<script>`, _template_ y `<style>`.
    
- El nombre de los componentes siempre debe comenzar con **Mayúscula** (PascalCase).
    
- La **Composición** es el principio de usar componentes dentro de otros, creando una jerarquía.
    
- Sigue el **Principio de Responsabilidad Única (SRP)**: separa los componentes en aquellos que solo _renderizan_ (puros) y aquellos que solo _gestionan la lógica_ (contenedores).
    

### Enlaces Internos

👉 La comunicación entre componentes usando `props` (la base de este capítulo) será el foco en [[06 - Props y comunicación entre componentes]].

👉 La siguiente etapa es dotar a estos componentes de memoria para que puedan ser dinámicos, lo que se logra con el **Estado** en [[05 - Estado en componentes]].

---

**Siguiente paso:** Hemos creado la estructura estática de nuestros componentes. Ahora, vamos a la parte más importante del desarrollo de aplicaciones: darle vida y memoria a estas piezas. Pasemos a [[05 - Estado en componentes]].