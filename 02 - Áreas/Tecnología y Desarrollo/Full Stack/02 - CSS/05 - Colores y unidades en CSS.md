Con la Cascada y la Especificidad entendidas, podemos dejar de preguntarnos _por qué_ se aplica un estilo para empezar a enfocarnos en el _qué_ y el _cuánto_. Esta clase cubre las dos herramientas que dan vida a nuestros diseños: **Colores** y **Unidades de Medida**.

Aquí tienes la quinta clase.

---

Aquí tienes los apuntes de la clase anterior: [[04 - Especificidad y herencia]]

# 05 - Colores y Unidades en CSS

## 📌 Introducción

Los colores y las unidades son los valores que asignamos a las propiedades CSS (ej: `color: blue; font-size: 16px;`). Elegir el formato de color correcto y la unidad de medida apropiada es crucial no solo para el diseño visual, sino también para crear interfaces web **responsivas** y **accesibles**.

En esta nota exploraremos los sistemas de color más comunes y la diferencia fundamental entre las unidades absolutas y las relativas.

---

## 🎨 Sistemas de Color en CSS

CSS ofrece varias maneras de expresar el mismo color. Los tres métodos más comunes son el hexadecimal, RGB y HSL.

### 1. Hexadecimal (Hex)

Es la forma más común y concisa. Utiliza el símbolo de almohadilla (`#`) seguido de seis caracteres alfanuméricos (0-9 y A-F). Estos seis caracteres se dividen en tres pares, representando la intensidad de los colores **R**ojo, **G**reen (Verde) y **B**lue (Azul).

Sintaxis: #RRGGBB

- **Rango:** Cada par va de `00` (mínima intensidad) a `FF` (máxima intensidad).
    
- **Abreviatura:** Si cada par es idéntico (ej: `#CCFF66`), se puede abreviar a tres caracteres (ej: `#CF6`).
    
- **Ejemplos:**
    
    - `#FF0000` (Rojo puro)
        
    - `#000000` (Negro)
        
    - `#FFFFFF` (Blanco)
        
    - `#336699` (Un azul oscuro)
        

### 2. RGB y RGBA

**RGB** (Red, Green, Blue) usa una función y valores decimales para representar la intensidad de los tres colores primarios de la luz.

Sintaxis: rgb(R,G,B)

- **Rango:** Cada valor (R, G, B) va de 0 a 255.
    
- **RGBA:** Añade el canal **A**lpha para controlar la **opacidad** (transparencia). El valor Alpha va de 0 (completamente transparente) a 1 (completamente opaco).
    

|Formato|Ejemplo|Descripción|
|---|---|---|
|**RGB**|`rgb(255, 0, 0)`|Rojo puro.|
|**RGBA**|`rgba(0, 128, 0, 0.5)`|Verde al 50% de opacidad.|

Exportar a Hojas de cálculo

### 3. HSL y HSLA

**HSL** (Hue, Saturation, Lightness) es a menudo más intuitivo para los humanos, ya que se relaciona más con la teoría del color que con la luz.

Sintaxis: hsl(H,S%,L%)

- **H (Matiz o Tono):** Valor en grados (0° a 360°) que se corresponde con la rueda de color (0°/360° es rojo, 120° es verde, 240° es azul).
    
- **S (Saturación):** Porcentaje (0% a 100%). 0% es gris, 100% es el color más puro.
    
- **L (Luminosidad o Claridad):** Porcentaje (0% a 100%). 0% es negro, 100% es blanco, 50% es la claridad "verdadera" del color.
    
- **HSLA:** Al igual que RGB, se le puede añadir un valor **Alpha** para la opacidad.
    

|Formato|Ejemplo|Descripción|
|---|---|---|
|**HSL**|`hsl(180, 100%, 50%)`|Un cian puro.|
|**HSLA**|`hsla(240, 50%, 75%, 0.9)`|Un azul claro y ligeramente transparente.|

Exportar a Hojas de cálculo

### 4. Nombres de Color y Palabras Clave

CSS también acepta nombres de colores comunes (ej: `red`, `blue`, `gold`) y palabras clave especiales (ej: `transparent`, `currentcolor`).

CSS

```
/* Ejemplo de uso de colores */
body {
    background-color: rgb(240, 240, 240); /* Gris muy claro */
}

h1 {
    color: #1e8449; /* Verde oscuro (Hex) */
    border-bottom: 2px solid rgba(0, 0, 0, 0.3); /* Línea negra transparente (RGBA) */
}

.caja-transparente {
    background-color: hsla(200, 70%, 50%, 0.7); /* Fondo azul con 70% de opacidad (HSLA) */
}
```

---

## 📏 Unidades de Medida en CSS

Las unidades definen el **tamaño** de las cosas (anchos, alturas, fuentes, márgenes, etc.). Se dividen en dos categorías cruciales para el diseño web moderno: **Absolutas** y **Relativas**.

