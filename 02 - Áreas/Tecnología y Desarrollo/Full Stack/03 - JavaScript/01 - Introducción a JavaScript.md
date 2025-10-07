# 01 - Introducción a JavaScript

Aquí encontraras todo el indice del curso en el mapa de contexto: [[03 - JavaScript/00 - MOC]]

## ✍️ Introducción: ¿Qué es este lenguaje?

Si nunca has programado, piensa en una página web como una casa. El **HTML** son los cimientos, las paredes y la estructura (el contenido). El **CSS** es la pintura, la decoración y el estilo (la apariencia).

Pero, ¿quién abre la puerta cuando tocas el timbre? ¿Quién enciende la luz? ¿Quién mueve las cortinas? Ese es el trabajo de **JavaScript (JS)**.

**JavaScript es el lenguaje de programación que añade interactividad y comportamiento dinámico a una página web.** Es lo que hace que los botones funcionen, que las imágenes se deslicen y que la información se actualice sin recargar toda la página.

---

## 💻 Explicación Detallada: Historia, Rol y Aplicaciones

### 📜 Historia y Evolución

JavaScript fue creado en 1995 por **Brendan Eich** en Netscape Communications en solo 10 días. Inicialmente fue llamado _Mocha_, luego _LiveScript_, y finalmente _JavaScript_ (un nombre de _marketing_ para aprovechar la popularidad de Java, aunque técnicamente no están relacionados).

A pesar de su inicio apresurado, se estandarizó rápidamente como **ECMAScript (ES)**, que es el nombre oficial del estándar técnico. Cuando hablamos de **ES6**, **ES2015** o **ESNext**, nos referimos a las versiones modernas y nuevas características de JavaScript.

### 🌐 Relación con HTML y CSS (El Trío Web)

Para que una página web funcione, estos tres lenguajes (sí, HTML y CSS se consideran lenguajes, aunque no de programación) trabajan en conjunto:

|Lenguaje|Rol en una web|Metáfora de la casa|
|---|---|---|
|**HTML**|Estructura y Contenido|Los cimientos y paredes.|
|**CSS**|Apariencia y Estilo|La pintura y decoración.|
|**JavaScript**|Comportamiento e Interactividad|Los interruptores, puertas y mecanismos.|

Exportar a Hojas de cálculo

**Ejemplo de cómo se conectan:**

1. **HTML** te da un botón (`<button id="miBoton">Click Me</button>`).
    
2. **CSS** lo hace ver bonito (fondo azul, texto blanco).
    
3. **JavaScript** le dice: "Cuando te hagan _click_, cambia el color del fondo de la página".
    

### 🚀 Aplicaciones de JavaScript: Más allá del Navegador

Aunque nació para los navegadores web, JavaScript ha trascendido esa barrera, convirtiéndose en uno de los lenguajes más versátiles del mundo:

1. **Desarrollo Web (Frontend):** Sigue siendo su rol principal. Frameworks como **React**, **Angular** y **Vue** permiten construir interfaces de usuario complejas y modernas.
    
2. **Desarrollo de Servidores (Backend):** Con la invención de **Node.js**, JavaScript se puede ejecutar fuera del navegador, en un servidor. Esto permite a los desarrolladores usar el mismo lenguaje para toda la aplicación (Stack **MERN** o **MEAN**).
    
3. **Aplicaciones Móviles:** Herramientas como **React Native** o **NativeScript** permiten crear aplicaciones nativas para iOS y Android usando JavaScript.
    
4. **Internet de las Cosas (IoT) y Desktop:** Incluso se usa para controlar pequeños dispositivos o crear aplicaciones de escritorio (como **VSCode** o **Spotify**) con **Electron**.
    

---

## 💡 Ejemplo Práctico: Tu Primera Interacción JS

Para entender cómo se integra JS, vamos a añadir un código muy simple directamente en un archivo HTML.

Copia el siguiente código en un archivo llamado `index.html` y ábrelo en cualquier navegador (Chrome, Firefox, etc.).

HTML

```
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Clase 01 - Mi primer JS</title>
</head>
<body>

    <h1>¡Hola Mundo!</h1>

    <p id="parrafo_principal">Soy un texto inicial estático.</p>

    <button onclick="cambiarTexto()">Haz click para interactuar</button>

    <script>
        // Función: Es una receta de cocina que hace algo.
        // En este caso, la receta es "cambiarTexto".
        function cambiarTexto() {
            // 1. Buscamos el elemento P por su 'id'
            var elementoParrafo = document.getElementById("parrafo_principal");

            // 2. Cambiamos el contenido de texto dentro de ese elemento
            elementoParrafo.innerHTML = "¡Hola! **JavaScript** me ha cambiado. ¡Ahora soy dinámico!";
            
            // 3. Mostramos algo en la "consola" (un lugar invisible para el usuario, 
            //    pero útil para el desarrollador, que veremos en la siguiente clase).
            console.log("El usuario hizo click y el texto fue modificado.");
        }
    </script>

</body>
</html>
```

**Resultado:** Al hacer clic en el botón, el texto dentro del párrafo cambiará instantáneamente, sin necesidad de recargar la página. **Esa es la magia de JavaScript.**

---

## ✅ Resumen de Puntos Clave

- **JavaScript (JS)** es el lenguaje que da **interactividad** y **comportamiento dinámico** a las páginas web.
    
- Trabaja en equipo con **HTML** (estructura) y **CSS** (estilo).
    
- Su estándar oficial se llama **ECMAScript (ES)**.
    
- JS se usa en **Frontend** (navegadores), **Backend** (Node.js), desarrollo **Móvil** y más.
    
- El código JS se incluye en HTML usando la etiqueta `<script>`.
    
- El concepto clave en el ejemplo fue manipular el **DOM** (Document Object Model) para cambiar el contenido de la página.
    

---

¡Excelente trabajo en tu primera lección! Has sentado la base conceptual.

En la siguiente clase, configuraremos las herramientas que usaremos a diario para escribir, ejecutar y depurar nuestro código.

👉 Continuar con [[02 - Configuración del entorno]]