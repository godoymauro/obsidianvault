Hemos visto las metodologías de organización, incluyendo el papel de JavaScript en el moderno CSS-in-JS. Ahora, nos enfocaremos en cómo JavaScript interactúa con el CSS en el modelo tradicional, manipulando los estilos de manera eficiente y dinámica.

Aquí tienes la vigesimocuarta clase.

---

Aquí tienes los apuntes de la clase anterior: [[23 - Metodologías modernas]]

# 24 - Integración de CSS con JavaScript

## 📌 Introducción

En cualquier aplicación web moderna, JavaScript (JS) es el motor que controla la **interactividad** y el **estado** de los elementos. CSS, por su parte, es el lenguaje que define la **apariencia** de esos estados.

La integración de CSS con JS permite crear interfaces dinámicas, como menús que aparecen al hacer clic, validación de formularios que cambia el color de los campos, o temas que cambian de claro a oscuro. El principio fundamental es: **JS debe manipular el _estado_ del elemento (clases o variables), y CSS debe manejar la _apariencia_ de ese estado.**

---

## 💻 Manipulación con `classList` (Método Preferido)

La forma más limpia y eficiente de cambiar estilos con JavaScript es manipular el atributo `class` del elemento. En lugar de cambiar directamente una propiedad CSS (como `elemento.style.color = 'red';`), JavaScript añade o elimina una clase predefinida en tu hoja de estilos.

### 1. ¿Por qué usar `classList`?

|Método|Cómo Cambia Estilos|Mantenibilidad|
|---|---|---|
|**`elemento.style.propiedad`**|Estilos en Línea (Alto en Especificidad)|Baja: Mezcla lógica (JS) y apariencia (CSS). Difícil de depurar.|
|**`elemento.classList`**|Estilos en Hojas de Estilo (Baja Especificidad)|**Alta:** JS solo controla el estado, CSS controla el diseño. Fácil de depurar.|

Exportar a Hojas de cálculo

### 2. Métodos Clave de `classList`

La propiedad `classList` es una colección (una lista) de todas las clases de un elemento.

|Método|Descripción|Ejemplo de Uso|
|---|---|---|
|**`add()`**|Añade una o más clases a la lista del elemento.|`nav.classList.add('nav--active');`|
|**`remove()`**|Elimina una o más clases de la lista.|`nav.classList.remove('nav--hidden');`|
|**`toggle()`**|Si la clase existe, la elimina; si no existe, la añade.|`menu.classList.toggle('is-open');` (Ideal para botones de menú)|
|**`contains()`**|Verifica si una clase existe y devuelve `true` o `false`.|`if (caja.classList.contains('error')) { ... }`|

Exportar a Hojas de cálculo

### ✍️ Ejemplo Práctico: Menú Desplegable

**HTML**

HTML

```
<button id="menu-btn">Menú</button>
<nav id="menu"></nav>
```

**CSS**

CSS

```
/* Estado inicial: oculto */
#menu {
    max-height: 0;
    opacity: 0;
    overflow: hidden;
    transition: all 0.5s ease-out; /* Animación de la transición (ver [[16 - Transiciones en CSS]]) */
}

/* Estado activo: visible y abierto */
.is-open {
    max-height: 300px !important; /* Necesitamos altura para ver la transición */
    opacity: 1 !important;
}
```

**JavaScript**

JavaScript

```
const menuBtn = document.getElementById('menu-btn');
const menu = document.getElementById('menu');

menuBtn.addEventListener('click', () => {
    // JS solo controla el estado (la clase), CSS controla la animación y apariencia.
    menu.classList.toggle('is-open'); 
});
```

---

## 🎨 Estilos Dinámicos con Variables CSS (Custom Properties)

Como vimos en [[19 - Variables en CSS (Custom Properties)]], JavaScript puede manipular directamente las variables CSS. Esto es perfecto para cambios de diseño sutiles o ajustes basados en la entrada del usuario (ej: un selector de color o tamaño de fuente).

### Manipulación de Variables CSS con JS

JavaScript

```
// 1. Seleccionamos el elemento raíz (donde definimos las variables globales)
const root = document.documentElement; // Es el elemento <html>

// 2. Función para cambiar el color primario de todo el sitio
function setThemeColor(newColor) {
    // Establece el valor de la variable --color-primario
    root.style.setProperty('--color-primario', newColor);
}

// 3. Uso
setThemeColor('#e74c3c'); // El nuevo color rojo se aplica a todos los elementos que usan var(--color-primario)
```

**Ventajas:** Permite a JavaScript cambiar docenas de propiedades de CSS (ej: `background-color`, `border-color`, `padding`) en múltiples elementos con **una sola línea de código** de JS, sin tocar ninguna clase en el HTML.

---

## 🚫 Manipulación Directa de `style` (Uso Limitado)

Aunque desaconsejada para cambios de apariencia generales, hay situaciones donde la manipulación directa de `elemento.style.propiedad` es necesaria:

1. **Valores Calculados:** Cuando la posición o el tamaño dependen de cálculos o datos de JS (ej: arrastrar y soltar un elemento).
    
    JavaScript
    
    ```
    elemento.style.left = `${posicionX}px`; 
    ```
    
2. **Propiedades Únicas/Temporales:** Para estilos muy puntuales y temporales.
    
    JavaScript
    
    ```
    // Aplicar una rotación temporal
    elemento.style.transform = 'rotate(180deg)';
    ```
    

**Recuerda:** Los estilos aplicados con `elemento.style` son estilos **en línea** (ver [[01 - Introducción a CSS]]) y tienen una **especificidad altísima** (ver [[04 - Especificidad y herencia]]), lo que significa que anularán casi cualquier otra regla CSS.

---

## ✅ Resumen de Puntos Clave

- La mejor práctica es que **JS controle el estado** y **CSS controle la apariencia**.
    
- El método preferido para la integración es manipular las clases de un elemento usando la propiedad **`classList`**.
    
- Los métodos **`add()`, `remove()`,** y especialmente **`toggle()`** son esenciales para cambiar el estado de un elemento (ej: `menu.classList.toggle('is-open')`).
    
- JavaScript puede manipular directamente las **Variables CSS (`--var`)** usando `element.style.setProperty()`, lo cual es ideal para cambiar temas o tamaños de forma global.
    
- La manipulación directa de `element.style.propiedad` debe limitarse a valores dinámicos calculados por JS (ej: posición `left`) debido a su alta especificidad.
    

---

👉 Has aprendido la teoría y las mejores prácticas. El siguiente y último paso es aplicar todos estos conocimientos en un proyecto de principio a fin, consolidando todo lo aprendido. Continúa con [[25 - Proyecto final Página completa]].