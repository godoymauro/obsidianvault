Hemos visto la base del Responsive Design con Media Queries. Ahora, vamos a refinar esa estrategia para que nuestros elementos no solo **reaccionen** a los cambios de tamaño, sino que se sientan naturalmente **fluidos** y se adapten de forma inteligente.

Aquí tienes la decimocuarta clase.

---

Aquí tienes los apuntes de la clase anterior: [[13 - Responsive Design]]

# 14 - Diseño Fluido y Adaptativo

## 📌 Introducción

En el contexto moderno de CSS, el término **Diseño Responsivo** se divide a menudo en dos conceptos:

1. **Diseño Adaptativo (Adaptive Design):** Cambiar el diseño a través de **breakpoints** fijos (Media Queries), ajustándose por pasos.
    
2. **Diseño Fluido (Fluid Design):** Usar unidades relativas (`%`, `vw`, `fr`) y técnicas flexibles (Flexbox/Grid) para que el _layout_ se redimensione suavemente **entre** esos breakpoints.
    

La combinación de ambos (lo que llamamos **Responsive Design**) resulta en la mejor experiencia de usuario, permitiendo que el contenido fluya y se adapte a cualquier ancho de pantalla, no solo a los anchos predefinidos.

---

## 🌊 Técnicas Clave para la Fluidez

El diseño fluido requiere pensar en términos de proporciones y espacio disponible, no de valores fijos en píxeles.

### 1. Dimensiones Basadas en Porcentaje (`%`)

Utilizar el porcentaje para `width` y `max-width` asegura que los elementos siempre sean una porción del espacio del contenedor padre.

CSS

```
/* Un diseño de dos columnas que siempre mantiene la proporción 70/30 */
.contenedor-principal {
    width: 90%; /* Ancho máximo 90% de la ventana, con margen auto se centra */
    margin: 0 auto;
}

.columna-contenido {
    width: 70%; /* Siempre será el 70% del contenedor-principal */
    float: left; /* Táctica clásica, aunque hoy es mejor usar Flex/Grid */
}

.columna-lateral {
    width: 30%; /* Siempre será el 30% restante */
    float: left;
}
```

### 2. Tipografía Fluida con `clamp()`

Si bien `rem` es excelente para la accesibilidad, a veces queremos que el tamaño de la fuente de los títulos escale de forma dramática con el tamaño del _viewport_. Aquí entra la función CSS **`clamp()`**.

`clamp(valor-mínimo, valor-preferido, valor-máximo)`

Esta función permite establecer un tamaño de fuente que:

1. **Nunca** sea más pequeño que el `valor-mínimo`.
    
2. **Nunca** sea más grande que el `valor-máximo`.
    
3. **Entre** esos límites, use el `valor-preferido` (a menudo una unidad `vw`) para escalar suavemente.
    

CSS

```
/* El tamaño de fuente será un mínimo de 2rem (32px), 
   un máximo de 5rem (80px), 
   y usará 8vw para escalar entre esos límites. */
h1 {
    font-size: clamp(2rem, 8vw, 5rem);
}
```

Esto elimina la necesidad de Media Queries solo para ajustar el tamaño de fuente de los títulos en cada breakpoint.

### 3. Imágenes con Proporción de Aspecto (`aspect-ratio`)

Anteriormente, usábamos `max-width: 100%; height: auto;` para las imágenes. Sin embargo, cuando la imagen carga, puede causar un "salto" en el diseño (_layout shift_).

La propiedad **`aspect-ratio`** permite definir la relación de ancho a alto de un contenedor. Esto es ideal para _iframes_, videos y tarjetas, ya que reserva el espacio antes de que el contenido cargue.

CSS

```
/* Reserva un espacio cuadrado (1:1) para el elemento, sin importar lo que contenga. */
.mini-carrusel {
    aspect-ratio: 1 / 1; 
}

/* Reserva un espacio de pantalla ancha (16:9) para un video */
.video-frame {
    aspect-ratio: 16 / 9;
}
```

---

## 🧠 Adaptación Inteligente con Media Features

Además del ancho, podemos hacer que el diseño se adapte al contexto del usuario, mejorando la experiencia y la accesibilidad (A11y).

