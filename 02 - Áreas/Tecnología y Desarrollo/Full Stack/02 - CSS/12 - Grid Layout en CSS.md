Si Flexbox es el campeón de la alineación en una dimensión (fila o columna), **Grid Layout** es la herramienta maestra para las estructuras complejas y bidimensionales. Es esencial para construir _layouts_ de página completos.

Aquí tienes la duodécima clase.

---

Aquí tienes los apuntes de la clase anterior: [[11 - Flexbox en CSS]]

# 12 - Grid Layout en CSS

## 📌 Introducción

**CSS Grid Layout** es un sistema de maquetación bidimensional que te permite organizar el contenido en **filas** y **columnas** simultáneamente, directamente en el elemento padre. Mientras Flexbox se enfoca en la distribución de contenido a lo largo de un solo eje, Grid se enfoca en la distribución de contenido en un plano de dos dimensiones, lo que lo hace perfecto para crear plantillas completas de páginas web (encabezado, barra lateral, contenido, pie de página).

Piensa en Grid como una **hoja de cálculo o una tabla**, donde puedes definir las líneas de la cuadrícula y luego colocar tus elementos exactamente donde quieras dentro de esas celdas.

## 🛠️ Conceptos Clave: Contenedor y Celdas

Al igual que Flexbox, Grid opera con un padre (`grid container`) y sus hijos directos (`grid items`).

1. **Líneas de la Cuadrícula (Grid Lines):** Las líneas horizontales y verticales que definen la estructura.
    
2. **Pistas de la Cuadrícula (Grid Tracks):** El espacio entre dos líneas de cuadrícula (filas o columnas).
    
3. **Celdas (Grid Cells):** El espacio entre dos filas y dos columnas (similar a una celda de tabla).
    
4. **Áreas (Grid Areas):** Un espacio rectangular definido por la combinación de varias celdas.
    

## ⚙️ Propiedades del Contenedor Grid (Padre)

Estas propiedades se aplican al elemento padre (`display: grid;`).

### 1. `display: grid;`

Convierte el elemento en un **contenedor de cuadrícula**, habilitando todas las propiedades Grid.

### 2. `grid-template-columns` y `grid-template-rows`

Son las propiedades más importantes; definen el número, el tamaño y la unidad de medida de las pistas (columnas y filas).

#### La Unidad `fr` (Fractional Unit)

Grid introduce la unidad **`fr`** (fracción), que representa una parte del espacio libre en el contenedor. Esto hace que las cuadrículas sean intrínsecamente _responsive_.

CSS

```
.contenedor-grid {
    display: grid;
    
    /* Tres columnas: la primera de 100px fijo, y las otras dos se dividen el espacio restante por igual */
    grid-template-columns: 100px 1fr 1fr;
    
    /* Dos filas: la primera de tamaño automático (según contenido), la segunda de 50px */
    grid-template-rows: auto 50px; 
}
```

### 3. `gap` (Espacio entre Celdas)

Define el espacio (canal) entre las filas y columnas de la cuadrícula. Sustituye el uso de márgenes en los ítems, que es más propenso a errores.

- **Shorthand:** `gap: 20px 10px;` (20px para las filas, 10px para las columnas) o `gap: 20px;` (para ambos).
    
- **Individuales:** `row-gap` y `column-gap`.
    

CSS

```
.contenedor-grid {
    gap: 15px;
}
```

### 4. `grid-template-areas` (Maquetación por Nombres)

Permite diseñar el _layout_ de la cuadrícula visualmente usando nombres. Es ideal para la estructura general de la página.

CSS

```
.layout {
    display: grid;
    grid-template-columns: 1fr 3fr 1fr; /* Columna Izquierda, Contenido, Columna Derecha */
    grid-template-rows: auto 1fr auto; /* Header, Main, Footer */
    
    /* Visualización del layout con nombres. Los puntos (.) significan celda vacía.
    El nombre 'header' ocupa 3 columnas.
    */
    grid-template-areas: 
        "header header header"
        "nav    main   sidebar"
        "footer footer footer";
}
```

### 5. Alineación del Contenido (Como Flexbox)

Grid usa propiedades similares a Flexbox para alinear el contenido _dentro_ de las celdas (cuando las celdas tienen más espacio que el ítem).

