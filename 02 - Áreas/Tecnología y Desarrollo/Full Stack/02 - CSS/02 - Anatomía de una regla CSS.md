¡Excelente! Con los fundamentos de CSS ya establecidos, es hora de profundizar en la unidad más básica y crucial de nuestro aprendizaje: **la regla CSS**.

Aquí tienes la segunda clase.

---

Aquí tienes los apuntes de la clase anterior: [[01 - Introducción a CSS]]

# 02 - Anatomía de una Regla CSS

## 📌 Introducción

Una **regla CSS** es el bloque fundamental de código que le dice al navegador qué elemento HTML debe ser afectado y cómo debe verse. Es como una instrucción completa que consta de tres partes esenciales que trabajan juntas para aplicar estilos.

Comprender la anatomía de esta regla es vital, ya que es la estructura que usarás literalmente en cada línea de CSS que escribas.

## 🧱 Las Partes de una Regla

Una regla CSS completa se compone de un **Selector** y un bloque de **Declaración**.

Regla CSS=Selector+Bloque de Declaracioˊn

### 1. El Selector

El selector es la parte que **apunta** o **selecciona** el elemento o elementos HTML a los que se les aplicará el estilo. El selector puede ser un nombre de etiqueta, una clase, un ID o muchas otras cosas que exploraremos en la siguiente lección.

- **Ejemplo:** En el código `h1 { ... }`, el selector es **`h1`**.
    

### 2. El Bloque de Declaración

El bloque de declaración contiene una o más **declaraciones** de estilo y está encerrado por llaves **`{ }`**.

### 3. La Declaración

Cada declaración dentro del bloque establece un estilo específico y siempre tiene dos partes separadas por dos puntos **`:`**: una **Propiedad** y un **Valor**, y termina con un punto y coma **`;`**.

Declaracioˊn=Propiedad:Valor;

#### A. Propiedad

La propiedad es el atributo de estilo que deseas cambiar. Hay cientos de propiedades en CSS (ej: `color`, `font-size`, `margin`, `background`).

- **Ejemplo:** En el código `color: blue;`, la propiedad es **`color`**.
    

#### B. Valor

El valor es la forma en la que deseas aplicar esa propiedad (ej: el color exacto, el tamaño exacto, etc.).

- **Ejemplo:** En el código `color: blue;`, el valor es **`blue`**.
    

---

## 🔬 Ejemplo Explicado Línea por Línea

Analicemos una regla CSS común para entender cada componente en acción:

CSS

```
/* Código CSS en styles.css */
p {
    color: #333333;
    font-size: 16px;
    line-height: 1.5;
}
```

|Componente|Tipo|Descripción|
|---|---|---|
|**`p`**|**Selector**|Apunta a todos los elementos de párrafo (`<p>`) en el HTML.|
|**`{`**...**`}`**|Bloque de Declaración|Contiene todas las instrucciones de estilo para los párrafos.|
|**`color`**|Propiedad (1)|Indica qué atributo vamos a modificar: el color del texto.|
|**`:`**|Separador|Separa la propiedad del valor.|
|**`#333333`**|Valor (1)|Define el color del texto, un gris oscuro (usando formato hexadecimal).|
|**`;`**|Terminador|Marca el final de la primera declaración. **¡Es obligatorio!**|
|**`font-size`**|Propiedad (2)|Indica qué atributo vamos a modificar: el tamaño de la fuente.|
|**`16px`**|Valor (2)|Define el tamaño de la fuente en 16 píxeles.|
|**`line-height`**|Propiedad (3)|Indica el espacio entre las líneas de texto.|
|**`1.5`**|Valor (3)|Define el espaciado de línea como 1.5 veces el tamaño de la fuente.|

Exportar a Hojas de cálculo

## 💡 Reglas de Sintaxis Fundamentales

1. **Terminador (`;`)**: Cada declaración dentro del bloque _debe_ terminar con un punto y coma. El último punto y coma es técnicamente opcional, pero es una **muy buena práctica** incluirlo siempre para evitar errores al añadir nuevas declaraciones después.
    
2. **Delimitadores de Bloque (`{ }`)**: El bloque de declaración siempre comienza con una llave de apertura y termina con una llave de cierre.
    
3. **Separador (`:`)**: Los dos puntos separan la propiedad de su valor.
    
4. **Comentarios**: Puedes añadir notas en tu CSS usando el formato **`/* Este es un comentario */`**. Los comentarios son ignorados por el navegador.
    

## ✍️ Ejemplos Prácticos de Código

Imagina que tienes el siguiente HTML:

HTML

```
<body>
    <h2 class="subtitulo">Mi Sección Principal</h2>
    <p>Este es un texto de ejemplo.</p>
</body>
```

Y aplicamos este CSS Externo:

CSS

```
/* Código CSS (styles.css) */

/* 1. Regla para el h2 */
h2 {
    /* Propiedad: Valor; */
    text-align: right;
    color: orange;
}

/* 2. Regla para el párrafo */
p {
    /* Múltiples declaraciones en una sola regla */
    font-weight: bold;
    background-color: lightgray;
    padding: 10px;
}
```

**Resultado:** El título h2 aparecerá alineado a la derecha y de color naranja. El párrafo p tendrá una fuente en negrita, un fondo gris claro y un relleno de 10 píxeles a su alrededor.

---

## ✅ Resumen de Puntos Clave

- Una **Regla CSS** es la unidad básica y consta de un **Selector** y un **Bloque de Declaración**.
    
- El **Selector** elige los elementos HTML a estilizar.
    
- El **Bloque de Declaración** se encierra en llaves **`{ }`**.
    
- Cada **Declaración** dentro del bloque se compone de una **Propiedad** y un **Valor**, separados por dos puntos **`:`** y terminada con un punto y coma **`;`**.
    
- La sintaxis correcta (uso de llaves, puntos y comas) es crítica para que el navegador interprete tus estilos.
    

👉 Hemos utilizado selectores simples como `h1` y `p`. Ahora es el momento de dominar el arte de la selección. Continúa con [[03 - Selectores en CSS]].