¡Entramos al Nivel 3! 🚀 Ahora pasamos de la teoría pura a la **interacción práctica**. En este nivel, aprenderás a usar JavaScript para interactuar con la interfaz del usuario: la página web. La clave para esto es el **DOM**.

---

Aquí tienes los apuntes de la clase anterior: [[10 - JSON y almacenamiento]]

# 11 - El DOM (Document Object Model)

## ✍️ Introducción: La Maqueta de la Web

Piensa en una página web como una casa construida con **HTML** (la estructura). El **DOM** (Document Object Model) es la **representación mental, jerárquica y programable** de esa casa que JavaScript utiliza.

El DOM es una interfaz de programación que trata a un documento HTML como un árbol de objetos. Cada etiqueta HTML (como `<body>`, `<h1>`, `<p>`) se convierte en un **nodo u objeto** con el que JavaScript puede interactuar:

- **Seleccionar:** Encontrar un nodo específico.
    
- **Inspeccionar:** Ver sus propiedades (contenido, atributos).
    
- **Modificar:** Cambiar su contenido, estilo o estructura.
    

---

## 💻 Explicación Detallada: La Estructura de Árbol

### 1. Nodos y Relaciones

El DOM se organiza como un **árbol genealógico**:

- **`document`:** Es el nodo raíz, la puerta de entrada a todo el DOM.
    
- **Nodos Padre:** Contienen otros nodos.
    
- **Nodos Hijo:** Nodos contenidos dentro de otro.
    
- **Nodos Hermanos:** Nodos al mismo nivel que comparten el mismo padre.
    

|Elemento HTML|Tipo de Objeto en el DOM|
|---|---|
|`<body>`|Nodo Elemento|
|`El texto dentro de una etiqueta`|Nodo Texto|
|`class="activo"`|Nodo Atributo|

Exportar a Hojas de cálculo

### 2. Selección de Elementos (Encontrar el Objetivo)

Antes de modificar algo, debemos encontrarlo. El objeto global `document` nos da métodos poderosos para la selección:

|Método de Selección|Descripción|Tipo de Retorno|
|---|---|---|
|**`document.getElementById()`**|Busca el elemento con un `id` específico. **El más rápido.**|Un único Elemento|
|**`document.querySelector()`**|Busca el **primer** elemento que coincida con un selector CSS (clase, ID, etiqueta, etc.).|Un único Elemento|
|**`document.querySelectorAll()`**|Busca **todos** los elementos que coincidan con un selector CSS.|Una **NodeList** (similar a un Array)|
|**`document.getElementsByClassName()`**|Busca todos los elementos con una clase específica.|Una **HTMLCollection**|

Exportar a Hojas de cálculo

JavaScript

```
// Ejemplo de selectores (Usaremos 'const' porque el elemento de referencia no cambiará)
const titulo = document.getElementById('tituloPrincipal'); // Selecciona por ID
const primerParrafo = document.querySelector('.parrafo:first-child'); // Selector CSS
const botones = document.querySelectorAll('.boton-accion'); // NodeList de todos los elementos con esa clase
```

### 3. Manipulación de Contenido (Cambiando la Información)

Una vez seleccionado un nodo, podemos alterar lo que el usuario ve:

|Propiedad|Descripción|Uso|
|---|---|---|
|**`.textContent`**|Solo manipula el **texto** dentro del elemento (ignora HTML). **Más seguro.**|`elemento.textContent = 'Nuevo Texto.';`|
|**`.innerHTML`**|Manipula el **contenido HTML** dentro del elemento (puede inyectar etiquetas, **cuidado** con la seguridad).|`elemento.innerHTML = '<b>Texto en negrita</b>';`|

Exportar a Hojas de cálculo

### 4. Manipulación de Atributos y Clases

Podemos cambiar los atributos estándar de HTML (como `src` en una imagen o `href` en un enlace) y gestionar las clases CSS:

JavaScript

