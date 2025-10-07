Hemos cubierto las herramientas de movimiento y organización. Ahora, regresamos a la precisión con la que seleccionamos elementos. Esta clase se enfoca en selectores avanzados que te permiten apuntar a elementos basándote en su posición o incluso insertar contenido que no existe en el HTML.

Aquí tienes la vigésima clase, la última del Nivel 4.

---

Aquí tienes los apuntes de la clase anterior: [[19 - Variables en CSS (Custom Properties)]]

# 20 - Pseudo-clases y Pseudo-elementos Avanzados

## 📌 Introducción

Ya conoces los selectores básicos y las pseudo-clases simples como `:hover` y `:focus` (vistos en [[03 - Selectores en CSS]]). Ahora, exploraremos los selectores más poderosos que nos permiten tomar decisiones de estilo basándonos en la posición de un elemento dentro de un grupo de hermanos, su tipo específico, o incluso generar contenido puramente decorativo.

El dominio de estas técnicas permite escribir CSS más limpio, ya que reducimos la necesidad de añadir clases en el HTML solo para estilizar el primer o último elemento de una lista.

## 🧐 Pseudo-clases de Estructura (`:nth-*`)

Estas pseudo-clases te permiten seleccionar elementos basándote en un patrón numérico, su posición o su tipo de etiqueta dentro de su contenedor padre.

|Pseudo-clase|Descripción|Uso Común|
|---|---|---|
|**`:first-child`**|Selecciona el primer elemento **hijo** entre todos sus hermanos, sin importar el tipo de etiqueta.|`p:first-child { color: red; }` (El primer hijo es un párrafo)|
|**`:last-child`**|Selecciona el último elemento **hijo** entre todos sus hermanos.|`li:last-child { border-bottom: none; }`|
|**`:nth-child(n)`**|Selecciona el elemento hijo basándose en una fórmula numérica `n`.|Ideal para alternar estilos (tablas, listas).|
|**`:only-child`**|Selecciona un elemento que **no tiene hermanos**. Es el único hijo de su padre.|`div > p:only-child { font-style: italic; }`|

Exportar a Hojas de cálculo

### 1. La Fórmula `:nth-child(n)`

La variable `n` en la fórmula puede ser un número entero, una palabra clave o una expresión matemática simple.

|Fórmula|Significado|Ejemplo|
|---|---|---|
|**`2`**|El segundo elemento hijo.|`.lista li:nth-child(2)`|
|**`odd`**|Elementos en posición impar (1, 3, 5...).|`tr:nth-child(odd)`|
|**`even`**|Elementos en posición par (2, 4, 6...).|`li:nth-child(even)`|
|**`2n`**|Cada segundo elemento (igual que `even`).|`.fila:nth-child(2n)`|
|**`3n+1`**|Cada tercer elemento, empezando por el primero (1, 4, 7...).|`a:nth-child(3n+1)` (Útil para _grids_ y layouts)|
|**`-n+3`**|Los primeros tres elementos (3, 2, 1).|`li:nth-child(-n+3)`|

Exportar a Hojas de cálculo

---

## 🏷️ Selectores Basados en Tipo

Las pseudo-clases anteriores pueden ser ambiguas si el orden de las etiquetas HTML no es consistente. Por ejemplo, si el primer hijo es un `div` en lugar de un `p`, `:first-child` fallará. Para resolver esto, usamos los selectores basados en **tipo de etiqueta**.

|Pseudo-clase|Descripción|
|---|---|
|**`:first-of-type`**|Selecciona la primera aparición de un tipo de elemento específico entre sus hermanos.|
|**`:last-of-type`**|Selecciona la última aparición de un tipo de elemento específico entre sus hermanos.|
|**`:nth-of-type(n)`**|Selecciona la aparición **n-ésima** de un tipo de elemento.|
|**`:only-of-type`**|Selecciona un elemento que **no tiene otros hermanos del mismo tipo de etiqueta**.|

Exportar a Hojas de cálculo

**Comparación Clave:**

- `p:first-child` solo funciona si el primer hijo de ese padre es un párrafo.
    
- `p:first-of-type` funciona si hay un `div` y luego un `p`; seleccionará ese primer `p`.
    

---

## ➕ Pseudo-elementos de Contenido (`::before` y `::after`)

