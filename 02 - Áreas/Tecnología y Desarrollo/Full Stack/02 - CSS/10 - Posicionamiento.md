Entendido el flujo normal (`display: block/inline/inline-block`), es hora de aprender a tomar el control total, sacando los elementos de ese flujo para posicionarlos con precisión.

Aquí tienes la décima clase, que marca el final de nuestro Nivel 2.

---

Aquí tienes los apuntes de la clase anterior: [[09 - Display y visibilidad]]

# 10 - Posicionamiento

## 📌 Introducción

La propiedad **`position`** de CSS permite controlar el posicionamiento exacto de un elemento en una página web. Por defecto, todos los elementos se colocan en el flujo normal del documento (`position: static;`), pero cuando cambiamos este valor, podemos superponer elementos, fijarlos a la ventana o moverlos en relación con sus posiciones iniciales.

El posicionamiento funciona en conjunto con las propiedades de desplazamiento: **`top`**, **`right`**, **`bottom`**, y **`left`**.

---

## 🧭 Tipos de Posicionamiento

CSS ofrece cinco valores principales para la propiedad `position`.

### 1. `position: static;` (El Flujo Normal)

- **Comportamiento:** Es el valor **predeterminado** de todos los elementos HTML. El elemento se coloca siguiendo el flujo normal del documento.
    
- **Desplazamiento:** Las propiedades `top`, `right`, `bottom` y `left` **no tienen ningún efecto** en los elementos estáticos.
    

### 2. `position: relative;` (Relativo a Sí Mismo)

- **Comportamiento:** El elemento se comporta como si fuera estático (mantiene su lugar en el flujo normal), pero luego se puede **desplazar** desde esa posición original usando `top`, `right`, `bottom`, o `left`.
    
- **Flujo:** Sigue ocupando su espacio original en el _layout_. Los demás elementos se comportan como si no se hubiera movido.
    
- **Uso Clave:** Es el valor más común para establecer un **contexto de apilamiento** (o contexto de posicionamiento) para sus elementos **hijo absolutos** (ver siguiente punto).
    

|Propiedad|Valor|Descripción|
|---|---|---|
|`position`|`relative`|Establece el punto de referencia.|
|`left`|`20px`|Mueve el elemento 20px **a la derecha** de su posición original (se mueve desde la izquierda).|
|`top`|`10px`|Mueve el elemento 10px **hacia abajo** de su posición original.|

Exportar a Hojas de cálculo

### 3. `position: absolute;` (Absoluto al Contenedor)

- **Comportamiento:** El elemento es **eliminado completamente** del flujo normal del documento. Esto significa que ya no ocupa espacio, y los elementos circundantes se comportan como si no estuviera ahí.
    
- **Posicionamiento:** Se posiciona en relación con su **ascendente posicionado más cercano** (el elemento padre, abuelo, etc., que tenga `position: relative`, `absolute`, `fixed`, o `sticky`).
    
    - Si no encuentra un ascendente posicionado, se posiciona en relación con el elemento `<body>` o `<html>`.
        

|Tarea|HTML|CSS|
|---|---|---|
|**Hijo Absoluto**|`<div class="hijo">`|`.hijo { position: absolute; top: 0; right: 0; }`|
|**Padre Relativo**|`<div class="padre">`|`.padre { position: relative; }`|

Exportar a Hojas de cálculo

### 4. `position: fixed;` (Fijo a la Ventana)

- **Comportamiento:** El elemento es removido del flujo normal y se posiciona en relación con la **ventana gráfica del navegador (viewport)**.
    
- **Flujo:** No ocupa espacio en el documento.
    
- **Efecto:** Permanece en su lugar (ej: `top: 0; left: 0;`) incluso cuando el usuario se desplaza por la página.
    
- **Uso Común:** Barras de navegación persistentes, botones de chat o "Volver arriba".
    

### 5. `position: sticky;` (Pegajoso)