- **`justify-items` / `justify-content`:** Alineación horizontal (eje de las filas).
    
- **`align-items` / `align-content`:** Alineación vertical (eje de las columnas).
    

---

## 📐 Propiedades de los Ítems Grid (Hijos)

Estas propiedades se aplican a los hijos directos del contenedor Grid.

### 1. Colocación por Número de Línea

Los ítems pueden colocarse especificando la línea de inicio y la línea final.

- **`grid-column-start` / `grid-column-end`**: Define el inicio y fin del ítem en el eje horizontal.
    
- **`grid-row-start` / `grid-row-end`**: Define el inicio y fin del ítem en el eje vertical.
    

CSS

```
.item-grande {
    /* El ítem inicia en la línea 1 de columna y termina en la línea 4 */
    grid-column-start: 1; 
    grid-column-end: 4; 
    
    /* Abreviado: inicia en la 1 y se extiende por 3 columnas */
    /* grid-column: 1 / span 3; */
}
```

**Abreviaturas (`grid-column` y `grid-row`):** Es la forma preferida de definir la colocación. `grid-column: 1 / 3;` significa "desde la línea 1 hasta la línea 3".

### 2. Colocación por Nombre de Área

Si usaste `grid-template-areas` en el padre, puedes asignar un nombre al hijo.

CSS

```
.caja-header {
    /* Asigna este ítem al área nombrada 'header' */
    grid-area: header; 
}
.caja-nav {
    grid-area: nav;
}
.caja-main {
    grid-area: main;
}
```

Esta forma es mucho más legible y fácil de modificar, especialmente en el diseño _responsive_ (ver [[13 - Responsive Design]]).

### 3. Alineación Individual (`justify-self` y `align-self`)

Permite anular la alineación del contenedor para un ítem específico dentro de su celda.

CSS

```
.item-logotipo {
    /* Alinea este elemento a la derecha dentro de su celda */
    justify-self: end; 
}
```

---

## ✍️ Ejemplo de Código: Layout de Página con Grid Areas

HTML

```
<div class="page-layout">
    <header>Encabezado</header>
    <nav>Navegación</nav>
    <main>Contenido Principal</main>
    <footer>Pie de Página</footer>
</div>
```

CSS

```
.page-layout {
    display: grid;
    /* Columnas: 1/4 para nav, 3/4 para main */
    grid-template-columns: 1fr 3fr; 
    /* Filas: Auto para header, el resto para el contenido, auto para footer */
    grid-template-rows: auto 1fr auto; 
    gap: 10px;
    height: 100vh; /* Para que veamos el efecto 1fr en el main */
    
    /* Definición del layout por nombres */
    grid-template-areas: 
        "header header"
        "nav    main"
        "footer footer";
}

/* Colocación de los hijos por nombre de área */
header { grid-area: header; background-color: #3498db; }
nav    { grid-area: nav;    background-color: #2ecc71; }
main   { grid-area: main;   background-color: #ecf0f1; }
footer { grid-area: footer; background-color: #95a5a6; }
```

**Resultado:** El encabezado y el pie de página abarcan todo el ancho. La navegación y el contenido principal se dividen el espacio restante en las proporciones 1:3.

---

## ✅ Resumen de Puntos Clave

- **Grid Layout** es un sistema de _layout_ **bidimensional** (filas y columnas), ideal para estructuras de página.
    
- Se activa con **`display: grid;`** en el contenedor padre.
    
- **`grid-template-columns`** y **`grid-template-rows`** definen la estructura.
    
- La unidad **`fr`** (fracción) es clave para crear columnas y filas flexibles que se adaptan al espacio sobrante.
    
- **`gap`** establece el espaciado entre filas y columnas de manera limpia.
    
- **`grid-template-areas`** permite crear _layouts_ complejos y legibles usando nombres para las celdas.
    
- Los ítems hijos se colocan usando **`grid-area`** (por nombre) o **`grid-column` / `grid-row`** (por número de línea).
    

👉 Con Flexbox (componentes) y Grid (estructura) dominados, estamos listos para asegurar que nuestros diseños se vean bien en cualquier pantalla. El siguiente paso es el **Responsive Design**. Continúa con [[13 - Responsive Design]].