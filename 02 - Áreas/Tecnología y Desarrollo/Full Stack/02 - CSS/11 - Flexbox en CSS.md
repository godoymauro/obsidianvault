¡Bienvenido al Nivel 3: Layouts Modernos!

Esta es, sin duda, la parte más emocionante y transformadora de CSS moderno. Dejamos atrás la maquetación basada en `float` y `inline-block` para adoptar **Flexbox**, la herramienta definitiva para alinear y distribuir elementos en una sola dimensión.

Aquí tienes la undécima clase.

---

Aquí tienes los apuntes de la clase anterior: [[10 - Posicionamiento]]

# 11 - Flexbox en CSS

## 📌 Introducción

El **Módulo de Caja Flexible (Flexbox)** es un modelo de _layout_ unidimensional diseñado para distribuir y alinear el espacio entre los ítems de un contenedor, incluso cuando el tamaño de esos ítems es dinámico o desconocido. En esencia, Flexbox resuelve la mayoría de los problemas de alineación, centrado y ordenamiento que solían ser frustrantes en CSS tradicional.

Piensa en Flexbox como una varita mágica para manejar listas de elementos, barras de navegación o cualquier componente que necesite una alineación perfecta en una **fila** o una **columna**.

## 🛠️ Conceptos Clave: Contenedor e Ítems

Flexbox opera con una relación estricta entre **padre** e **hijos**:

1. **Contenedor Flexible (Flex Container):** El elemento padre al que aplicas `display: flex;`.
    
2. **Ítems Flexibles (Flex Items):** Los hijos directos del contenedor flexible.
    

### Ejes de Flexbox

Flexbox organiza el espacio a lo largo de dos ejes:

1. **Eje Principal (Main Axis):** El eje primario de la dirección del flujo. Por defecto es horizontal (fila).
    
2. **Eje Secundario o Transversal (Cross Axis):** El eje perpendicular al principal. Por defecto es vertical (columna).
    

## ⚙️ Propiedades del Contenedor Flexible (Padre)

Las siguientes propiedades se aplican al elemento padre (`display: flex;`) y controlan la dirección y la distribución del espacio entre los ítems.

### 1. `display: flex;`

Convierte el elemento en un contenedor flexible, habilitando todas las demás propiedades Flexbox.

### 2. `flex-direction`

Establece la dirección del Eje Principal (Main Axis).

|Valor|Dirección|Eje Principal|
|---|---|---|
|**`row`** (Defecto)|Horizontal|De izquierda a derecha.|
|**`row-reverse`**|Horizontal|De derecha a izquierda.|
|**`column`**|Vertical|De arriba a abajo.|
|**`column-reverse`**|Vertical|De abajo a arriba.|

Exportar a Hojas de cálculo

### 3. `justify-content` (Alineación en el Eje Principal)

Controla cómo se distribuye el espacio **entre** y **alrededor** de los ítems a lo largo del Eje Principal. Es la propiedad clave para centrar horizontalmente.

|Valor|Descripción|
|---|---|
|**`flex-start`** (Defecto)|Los ítems se agrupan al inicio del eje.|
|**`flex-end`**|Los ítems se agrupan al final del eje.|
|**`center`**|Los ítems se agrupan y centran en el medio del eje.|
|**`space-between`**|Distribuye el espacio sobrante uniformemente **entre** los ítems (los extremos tocan los bordes).|
|**`space-around`**|Distribuye el espacio sobrante uniformemente **alrededor** de cada ítem.|
|**`space-evenly`**|Distribuye el espacio de manera que el espacio entre ítems y los bordes sea **exactamente igual**.|

Exportar a Hojas de cálculo

### 4. `align-items` (Alineación en el Eje Secundario)

Controla cómo se alinean los ítems individualmente a lo largo del Eje Secundario. Es la propiedad clave para centrar verticalmente.

|Valor|Descripción|
|---|---|
|**`stretch`** (Defecto)|Los ítems estiran su altura (en `row`) o ancho (en `column`) para llenar el contenedor.|
|**`flex-start`**|Se alinean al inicio del eje secundario.|
|**`flex-end`**|Se alinean al final del eje secundario.|
|**`center`**|Se centran en el medio del eje secundario.|

Exportar a Hojas de cálculo

### 5. `flex-wrap`

Controla si los ítems deben ajustarse a una sola línea o si pueden envolverse en múltiples líneas si no caben.

|Valor|Descripción|
|---|---|
|**`nowrap`** (Defecto)|Los ítems se comprimen en una sola línea, sin importar si exceden el ancho.|
|**`wrap`**|Permite a los ítems envolverse en nuevas líneas, de arriba a abajo.|
|**`wrap-reverse`**|Permite a los ítems envolverse en nuevas líneas, de abajo a arriba.|

