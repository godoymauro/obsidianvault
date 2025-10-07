Hemos visto cómo iniciar y coreografiar el movimiento. Ahora, vamos a la propiedad que es la espina dorsal de todas las animaciones y transiciones de alto rendimiento: **`transform`**. Esta propiedad te permite manipular la geometría de los elementos en 2D y 3D.

Aquí tienes la decimoctava clase.

---

Aquí tienes los apuntes de la clase anterior: [[17 - Animaciones con @keyframes]]

# 18 - Transformaciones

## 📌 Introducción

La propiedad **`transform`** en CSS te permite aplicar efectos de transformación a un elemento. Estos efectos incluyen rotación, escalado, inclinación y traslación (movimiento). La gran ventaja es que, al igual que `opacity`, el navegador puede optimizar estas transformaciones utilizando la **GPU** (tarjeta gráfica), lo que resulta en animaciones mucho más rápidas y suaves, a 60 FPS.

Usar `transform` para el movimiento es la mejor práctica de rendimiento en CSS.

## 🎛️ Funciones de Transformación 2D

La propiedad `transform` toma una o más funciones de transformación separadas por espacios. Las transformaciones 2D son las más comunes y operan en los ejes X (horizontal) y Y (vertical).

### 1. Traslación (`translate`)

Mueve el elemento de su posición actual.

|Función|Descripción|Ejemplo de Uso|
|---|---|---|
|**`translate(x, y)`**|Mueve el elemento en el eje X y Y.|`transform: translate(50px, 10px);` (Mueve 50px a la derecha y 10px hacia abajo)|
|**`translateX(x)`**|Solo en el eje horizontal.|`transform: translateX(-100%);` (Mueve 100% de su propio ancho a la izquierda)|
|**`translateY(y)`**|Solo en el eje vertical.|`transform: translateY(50%);` (Mueve 50% de su propia altura hacia abajo)|

Exportar a Hojas de cálculo

> 💡 **Tip Clave: Centrado Perfecto** Una de las mejores formas de centrar un elemento con `position: absolute;` o `fixed;` es usar `translate(-50%, -50%)`.

CSS

```
/* Centrado horizontal y vertical de un modal de ancho y alto desconocido */
.modal {
    position: absolute;
    top: 50%; /* Mueve el punto superior al 50% de la altura del padre */
    left: 50%; /* Mueve el punto izquierdo al 50% del ancho del padre */
    
    /* Corrige el centro moviendo el elemento hacia atrás la mitad de su propio tamaño */
    transform: translate(-50%, -50%); 
}
```

---

### 2. Escalado (`scale`)

Cambia el tamaño del elemento.

|Función|Descripción|Ejemplo de Uso|
|---|---|---|
|**`scale(x, y)`**|Escala el elemento por el factor x y y.|`transform: scale(1.5, 2);` (1.5x ancho, 2x alto)|
|**`scale(n)`**|Escala uniformemente (x y y son iguales).|`transform: scale(1.2);` (Aumenta el tamaño un 20%)|

Exportar a Hojas de cálculo

CSS

```
.imagen:hover {
    transition: transform 0.3s ease;
    transform: scale(1.1); /* Hace la imagen un 10% más grande */
}
```

---

### 3. Rotación (`rotate`)

Gira el elemento en torno a su punto central (por defecto).

|Función|Descripción|Ejemplo de Uso|
|---|---|---|
|**`rotate(angle)`**|Rota el elemento en 2D.|`transform: rotate(45deg);` (Rota 45 grados en sentido horario)|

Exportar a Hojas de cálculo

CSS

```
/* Rotación infinita para un spinner */
@keyframes girar {
    to { transform: rotate(360deg); }
}

.icono-carga {
    animation: girar 1s linear infinite;
}
```

---

### 4. Inclinación (`skew`)

Inclina el elemento en los ejes X y/o Y.

|Función|Descripción|Ejemplo de Uso|
|---|---|---|
|**`skew(x-angle, y-angle)`**|Inclina en los ejes X y Y.|`transform: skew(20deg, 0);` (Inclina 20 grados en el eje X)|

Exportar a Hojas de cálculo

---

