Hemos cubierto el cómo (selectores), el por qué (cascada) y el cuánto (unidades y colores). Es hora de embellecer el contenido textual con el que trabajaremos en todas nuestras páginas: la **tipografía**.

Aquí tienes la sexta clase, lista para tu sistema de conocimiento.

---

Aquí tienes los apuntes de la clase anterior: [[05 - Colores y unidades en CSS]]

# 06 - Tipografía en CSS

## 📌 Introducción

La tipografía es un elemento fundamental en el diseño web. Más del 90% de la información en la web es texto, por lo que la elección y el estilo de la fuente tienen un impacto directo en la legibilidad, la accesibilidad y el tono general de tu sitio web. CSS nos da un control absoluto sobre cómo se presenta este texto.

En esta clase, exploraremos las propiedades CSS clave para controlar familias de fuentes, tamaño, peso, alineación y el espaciado.

---

## 🔡 Propiedades de Familia de Fuente (`font-family`)

Esta propiedad es la más importante y define el tipo de letra que el navegador debe usar.

### 1. La Pila de Fuentes (Font Stack)

Es una lista de nombres de fuentes, separados por comas, que el navegador intentará cargar secuencialmente. Esto garantiza que si la primera fuente no está disponible en el sistema del usuario, se intente con la siguiente, y así sucesivamente.

Sintaxis: font-family:Fuente Preferida,Fuente Alternativa,Familia Geneˊrica;

- **Fuentes con espacios:** Si el nombre de una fuente contiene espacios, debe ir entre comillas (ej: `"Times New Roman"`).
    
- **Familia Genérica:** La última fuente en la pila **siempre** debe ser una familia genérica. Esto asegura que el navegador siempre tendrá una fuente de respaldo para el usuario. Las principales familias genéricas son:
    
    - **`serif`:** Fuentes con remates o "patitas" (ej: Times New Roman, Georgia). Buenas para textos largos.
        
    - **`sans-serif`:** Fuentes sin remates (ej: Arial, Helvetica, Verdana). Modernas, limpias y muy comunes en pantalla.
        
    - **`monospace`:** Fuentes donde cada carácter ocupa el mismo espacio (ej: Courier New). Usadas para código.
        
    - **`cursive`:** Fuentes cursivas o caligráficas.
        
    - **`fantasy`:** Fuentes decorativas.
        

CSS

```
/* Ejemplo de Pila de Fuentes Recomendada */
body {
    font-family: Arial, "Helvetica Neue", Helvetica, sans-serif;
}
```

### 2. Fuentes Web (@font-face)

Para usar fuentes que no están instaladas en el sistema del usuario (como Google Fonts), debemos cargarlas usando la regla `@font-face`.

CSS

```
/* Definición de la fuente, generalmente en la parte superior del CSS */
@font-face {
    font-family: 'MiFuenteEspecial';
    src: url('fonts/mifuente.woff2') format('woff2'),
         url('fonts/mifuente.woff') format('woff');
    font-weight: normal;
    font-style: normal;
}

/* Uso de la fuente */
h1 {
    font-family: 'MiFuenteEspecial', serif;
}
```

---

## 📐 Propiedades de Tamaño y Espaciado

Estas propiedades controlan la métrica del texto para optimizar la legibilidad.

### 1. Tamaño de Fuente (`font-size`)

Define el tamaño de la letra. Se recomienda usar unidades relativas como `rem` o `em` para mejorar la accesibilidad y la adaptabilidad (ver [[05 - Colores y unidades en CSS]]).

|Valor|Descripción|
|---|---|
|**`16px`**|Valor absoluto (la base por defecto).|
|**`1.2rem`**|1.2 veces el tamaño de fuente del elemento raíz (`<html>`).|
|**`150%`**|150% del tamaño de fuente del elemento padre.|

Exportar a Hojas de cálculo

CSS

```
p {
    font-size: 1rem; /* Base para párrafos */
}
h2 {
    font-size: 1.8rem;
}
```

### 2. Altura de Línea (`line-height`)

Establece la altura total de una línea de texto, incluyendo el espacio superior e inferior. Es crucial para la legibilidad.

- **Valor numérico (Recomendado):** Un número sin unidad (ej: `1.5`) que se multiplica por el `font-size` del elemento. Esto asegura que el espaciado se mantenga relativo incluso si se hereda.
    
- **Valor de unidad:** Píxeles, `em`, `rem`.
    

