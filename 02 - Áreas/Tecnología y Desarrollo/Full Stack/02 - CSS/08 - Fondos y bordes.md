Ya tienes el marco conceptual del Modelo de Caja. Ahora, vamos a darle color y textura a ese marco. Esta clase se centra en cómo hacer que tus cajas se vean bien, controlando sus fondos y dándoles acabados con bordes.

Aquí tienes la octava clase, lista para Obsidian.

---

Aquí tienes los apuntes de la clase anterior: [[07 - El modelo de caja (Box Model)]]

# 08 - Fondos y Bordes

## 📌 Introducción

Una vez que definimos el espacio de nuestro elemento con el Modelo de Caja, necesitamos llenarlo y enmarcarlo. Las propiedades de fondo (`background`) y borde (`border`) nos permiten aplicar colores, imágenes, gradientes y líneas decorativas a nuestros elementos, transformando simples cajas rectangulares en componentes visualmente atractivos.

Recuerda que el **fondo** se extiende sobre el área de **Content** y **Padding**, y el **borde** es la capa exterior visible.

---

## 🖼️ Propiedades de Fondo (`background`)

La propiedad `background` es una propiedad abreviada que combina múltiples subpropiedades para controlar el aspecto del fondo de un elemento.

### 1. Color de Fondo (`background-color`)

Establece el color de fondo para el elemento. Acepta cualquier formato de color de CSS (hex, RGB, HSL, nombres) (👉 Ver también [[05 - Colores y unidades en CSS]]).

CSS

```
.tarjeta {
    background-color: #f8f8ff; /* Blanco azulado claro */
}

.caja-transparente {
    background-color: rgba(0, 0, 0, 0.5); /* Negro con 50% de opacidad */
}
```

### 2. Imagen de Fondo (`background-image`)

Permite usar una o más imágenes como fondo. Se utiliza la función `url()`.

CSS

```
.hero-seccion {
    background-image: url("img/hero-background.jpg");
}
```

### 3. Repetición de Imagen (`background-repeat`)

Controla cómo se comporta una imagen de fondo si es más pequeña que el elemento.

|Valor|Descripción|
|---|---|
|**`repeat`**|El patrón por defecto. La imagen se repite tanto horizontal como verticalmente para cubrir el fondo.|
|**`no-repeat`**|La imagen se muestra una sola vez.|
|**`repeat-x`**|La imagen se repite solo horizontalmente.|
|**`repeat-y`**|La imagen se repite solo verticalmente.|

Exportar a Hojas de cálculo

### 4. Posición de Imagen (`background-position`)

Define dónde se coloca la imagen de fondo dentro del elemento. Se puede definir con palabras clave (ej: `center`, `top`, `bottom`) o unidades (ej: `20px 50%`).

CSS

```
.logo-fondo {
    background-image: url("img/logo.png");
    background-repeat: no-repeat;
    /* Centrado horizontal y vertical */
    background-position: center center; 
}
```

### 5. Tamaño de Imagen (`background-size`)

Controla el tamaño de la imagen de fondo. Crucial para manejar imágenes en diferentes dispositivos.

|Valor|Descripción|
|---|---|
|**`auto`**|El tamaño real de la imagen.|
|**`cover`**|La imagen escala para **cubrir** completamente el contenedor, recortando las partes que sobran. **Ideal para fondos de héroe.**|
|**`contain`**|La imagen escala para **encajar** dentro del contenedor, sin ser recortada, dejando a veces espacios vacíos.|
|**`100% 50%`**|Dimensiones explícitas (100% de ancho, 50% de alto del elemento).|

Exportar a Hojas de cálculo

### 6. Propiedad Abreviada de Fondo

Al igual que muchas otras propiedades, puedes combinar las más importantes en una sola línea.

background:[color][image][repeat][position]/[size];

CSS

```
.galeria {
    /* Color de fondo, Imagen, Repetición, Posición, Tamaño */
    background: #f4f4f4 url('patron.png') no-repeat center / contain;
}
```

