Saber manipular el DOM es genial, pero la web solo cobra vida cuando responde a lo que el usuario hace. El mecanismo para lograr esa respuesta son los **Eventos**.

---

Aquí tienes los apuntes de la clase anterior: [[11 - El DOM (Document Object Model)]]

# 12 - Eventos en JavaScript

## ✍️ Introducción: Detección de Interacciones

Un **Evento** es algo que ocurre en el navegador (o en el DOM) a lo que JavaScript puede reaccionar. Es la señal que dispara la ejecución de una función.

Piensa en los eventos como **alarmas 🚨**. El navegador está esperando silenciosamente, y cuando sucede algo (la alarma suena), el código JavaScript que conectaste a esa alarma se ejecuta automáticamente.

Los eventos más comunes son:

- **Mouse:** `click`, `mouseover`, `mousedown`.
    
- **Teclado:** `keydown`, `keyup`.
    
- **Formulario:** `submit`, `change`.
    
- **Ventana:** `load`, `resize`, `scroll`.
    

---

## 💻 Explicación Detallada: Conexión de Eventos

Para manejar un evento, necesitamos dos cosas:

1. El **Elemento** del DOM que va a escuchar el evento.
    
2. La **Función** (el _Event Handler_ o manejador de eventos) que se ejecutará cuando el evento ocurra.
    

La forma moderna y más recomendada de conectar eventos es usando el método **`addEventListener()`**.

### 1. El Método `addEventListener()`

Este método nos permite adjuntar una función a un evento específico de un elemento sin modificar el HTML, lo que mantiene el código JavaScript y el HTML separados y limpios (una buena práctica).

#### **Sintaxis:**

JavaScript

```
elemento.addEventListener('nombreDelEvento', funcionAEjecutar);
```

|Parte|Descripción|Ejemplo|
|---|---|---|
|**`elemento`**|El nodo DOM que seleccionamos (ej: un botón).|`const miBoton = ...`|
|**`nombreDelEvento`**|El evento que queremos escuchar (siempre en minúsculas y como un **String**).|`'click'`, `'mouseover'`|
|**`funcionAEjecutar`**|La función (_callback_) que se llama cuando el evento ocurre.|`manejarClick` o una función flecha.|

Exportar a Hojas de cálculo

### 2. El Objeto `Event`

Cuando un evento se dispara, JavaScript automáticamente crea un **Objeto Evento** y lo pasa como primer argumento a la función manejadora. Este objeto contiene toda la información útil sobre lo que acaba de ocurrir.

|Propiedad del Objeto Evento (`e`)|Descripción|
|---|---|
|**`e.target`**|Una referencia al elemento del DOM que disparó el evento.|
|**`e.preventDefault()`**|Método crucial para detener la acción por defecto del navegador (ej: detener el envío de un formulario).|
|**`e.stopPropagation()`**|Detiene la propagación del evento (ver punto 4).|
|**`e.key`**|En eventos de teclado, qué tecla fue presionada.|

Exportar a Hojas de cálculo

JavaScript

```
// El parámetro 'e' (o 'event') es el Objeto Evento
miBoton.addEventListener('click', (e) => {
    console.log("El evento fue disparado por:", e.target);
    console.log("El tipo de evento fue:", e.type); // Muestra: 'click'
});
```

### 3. Eliminar un Evento

Si por alguna razón necesitas dejar de escuchar un evento (ej: después de que el usuario lo ha hecho un número limitado de veces), usas `removeEventListener()`. **Nota:** La función _callback_ debe tener un nombre (no puede ser anónima) para poder referenciarla.

JavaScript

```
function manejarUnicaVez() {
    console.log("Solo me ejecuto una vez.");
    // Después de ejecutar, elimino el propio listener
    miBoton.removeEventListener('click', manejarUnicaVez); 
}

const miBoton = document.getElementById('btn-unico');
miBoton.addEventListener('click', manejarUnicaVez); 
```

### 4. Propagación de Eventos (Bubbling y Capturing)

Cuando haces clic en un elemento, el evento no se queda solo en él. El evento viaja por el DOM.

- **Bubbling (Burbujeo):** El evento viaja desde el elemento más interno (`<button>`) hacia afuera, subiendo por sus padres (`<div>`, `<body>`, `<html>`, `document`).
    
- **Capturing (Captura):** El evento viaja de arriba abajo (desde `document` hacia el elemento objetivo) **antes** de burbujear.
    

La mayoría de las veces, solo nos interesa el **Bubbling** (el comportamiento por defecto). El burbujeo es lo que permite la **Delegación de Eventos**.

### 5. Delegación de Eventos

