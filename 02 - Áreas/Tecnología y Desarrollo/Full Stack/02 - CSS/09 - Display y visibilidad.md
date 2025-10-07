Con nuestras cajas decoradas, necesitamos entender cómo se comportan y cómo se relacionan entre sí en la página. Esta clase es crucial para la maquetación, ya que define el "temperamento" de cada elemento.

Aquí tienes la novena clase, lista para tu sistema de conocimiento.

---

Aquí tienes los apuntes de la clase anterior: [[08 - Fondos y bordes]]

# 09 - Display y Visibilidad

## 📌 Introducción

La propiedad **`display`** es una de las más importantes en CSS, pues determina cómo un elemento se comporta en el flujo del documento y cómo interactúa con los elementos a su alrededor. Es la propiedad que define si un elemento ocupa todo el ancho disponible, si se alinea con el texto, o si se convierte en un contenedor flexible (lo veremos en [[11 - Flexbox en CSS]] y [[12 - Grid Layout en CSS]]).

Además, veremos la propiedad **`visibility`**, que controla si un elemento es visible o no, con un impacto diferente al de `display`.

---

## 💻 La Propiedad `display`

Hay tres valores principales (clásicos) de `display` que definen el flujo de la mayoría de los elementos HTML: `block`, `inline` e `inline-block`.

### 1. Elementos de Bloque (`block`)

Los elementos con `display: block;` se comportan como cajas independientes que ocupan todo el ancho disponible.

- **Comportamiento:**
    
    - Siempre comienzan en una **nueva línea**.
        
    - Ocupan el **100% del ancho** de su contenedor por defecto (a menos que se especifique un `width` menor).
        
    - Aceptan todas las propiedades del Modelo de Caja (ancho, alto, _padding_, _margin_).
        
- **Ejemplos de etiquetas por defecto:** `<div>`, `<h1>` - `<h6>`, `<p>`, `<ul>`, `<li>`.
    

### 2. Elementos en Línea (`inline`)

Los elementos con `display: inline;` se comportan como pequeñas cajas que fluyen con el contenido de la línea (como palabras en un párrafo).

- **Comportamiento:**
    
    - **No** comienzan en una nueva línea; se colocan uno al lado del otro.
        
    - Su ancho y alto se limitan **estrictamente al contenido** que tienen dentro.
        
    - **Ignoran** las propiedades de `width` y `height`.
        
    - **Solo aceptan** _padding_ y _margin_ horizontales (izquierda/derecha). Ignoran _padding_ y _margin_ verticales (arriba/abajo).
        
- **Ejemplos de etiquetas por defecto:** `<span>`, `<a>`, `<strong>`, `<em>`, `<img>`.
    

### 3. Elementos Bloque en Línea (`inline-block`)

Este valor combina lo mejor de ambos mundos, siendo un puente crucial entre los modelos `block` e `inline`.

- **Comportamiento:**
    
    - Fluyen en línea (se colocan **uno al lado del otro** como los `inline`).
        
    - **Aceptan** todas las propiedades del Modelo de Caja: `width`, `height`, _padding_ y _margin_ verticales.
        
- **Uso Común:** Crear menús de navegación horizontales donde cada elemento necesita un tamaño definido.
    

|Característica|`block`|`inline`|`inline-block`|
|---|---|---|---|
|**Flujo**|Comienza en nueva línea|Fluye con el texto|Fluye con el texto|
|**Ancho por Defecto**|100% (Ancho del padre)|Solo el contenido|Solo el contenido|
|**Acepta `width`/`height`**|Sí|**No**|**Sí**|
|**Acepta _Margin_ Vertical**|Sí|**No**|**Sí**|

Exportar a Hojas de cálculo

---

## 🆕 Valores Modernos de `display` (Introducción)

En el desarrollo web actual, los siguientes dos valores han revolucionado la forma en que construimos _layouts_:

### 4. `display: flex;`

Convierte el elemento en un **contenedor flexible**. Todos sus hijos directos se convierten en **ítems flexibles** y pueden ser alineados y distribuidos fácilmente en una dimensión (fila o columna).

