¡Perfecto! Hemos visto qué es CSS y cómo se estructura una regla. Ahora, vamos a la parte crucial: cómo decirle a CSS **exactamente a qué elemento** aplicar los estilos. El poder de CSS reside en sus **selectores**.

Aquí tienes la tercera clase, lista para Obsidian.

---

Aquí tienes los apuntes de la clase anterior: [[02 - Anatomía de una regla CSS]]

# 03 - Selectores en CSS

## 📌 Introducción

Los **Selectores CSS** son la herramienta más importante que tenemos. Son patrones utilizados para apuntar a los elementos HTML a los que queremos aplicar una hoja de estilos. Si la Anatomía de una Regla CSS es la _Instrucción_, el Selector es la _Dirección_.

Dominar los selectores te permite aplicar estilos con una precisión quirúrgica, evitando estilos accidentales y simplificando tu código.

---

## 🎯 Tipos Fundamentales de Selectores

La mayoría de los selectores se construyen basándose en la etiqueta, el atributo `class` o el atributo `id` de los elementos HTML.

### 1. Selector Universal (`*`)

Selecciona **todos** los elementos en la página HTML. Se usa típicamente para aplicar un estilo base o para reiniciar los estilos por defecto del navegador.

|Selector|Significado|Ejemplo de Código CSS|
|---|---|---|
|`*`|Todos los elementos.|`* { margin: 0; padding: 0; }`|

Exportar a Hojas de cálculo

### 2. Selector de Tipo (Etiqueta)

Selecciona todos los elementos que coinciden con el nombre de la etiqueta HTML. Es el selector más básico que ya hemos usado.

|Selector|Significado|Ejemplo de Código CSS|
|---|---|---|
|`p`|Todos los párrafos.|`p { font-family: sans-serif; }`|
|`h1`|Todos los títulos de nivel 1.|`h1 { color: #007bff; }`|

Exportar a Hojas de cálculo

### 3. Selector de Clase (`.`)

Selecciona todos los elementos que tienen un atributo `class` específico. Las clases son **reutilizables** y puedes aplicarlas a tantos elementos como quieras. Son el tipo de selector más común en el desarrollo profesional.

|Selector|Significado|Ejemplo de Código HTML|Ejemplo de Código CSS|
|---|---|---|---|
|`.nombre`|Elementos con `class="nombre"`.|`<button class="btn">...</button>`|`.btn { background-color: blue; }`|

Exportar a Hojas de cálculo

> 💡 **Nota:** En el HTML se usa solo el nombre de la clase (`btn`), pero en el CSS debes anteponer un punto (`.btn`).

### 4. Selector de ID (`#`)

Selecciona un único elemento que tiene un atributo `id` específico. El valor del `id` debe ser **único** en toda la página.

|Selector|Significado|Ejemplo de Código HTML|Ejemplo de Código CSS|
|---|---|---|---|
|`#identificador`|El elemento con el `id="identificador"`.|`<div id="menu-principal">...</div>`|`#menu-principal { width: 300px; }`|

Exportar a Hojas de cálculo

> 💡 **Nota:** En el HTML se usa solo el nombre del ID (`menu-principal`), pero en el CSS debes anteponer el símbolo de almohadilla (`#menu-principal`).

---

## ⛓️ Selectores Combinadores y de Atributos

Estos selectores te permiten aplicar estilos basándote en la relación entre los elementos o en sus atributos.

### 5. Selectores Combinadores

|Selector|Nombre|Descripción|Ejemplo de Código CSS|
|---|---|---|---|
|**Descendiente** ( )|Espacio|Selecciona los elementos **A** que están **dentro** de un elemento **B** (sin importar la profundidad).|`ul li { list-style: none; }` (Cualquier `<li>` dentro de cualquier `<ul>`)|
|**Hijo Directo** (`>`)|Mayor que|Selecciona los elementos **A** que son **hijos inmediatos** de un elemento **B**.|`div > p { color: red; }` (Solo los `<p>` que están directamente dentro de un `<div>`)|
|**Hermano Adyacente** (`+`)|Más|Selecciona un elemento **A** que está **inmediatamente después** de un elemento **B** en el mismo nivel.|`h2 + p { margin-top: 0; }` (El `<p>` que sigue inmediatamente a un `<h2>`)|
|**Hermano General** (`~`)|Tilde|Selecciona un elemento **A** que está **después** de un elemento **B** en el mismo nivel (sin ser adyacente).|`h1 ~ p { background: yellow; }` (Cualquier `<p>` que sigue a un `<h1>`)|

