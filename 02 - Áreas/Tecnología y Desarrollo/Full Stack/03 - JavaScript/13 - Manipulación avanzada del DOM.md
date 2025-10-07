Hemos llegado al punto donde JavaScript realmente construye y destruye la interfaz. En esta clase, vamos a dejar de solo modificar lo que ya existe y aprenderemos a crear contenido dinámicamente.

---

Aquí tienes los apuntes de la clase anterior: [[12 - Eventos en JavaScript]]

# 13 - Manipulación avanzada del DOM

## ✍️ Introducción: Construcción Dinámica de Interfaces

En la [[11 - El DOM (Document Object Model)]] aprendimos a seleccionar y modificar. Sin embargo, en aplicaciones web modernas, raramente el HTML viene completo. Es más común que JavaScript reciba datos (usando [[15 - Fetch API y AJAX]]) y **construya elementos HTML completos** a partir de esos datos.

Aquí cubriremos los métodos clave para crear, insertar y eliminar nodos del árbol DOM.

---

## 💻 Explicación Detallada: Crear, Insertar y Eliminar Nodos

### 1. Creación de Nodos

Los métodos de creación se encuentran en el objeto `document`.

|Método|Propósito|Resultado|
|---|---|---|
|**`document.createElement(tagName)`**|Crea un nuevo **Nodo Elemento** (una etiqueta HTML).|`<div>`, `<span>`, `<li>`|
|**`document.createTextNode(texto)`**|Crea un nuevo **Nodo Texto** (el contenido de una etiqueta).|El texto en sí.|
|**`document.createDocumentFragment()`**|Crea un fragmento ligero para contener temporalmente nodos.|Usado para inserciones eficientes.|

Exportar a Hojas de cálculo

JavaScript

```
// 1. Crear el elemento <li>
const nuevoElementoLista = document.createElement('li');

// 2. Crear el nodo de texto
const textoLista = document.createTextNode('Nuevo ítem creado por JS');

// 3. Juntar el texto dentro del elemento
nuevoElementoLista.appendChild(textoLista); 
```

### 2. Inserción de Nodos (El Árbol Genealógico)

Una vez creado, el nuevo nodo es solo un fantasma en memoria. Debes insertarlo en el DOM real para que sea visible. Esto se hace estableciendo relaciones de padre/hijo.

|Método de Inserción|Propósito|Sintaxis|
|---|---|---|
|**`appendChild()`**|Inserta un nodo como el **último hijo** del padre.|`padre.appendChild(hijo)`|
|**`prepend()`**|Inserta un nodo como el **primer hijo** del padre (ES6+).|`padre.prepend(hijo)`|
|**`insertBefore()`**|Inserta un nodo **antes** de un hijo de referencia.|`padre.insertBefore(nuevoHijo, hijoDeReferencia)`|
|**`insertAdjacentElement()`**|Inserción más flexible en relación a un elemento existente.|`elemento.insertAdjacentElement('afterend', nuevoNodo)`|

Exportar a Hojas de cálculo

**Posiciones de `insertAdjacentElement()`:**

|Posición|Descripción|
|---|---|
|`'beforebegin'`|Antes del elemento mismo.|
|`'afterbegin'`|Justo dentro, antes de su primer hijo.|
|`'beforeend'`|Justo dentro, después de su último hijo (igual a `appendChild`).|
|`'afterend'`|Después del elemento mismo.|

Exportar a Hojas de cálculo

### 3. Eliminar y Reemplazar Nodos

|Método|Propósito|
|---|---|
|**`removeChild(hijo)`**|Elimina un nodo hijo específico del padre.|
|**`remove()`**|Elimina el nodo directamente (método más simple, moderno).|
|**`replaceChild(nuevo, viejo)`**|Reemplaza un nodo hijo existente por uno nuevo.|

Exportar a Hojas de cálculo

JavaScript

```
// Si tengo el nodo hijo, puedo simplemente...
hijoAEliminar.remove(); // ¡Sencillo!

// Si solo tengo el padre...
padre.removeChild(hijoAEliminar);
```

### 4. El Gran Debate: `innerHTML` vs. `createElement`

Cuando tienes que generar HTML complejo (ej: una lista de 20 productos), hay dos caminos:

#### **Opción A: Inyección con `innerHTML` (El camino rápido)**

Creas una gran cadena de texto que contiene todo el HTML y la inyectas en un contenedor.

JavaScript

```
// Datos de ejemplo
const productos = [{ id: 1, nombre: "A"}, { id: 2, nombre: "B"}];

let htmlGenerado = '';
productos.forEach(p => {
    htmlGenerado += `<div class="card" id="prod-${p.id}"><h2>${p.nombre}</h2></div>`;
});

contenedor.innerHTML = htmlGenerado; 
```

**Ventajas:** Es rápido de escribir. **Desventajas:** Puede ser lento si la cadena es muy grande, y tiene riesgos de seguridad (inyección XSS) si el texto contiene contenido que viene del usuario.

#### **Opción B: Creación de Nodos con `createElement` (El camino seguro)**

Creas cada elemento, le añades atributos, y luego lo insertas.

JavaScript

