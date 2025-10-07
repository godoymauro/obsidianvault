Hemos aprendido a dar movimiento. Ahora, vamos a la propiedad que es vital para la **organización, la mantenibilidad y la escalabilidad** de tu CSS: las **Variables** o **Propiedades Personalizadas**.

Aquí tienes la decimonovena clase.

---

Aquí tienes los apuntes de la clase anterior: [[18 - Transformaciones]]

# 19 - Variables en CSS (Custom Properties)

## 📌 Introducción

Las **Variables CSS** (oficialmente llamadas _Custom Properties_ o Propiedades Personalizadas) permiten almacenar un valor en un lugar, darle un nombre significativo, y reutilizar ese valor a lo largo de tu hoja de estilos. Esto transforma el CSS, que solía ser un lenguaje estático y repetitivo, en algo más dinámico, modular y fácil de mantener.

Piensa en ellas como las variables que conoces de programación, pero que viven y son accesibles directamente en tu código CSS. Son esenciales para el diseño a gran escala y el _responsive design_ moderno.

## 🛠️ Sintaxis: Definición y Uso

El uso de variables CSS se divide en dos pasos clave: la **definición** y la **utilización**.

### 1. Definición (Declaración)

Las variables se definen siempre comenzando con doble guion medio (`--`).

Sintaxis: –nombre-de-variable:valor;

Es una práctica estándar definir las variables globales en el selector `:root`. El selector `:root` apunta al elemento `<html>` y asegura que las variables sean accesibles desde cualquier lugar de la hoja de estilos, gracias a la herencia (ver [[04 - Especificidad y herencia]]).

CSS

```
:root {
    /* Colores */
    --color-primario: #3498db;      /* Azul */
    --color-secundario: #2ecc71;    /* Verde */
    --color-texto-claro: white;
    
    /* Tipografía y Espaciado */
    --fuente-principal: 'Arial', sans-serif;
    --espacio-base: 1rem; /* 16px */
}
```

### 2. Utilización (Llamada)

Para usar el valor almacenado en la variable, empleamos la función **`var()`**.

Sintaxis: propiedad:var(–nombre-de-variable);

CSS

```
/* Usando las variables definidas arriba */
body {
    background-color: var(--color-primario);
    font-family: var(--fuente-principal);
}

.boton {
    background-color: var(--color-secundario);
    color: var(--color-texto-claro);
    padding: var(--espacio-base); /* 1rem */
}
```

---

## 🌳 Ámbito (Scope) y Herencia

Las variables CSS respetan el flujo de herencia normal de CSS.

1. **Ámbito Global:** Si defines una variable en `:root`, está disponible para **todo** el documento.
    
2. **Ámbito Local:** Si defines una variable dentro de un selector específico (ej: `.tarjeta`), solo estará disponible para ese elemento y sus elementos descendientes (hijos, nietos, etc.).
    

**Ejemplo de Ámbito Local:**

CSS

```
/* La variable --color-tarjeta solo existe dentro de la .tarjeta y sus descendientes */
.tarjeta {
    --color-tarjeta: #f0f0f0;
    background-color: var(--color-tarjeta);
    padding: 20px;
}

.tarjeta-titulo {
    /* Aquí se hereda --color-tarjeta del padre (.tarjeta) */
    border-bottom: 1px solid var(--color-tarjeta); 
}
```

## ⚙️ Valores de Reserva (Fallback Values)

La función `var()` te permite definir un valor alternativo o de **reserva** (fallback) en caso de que la variable no esté definida o no sea válida. Esto mejora la robustez del código.

Sintaxis: var(–nombre-de-variable,valor-de-reserva);

CSS

```
.elemento {
    /* Si --color-especial no existe, usa #333 (gris oscuro) */
    color: var(--color-especial, #333); 
}
```

---

## 💡 Buenas Prácticas y Ventajas

### 1. Mantenibilidad y Temas

Si decides cambiar el color principal de tu sitio, solo necesitas modificar una línea de código en `:root`. Sin variables, tendrías que buscar y reemplazar ese color en docenas de lugares. Esto es la base para la implementación de temas (claro/oscuro).

### 2. Manipulación con JavaScript

Una de las características más potentes es que JavaScript puede leer y escribir variables CSS en tiempo real. Esto permite crear diseños dinámicos sin tener que añadir o eliminar clases CSS masivamente.

JavaScript

```
// Obtener el valor actual de una variable CSS
const colorActual = getComputedStyle(document.documentElement).getPropertyValue('--color-primario');
console.log(colorActual); // Muestra: #3498db

// Cambiar el valor de una variable CSS (ej: para Modo Oscuro)
document.documentElement.style.setProperty('--color-primario', '#c0392b'); 
// El CSS de la página cambia al instante, sin recargar.
```

### 3. Evitar Repetición y Cálculos con `calc()`

Las variables se pueden usar dentro de otras funciones CSS, como `calc()`, facilitando los diseños basados en espaciado modular.

CSS

```
:root {
    --espacio-base: 1rem;
    --ancho-contenido-max: 1200px;
}

.seccion {
    /* Define un espaciado consistente que se puede cambiar globalmente */
    padding-top: calc(2 * var(--espacio-base)); 
    margin-bottom: var(--espacio-base);
}

.contenedor {
    max-width: var(--ancho-contenido-max);
}
```

### 4. Usar Variables dentro de Media Queries

Puedes redefinir variables dentro de tus Media Queries para aplicar ajustes de diseño responsivo de manera modular.

CSS

```
:root {
    --espaciado-tarjeta: 1rem;
}

@media screen and (min-width: 768px) {
    /* En pantallas grandes, redefinimos la variable de espaciado */
    :root { 
        --espaciado-tarjeta: 2rem; 
    }
}

.tarjeta {
    /* El padding se ajustará automáticamente a 2rem en pantallas grandes */
    padding: var(--espaciado-tarjeta); 
}
```

---

## ✅ Resumen de Puntos Clave

- Las **Variables CSS** (o _Custom Properties_) permiten almacenar y reutilizar valores en tu hoja de estilos.
    
- Se definen con doble guion medio: **`--nombre-variable: valor;`**.
    
- Se utilizan con la función **`var(--nombre-variable)`**.
    
- La práctica estándar es definirlas globalmente en el selector **`:root`** (el elemento `<html>`) para aprovechar la herencia.
    
- Puedes definir un **valor de reserva** o _fallback_ con `var(--var-name, fallback-value)`.
    
- Su mayor valor es la **mantenibilidad** (cambiar un valor globalmente) y la capacidad de ser manipuladas directamente por **JavaScript** para temas dinámicos.
    

👉 Ya hemos cubierto las herramientas más importantes de CSS moderno. Es hora de volver a los selectores para dominar el nivel avanzado y sacar el máximo provecho de nuestras nuevas herramientas. Continúa con [[20 - Pseudo-clases y pseudo-elementos avanzados]].