Exportar a Hojas de cálculo

### 6. Selectores de Atributos

Selecciona elementos basados en la presencia o el valor de un atributo HTML.

|Selector|Descripción|Ejemplo de Código HTML|Ejemplo de Código CSS|
|---|---|---|---|
|`[attr]`|Elementos con el atributo `attr`.|`<a href="link">...</a>`|`[href] { text-decoration: none; }`|
|`[attr="val"]`|Elementos con atributo `attr` igual a `val`.|`<input type="email">`|`input[type="email"] { border: 1px solid blue; }`|
|`[attr*="val"]`|Elementos cuyo atributo **contiene** `val` en cualquier parte.|`<a title="external link">...</a>`|`[title*="external"] { color: green; }`|

Exportar a Hojas de cálculo

---

## ✨ Pseudo-clases y Pseudo-elementos

Son selectores que te permiten apuntar a elementos basándote en un **estado** o una **porción** específica, y no solo en la estructura.

### 7. Pseudo-clases (`:`)

Definen un estado especial de un elemento. Se utilizan con un solo dos puntos (`:`).

|Pseudo-clase|Descripción|Uso Común|
|---|---|---|
|`:hover`|El elemento cuando el ratón está sobre él.|`a:hover { color: red; }`|
|`:active`|El elemento cuando está siendo clickeado.|`button:active { background: darkgray; }`|
|`:focus`|El elemento que tiene el foco (ej: un campo de formulario).|`input:focus { border-color: orange; }`|
|`:first-child`|El primer elemento entre sus hermanos.|`li:first-child { font-weight: bold; }`|
|`:nth-child(n)`|El hijo número `n` (ej: `odd`, `even`, `3`).|`li:nth-child(2n) { background: #eee; }` (Elementos pares)|

Exportar a Hojas de cálculo

### 8. Pseudo-elementos (`::`)

Definen una porción del elemento que puede ser estilizada. Se utilizan con doble dos puntos (`::`), aunque navegadores antiguos aceptan el formato con un solo dos puntos.

|Pseudo-elemento|Descripción|Uso Común|
|---|---|---|
|`::before`|Inserta contenido antes del contenido del elemento.|`p::before { content: "» "; }`|
|`::after`|Inserta contenido después del contenido del elemento.|`h2::after { content: ""; display: block; height: 1px; background: black; }` (Línea decorativa)|
|`::first-letter`|Selecciona la primera letra de un texto.|`p::first-letter { font-size: 200%; }` (Letra capital)|

Exportar a Hojas de cálculo

---

## ✍️ Ejemplo de Código Combinado

HTML

```
<div id="contenedor">
    <a href="#" class="enlace">Inicio</a>
    <p>Primer párrafo.</p>
    <p class="resaltado">Segundo párrafo.</p>
</div>
```

CSS

```
/* Código CSS */

/* Selector de ID + Selector de clase: solo el enlace dentro del contenedor */
#contenedor .enlace {
    color: green;
}

/* Selector de clase: para todos los elementos con la clase "resaltado" */
.resaltado {
    font-style: italic;
}

/* Pseudo-clase: cambia el color del enlace al pasar el ratón */
.enlace:hover {
    color: red;
    text-decoration: underline;
}

/* Combinador Descendiente: todos los párrafos dentro del div con ID "contenedor" */
#contenedor p {
    margin-bottom: 10px;
}
```

---

## ✅ Resumen de Puntos Clave

- Los selectores son esenciales para apuntar a elementos HTML.
    
- Los selectores más comunes y recomendados para la modularidad son los de **Clase** (`.`).
    
- El **ID** (`#`) debe ser único y tiene una alta prioridad.
    
- Los **Combinadores** ( , `>`, `+`, `~`) te permiten seleccionar elementos basándote en sus relaciones estructurales (padre-hijo, hermanos).
    
- Las **Pseudo-clases** (`:hover`) seleccionan elementos en un estado particular.
    
- Los **Pseudo-elementos** (`::before`) seleccionan y estilizan partes específicas de un elemento.
    

👉 Con tantos tipos de selectores, surge una pregunta clave: ¿Qué pasa si múltiples reglas apuntan al mismo elemento? Esto nos lleva a [[04 - Especificidad y herencia]].