```
productos.forEach(p => {
    const div = document.createElement('div');
    div.classList.add('card');
    div.id = `prod-${p.id}`;
    
    const h2 = document.createElement('h2');
    h2.textContent = p.nombre;
    
    div.appendChild(h2);
    contenedor.appendChild(div); // Insertamos el nodo creado
});
```

**Ventajas:** Es más seguro (el texto nunca se interpreta como HTML), y el rendimiento es mejor para aplicaciones muy complejas. **Recomendado para producción.**

### 5. Optimización: `DocumentFragment`

Si vas a insertar 100 elementos de lista usando `createElement`, hacer 100 llamadas a `contenedor.appendChild()` es ineficiente, ya que el navegador tiene que **redibujar (reflow)** la página 100 veces.

El `DocumentFragment` permite agrupar todos los nodos en memoria primero y luego insertar el fragmento completo en el DOM con una **única** operación.

JavaScript

```
const fragmento = document.createDocumentFragment();

// Creamos los 100 ítems...
for (let i = 0; i < 100; i++) {
    const item = document.createElement('li');
    item.textContent = `Ítem #${i}`;
    fragmento.appendChild(item); // Los agregamos al fragmento
}

// SOLO una operación en el DOM real
listaPadre.appendChild(fragmento); 
// El fragmento se "desempaqueta" y solo se redibuja una vez. ¡Máximo rendimiento!
```

---

## 🛠️ Ejemplos Prácticos: Creación Dinámica

Copia este código en un archivo `index.html` y ábrelo en el navegador.

HTML

```
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Clase 13 - DOM Avanzado</title>
</head>
<body>

    <ul id="lista-tareas">
        <li class="tarea-item" id="tarea-1">Comprar leche <button class="eliminar">x</button></li>
        <li class="tarea-item" id="tarea-2">Estudiar JS <button class="eliminar">x</button></li>
    </ul>

    <div id="contenedor-mensaje"></div>
    <button id="btn-crear">Crear 5 Tareas (Fragment)</button>

    <script>
        const listaTareas = document.getElementById('lista-tareas');
        const contenedorMensaje = document.getElementById('contenedor-mensaje');
        const btnCrear = document.getElementById('btn-crear');

        // --- 1. Eliminar Elementos (usando Delegación de Eventos) ---
        listaTareas.addEventListener('click', (e) => {
            // Verificamos si el elemento clickeado es un botón de eliminar
            if (e.target.classList.contains('eliminar')) {
                // El padre del botón es el <li>. ¡Lo eliminamos!
                e.target.parentElement.remove();
            }
        });

        // --- 2. Crear e Insertar Elementos Dinámicamente ---
        btnCrear.addEventListener('click', () => {
            
            // Usamos DocumentFragment para la optimización
            const fragmentoTareas = document.createDocumentFragment();
            
            for (let i = 3; i < 8; i++) {
                // 1. Crear el <li>
                const item = document.createElement('li');
                item.classList.add('tarea-item');
                item.textContent = `Tarea creada #${i} `; // Nota el espacio

                // 2. Crear el botón de eliminar
                const btnEliminar = document.createElement('button');
                btnEliminar.classList.add('eliminar');
                btnEliminar.textContent = 'x';

                // 3. Ensamblar: Botón dentro del <li>
                item.appendChild(btnEliminar);

                // 4. Agregar el <li> al fragmento (no al DOM real)
                fragmentoTareas.appendChild(item);
            }
            
            // 5. UNA SOLA INSERCIÓN al DOM real (eficiente)
            listaTareas.appendChild(fragmentoTareas);

            // Mensaje de éxito usando insertAdjacentElement
            contenedorMensaje.insertAdjacentElement('afterbegin', crearAlerta("¡Tareas añadidas con éxito!"));
        });
        
        // Función auxiliar para crear un nodo
        function crearAlerta(texto) {
            const p = document.createElement('p');
            p.textContent = texto;
            p.style.color = 'green';
            return p;
        }

    </script>

</body>
</html>
```

---

## ✅ Resumen de Puntos Clave

- La **manipulación avanzada del DOM** se centra en la creación y gestión dinámica de nodos.
    
- **`document.createElement()`** y **`document.createTextNode()`** son los métodos base para la creación.
    
- **`appendChild()`** (al final) y **`prepend()`** (al principio) son los métodos más comunes para insertar un nodo como hijo.
    
- **`elemento.remove()`** es la forma moderna de eliminar un nodo.
    
- **`innerHTML`** es rápido para generar HTML, pero menos seguro; **`createElement`** es más seguro y recomendable.
    
- **`document.createDocumentFragment()`** es una técnica de optimización crucial para agrupar múltiples nodos y hacer una **única inserción** al DOM, minimizando el redibujado.
    
- **`insertAdjacentElement()`** ofrece un control flexible sobre la posición de inserción (`beforebegin`, `afterend`, etc.).
    

---

Has dominado el DOM, la parte visual. Pero en una web moderna, no puedes hacer que todo se detenga mientras esperas que la información cargue de un servidor. Por eso, el siguiente tema es esencial: la **Asincronía**.

👉 Continuar con [[14 - Asincronía en JavaScript]]