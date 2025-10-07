## 01 - Introducción a Frontend Frameworks

Aquí encontraras todo el indice del curso en el mapa de contexto: [[04 - Frontend Frameworks/00 - MOC]]

### Introducción al tema: Contexto y Utilidad

En la era del desarrollo web moderno, la complejidad de las interfaces de usuario ha escalado drásticamente. Las aplicaciones de hoy no son solo documentos estáticos, sino **Aplicaciones de Una Sola Página (SPA)** o complejas interfaces que requieren gestionar grandes cantidades de **estado (data)** y reaccionar de manera eficiente a las interacciones del usuario.

Aquí es donde entran los **Frontend Frameworks** (o librerías como React). Su principal utilidad es abstraer las manipulaciones directas del **DOM (Document Object Model)**, proporcionando un modelo de programación declarativo y basado en componentes. Esto nos permite construir interfaces complejas, modulares y fáciles de mantener, que se actualizan automáticamente al cambiar la data.

### Prerrequisitos

- Conocimiento sólido de **HTML5** y **CSS3**.
    
- Dominio de **JavaScript (ES6+)**: manejo de variables (`let`, `const`), funciones flecha, promesas, `async/await`, y módulos (`import`/`export`).
    

### Explicación Completa y Extendida

#### ¿Qué es un Frontend Framework?

Un framework (o librería, siendo React técnicamente una librería, aunque a menudo se le llame framework por su ecosistema) es un conjunto de herramientas, convenciones y patrones que facilitan la construcción de interfaces de usuario interactivas.

La principal diferencia con **JavaScript vanilla (puro)** radica en cómo se gestiona el DOM:

|Característica|JavaScript Vanilla|Frontend Frameworks (React, Vue, Svelte)|
|---|---|---|
|**DOM**|Manipulación directa e imperativa (`document.getElementById()`, `.innerHTML = '...'`)|Manipulación indirecta y **declarativa** (usando un DOM Virtual o compilador)|
|**Estructura**|Depende del desarrollador, a menudo se vuelve spaghetti code en apps grandes.|**Basado en Componentes**, fomentando la modularidad y reutilización.|
|**Actualización**|El desarrollador debe rastrear y actualizar manualmente los elementos afectados.|El framework rastrea el estado y **actualiza solo lo necesario** (eficiencia).|

Exportar a Hojas de cálculo

#### La clave: Reactividad y el DOM Virtual

Los frameworks modernos logran la eficiencia y la simplicidad mediante la **reactividad** y, en el caso de React y Vue, el **DOM Virtual (VDOM)**.

1. **Reactividad:** Es la capacidad de la interfaz de usuario de responder automáticamente a los cambios en el estado de la aplicación.
    
2. **DOM Virtual (VDOM):** Una representación en memoria, ligera y abstracta del DOM real.
    

**¿Cómo funciona el VDOM (React/Vue)?**

1. Cuando el estado (data) de un componente cambia, React/Vue crea un nuevo VDOM.
    
2. Compara (o hace un **"diff"**) el nuevo VDOM con el VDOM anterior.
    
3. Identifica la **mínima cantidad de cambios** necesarios.
    
4. Aplica esos cambios al **DOM real**.
    

Esto es mucho más rápido que manipular directamente el DOM real, que es la operación más lenta en el navegador.

**El enfoque de Svelte (El Compilador)**

Svelte toma un camino diferente: **no usa VDOM**. En su lugar, es un **compilador** que traduce tus componentes a código JavaScript altamente optimizado en tiempo de _build_ (compilación). Este código tiene instrucciones quirúrgicas para manipular el DOM directamente, haciendo que las aplicaciones Svelte sean extremadamente ligeras y rápidas.

#### Frameworks Populares en 2025 y sus Roles

|Framework|Tipo Principal|Filosofía Clave|Cuándo usarlo|
|---|---|---|---|
|**React**|Librería (Meta)|**VDOM, JSX, Hooks**. Máxima flexibilidad y ecosistema.|Proyectos grandes, equipos con experiencia en JS, máxima personalización de la tooling.|
|**Vue**|Framework (Evan You)|**VDOM, Templates, Reactividad integrada**. Curva de aprendizaje suave.|Proyectos medianos a grandes, equipos que buscan estructura y sintaxis limpia.|
|**Svelte**|Compilador (Rich Harris)|**Sin VDOM, Componentes autocontenidos**. Rendimiento y ligereza.|Proyectos que priorizan el rendimiento puro y la menor cantidad de código posible.|
|**SolidJS**|Librería|**Sin VDOM, Reactividad granular**. Similar a React en sintaxis, más rápido en actualización.|Buscando la velocidad de Svelte con la sintaxis de Hooks de React.|

Exportar a Hojas de cálculo

### Ejemplos de Componente Básico (La base de todo)

