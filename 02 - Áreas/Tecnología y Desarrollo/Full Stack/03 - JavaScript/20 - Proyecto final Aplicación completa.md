Has recorrido un camino impresionante, dominando la sintaxis, las estructuras de datos, la asincronía y las buenas prácticas. Ahora, es el momento de poner toda esa teoría en acción y construir algo tangible.

---

Aquí tienes los apuntes de la clase anterior: [[19 - Testing en JavaScript]]

# 20 - Proyecto final: Aplicación completa

## ✍️ Introducción: Construyendo una Lista de Tareas (To-Do List) 🚀

El objetivo de este proyecto es integrar todos los conocimientos de los niveles 1, 2 y 3:

- **Fundamentos (Nivel 1):** Variables, estructuras de control (`if/else`, `for`).
    
- **Funciones y Datos (Nivel 2):** Arrays, Objetos, JSON, `localStorage`.
    
- **DOM y Asincronía (Nivel 3):** Selección del DOM, Eventos (`click`), manipulación dinámica (`createElement`).
    

Crearemos una aplicación simple, pero funcional: una **Lista de Tareas** que permite añadir, marcar como completadas y eliminar tareas, y que **persiste** los datos en el navegador.

---

## 💻 Estructura del Proyecto

El proyecto requiere un único archivo HTML que contendrá tanto la estructura (HTML), los estilos (CSS simple) y el código JavaScript.

### 1. HTML Base

Creamos los contenedores principales y el formulario de entrada.

HTML

```
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Lista de Tareas JS - Proyecto Final</title>
    <style>
        /* Estilos CSS básicos para el proyecto */
        body { font-family: sans-serif; max-width: 600px; margin: 0 auto; padding: 20px; }
        .tarea-item { padding: 10px; border-bottom: 1px solid #eee; display: flex; justify-content: space-between; align-items: center; }
        .tarea-completada .descripcion { text-decoration: line-through; color: #888; }
        .btn-eliminar { background: red; color: white; border: none; padding: 5px 10px; cursor: pointer; border-radius: 3px; }
        #form-agregar { display: flex; margin-bottom: 20px; }
        #tarea-input { flex-grow: 1; padding: 10px; }
    </style>
</head>
<body>

    <h1>Mi Lista de Tareas</h1>
    
    <form id="form-agregar">
        <input type="text" id="tarea-input" placeholder="Escribe una nueva tarea..." required>
        <button type="submit">Agregar</button>
    </form>
    
    <ul id="lista-tareas">
        </ul>

    <script>
        // ¡El código JavaScript va aquí!
    </script>

</body>
</html>
```

---

## 💻 Lógica JavaScript (Paso a Paso)

El código JS debe ir dentro de la etiqueta `<script>` del HTML.

### Paso 1: Variables Globales y Carga Inicial

Necesitamos una lista (Array) para guardar las tareas y las referencias del DOM. Usaremos `localStorage` para que la lista persista (ver [[10 - JSON y almacenamiento]]).

JavaScript

```
const TAREAS_STORAGE_KEY = 'listaTareas';
const formAgregar = document.getElementById('form-agregar');
const tareaInput = document.getElementById('tarea-input');
const listaTareasDOM = document.getElementById('lista-tareas');

// 1. Inicializar la lista de tareas desde localStorage o un array vacío
// Usamos JSON.parse para convertir el String de vuelta a un Array/Objeto JS
// Si localStorage.getItem devuelve null, usamos [] por defecto
let tareas = JSON.parse(localStorage.getItem(TAREAS_STORAGE_KEY)) || [];

/**
 * Guarda el array 'tareas' en localStorage.
 * DRY y Responsabilidad Única (ver Clase 17).
 */
function guardarTareas() {
    // Usamos JSON.stringify para convertir el Array de Objetos a un String JSON
    localStorage.setItem(TAREAS_STORAGE_KEY, JSON.stringify(tareas));
}

// Llama a la función que dibuja el DOM al inicio
document.addEventListener('DOMContentLoaded', () => {
    renderizarTareas(); 
});
```

---

### Paso 2: La Función Clave: `renderizarTareas()`

Esta función es la responsable de **reflejar** el estado del Array `tareas` en el HTML (DOM). Se llama cada vez que el Array cambia (se añade, se elimina o se marca una tarea).

JavaScript

```
/**
 * Limpia el UL y lo reconstruye a partir del array 'tareas'.
 */
function renderizarTareas() {
    // Limpiamos el contenido anterior del <ul> (el padre)
    listaTareasDOM.innerHTML = ''; 
    
    // Recorremos el array (usamos forEach para iteración simple)
    tareas.forEach(tarea => {
        // Creamos el elemento de lista (<li>)
        const li = document.createElement('li');
        li.classList.add('tarea-item');
        
        // Si la tarea está completada, añadimos la clase CSS para tacharla
        if (tarea.completada) {
            li.classList.add('tarea-completada');
        }
        
        // Usamos el id de la tarea para el manejo de eventos (data-id)
        li.setAttribute('data-id', tarea.id);

        // Contenido del <li> (usamos innerHTML para simplicidad en la estructura interna)
        li.innerHTML = `
            <span class="descripcion">${tarea.texto}</span>
            <div>
                <input type="checkbox" ${tarea.completada ? 'checked' : ''} class="check-completada">
                <button class="btn-eliminar">Eliminar</button>
            </div>
        `;
        
        // Usamos appendChild para añadir el nuevo <li> al <ul>
        listaTareasDOM.appendChild(li); 
    });
}
```