### 1. Unidades Absolutas (Fijas)

Las unidades absolutas son fijas y no cambian en función de nada más. No se recomiendan para diseños fluidos, ya que no se adaptan a diferentes tamaños de pantalla.

|Unidad|Nombre|Descripción|Uso Recomendado|
|---|---|---|---|
|**`px`**|Píxeles|Es la unidad más común. Un píxel es el punto más pequeño en la pantalla. **Es la base de la mayoría de los diseños.**|Dimensiones de bordes, sombras, elementos muy pequeños.|
|`cm`, `mm`, `in`|Centímetros, Milímetros, Pulgadas|Medidas físicas.|Solo para hojas de estilo de impresión.|
|`pt`, `pc`|Puntos, Picas|Medidas tipográficas.|Solo para hojas de estilo de impresión.|

Exportar a Hojas de cálculo

### 2. Unidades Relativas (Adaptables)

Las unidades relativas cambian su valor en función de otra cosa, lo que las hace ideales para el **Responsive Design** (diseño adaptable).

#### Relativas a la Fuente

|Unidad|Relativa a...|Descripción|Uso Recomendado|
|---|---|---|---|
|**`em`**|**El tamaño de fuente del elemento padre.**|Si el padre tiene `font-size: 20px;`, entonces `1em` = `20px`.|Márgenes y rellenos internos (padding) que deben escalar con el texto.|
|**`rem`**|**El tamaño de fuente del elemento raíz (root), es decir, el elemento `<html>`.**|Esto proporciona una base de tamaño única y estable.|Dimensiones de fuente, espaciado, y el diseño general.|
|**`%`**|**El tamaño del elemento padre.**|El 50% del ancho del padre.|Dimensiones de elementos (ancho, alto) dentro de un contenedor.|

Exportar a Hojas de cálculo

#### Relativas a la Ventana (Viewport)

|Unidad|Relativa a...|Descripción|Uso Recomendado|
|---|---|---|---|
|**`vw`**|**1% del ancho de la ventana gráfica (viewport width).**|`100vw` es todo el ancho de la pantalla.|Elementos que deben escalar con el ancho de la pantalla (ej: títulos muy grandes).|
|**`vh`**|**1% de la altura de la ventana gráfica (viewport height).**|`100vh` es toda la altura de la pantalla.|Contenedores que deben ocupar toda la pantalla (ej: _landing pages_).|
|`vmin`|El valor menor entre `vw` y `vh`.||Para elementos que siempre deben ser visibles.|
|`vmax`|El valor mayor entre `vw` y `vh`.||Para diseño que prioriza el espacio.|

Exportar a Hojas de cálculo

## ✍️ Ejemplo Práctico de Unidades

HTML

```
<html>
<head>
    <style>
        html { font-size: 16px; } /* Establecemos la base */
        
        .contenedor {
            width: 80%; /* 80% del ancho de su padre (el <body>) */
            padding: 1em; /* 1em = el font-size de .contenedor (que hereda de body, que hereda de html = 16px) */
        }
        
        h2 {
            font-size: 2rem; /* 2rem = 2 * 16px = 32px (se basa en el <html>) */
            margin-bottom: 0.5rem;
        }

        .hero {
            height: 50vh; /* La mitad de la altura de la pantalla del usuario */
        }
    </style>
</head>
<body>
    <div class="contenedor">
        <h2>Unidad REM</h2>
        <p>Un texto con fuente estándar.</p>
    </div>
    <div class="hero"></div>
</body>
</html>
```

El uso de **`rem`** en lugar de **`px`** permite a los usuarios con discapacidades visuales ajustar el tamaño de fuente base de su navegador, haciendo que todo el diseño escale proporcionalmente, mejorando la **accesibilidad**.

---

## ✅ Resumen de Puntos Clave

- Los colores se pueden definir por **Hexadecimal** (`#RRGGBB`), **RGB/RGBA** (`rgb(r,g,b,a)`) o **HSL/HSLA** (`hsl(h,s,l,a)`). **RGBA** y **HSLA** permiten controlar la **opacidad** con el canal Alpha.
    
- Las **Unidades Absolutas** (`px`) son fijas y no escalan. Úsalas con moderación.
    
- Las **Unidades Relativas** (`em`, `rem`, `%`, `vw`, `vh`) escalan automáticamente, lo que es esencial para el Responsive Design.
    
- **`rem`** (relativa al tamaño de fuente raíz `<html>`) es la unidad preferida para tamaños de fuente y espaciado, por su estabilidad y por mejorar la accesibilidad.
    
- **`%`** y **`vw`** se usan para el dimensionamiento de contenedores en relación con el padre o la ventana.
    

👉 Ahora que sabemos medir y colorear, es momento de aplicar estos conceptos al estilo más importante: el texto. Continúa con [[06 - Tipografía en CSS]].