### 1. Modo Oscuro (`prefers-color-scheme`)

Permite aplicar estilos específicos basados en si el usuario tiene activado el tema oscuro o claro en su sistema operativo.

CSS

```
/* Estilos por defecto (Modo Claro) */
body {
    background-color: white;
    color: black;
}

/* Adaptación inteligente al Modo Oscuro */
@media (prefers-color-scheme: dark) {
    body {
        background-color: #1a1a1a; /* Fondo muy oscuro */
        color: #f0f0f0;            /* Texto casi blanco */
    }
    a {
        color: #64b5f6; /* Azul claro para los enlaces, mejor contraste */
    }
}
```

### 2. Reducción de Movimiento (`prefers-reduced-motion`)

Ideal para la accesibilidad. Permite desactivar animaciones complejas o movimientos innecesarios para usuarios que sufren de mareos o distracción.

CSS

```
/* Estilos por defecto (Con animación de fondo) */
.fondo-animado {
    animation: slide 10s infinite;
}

/* Adaptación: Si el usuario prefiere menos movimiento, desactivamos la animación */
@media (prefers-reduced-motion) {
    .fondo-animado {
        animation: none; /* Desactiva la animación */
        transition: none; /* Desactiva transiciones */
    }
}
```

### 3. Orientación (`orientation`)

Permite aplicar estilos basados en si el dispositivo está en modo vertical (retrato) u horizontal (paisaje).

CSS

```
@media (orientation: landscape) {
    /* En tablet o móvil horizontal, usa Flexbox para colocar los ítems en fila */
    .caja-fotos {
        display: flex;
    }
}
@media (orientation: portrait) {
    /* En modo vertical, los apilamos por defecto */
    .caja-fotos {
        display: block; 
    }
}
```

---

## ✍️ Ejemplo de Código: Combinación Fluido-Adaptativa

HTML

```
<div class="header">Título del Sitio</div>
```

CSS

```
/* 1. Base (Mobile-First) */
:root { /* Usamos variables para los colores base */
    --color-fondo: white;
    --color-texto: black;
}
body {
    background-color: var(--color-fondo);
    color: var(--color-texto);
    margin: 0;
}

/* 2. Tipografía Fluida (Escalado suave) */
.header {
    /* Min: 2rem, Max: 6rem, preferido: 10vw */
    font-size: clamp(2rem, 10vw, 6rem); 
    text-align: center;
    padding: 20px 0;
}

/* 3. Adaptativo por Media Query (Rompe el diseño de 1 columna a 2) */
@media screen and (min-width: 768px) {
    .header {
        /* En pantallas grandes, centramos mejor usando grid y centrado de contenido */
        display: grid;
        place-items: center; /* Centrado perfecto con Grid */
        padding: 40px 0;
    }
}

/* 4. Adaptativo por Preferencia (Modo Oscuro) */
@media (prefers-color-scheme: dark) {
    :root {
        --color-fondo: #121212;
        --color-texto: #e0e0e0;
    }
}
```

---

## ✅ Resumen de Puntos Clave

- El **Diseño Fluido** se enfoca en el uso de unidades relativas (`%`, `vw`, `fr`) para un escalado suave **entre** breakpoints.
    
- El **Diseño Adaptativo** usa **Media Queries** (`min-width`) para ajustes de _layout_ basados en breakpoints específicos.
    
- La función **`clamp(min, preferido, max)`** es la herramienta moderna para crear **tipografía fluida** que escala suavemente pero dentro de límites de accesibilidad.
    
- **`aspect-ratio`** permite reservar espacio para evitar saltos en el _layout_ al cargar imágenes o videos.
    
- Las **Media Features** como `(prefers-color-scheme: dark)` y `(prefers-reduced-motion)` permiten una adaptación inteligente basada en las preferencias del usuario, mejorando la Accesibilidad (A11y).
    

👉 Hemos cubierto la teoría de cómo construir _layouts_ complejos. A menudo, en la práctica, no partimos de cero. La siguiente clase explora las herramientas que aceleran este proceso: los _Frameworks_ CSS. Continúa con [[15 - Frameworks CSS y librerías]].