👉 Veremos en profundidad en [[11 - Flexbox en CSS]].

### 5. `display: grid;`

Convierte el elemento en un **contenedor de cuadrícula**. Permite crear _layouts_ bidimensionales (filas y columnas) complejos y precisos.

👉 Veremos en profundidad en [[12 - Grid Layout en CSS]].

### 6. `display: none;`

Este valor elimina completamente el elemento del flujo del documento. Es la forma más común de ocultar elementos para que no sean visibles ni ocupen espacio.

- **Efecto:** Es como si el elemento **nunca hubiera existido**. No ocupa espacio, y no se puede interactuar con él (ej: haciendo clic).
    
- **Uso Común:** Para ocultar menús desplegables o modales antes de activarlos con JavaScript.
    

---

## 🙈 La Propiedad `visibility`

Esta propiedad también se usa para ocultar elementos, pero lo hace de manera diferente a `display: none;`.

### 1. `visibility: hidden;`

Oculta visualmente el elemento, pero lo mantiene en el flujo del documento.

- **Efecto:** El elemento **no es visible**, pero **sigue ocupando el espacio** que le corresponde en el _layout_.
    
- **Uso Común:** Cuando necesitas que el espacio del elemento se mantenga para evitar que el diseño "salte" o cambie de posición al mostrarlo/ocultarlo.
    

### 2. `visibility: visible;`

El valor por defecto. Muestra el elemento.

|Propiedad|Oculta el Elemento|Mantiene el Espacio|Interacción (click, tab)|
|---|---|---|---|
|**`display: none;`**|Sí (Lo elimina del DOM)|**No**|Imposible|
|**`visibility: hidden;`**|Sí (Lo hace transparente)|**Sí**|Imposible|

Exportar a Hojas de cálculo

---

## ✍️ Ejemplo de Código Práctico

Imagina un menú horizontal creado con listas.

HTML

```
<ul class="menu">
    <li><a href="#">Inicio</a></li>
    <li><a href="#">Productos</a></li>
    <li><a href="#">Contacto</a></li>
</ul>
```

CSS

```
/* 1. Hacemos que la lista no tenga viñetas y que los <li> se pongan uno al lado del otro */
.menu {
    list-style: none; /* Elimina las viñetas */
    padding: 0;
    margin: 0;
}

/* 2. El truco del menú: convertimos el <li> en inline-block */
.menu li {
    /* ¡Magia! Ahora los ítems se alinean horizontalmente */
    display: inline-block; 
    
    /* Y ahora sí podemos darle dimensiones y margen vertical, que inline no permitía */
    width: 150px; 
    text-align: center;
    margin: 5px 10px;
    padding: 10px;
    background-color: #eee;
}

/* Ejemplo de uso de display: none y visibility: hidden */
.aviso {
    display: none; /* No ocupa espacio, es invisible y no afecta el layout */
}

.placeholder {
    visibility: hidden; /* Oculto, pero su espacio se mantiene */
    height: 100px; /* Sigue forzando 100px de altura en la página */
}
```

---

## ✅ Resumen de Puntos Clave

- La propiedad **`display`** define la naturaleza del elemento en el flujo del documento.
    
- **`block`** (ej: `div`) ocupa el 100% del ancho y comienza en una nueva línea.
    
- **`inline`** (ej: `span`) fluye con el texto, ignora `width`, `height` y _margin_ vertical.
    
- **`inline-block`** fluye en línea (horizontalmente) pero acepta todas las propiedades del Modelo de Caja.
    
- **`display: none;`** elimina completamente el elemento del _layout_ y no ocupa espacio.
    
- **`visibility: hidden;`** oculta el elemento, pero mantiene su espacio reservado en la página.
    
- Los valores `flex` y `grid` son los cimientos de la maquetación moderna (próximos niveles).
    

👉 Entender el flujo normal es el primer paso. El siguiente es cómo "sacar" o reposicionar los elementos de ese flujo para maquetaciones más complejas. Continúa con [[10 - Posicionamiento]].