Si las transiciones son para movimientos de dos estados, las **Animaciones con `@keyframes`** son el siguiente nivel: te permiten coreografiar una secuencia de estilos a lo largo del tiempo, creando efectos complejos, bucles infinitos y más control.

Aquí tienes la decimoséptima clase.

---

Aquí tienes los apuntes de la clase anterior: [[16 - Transiciones en CSS]]

# 17 - Animaciones con `@keyframes`

## 📌 Introducción

Las **Animaciones CSS** son una serie de estilos que el navegador aplica secuencialmente. A diferencia de las transiciones, las animaciones no necesitan un evento (_hover_ o _click_) para activarse, y permiten definir múltiples puntos intermedios (o "fotogramas clave") entre el estado inicial y el final.

El corazón de una animación CSS es la regla **`@keyframes`**, que es donde defines el guion del movimiento.

## 🎬 La Regla `@keyframes`

Esta regla define el comportamiento de la animación, pero no la aplica al elemento. Simplemente crea la secuencia de movimiento con un nombre que usarás después.

@keyframes nombre-de-animacion{fotogramas clave (from, to, o porcentajes)}

### 1. Definición de Fotogramas Clave

Dentro de `@keyframes`, defines el estado de la animación en puntos específicos de su duración. Puedes usar las palabras clave `from` y `to` o porcentajes:

- **`from` y `to`**: Son equivalentes a `0%` (inicio) y `100%` (final).
    
- **Porcentajes**: Permiten definir pasos intermedios (ej: 25%, 50%, 75%).
    

CSS

```
/* Ejemplo: Una animación simple de entrada (fade-in) */
@keyframes aparecer {
    from {
        opacity: 0; /* Al inicio (0%), la opacidad es 0 */
    }
    to {
        opacity: 1; /* Al final (100%), la opacidad es 1 */
    }
}

/* Ejemplo: Un rebote que se mueve de izquierda a derecha */
@keyframes rebotar {
    0% {
        transform: translateY(0); /* Estado inicial */
    }
    50% {
        transform: translateY(-50px); /* A la mitad del tiempo, se eleva 50px */
    }
    100% {
        transform: translateY(0); /* Estado final, vuelve a la posición inicial */
    }
}
```

---

## ⚙️ Propiedades de Aplicación (Animación)

Para aplicar la secuencia definida en `@keyframes` a un elemento HTML, usamos la propiedad **`animation`** o sus subpropiedades en el selector del elemento.

|Propiedad|Descripción|Ejemplo de Valor|
|---|---|---|
|**`animation-name`**|**Obligatorio.** El nombre de la `@keyframes` a usar (ej: `aparecer`).|`aparecer`|
|**`animation-duration`**|**Obligatorio.** Cuánto tiempo dura un ciclo de la animación.|`2s`|
|**`animation-timing-function`**|La curva de velocidad del movimiento. (Igual que en transiciones, ej: `ease-in`).|`ease-in-out`|
|**`animation-delay`**|Un retraso antes de que la animación comience.|`0.5s`|
|**`animation-iteration-count`**|Cuántas veces se debe repetir la animación.|`3` o **`infinite`**|
|**`animation-direction`**|Si debe reproducirse hacia adelante, hacia atrás o alternar.|`normal`, `reverse`, **`alternate`**|
|**`animation-fill-mode`**|Define el estilo del elemento antes de que empiece y después de que termine.|**`forwards`**, `backwards`, `both`|
|**`animation-play-state`**|Controla si la animación está en pausa o corriendo.|`running` o **`paused`**|

Exportar a Hojas de cálculo

### 1. `animation-iteration-count: infinite;`

Este valor es crucial para efectos de bucle, como indicadores de carga o fondos animados.

### 2. `animation-direction: alternate;`

Si la animación va de A a B, `alternate` hace que el siguiente ciclo vaya de B a A, creando un movimiento de vaivén suave.

### 3. `animation-fill-mode: forwards;`

Define que, al finalizar la animación, el elemento debe mantener los estilos del último fotograma clave (`100%`). Sin esto, el elemento regresa a su estado inicial después de terminar.

---

## ✍️ Propiedad Abreviada (`animation`)

La forma estándar es utilizar la abreviatura, la cual sigue un orden específico:

animation:[name][duration][timing-function][delay][iteration-count][direction][fill-mode];

CSS

```
/* Código CSS */

/* 1. Definición del movimiento */
@keyframes latido {
    0%   { transform: scale(1); }
    50%  { transform: scale(1.1); }
    100% { transform: scale(1); }
}

/* 2. Aplicación del movimiento al elemento */
.corazon {
    /* La animación se llama 'latido', dura 1.5s, tiene un retraso de 0s, y se repite infinitamente */
    animation: latido 1.5s ease-in-out 0s infinite;
    
    /* Nota: 'normal' y 'forwards' son los valores por defecto y no se necesitan */
    
    /* Alternativamente, para un latido suave de ida y vuelta: */
    /* animation: latido 1.5s ease-in-out infinite alternate; */
}

/* 3. Animación de "Spinner" (Carga) */
@keyframes girar {
    from { transform: rotate(0deg); }
    to { transform: rotate(360deg); }
}

.spinner {
    /* Duración: 1s, Lineal, Infinito */
    animation: girar 1s linear infinite; 
    /* Usamos 'linear' para una velocidad de rotación constante */
}
```

---

## 💡 Rendimiento: Animando `transform` y `opacity`

Al igual que con las transiciones (ver [[16 - Transiciones en CSS]]), para crear animaciones fluidas que no saturen el procesador, se recomienda limitar las propiedades animadas a:

- **`transform`**: Para mover (`translate`), escalar (`scale`) o rotar (`rotate`).
    
- **`opacity`**: Para efectos de fundido.
    

Estas propiedades son más fáciles para el navegador optimizar en la GPU, resultando en 60 fotogramas por segundo (FPS) y una experiencia sin _lag_.

---

## ✅ Resumen de Puntos Clave

- Las **Animaciones CSS** permiten coreografiar secuencias de movimiento con múltiples puntos intermedios.
    
- La regla **`@keyframes`** define el guion de la animación, especificando los estilos en porcentajes (`0%` a `100%`).
    
- La propiedad **`animation`** (o sus subpropiedades) aplica los `@keyframes` a un elemento.
    
- **`animation-duration`** y **`animation-name`** son obligatorias.
    
- Utiliza **`animation-iteration-count: infinite;`** para bucles continuos (ej: _loaders_).
    
- Utiliza **`animation-direction: alternate;`** para que la animación se reproduzca en sentido inverso.
    
- Utiliza **`animation-fill-mode: forwards;`** para mantener el estado final de la animación.
    
- Prioriza animar las propiedades **`transform`** y **`opacity`** por razones de rendimiento.
    

👉 Acabamos de ver las dos propiedades más importantes para el movimiento: mover (`translate`), rotar (`rotate`) y escalar (`scale`). Estas son parte de una propiedad madre clave. Continúa con [[18 - Transformaciones]].