```
const imagen = document.querySelector('img'); 
const boton = document.querySelector('#btn-tema');

// Cambiar un atributo (ej: la fuente de la imagen)
imagen.setAttribute('src', 'nueva-imagen.jpg');

// Manipular Clases CSS (¡Muy común!)
boton.classList.add('activo');        // Añade la clase 'activo'
boton.classList.remove('inactivo');   // Elimina la clase 'inactivo'
const estaActivo = boton.classList.contains('activo'); // Pregunta si tiene la clase (true/false)
boton.classList.toggle('modo-oscuro'); // Si tiene la clase, la quita; si no la tiene, la pone.
```

### 5. Manipulación de Estilos Directos

Aunque se recomienda usar CSS para el estilo, a veces se requiere cambiar una propiedad individual directamente en JS:

JavaScript

```
const caja = document.getElementById('miCaja');

// Se usa notación camelCase para las propiedades CSS (ej: background-color -> backgroundColor)
caja.style.backgroundColor = 'blue';
caja.style.fontSize = '20px';
```

---

## 🛠️ Ejemplo Práctico: Manipulando el DOM

Copia este código en un archivo `index.html` y ábrelo en tu navegador.

HTML

```
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Clase 11 - DOM</title>
    <style>
        .resaltado {
            background-color: yellow;
            border: 1px solid orange;
        }
    </style>
</head>
<body>

    <h1 id="tituloPrincipal">Título Inicial</h1>
    
    <div class="tarjeta" id="tarjeta1">
        <p class="contenido">Este es el párrafo #1.</p>
    </div>

    <div class="tarjeta" id="tarjeta2">
        <p class="contenido">Este es el párrafo #2.</p>
    </div>

    <button id="btn-cambiar">¡Click para manipular el DOM!</button>

    <script>
        // 1. SELECTORES
        const titulo = document.getElementById('tituloPrincipal');
        const tarjeta1 = document.getElementById('tarjeta1');
        const botonCambiar = document.querySelector('#btn-cambiar');
        const parrafos = document.querySelectorAll('.contenido'); // NodeList de 2 elementos

        // 2. FUNCIÓN DE MANIPULACIÓN
        function manipularDOM() {
            // A. Cambiar contenido de texto
            titulo.textContent = "El DOM ha sido Modificado por JS";

            // B. Cambiar estilos y atributos
            tarjeta1.style.backgroundColor = '#e0f7fa'; // Fondo azul claro
            tarjeta1.setAttribute('data-estado', 'activo'); // Añadimos un atributo de datos

            // C. Iterar sobre la NodeList y cambiar clases
            parrafos.forEach((parrafo, index) => {
                parrafo.textContent = `Contenido del Párrafo ${index + 1} modificado.`;
                parrafo.classList.add('resaltado'); // Le añadimos la clase CSS 'resaltado'
            });

            // D. Cambiar el texto del botón
            botonCambiar.textContent = "¡DOM Manipulado!";
        }

        // 3. Ejecución de la función (por ahora, solo la llamamos directamente)
        manipularDOM();
        
        // NOTA: En la próxima clase, haremos que la función se ejecute al HACER CLICK en el botón.
    </script>

</body>
</html>
```

---

## ✅ Resumen de Puntos Clave

- El **DOM (Document Object Model)** es la representación jerárquica (en forma de árbol) de la estructura HTML que JS utiliza.
    
- Cada etiqueta HTML es un **nodo** con propiedades.
    
- Los métodos principales de selección son: **`getElementById()`**, **`querySelector()`** (el primer elemento que coincide), y **`querySelectorAll()`** (una lista de elementos).
    
- Se usa **`.textContent`** para manipular solo texto (más seguro) e **`.innerHTML`** para inyectar o leer código HTML.
    
- La propiedad **`.classList`** permite añadir (`add`), eliminar (`remove`) o alternar (`toggle`) clases CSS fácilmente.
    
- La propiedad **`.style`** permite cambiar estilos CSS directamente usando la notación `camelCase`.
    

---

Acabas de ver cómo JavaScript puede cambiar la apariencia y el contenido de tu web. El siguiente paso crucial es hacer que esto suceda **cuando el usuario interactúa** (hace clic, escribe, etc.).

👉 Continuar con [[12 - Eventos en JavaScript]]