- **Comportamiento:** Es un híbrido entre `relative` y `fixed`.
    
    - Mientras está en la vista, se comporta como `position: relative;`.
        
    - Cuando el usuario se desplaza y el elemento toca una posición de desplazamiento definida (`top`, `bottom`, etc.), se "pega" o se comporta como `position: fixed;` hasta que su contenedor padre lo saca de la vista.
        
- **Uso Común:** Encabezados de tabla que permanecen visibles al desplazarse, o barras de navegación que aparecen al hacer _scroll_.
    

CSS

```
/* Sticky requiere al menos una propiedad de desplazamiento para saber cuándo "pegarse" */
.sticky-header {
    position: sticky;
    top: 0; /* Se pegará en la parte superior de la ventana al alcanzar el top: 0 */
    background: #fff;
    z-index: 100; /* Asegura que esté por encima de otros elementos */
}
```

---

## 🎭 La Propiedad `z-index`

Cuando usamos posicionamiento (excepto `static`), los elementos pueden superponerse. La propiedad **`z-index`** controla el orden de apilamiento en el eje Z (profundidad).

- **Comportamiento:** Un elemento con un `z-index` más alto se mostrará **por encima** de un elemento con un `z-index` más bajo.
    
- **Requisito:** `z-index` solo funciona en elementos que tienen un `position` diferente a `static` (es decir, `relative`, `absolute`, `fixed`, o `sticky`).
    
- **Valores:** Acepta cualquier número entero (positivo o negativo). Por defecto es `auto` (o 0).
    

CSS

```
/* La ventana modal siempre debe estar en la capa superior */
.modal-overlay {
    position: fixed;
    z-index: 1000; 
}

/* Un botón de edición puede estar por debajo */
.edit-button {
    position: absolute;
    z-index: 50; 
}
```

---

## ✍️ Ejemplo de Código: Contexto de Posicionamiento

Este es el patrón más común para el posicionamiento absoluto: usar un padre relativo para contener un hijo absoluto.

HTML

```
<div class="tarjeta-producto">
    <img src="producto.jpg" alt="Producto">
    <span class="etiqueta-oferta">¡Oferta!</span>
    <p>Descripción del producto...</p>
</div>
```

CSS

```
/* 1. Establecer el contexto de posicionamiento: el padre es relative */
.tarjeta-producto {
    position: relative; /* Clave para que el hijo absoluto se posicione respecto a esta tarjeta */
    width: 300px;
    border: 1px solid gray;
}

/* 2. Posicionar el hijo absolutamente dentro del padre */
.etiqueta-oferta {
    position: absolute; /* Sacado del flujo normal */
    
    top: 10px; /* 10px desde el borde superior del padre (.tarjeta-producto) */
    right: 10px; /* 10px desde el borde derecho del padre */
    
    background-color: red;
    color: white;
    padding: 5px;
    z-index: 10; /* Asegura que esté por encima de la imagen */
}
```

---

## ✅ Resumen de Puntos Clave

- **`position: static;`** es el valor por defecto; no acepta las propiedades de desplazamiento (`top`, `left`, etc.).
    
- **`position: relative;`** permite mover el elemento desde su posición original, pero **mantiene su espacio** en el flujo.
    
- **`position: absolute;`** saca el elemento del flujo y lo posiciona con respecto a su **ascendente posicionado más cercano**.
    
- **`position: fixed;`** saca el elemento del flujo y lo fija a la **ventana gráfica** del navegador.
    
- **`position: sticky;`** es un híbrido que se comporta como `relative` hasta que alcanza una posición de desplazamiento, momento en el que se vuelve `fixed`.
    
- **`z-index`** controla el orden de apilamiento y solo funciona en elementos **posicionados** (no `static`).
    

---

🎉 ¡Felicidades! Has completado el Nivel 2: Estilos y Cajas. Estás listo para entrar en el mundo de los layouts avanzados. El siguiente nivel se centra en las herramientas modernas para crear diseños complejos y _responsive_.

👉 El primer gran paso en el layout moderno es dominar la alineación en una dimensión. Continúa con [[11 - Flexbox en CSS]].