## 🌎 Transformaciones 3D

Puedes mover elementos en el espacio 3D, lo que requiere la perspectiva. Aunque menos comunes, son poderosas.

### 1. Rotación en 3D

|Función|Descripción|Ejemplo de Uso|
|---|---|---|
|**`rotateX(angle)`**|Rota en el eje X (parece que cae hacia atrás).|`transform: rotateX(60deg);`|
|**`rotateY(angle)`**|Rota en el eje Y (parece un giro de página).|`transform: rotateY(180deg);`|
|**`rotateZ(angle)`**|Rota en el eje Z (es igual a `rotate(angle)` en 2D).|`transform: rotateZ(30deg);`|

Exportar a Hojas de cálculo

### 2. Perspectiva (`perspective`)

Para ver los efectos 3D, debes definir una perspectiva en el **elemento padre**. Esto crea la ilusión de profundidad para la cámara virtual.

CSS

```
.escena {
    /* Define la distancia de la cámara (cuanto más pequeño, más extremo el efecto 3D) */
    perspective: 1000px; 
}
```

---

## ➕ Combinando Transformaciones

Puedes aplicar múltiples funciones de transformación a un elemento en una sola propiedad `transform`. Es crucial que recuerdes que las transformaciones se aplican en el **orden en que se escriben**.

CSS

```
/* El elemento primero se escala 1.2 veces, y LUEGO se rota 15 grados */
.combinado {
    transform: scale(1.2) rotate(15deg);
}

/* Si se invierte el orden, el resultado visual es diferente, ya que la rotación se aplica al sistema de coordenadas ya escalado. */
.combinado-inverso {
    transform: rotate(15deg) scale(1.2);
}
```

## ⚙️ El Punto de Origen (`transform-origin`)

Por defecto, todas las transformaciones (rotar, escalar) se realizan desde el **centro** del elemento (`50% 50%`). La propiedad `transform-origin` te permite cambiar ese punto de origen.

CSS

```
.origen-cambiado {
    /* Rota desde la esquina superior izquierda */
    transform-origin: top left; 
    
    /* O usando unidades: 0% 0% es la esquina superior izquierda */
    /* transform-origin: 0 0; */
}
```

---

## ✍️ Ejemplo de Código con Transiciones

HTML

```
<div class="tarjeta-flip">Pasa el ratón</div>
```

CSS

```
.tarjeta-flip {
    /* Alto rendimiento: animamos solo transform */
    transition: transform 0.6s ease;
    
    /* Establecer la perspectiva en el padre ayuda a los efectos 3D si se necesitan */
    /* Aunque en este caso, rotateX es 2D, rotateY o Z ya son 3D */
    
    background-color: teal;
    color: white;
    width: 200px;
    height: 100px;
    
    /* Aseguramos que la rotación comience en el centro */
    transform-origin: center center; 
}

/* Estado de destino: Rotar al pasar el ratón */
.tarjeta-flip:hover {
    /* Rota 10 grados en el sentido horario y la mueve ligeramente */
    transform: rotate(10deg) translateX(5px);
}
```

---

## ✅ Resumen de Puntos Clave

- La propiedad **`transform`** se utiliza para manipular la geometría de los elementos.
    
- Es crucial para el **rendimiento**, ya que es acelerada por la GPU (junto con `opacity`).
    
- Las funciones 2D clave son: **`translate`** (mover), **`scale`** (cambiar tamaño), **`rotate`** (girar) y **`skew`** (inclinar).
    
- La función `translate(-50%, -50%)` es el método moderno para el **centrado perfecto** de elementos posicionados.
    
- Puedes encadenar múltiples transformaciones, pero el **orden** en que se escriben afecta el resultado.
    
- **`transform-origin`** permite cambiar el punto alrededor del cual se aplican las transformaciones (por defecto es el centro).
    

👉 Hemos visto cómo darle vida al diseño. Ahora, vamos a mejorar la forma en que escribimos y organizamos nuestro código CSS para que sea más limpio, potente y fácil de mantener. Empezaremos con la herramienta más importante para la organización: **Variables en CSS**. Continúa con [[19 - Variables en CSS (Custom Properties)]].