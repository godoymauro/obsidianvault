Entramos en el Nivel 4, donde transformaremos nuestros diseños estáticos en interfaces dinámicas y atractivas. Empezamos con el movimiento más simple y fundamental: las **Transiciones**.

Aquí tienes la decimosexta clase.

---

Aquí tienes los apuntes de la clase anterior: [[15 - Frameworks CSS y librerías]]

# 16 - Transiciones en CSS

## 📌 Introducción

Las **Transiciones CSS** son una forma sencilla de crear animaciones cortas y suaves cuando una propiedad de un elemento cambia de un estado a otro. En lugar de un cambio abrupto (un elemento se vuelve rojo al instante), la transición define un lapso de tiempo durante el cual el cambio ocurre gradualmente (el elemento se desvanece suavemente a rojo).

Las transiciones son esenciales para mejorar la experiencia del usuario (UX), haciendo que las interacciones comunes (como pasar el ratón sobre un botón) se sientan fluidas y profesionales.

## ✨ Propiedades Clave de la Transición

Una transición requiere que definas **qué** propiedad quieres animar y **cuánto tiempo** debe durar esa animación. Se definen en el estado **inicial** del elemento (antes de la interacción).

### 1. `transition-property` (El Qué)

Especifica la propiedad CSS que deseas animar. Solo las propiedades con valores numéricos que pueden ser interpolados (mezclados) a lo largo del tiempo pueden ser transicionadas (ej: `color`, `width`, `opacity`, `transform`).

|Valor|Descripción|
|---|---|
|**`all`** (Defecto)|Aplica la transición a todas las propiedades que cambien.|
|**`width`**|Aplica la transición solo a la propiedad `width`.|
|**`color, background-color`**|Puedes separar múltiples propiedades con comas.|

Exportar a Hojas de cálculo

### 2. `transition-duration` (El Cuánto Tiempo)

Define el tiempo que debe durar la transición.

- **Valores:** Se expresa en segundos (`s`) o milisegundos (`ms`).
    
- **Requisito:** Si esta propiedad no se define, la transición será de `0s` y no habrá efecto.
    

CSS

```
.boton {
    /* La transición del color tomará 0.3 segundos */
    transition-property: background-color;
    transition-duration: 0.3s; 
}
```

---

## ⏳ Refinando el Movimiento

Una vez que estableces la base, puedes añadir control sobre la velocidad y el retraso de la transición.

### 3. `transition-timing-function` (El Cómo)

Define la curva de aceleración de la transición. Esto es lo que hace que el movimiento se sienta natural o robótico.

|Valor|Descripción|Ejemplo de Uso|
|---|---|---|
|**`ease`** (Defecto)|Empieza lento, acelera en el medio y termina lento. (El más natural).|Movimiento de UI estándar.|
|**`linear`**|Velocidad constante de principio a fin.|Animaciones de _loading_ o procesos.|
|**`ease-in`**|Empieza lento, termina rápido (aceleración).|Elementos que "entran" rápidamente.|
|**`ease-out`**|Empieza rápido, termina lento (desaceleración).|Elementos que "salen" o se detienen.|
|**`cubic-bezier(...)`**|Te permite definir tu propia curva de aceleración avanzada.|Para animaciones de marca muy específicas.|

Exportar a Hojas de cálculo

### 4. `transition-delay` (El Cuándo)

Define un retraso (en segundos o milisegundos) antes de que la transición comience.

CSS

```
.caja {
    /* La caja cambia de color después de 1 segundo de pasar el ratón */
    transition-delay: 1s; 
}
```

---

## ✍️ Propiedad Abreviada (`transition`)

Al igual que con el _border_ o el _font_, existe una propiedad abreviada que es la forma más común de escribir transiciones:

transition:[property][duration][timing-function][delay];

**El orden de `duration` y `delay` es importante:** El primer valor de tiempo que proporcionas es la duración, y el segundo es el retraso (si solo proporcionas uno, es la duración).

### Ejemplo de Código Abreviado

CSS

```
/* Código CSS */
.tarjeta {
    width: 200px;
    height: 100px;
    background-color: blue;
    
    /* ¡La transición clave! */
    /* Animar TODAS las propiedades que cambien, durante 0.5s, usando una curva de ease-out, y sin retraso */
    transition: all 0.5s ease-out 0s; 
}

/* El estado de destino (se activa al pasar el ratón) */
.tarjeta:hover {
    width: 250px;
    background-color: red;
    /* La transición se aplica automáticamente al pasar de blue a red, y de 200px a 250px */
}
```

---

## 💡 Transicionando Múltiples Propiedades

Puedes transicionar diferentes propiedades con diferentes duraciones en la misma regla `transition` separándolas por comas.

CSS

```
.menu-item {
    background-color: #333;
    color: white;
    
    /* Transición de dos propiedades: 
       1. background-color dura 0.2s.
       2. color dura 0.4s y tiene un retraso de 0.1s.
    */
    transition: 
        background-color 0.2s ease-in, 
        color 0.4s ease-out 0.1s;
}

.menu-item:hover {
    background-color: gold;
    color: black;
}
```

En este ejemplo, el fondo cambiará rápidamente, y el color del texto lo hará un poco más tarde y de forma más gradual.

---

## ⚠️ Advertencia: Propiedades NO Animables

No todas las propiedades pueden ser transicionadas. Las propiedades que cambian de forma discreta o no numérica (ej: `display: block` a `display: none`) no funcionan con transiciones.

**Propiedades recomendadas para transicionar (por rendimiento):**

- **`opacity`**: Para efectos de fundido.
    
- **`transform`**: Para mover (`translate`), rotar (`rotate`) o escalar (`scale`). (Ver [[18 - Transformaciones]]).
    

**Evita transicionar propiedades costosas (por rendimiento):**

- `width` y `height` (a menos que sea necesario).
    
- `margin` y `padding`.
    

El navegador puede optimizar las animaciones de `opacity` y `transform` utilizando la GPU (tarjeta gráfica), lo que resulta en animaciones mucho más fluidas.

---

## ✅ Resumen de Puntos Clave

- Las **Transiciones CSS** permiten cambios suaves y graduales entre estados.
    
- Se requiere definir la **`transition-duration`** (cuánto tiempo) para que una transición ocurra.
    
- **`transition-property`** define qué propiedad animar (ej: `all`, `color`, `transform`).
    
- **`transition-timing-function`** (ej: `ease`, `linear`) define la curva de velocidad del movimiento.
    
- La sintaxis abreviada **`transition: [property] [duration] [timing-function] [delay];`** es la más común.
    
- Prioriza la animación de **`opacity`** y **`transform`** para un mejor rendimiento y fluidez.
    

👉 Las transiciones son ideales para dos estados (normal y _hover_). Para animaciones complejas que involucran múltiples pasos, necesitamos **Keyframes**. Continúa con [[17 - Animaciones con @keyframes]].