---

## 🖼️ Bordes (`border`) y Esquinas (`border-radius`)

El borde es la línea que delimita el _padding_ de la _margin_. Consta de tres subpropiedades esenciales.

### 1. Estilo de Borde (`border-style`)

Define la apariencia del borde. **Un borde no será visible a menos que se defina un estilo.**

|Valor|Descripción|
|---|---|
|**`solid`**|Una línea sólida y continua.|
|**`dotted`**|Puntos.|
|**`dashed`**|Guiones.|
|**`double`**|Dos líneas paralelas.|
|`none`|Sin borde (valor por defecto).|

Exportar a Hojas de cálculo

### 2. Ancho de Borde (`border-width`)

Define el grosor de la línea del borde (usando unidades como `px`, `em`, `rem`).

### 3. Color de Borde (`border-color`)

Define el color del borde (usando cualquier formato de color de CSS).

### 4. Propiedad Abreviada de Borde (¡Recomendada!)

La forma más rápida y común de establecer un borde.

border:[width][style][color];

CSS

```
/* Un borde simple alrededor de toda la caja */
.marco {
    border: 2px solid #3498db; /* 2px de ancho, sólido, azul */
}

/* Borde solo en la parte inferior */
.division {
    border-bottom: 1px dashed gray;
}
```

### 5. Bordes Redondeados (`border-radius`)

Esta propiedad suaviza las esquinas de la caja, incluyendo el fondo, el contenido y el borde. Es clave para el diseño moderno.

|Sintaxis|Valor|Descripción|
|---|---|---|
|**1 valor**|`border-radius: 10px;`|Aplica un radio de 10px a **todas** las cuatro esquinas.|
|**4 valores**|`border-radius: 10px 5px 20px 0;`|Sentido horario: Arriba-izquierda, Arriba-derecha, Abajo-derecha, Abajo-izquierda.|
|**Valor grande**|`border-radius: 50%;`|Si se usa en un elemento cuadrado o circular, lo convierte en un círculo o elipse. **Ideal para fotos de perfil.**|

Exportar a Hojas de cálculo

CSS

```
.boton {
    border-radius: 5px; /* Esquinas ligeramente redondeadas */
}

.avatar {
    width: 100px;
    height: 100px;
    border-radius: 50%; /* Esto crea un círculo perfecto */
}
```

---

## ✍️ Ejemplo de Código Combinado

HTML

```
<div class="banner">
    <h3>Promoción de Verano</h3>
</div>
```

CSS

```
.banner {
    padding: 20px;
    
    /* Fondo: Imagen, no se repite, centrada, cubre el contenedor */
    background: url('img/verano.jpg') no-repeat center center / cover;
    color: white; /* Texto blanco para contrastar con la imagen */
    
    /* Borde: Sólido, grueso y con esquinas redondeadas */
    border: 8px solid #f39c12; /* Naranja intenso */
    border-radius: 15px;
    
    /* La sombra da profundidad a la caja */
    box-shadow: 5px 5px 15px rgba(0, 0, 0, 0.4);
}
```

---

## ✅ Resumen de Puntos Clave

- Las propiedades de **Fondo** (`background`) se aplican al área de contenido y relleno (_padding_).
    
- **`background-image`** permite usar imágenes, mientras que **`background-color`** establece el color base.
    
- **`background-repeat`** controla si la imagen se mosaica o aparece una sola vez (`no-repeat`).
    
- **`background-size: cover;`** es esencial para imágenes de fondo que deben ocupar el 100% del elemento, independientemente del tamaño de la pantalla.
    
- Las propiedades de **Borde** (`border`) requieren definir `border-width`, `border-style` y `border-color`.
    
- **`border-radius`** permite crear esquinas redondeadas o formas circulares (usando `50%`).
    

👉 Hemos aprendido a construir y decorar nuestras cajas. El siguiente paso es crucial para la maquetación: entender cómo interactúan y se colocan estas cajas en la página. Continúa con [[09 - Display y visibilidad]].