Todos los frameworks se centran en los **Componentes**: piezas aisladas, reutilizables y autocontenidas de la interfaz.

**React (usando JSX)**

JavaScript

```
// 1. Definimos una función que retorna la interfaz (JSX)
function SaludoReact() {
  // 2. Aquí iría la lógica (aún sin estado)
  const nombre = "Mundo"; 
  
  // 3. Retorna la estructura (algo que parece HTML pero es JSX)
  return (
    <div>
      <h1>Hola, {nombre}!</h1>
      <p>Esto es un componente React.</p>
    </div>
  );
}

// Para que sea visible, se "renderiza" en el DOM real.
// ReactDOM.createRoot(document.getElementById('root')).render(<SaludoReact />);
```

**Vue (usando Templates)**

HTML

```
<template>
  <div>
    <h1>Hola, {{ nombre }}!</h1>
    <p>Esto es un componente Vue.</p>
  </div>
</template>

<script>
export default {
  // data() es donde Vue maneja el estado
  data() {
    return {
      nombre: 'Mundo' 
    }
  }
}
</script>
```

**Svelte (Componente de un solo archivo .svelte)**

HTML

```
<script>
  // La reactividad está integrada:
  let nombre = 'Mundo';
</script>

<div>
  <h1>Hola, {nombre}!</h1>
  <p>Esto es un componente Svelte.</p>
</div>

<style>
  /* El CSS es local por defecto */
  h1 {
    color: teal;
  }
</style>
```

### Ejercicios Prácticos Paso a Paso

**Objetivo:** Entender el concepto de Componente y Declaratividad.

1. **Imaginación:** Piensa en un componente "Tarjeta de Usuario". ¿Qué partes tiene? (Imagen, Nombre, Botón de Seguir).
    
2. **JS Vanilla:** Escribe mentalmente (o en papel) los pasos para crear esta tarjeta, añadir la imagen, el nombre, y luego adjuntar un _event listener_ al botón con JS vanilla. (Serán muchas líneas de `document.createElement`, `element.setAttribute`, etc.)
    
3. **Framework (React):** Ahora, crea un boceto de un componente React. Nota cómo solo describes **qué** quieres ver (declaratividad) y no **cómo** crear cada elemento del DOM (imperatividad).
    

JavaScript

```
// Boceto del Componente TarjetaUsuario en React
function TarjetaUsuario() {
  // En lugar de manipular el DOM, definimos la estructura
  return (
    <div className="card"> 
      <img src="avatar.jpg" alt="Avatar"/>
      <h2>Nombre de Usuario</h2>
      <button onClick={/* Función para seguir */}>Seguir</button>
    </div>
  );
}
// La interfaz se describe, no se construye paso a paso.
```

### Errores Comunes y Troubleshooting

- **Intentar Manipular el DOM directamente:** El error más común al pasar de vanilla JS a un framework. **Nunca** uses `document.getElementById` o jQuery para cambiar algo que el framework ya está gestionando. Esto rompe el VDOM y causa _bugs_ difíciles de rastrear.
    
- **Confundir HTML con JSX:** JSX (en React) parece HTML pero es JavaScript. Las clases se nombran `className`, no `class`. Los atributos deben usar _camelCase_ (`onClick`, no `onclick`).
    
- **Olvidar la Reactividad:** En frameworks como React o Vue, simplemente cambiar una variable (ej: `let count = 0; count = 1;`) **no** actualizará la interfaz automáticamente. Debes usar las funciones específicas del framework para gestionar el **estado** (como `useState` en React). Esto lo veremos en [[05 - Estado en componentes]].
    

### Resumen de Puntos Clave

- Los Frameworks Frontend resuelven la complejidad de las SPAs mediante un modelo de programación **declarativo** y basado en **componentes**.
    
- El desarrollo pasa de la manipulación directa del DOM (imperativo) a la descripción de la interfaz (declarativo).
    
- **React** y **Vue** usan un **DOM Virtual (VDOM)** para optimizar las actualizaciones del DOM real.
    
- **Svelte** es un **compilador** que genera código JS optimizado sin la sobrecarga del VDOM.
    
- Todos los frameworks se construyen a partir de **Componentes**: piezas reutilizables y aisladas de la UI.
    
- En React, usamos **JSX**, una extensión de sintaxis que permite escribir HTML dentro de JavaScript.
    
- El error clave es intentar manipular el DOM directamente; debemos dejar que el framework maneje las actualizaciones mediante el **estado**.
    

### Enlaces Internos

👉 La sintaxis de los templates se profundiza en [[03 - JSX y templates]].

👉 La base de la reutilización es la creación de piezas en [[04 - Componentes básicos]].

---

**Siguiente paso:** ¿Quieres que pasemos a la [[02 - Configuración del entorno con frameworks]] donde prepararemos las herramientas para empezar a codificar, o prefieres profundizar en la diferencia entre JSX y Templates?