Los pseudo-elementos **`::before`** y **`::after`** son cruciales para el diseño. Permiten inyectar contenido decorativo o funcional sin modificar el HTML.

- **Comportamiento:** Crean un elemento hijo **virtual** que existe antes o después del contenido real del elemento.
    
- **Requisito:** Para que sean visibles, **siempre** deben incluir la propiedad **`content`**.
    
- **Flujo:** Por defecto, los pseudo-elementos se comportan como elementos **`inline`**, pero puedes cambiar su comportamiento con `display: block;` o `display: absolute;`.
    

|Pseudo-elemento|Descripción|Uso Común|
|---|---|---|
|**`::before`**|Crea un hijo virtual _antes_ del contenido del elemento.|Iconos, comillas decorativas, marcos.|
|**`::after`**|Crea un hijo virtual _después_ del contenido del elemento.|Líneas divisorias, triángulos, efectos de _hover_.|

Exportar a Hojas de cálculo

### Inserción de Contenido

El valor de la propiedad `content` puede ser:

1. **Texto:** `content: "» ";`
    
2. **Un carácter especial o icono:** `content: "\2713";` (un _tick_ de verificación Unicode)
    
3. **Vacío (para decoración):** `content: "";` (El más común, usado para crear formas y líneas)
    

CSS

```
/* Ejemplo 1: Comillas decorativas */
q::before {
    content: "«";
    color: gray;
    font-size: 1.2rem;
}

/* Ejemplo 2: Línea de división invisible (usando content: "") */
.titulo-seccion::after {
    content: "";
    display: block; /* Lo hace un bloque para poder controlar su ancho y alto */
    width: 50px;
    height: 3px;
    background-color: var(--color-primario);
    margin: 5px auto 0; /* Centra la línea */
}
```

---

## ✍️ Ejemplo de Código Combinado

HTML

```
<ul class="lista-productos">
    <li>Producto A</li>
    <li>Producto B</li>
    <li>Producto C</li>
    <li>Producto D</li>
    <li>Producto E</li>
</ul>
```

CSS

```
.lista-productos li {
    background-color: #f4f4f4;
    padding: 10px;
    margin-bottom: 5px;
    list-style: none;
}

/* 1. Pseudo-elemento: Añadir un número de icono antes de cada ítem */
.lista-productos li::before {
    content: "⭐"; /* Icono de estrella Unicode */
    margin-right: 10px;
}

/* 2. Pseudo-clase Estructural: Alternar el color de fondo para mejorar la legibilidad */
.lista-productos li:nth-child(even) {
    background-color: #e9e9e9; /* Fondo gris más oscuro para los pares */
}

/* 3. Pseudo-clase de Tipo y Posición: Estilo especial para el último ítem (envío gratis) */
.lista-productos li:last-of-type {
    font-weight: bold;
    color: green;
}

/* 4. Pseudo-clase de Interacción: Efecto de color al pasar el ratón */
.lista-productos li:hover {
    background-color: #ccc;
    transition: background-color 0.2s ease; /* Transición suave (Ver [[16 - Transiciones en CSS]]) */
}
```

---

## ✅ Resumen de Puntos Clave

- Las pseudo-clases avanzadas (ej: `:nth-child`, `:last-of-type`) permiten seleccionar elementos basándose en su **posición o tipo** dentro del árbol DOM, no en una clase o ID.
    
- **`:nth-child(n)`** permite aplicar estilos en patrones alternos (ej: `even`, `odd`, `3n+1`).
    
- Los selectores **`-of-type`** son más robustos ya que se enfocan en la etiqueta específica, no en el orden absoluto de todos los hijos.
    
- Los pseudo-elementos **`::before`** y **`::after`** inyectan contenido puramente visual o decorativo **sin tocar el HTML**.
    
- **`content: "";`** es necesario para que `::before` y `::after` existan y puedan ser estilizados (ej: crear líneas, triángulos).
    

---

🎉 ¡Felicidades! Has completado el Nivel 4: Estilos Avanzados. Estás escribiendo CSS limpio, responsivo, animado y organizado.

👉 Ahora, abordaremos el último nivel: cómo escribir CSS de forma profesional para que sea accesible y fácil de mantener en proyectos grandes. Continúa con [[21 - Accesibilidad con CSS (A11y)]].