CSS

```
/* 1.5 veces el tamaño de la fuente. Ideal para cuerpo de texto. */
body {
    line-height: 1.5;
}
```

### 3. Espaciado entre Letras (`letter-spacing`)

Define el espacio horizontal entre los caracteres de una palabra.

CSS

```
h1 {
    letter-spacing: 0.1em; /* Un poco más de espacio, ideal para títulos */
}
```

### 4. Espaciado entre Palabras (`word-spacing`)

Define el espacio horizontal entre las palabras.

---

## 🔠 Propiedades de Apariencia y Estilo

Controlan la presentación visual del texto.

### 1. Peso de la Fuente (`font-weight`)

Define el grosor de los caracteres (negrita, regular, delgada).

|Valor|Descripción|
|---|---|
|**`normal`**|El peso estándar (típicamente 400).|
|**`bold`**|Negrita (típicamente 700).|
|**`100` a `900`**|Valores numéricos que definen el grosor. (Solo si la fuente lo soporta).|

Exportar a Hojas de cálculo

CSS

```
.destacado {
    font-weight: 600; /* Un poco más grueso que el normal */
}
```

### 2. Estilo de Fuente (`font-style`)

Define si el texto es normal, cursiva (`italic`) u oblicua (`oblique`).

CSS

```
em {
    font-style: italic; /* Ya es el valor por defecto de <em>, pero se puede anular */
}
```

### 3. Alineación del Texto (`text-align`)

Controla cómo se alinea el contenido en bloque.

|Valor|Descripción|
|---|---|
|**`left`**|Alineado a la izquierda (por defecto).|
|**`right`**|Alineado a la derecha.|
|**`center`**|Centrado.|
|**`justify`**|Justificado (alineado a ambos lados).|

Exportar a Hojas de cálculo

CSS

```
.pie-de-foto {
    text-align: center;
}
```

### 4. Decoración del Texto (`text-decoration`)

Usado más comúnmente para eliminar el subrayado de los enlaces.

|Valor|Descripción|
|---|---|
|**`none`**|Elimina cualquier decoración.|
|**`underline`**|Subrayado.|
|**`overline`**|Línea encima.|
|**`line-through`**|Tachado.|

Exportar a Hojas de cálculo

CSS

```
a {
    text-decoration: none; /* Estándar para quitar el subrayado a los enlaces */
}
```

### 5. Transformación del Texto (`text-transform`)

Cambia la capitalización de un texto sin modificar el HTML.

|Valor|Descripción|
|---|---|
|**`uppercase`**|Todo en mayúsculas.|
|**`lowercase`**|Todo en minúsculas.|
|**`capitalize`**|La primera letra de cada palabra en mayúscula.|

Exportar a Hojas de cálculo

CSS

```
.titulo-seccion {
    text-transform: uppercase;
}
```

---

## ✍️ Propiedad Abreviada (`font`)

CSS permite combinar varias propiedades de fuente en una sola línea usando la propiedad abreviada `font`. El orden de los valores es estricto:

font:[style][weight][size]/[line-height][family];

CSS

```
/* CSS largo */
p {
    font-style: italic;
    font-weight: bold;
    font-size: 16px;
    line-height: 1.5;
    font-family: Georgia, serif;
}

/* CSS abreviado (Equivalente) */
p {
    font: italic bold 16px/1.5 Georgia, serif;
}
```

---

## ✅ Resumen de Puntos Clave

- **`font-family`** define la fuente y debe incluir una **pila de fuentes** que termine con una **familia genérica** (`sans-serif`, `serif`) como respaldo.
    
- **`font-size`** se recomienda usar con unidades relativas como **`rem`** para accesibilidad.
    
- **`line-height`** (altura de línea) es clave para la legibilidad; es mejor usar un **valor numérico** sin unidad (ej: `1.5`).
    
- **`font-weight`** controla el grosor (de `100` a `900` o `bold`/`normal`).
    
- **`text-align`** centra, justifica o alinea el texto.
    
- **`text-decoration: none;`** se usa comúnmente para eliminar el subrayado de los enlaces.
    
- La propiedad **`font`** es una forma abreviada de establecer varias propiedades de tipografía a la vez.
    

👉 Hemos estilizado el texto; ahora debemos darle espacio y estructura física a los elementos que lo contienen. El siguiente paso es entender la ley fundamental del layout en CSS: [[07 - El modelo de caja (Box Model)]].