Es una técnica crucial de optimización. En lugar de adjuntar un _listener_ a **cada** elemento de una lista (ej: 100 botones), adjuntas un **único _listener_ al contenedor padre**.

Cuando un hijo dispara el evento, el evento **burbujea** hasta el padre, y el padre lo detecta. Usando `e.target`, el manejador puede identificar qué hijo fue el que realmente se hizo clic.

JavaScript

```
// Mala práctica: 100 listeners
// listaDeBotones.forEach(btn => btn.addEventListener('click', hacerAlgo)); 

// Buena práctica: 1 solo listener al padre
contenedorPadre.addEventListener('click', (e) => {
    // Si el elemento clickeado (target) tiene la clase 'item-lista'
    if (e.target.classList.contains('item-lista')) {
        console.log("Hiciste click en el ítem:", e.target.textContent);
    }
});
```

Esto ahorra memoria y mejora el rendimiento.

---

## 🛠️ Ejemplos Prácticos: Implementando Eventos

Copia este código en un archivo `index.html` (reemplazando el de la clase anterior) y ábrelo en el navegador.

HTML

```
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Clase 12 - Eventos</title>
</head>
<body>

    <h1 id="titulo">Gestor de Eventos</h1>
    
    <div id="contenedor-padre" style="padding: 20px; border: 1px solid black;">
        <p>Contenedor Padre (Bubbling)</p>
        <button id="btn-saludo" class="btn-interactivo">Saludar</button>
        <button id="btn-cambio" class="btn-interactivo">Cambiar Título</button>
    </div>

    <form id="formulario-contacto">
        <input type="text" placeholder="Escribe algo..." id="input-texto">
        <button type="submit">Enviar Formulario</button>
    </form>

    <script>
        const btnSaludo = document.getElementById('btn-saludo');
        const btnCambio = document.getElementById('btn-cambio');
        const titulo = document.getElementById('titulo');
        const inputTexto = document.getElementById('input-texto');
        const formulario = document.getElementById('formulario-contacto');
        const contenedorPadre = document.getElementById('contenedor-padre');


        // --- 1. Evento CLICK Básico ---
        btnSaludo.addEventListener('click', () => {
            console.log("¡Hola! Hiciste click en el botón Saludar.");
            alert("¡Hola, mundo de Eventos!");
        });

        // --- 2. Accediendo al Objeto Evento ---
        btnCambio.addEventListener('click', (e) => {
            console.log("El elemento que disparó esto es el botón:", e.target.id);
            titulo.textContent = `Título Actualizado en: ${new Date().toLocaleTimeString()}`;
        });

        // --- 3. Eventos de Formulario (y preventDefault) ---
        formulario.addEventListener('submit', (e) => {
            // e.preventDefault() detiene la acción por defecto (recargar la página al enviar)
            e.preventDefault(); 
            console.log("Formulario Interceptado. Valor:", inputTexto.value);
            alert("Formulario enviado (simulado)");
        });

        // --- 4. Eventos de Teclado ---
        inputTexto.addEventListener('keyup', (e) => {
            // 'keyup' se dispara cuando la tecla es liberada
            // console.log("Tecla presionada:", e.key);
            titulo.textContent = `Escribiendo: ${inputTexto.value}`;
        });

        // --- 5. Demostración de Bubbling (Delegación de Eventos) ---
        contenedorPadre.addEventListener('click', (e) => {
            if (e.target.classList.contains('btn-interactivo')) {
                 // El padre detecta el click, pero verifica que sea un botón hijo
                console.log(`[DELEGACIÓN] El padre detectó un click en el botón con ID: ${e.target.id}`);
            }
        });

    </script>

</body>
</html>
```

---

## ✅ Resumen de Puntos Clave

- Un **Evento** es una acción del usuario o del navegador a la que JS puede reaccionar.
    
- La mejor forma de conectar una función a un evento es con **`addEventListener('evento', funcion)`**.
    
- El manejador de eventos recibe un **Objeto Evento** (`e`), que contiene información sobre el suceso.
    
- **`e.preventDefault()`** es crucial para evitar el comportamiento predeterminado del navegador (ej: enviar formularios o seguir un enlace).
    
- Los eventos por defecto viajan de adentro hacia afuera (**Bubbling**).
    
- La **Delegación de Eventos** es una técnica de rendimiento donde el _listener_ se adjunta al elemento padre, y luego se usa `e.target` para identificar al hijo que generó el evento.
    

---

Ya sabes seleccionar elementos y hacer que reaccionen a los eventos. El siguiente paso es llevar la manipulación del DOM a un nivel superior: creando nuevos elementos, insertándolos y usando técnicas más avanzadas para generar interfaces dinámicas.

👉 Continuar con [[13 - Manipulación avanzada del DOM]]