---

### Paso 3: Manejo de Eventos (Formulario y Clics en la Lista)

#### A) Añadir Nueva Tarea (Evento `submit`)

JavaScript

```
formAgregar.addEventListener('submit', (e) => {
    // 1. Prevenimos el comportamiento por defecto (recargar la página)
    e.preventDefault(); 
    
    const textoTarea = tareaInput.value.trim(); // Limpiamos espacios en blanco
    
    if (textoTarea !== "") {
        // 2. Creamos un nuevo objeto (Clase 09) para la tarea
        const nuevaTarea = {
            id: Date.now(), // ID único basado en el tiempo (Clase 09)
            texto: textoTarea,
            completada: false
        };
        
        // 3. Modificamos el array de datos (Mutación)
        tareas.push(nuevaTarea);
        
        // 4. Actualizamos el almacenamiento y el DOM
        guardarTareas();
        renderizarTareas();
        
        // 5. Limpiamos el input para la siguiente tarea
        tareaInput.value = '';
    }
});
```

#### B) Eliminar o Completar (Delegación de Eventos)

Usamos la **Delegación de Eventos** (ver [[12 - Eventos en JavaScript]]) en el padre (`listaTareasDOM`) para manejar los clics en los botones y _checkboxes_ de los hijos.

JavaScript

```
listaTareasDOM.addEventListener('click', (e) => {
    const elementoClick = e.target;
    // Buscamos el <li> padre para obtener su 'data-id'
    const liPadre = elementoClick.closest('li'); 
    
    if (!liPadre) return; // Salir si el clic no fue en un <li>

    const tareaId = parseInt(liPadre.getAttribute('data-id'));
    
    // ------------------------------------
    // Lógica de Eliminación
    // ------------------------------------
    if (elementoClick.classList.contains('btn-eliminar')) {
        // Usamos el método filter para crear un nuevo array SIN la tarea eliminada
        // (Buena práctica de inmutabilidad - Clase 08)
        tareas = tareas.filter(t => t.id !== tareaId); 
        
        guardarTareas();
        renderizarTareas();
    } 
    
    // ------------------------------------
    // Lógica de Completado
    // ------------------------------------
    else if (elementoClick.classList.contains('check-completada')) {
        // Usamos map para crear un nuevo array con el estado actualizado
        tareas = tareas.map(t => {
            if (t.id === tareaId) {
                // Invertimos el estado de completada
                return { ...t, completada: !t.completada }; 
                // Usamos Spread Operator (Clase 18) para copiar y actualizar
            }
            return t;
        });
        
        guardarTareas();
        renderizarTareas();
    }
});
```

---

## ✅ Resumen del Proyecto y Logros

Has implementado las siguientes técnicas clave:

- **Persistencia de datos:** Uso de `localStorage` y conversión JSON (`JSON.stringify`/`JSON.parse`).
    
- **Manipulación del DOM:** Selección de elementos, limpieza de `innerHTML` y creación dinámica con `createElement` (dentro de `renderizarTareas`).
    
- **Manejo de Eventos:** Escucha del evento `submit` y uso avanzado de la **Delegación de Eventos** en el contenedor padre.
    
- **Estructuras de Datos:** Almacenamiento de tareas como un **Array de Objetos** con propiedades (`id`, `texto`, `completada`).
    
- **Funciones de Array modernas:** Uso de `filter` y `map` para manipular el estado del Array de forma limpia e inmutable (idealmente).
    
- **Buenas Prácticas:** Funciones cortas con responsabilidad única (`guardarTareas`, `renderizarTareas`).
    

¡Felicidades! Has completado el curso y construido una aplicación web moderna y funcional. Esto demuestra que dominas JavaScript desde los fundamentos hasta la interacción con el DOM y la gestión de datos.

Te recomiendo revisar las siguientes notas clave para consolidar los puntos más importantes de este proyecto:

- 👉 [[10 - JSON y almacenamiento]] (localStorage)
    
- 👉 [[12 - Eventos en JavaScript]] (Delegación de Eventos)
    
- 👉 [[08 - Arrays en profundidad]] (map y filter)
    
- 👉 [[18 - ES6+ y nuevas características]] (Spread Operator)
    

¿Qué te pareció la construcción del proyecto? ¿Te gustaría explorar cómo podríamos añadir la función de **Asincronía** a este proyecto, por ejemplo, guardando las tareas en una API de prueba en lugar de `localStorage`?

Ir a los apuntes de Frontend Frameworks: [[01 - Introducción a Frontend Frameworks]]