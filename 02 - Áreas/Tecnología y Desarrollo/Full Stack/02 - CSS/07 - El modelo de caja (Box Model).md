Hemos estilizado el texto; ahora debemos darle espacio y estructura física a los elementos que lo contienen. Esta es, quizás, la regla de oro para el _layout_ en CSS. Si entiendes este concepto, entiendes cómo se disponen todos los elementos en la web.

Aquí tienes la séptima clase.

---

Aquí tienes los apuntes de la clase anterior: [[06 - Tipografía en CSS]]

# 07 - El Modelo de Caja (Box Model)

## 📌 Introducción

El **Modelo de Caja (Box Model)** es el concepto más fundamental en CSS para el diseño y la maquetación. En el mundo de CSS, **cada elemento HTML es tratado como una caja rectangular**.

Comprender cómo se construyen estas cajas, cómo se miden y cómo interactúan entre sí es esencial para controlar el espacio, la posición y las dimensiones de cualquier elemento en tu página.

El Modelo de Caja se compone de cuatro áreas principales que se superponen: **Content**, **Padding**, **Border** y **Margin**.

---

## 📦 Componentes del Modelo de Caja

Imagina una caja de regalo:

### 1. Content (Contenido)

- **¿Qué es?** El área donde reside el contenido real del elemento (texto, imágenes u otros elementos HTML anidados).
    
- **Propiedades:** Su tamaño se controla con las propiedades `width` (ancho) y `height` (alto).
    

### 2. Padding (Relleno)

- **¿Qué es?** El espacio transparente que rodea al contenido, actuando como un **"colchón"** interno.
    
- **Efecto Visual:** El _padding_ está dentro de la caja, por lo que hereda el color de fondo (`background-color`) del elemento.
    
- **Propiedades:** `padding-top`, `padding-right`, `padding-bottom`, `padding-left`, o la propiedad abreviada `padding`.
    

### 3. Border (Borde)

- **¿Qué es?** Una línea sólida (o de otro estilo) que rodea el _padding_ y el _content_. Es el límite visible de la caja.
    
- **Efecto Visual:** Define la frontera de la caja.
    
- **Propiedades:** `border-width`, `border-style`, `border-color`, o la propiedad abreviada `border`.
    

### 4. Margin (Margen)

- **¿Qué es?** El espacio transparente que rodea el borde de la caja, actuando como un **"espacio personal"** para separar la caja de otras cajas adyacentes.
    
- **Efecto Visual:** El _margin_ es el único componente que **nunca** hereda el color de fondo. Es completamente transparente y empuja a otros elementos.
    
- **Propiedades:** `margin-top`, `margin-right`, `margin-bottom`, `margin-left`, o la propiedad abreviada `margin`.
    

---

## 📐 Propiedades Abreviadas (Shorthand)

El uso de propiedades abreviadas para _padding_ y _margin_ es una práctica estándar, ya que simplifica mucho el código.

### 1. Abreviatura de Margin y Padding

|Sintaxis|Valor|Descripción|
|---|---|---|
|**1 valor**|`padding: 20px;`|**Todos** los lados (arriba, derecha, abajo, izquierda).|
|**2 valores**|`margin: 10px 5px;`|**Vertical** (arriba y abajo), luego **Horizontal** (derecha e izquierda).|
|**3 valores**|`padding: 10px 5px 20px;`|**Arriba**, luego **Horizontal** (derecha e izquierda), luego **Abajo**.|
|**4 valores**|`margin: 10px 5px 20px 0;`|Sentido horario: **Arriba**, **Derecha**, **Abajo**, **Izquierda**.|

Exportar a Hojas de cálculo

### 2. Abreviatura de Border

La propiedad `border` requiere al menos tres valores para un borde simple: el ancho, el estilo y el color.

CSS

```
/* Código CSS */
.caja-ejemplo {
    /* Ancho, Estilo, Color */
    border: 3px solid #ff5733; 
}

/* Es equivalente a: */
.caja-ejemplo {
    border-width: 3px;
    border-style: solid;
    border-color: #ff5733;
}
```

👉 Profundizaremos en más detalles de bordes y fondos en [[08 - Fondos y bordes]].

---

## 💥 Colapso de Márgenes (Margin Collapsing)

