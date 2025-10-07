## Clase 03 - Etiquetas de texto y formato ✍️

Aquí tienes los apuntes de la clase anterior: [[02 - Estructura básica de un documento HTML]]

### Introducción

En esta clase, vamos a aprender a usar las etiquetas más comunes y fundamentales de HTML: aquellas que dan **estructura jerárquica** y **semántica** al texto. Un documento bien marcado no solo se ve mejor, sino que es esencial para la **accesibilidad** y el **SEO** (optimización para motores de búsqueda). Entenderemos la diferencia crucial entre dar formato visual (tarea de CSS) y dar **significado** al contenido (tarea de HTML).

---

### 1. Elementos de Bloque vs. Elementos en Línea

Antes de sumergirnos en las etiquetas, debemos entender una clasificación clave de HTML:

|Tipo de Elemento|Comportamiento|Ejemplos Comunes|
|---|---|---|
|**Bloque (`block`)**|Ocupa todo el ancho disponible y fuerza un salto de línea antes y después. Crea una "caja" apilada.|`<h1>`, `<p>`, `<div>`, `<section>`|
|**Línea (`inline`)**|Ocupa solo el espacio necesario para su contenido y no fuerza saltos de línea. Se ubica junto a otros elementos en la misma línea.|`<a>`, `<strong>`, `<span>`, `<img>`|

Esta distinción es fundamental para el diseño con CSS, pero en HTML define cómo se presentan los flujos de texto.

---

### 2. Jerarquía y Estructura: Encabezados y Párrafos

#### a) Párrafos: El elemento `<p>`

El elemento más básico para el texto es el párrafo (`<p>`).

- **Uso Semántico:** Se usa para bloques de texto generales. Nunca debe usarse solo para crear espacio vertical; esa es responsabilidad de CSS.
    
- **Comportamiento:** Es un elemento de **bloque**.
    

```
<p>Este es el primer párrafo de mi documento. Se mostrará en una línea.</p>
<p>Este es el segundo párrafo. Automáticamente se separará del anterior.</p>
```

#### b) Encabezados: Los elementos `<h1>` a `<h6>`

Los encabezados definen la estructura jerárquica del contenido. Piensa en ellos como el índice de un libro.

- **Uso Semántico:**
    
    - **`<h1>`**: El título más importante. **Debe haber solo uno** por página.
        
    - **`<h2>`**: Títulos de secciones principales.
        
    - **`<h3>` a `<h6>`**: Subsecciones y detalles más finos.
        
- **Regla de Jerarquía:** Siempre debes seguir la estructura numérica: `<h1>` → `<h2>` → `<h3>`. **Nunca** saltes niveles (ej: de `<h1>` a `<h4>`), ya que confunde a los lectores de pantalla y a los motores de búsqueda.
    
- **Comportamiento:** Son elementos de **bloque**.
    

```
<h1>Título principal de toda la página (Solo uno)</h1>

<section>
    <h2>Sección 1: Primer Tema</h2>
    <p>Contenido principal de la sección...</p>
    
    <h3>Subtema A de la Sección 1</h3>
    <p>Detalles específicos...</p>
</section>
```

---

### 3. Énfasis y Significado Semántico

Estos elementos son de **línea** y se usan para resaltar o darle un significado especial a una porción de texto.

|Etiqueta|Significado (Semántico)|Apariencia (Por defecto)|¿Cuándo usar?|
|---|---|---|---|
|**`<strong>`**|Importancia, severidad, urgencia.|Negrita|Para indicar que algo es **crítico** o **importante**.|
|**`<em>`**|Énfasis. Un cambio de tono o significado.|Cursiva|Para destacar una palabra por **énfasis** o **énfasis** contextual.|
|**`<b>`**|Llamada de atención sin importancia extra.|Negrita|Para nombres de productos, palabras clave en resumen (uso limitado).|
|**`<i>`**|Texto en una voz o estado de ánimo diferente (idiomático).|Cursiva|Para términos técnicos, frases en otro idioma, o nombres científicos.|

> **Nota:** La distinción entre `<strong>`/`<b>` y `<em>`/`<i>` es crucial. HTML5 nos pide usar `<strong>` y `<em>` cuando queremos darle **significado semántico** a ese texto. Usa `<b>` o `<i>` solo cuando quieres un estilo visual sin cambiar el significado o el peso del texto.