Exportar a Hojas de cálculo

> 💡 **Abreviatura:** **`flex-flow`** combina `flex-direction` y `flex-wrap` (ej: `flex-flow: row wrap;`).

---

## 📐 Propiedades de los Ítems Flexibles (Hijos)

Las siguientes propiedades se aplican a los elementos **hijo directo** dentro del contenedor flexible y controlan su tamaño y alineación individual.

### 1. `flex-grow`

Define la capacidad de un ítem para **crecer** y ocupar el espacio libre disponible en el contenedor.

- **Valor:** Un número (por defecto es 0).
    
    - `flex-grow: 1;` asegura que el ítem tome una parte proporcional del espacio restante. Si todos tienen `1`, todos crecen por igual.
        
    - Si un ítem tiene `2` y los otros `1`, ese ítem ocupará el doble de espacio restante que los otros.
        

### 2. `flex-shrink`

Define la capacidad de un ítem para **encogerse** cuando no hay suficiente espacio.

- **Valor:** Un número (por defecto es 1).
    
    - `flex-shrink: 0;` evita que el ítem se encoja por debajo de su tamaño base (muy útil).
        

### 3. `flex-basis`

Define el tamaño inicial de un ítem **antes** de que se distribuya el espacio libre (antes de `flex-grow` o `flex-shrink`). Se comporta como `width` o `height` dependiendo de la `flex-direction`.

- **Valor:** Cualquier unidad de medida (ej: `200px`, `25%`, `auto`).
    

> 💡 **Abreviatura:** **`flex`** combina `flex-grow`, `flex-shrink` y `flex-basis` (ej: `flex: 1 1 200px;` o, para una configuración común: `flex: 1;` que equivale a `flex: 1 1 0%;`).

### 4. `order`

Permite cambiar el orden visual de los ítems **sin modificar el HTML subyacente**.

- **Valor:** Un número entero (por defecto es 0). Los ítems se colocan en orden ascendente (los valores negativos van primero).
    

### 5. `align-self` (Alineación Individual)

Permite anular el valor de `align-items` del contenedor para un ítem flexible específico.

- **Valores:** Acepta los mismos valores que `align-items` (`flex-start`, `center`, `stretch`, etc.).
    

---

## ✍️ Ejemplo de Código: Centrado Perfecto y Barra de Navegación

### 1. Centrado Perfecto (Horizontal y Vertical)

HTML

```
<div class="contenedor-centrador">
    <div class="caja-centrada">Centrado</div>
</div>
```

CSS

```
.contenedor-centrador {
    display: flex; 
    
    /* 1. Centra horizontalmente (Eje Principal) */
    justify-content: center; 
    
    /* 2. Centra verticalmente (Eje Secundario) */
    align-items: center; 
    
    height: 300px; /* Necesitas altura para ver el centrado vertical */
    border: 1px solid black;
}
```

### 2. Barra de Navegación Distribuida

HTML

```
<nav class="navbar">
    <a href="#">Logo</a>
    <div class="links">
        <a href="#">Inicio</a>
        <a href="#">Acerca</a>
        <a href="#">Contacto</a>
    </div>
</nav>
```

CSS

```
.navbar {
    display: flex;
    height: 60px;
    background-color: #333;
    
    /* Distribuye el espacio sobrante entre el logo y los links */
    justify-content: space-between; 
    
    /* Centra los elementos (Logo y Links) verticalmente */
    align-items: center;
    padding: 0 20px;
}

.links {
    display: flex; /* Convierte los links en un sub-contenedor flexible */
    gap: 20px; /* (Nueva característica) Espacio entre los ítems */
}
```

---

## ✅ Resumen de Puntos Clave

- **Flexbox** es un sistema de _layout_ **unidimensional** para alinear y distribuir ítems en una fila o columna.
    
- Se activa con **`display: flex;`** en el contenedor padre.
    
- El **Eje Principal** (`flex-direction`) define si el flujo es horizontal (`row`) o vertical (`column`).
    
- **`justify-content`** controla la distribución del espacio en el Eje Principal.
    
- **`align-items`** controla la alineación de los ítems en el Eje Secundario.
    
- El **Centrado Perfecto** se logra con `display: flex;` + `justify-content: center;` + `align-items: center;`.
    
- Las propiedades de los ítems (`flex-grow`, `flex-shrink`, `flex-basis`) controlan su capacidad de crecer o encogerse.
    

👉 Flexbox es excelente para componentes, pero cuando necesitamos una estructura de página completa basada en filas y columnas al mismo tiempo, necesitamos **Grid Layout**. Continúa con [[12 - Grid Layout en CSS]].