Esta es una fuente común de confusión para los principiantes:

- **¿Qué es?** Cuando dos elementos con márgenes verticales se tocan (ej: el `margin-bottom` de un título toca el `margin-top` de un párrafo), en lugar de sumarse, el navegador **fusiona (o colapsa)** los márgenes.
    
- **Resultado:** Solo prevalece el margen **más grande** de los dos.
    

**Ejemplo:**

Si tienes:

- `h1` con `margin-bottom: 50px;`
    
- `p` con `margin-top: 20px;`
    

La separación vertical entre ellos será de **50px** (el valor más grande), **no 70px** (la suma).

- ⚠️ **Importante:** El colapso de márgenes solo aplica a los márgenes **verticales** (top y bottom) en el flujo normal del documento. Los márgenes horizontales y el _padding_ **nunca** colapsan.
    

---

## 📏 El Problema del `Box-sizing` y `width`

Por defecto, en el modelo CSS clásico, cuando especificas el `width` (ancho) de una caja, este valor solo se aplica al área de **Content**.

Ancho Total=Content Width+Padding(izquierda+derecha)+Border(izquierda+derecha)

Esto es problemático porque si quieres que una caja ocupe exactamente 200 píxeles, pero luego le añades `padding: 10px;` y `border: 2px;`, el ancho final será: 200px+10px+10px+2px+2px=∗∗224px∗∗. Esto hace que la maquetación sea frustrante y que los elementos "se salgan" de donde deberían estar.

### La Solución: `box-sizing: border-box`

La propiedad `box-sizing` cambia el modo en que el navegador calcula el ancho y el alto total de un elemento.

|Valor|Modelo|Comportamiento|
|---|---|---|
|**`content-box`**|**Modelo Clásico (Por defecto)**|El `width` solo incluye el Content. El padding y el border se **añaden** al ancho total.|
|**`border-box`**|**Modelo Moderno (Recomendado)**|El `width` que defines incluye el Content, el Padding y el Border. El padding y el border se **restan** del Content.|

Exportar a Hojas de cálculo

Con `box-sizing: border-box;`, si estableces un `width: 200px;`, la caja siempre tendrá 200px de ancho total, y el navegador reducirá el espacio de contenido para acomodar el padding y el border. **Esto es indispensable para layouts modernos.**

**Recomendación Global (El Reset CSS moderno):** Casi todos los proyectos profesionales inician su CSS forzando `border-box` en todos los elementos:

CSS

```
/* Reset CSS Básico y Esencial */
html {
    box-sizing: border-box;
}
*, *::before, *::after {
    box-sizing: inherit;
}
```

---

## ✍️ Ejemplo de Código Completo

HTML

```
<div class="card">
    <h2>Título de la Tarjeta</h2>
    <p>Este es el contenido de la caja.</p>
</div>
```

CSS

```
/* Aplicando el box-sizing moderno */
*, *::before, *::after {
    box-sizing: border-box; 
}

.card {
    width: 300px; /* El ancho TOTAL de la tarjeta será 300px */
    
    background-color: #f0f8ff; /* Color de fondo para ver el padding */
    
    /* Padding: Espacio entre el contenido y el borde */
    padding: 15px 20px; /* 15px arriba/abajo, 20px izquierda/derecha */
    
    /* Border: El marco visible */
    border: 5px solid #007bff;
    
    /* Margin: Espacio fuera del borde */
    margin: 30px auto; /* 30px arriba/abajo, 'auto' para centrar horizontalmente */
}
```

---

## ✅ Resumen de Puntos Clave

- Todo elemento HTML es una **Caja** compuesta por **Content**, **Padding**, **Border** y **Margin**.
    
- **Padding** y **Border** añaden tamaño al ancho y alto del elemento en el modelo por defecto (`content-box`).
    
- **Margin** crea espacio _exterior_ y es el único que **colapsa** verticalmente (solo prevalece el valor más grande).
    
- La propiedad **`box-sizing: border-box;`** es esencial y moderna. Asegura que el `width` y `height` definidos **incluyan** el padding y el border, simplificando la maquetación.
    

👉 Ahora que entendemos las capas de la caja, vamos a profundizar en las propiedades visuales que afectan al _background_ y los _borders_. Continúa con [[08 - Fondos y bordes]].