```
<p>
    Para continuar, es <strong>extremadamente importante</strong> que uses la etiqueta <code>&lt;p&gt;</code>. 
    De lo contrario, la validación <em>fallará catastróficamente</em>. 
</p>
```

#### Elemento para Citas: `<blockquote>`

- **Uso:** Define una sección que se cita de otra fuente (normalmente un bloque grande de texto).
    
- **Comportamiento:** Es un elemento de **bloque**.
    

```
<blockquote>
    <p>El lenguaje de marcado HTML es la columna vertebral de la Web.</p>
    <footer>— Tim Berners-Lee, Inventor de la World Wide Web</footer>
</blockquote>
```

---

### 4. Elementos de Formato Misceláneos

|Etiqueta|Propósito|Ejemplo|
|---|---|---|
|**`<br>`**|Inserta un **salto de línea** (etiqueta vacía).|`Línea 1<br>Línea 2`|
|**`<hr>`**|Inserta una **línea temática horizontal** (etiqueta vacía).|`<hr>` (indica un cambio de tema)|
|**`<code>`**|Muestra código de programación o informático.|`La variable es <code>x = 5;</code>`|
|**`<pre>`**|Preserva el formato preformateado (espacios, saltos de línea).|Muestra código o texto plano en su formato original (útil para código).|
|**`<span>`**|Un contenedor genérico **en línea** sin significado.|Se usa para aplicar estilos CSS a una pequeña porción de texto.|

---

### Práctica recomendada

**Objetivo:** Crear una estructura de blog o artículo usando la jerarquía correcta.

1. Abre el archivo `plantilla.html` de la [[02 - Estructura básica de un documento HTML]].
    
2. Dentro del `<body>`, reemplaza el contenido con el siguiente código y añádele tus propios textos:
    

```
<body>
    <h1>Mi Primer Artículo en HTML5: Dominando la Estructura</h1>
    <p>Fecha de publicación: <time datetime="2025-01-01">1 de Octubre, 2025</time></p>
    
    <hr> <h2>¿Qué es la Semántica?</h2>
    <p>La semántica en HTML es <strong>vital</strong>. Significa darle un <mark>significado</mark> real a cada pieza de contenido, no solo apariencia.</p>

    <h3>La Importancia de los Párrafos y Encabezados</h3>
    <p>Cada párrafo debe estar dentro de la etiqueta <code>&lt;p&gt;</code>. Si lo olvidas, el documento será sintácticamente incorrecto.</p>
    
    <blockquote>
        "La web está construida sobre información y enlaces, y HTML es el lenguaje de esa información."
    </blockquote>
    
    <h2>Elementos en Línea</h2>
    <p>Usaremos `<em>em`</em> para el énfasis y <strong>`strong`</strong> para la importancia. Si necesitamos forzar un salto, usaremos `<br>`. </p>
</body>
```

3. Visualiza y comprueba:
    
    - El `<h1>` y los `<h2>` deben ser grandes y estar bien separados.
        
    - El texto dentro de `<strong>` y `<em>` debe aparecer en negrita y cursiva, respectivamente.
        
    - El `<blockquote>` debe tener una sangría.
        

---

### Cierre

Has aprendido a estructurar el texto de manera profesional usando la jerarquía de `<h1>` a `<h6>` y la semántica de `<strong>` y `<em>`. Recuerda: **HTML es significado**, **CSS es estilo**. Mantener esta separación es la base de un código limpio y moderno.

---

## Resumen de puntos claves

|Elemento|Tipo|Función Semántica|
|---|---|---|
|**`<h1>` a `<h6>`**|Bloque|**Jerarquía.** Define la estructura del contenido. (Solo un `<h1>` por página).|
|**`<p>`**|Bloque|**Párrafo.** El contenedor principal para texto general.|
|**`<strong>`**|Línea|**Importancia.** Resalta contenido crítico.|
|**`<em>`**|Línea|**Énfasis.** Resalta para cambiar el significado o tono.|
|**`<code>`**|Línea|**Código.** Indica texto de programación o informático.|
|**`<br>` y `<hr>`**|Vacías|Saltos de línea (`<br>`) y separadores temáticos (`<hr>`).|

**👉 Ahora que podemos escribir texto, es hora de navegar. Vamos a la siguiente clase:** [[04 - Enlaces y navegación]]