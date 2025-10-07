¡Absolutamente! Has sentado las bases de la estructura y la funcionalidad. Ahora, vamos a la clase que te distinguirá como un desarrollador de calidad: **la accesibilidad**. Un sitio web moderno debe ser usable por _todas_ las personas.

---

Aquí tienes los apuntes de la clase anterior: [[09 - Atributos globales y buenas prácticas]]

## Clase 10 - Accesibilidad en HTML5 (WAI-ARIA, atributos alt, roles) 🧑‍🦯

### Introducción

La accesibilidad web (A11Y, por el acrónimo _Accessibility_ con 11 letras intermedias) no es opcional, es una **obligación ética y legal**, y un factor de **calidad** del código. Se trata de diseñar y codificar sitios web que puedan ser percibidos, entendidos y operados por personas con discapacidades (visuales, auditivas, motoras o cognitivas), a menudo utilizando **tecnologías de asistencia** como lectores de pantalla. HTML5 y el estándar **WAI-ARIA** (Web Accessibility Initiative - Accessible Rich Internet Applications) son nuestras herramientas clave.

---

### 1. El Pilar de la Accesibilidad: El HTML Semántico

La mejor manera de empezar con la accesibilidad es aplicando todo lo que ya aprendiste sobre semántica [[08 - Elementos semánticos]].

#### a) Regla 1: Usa la Etiqueta Correcta

- **Evita el "Divitis"**: Si usas un `<div>` y le pones un manejador de clics para que actúe como un botón, estás obligando al usuario de teclado y al lector de pantalla a adivinar.
    
- **La Solución**: Usa el elemento **`<button>`**. El navegador sabe que es un botón, lo hace enfocable con el teclado, y el lector de pantalla anuncia "botón". Esta es la **Regla de oro de ARIA**: Usa HTML nativo si existe una etiqueta para lo que quieres hacer.
    

|Tarea|¡Mal!|¡Bien!|
|---|---|---|
|**Hacer un botón**|`<div onclick="...">Haz clic</div>`|`<button onclick="...">Haz clic</button>`|
|**Contenido principal**|`<div id="main-content">...</div>`|`<main>...</main>`|
|**Menú de enlaces**|`<div class="menu">...</div>`|`<nav><ul>...</ul></nav>`|
#### b) Regla 2: El Atributo `alt` (Revisión)

Como vimos en [[05 - Imágenes y multimedia]], el atributo **`alt`** de la etiqueta `<img>` es esencial.

- **`alt` descriptivo:** Para imágenes funcionales o de contenido, describe lo que la imagen _transmite_ o _hace_. (Ej: `alt="Logotipo de Expero en HTML5"`)
    
- **`alt` vacío:** Para imágenes puramente decorativas que no añaden información. (Ej: `alt=""`)
    
- **NUNCA** lo omitas.
    
---

### 2. WAI-ARIA: El Kit de Herramientas para la Interfaz Rica

WAI-ARIA es un conjunto de atributos que se añaden a los elementos HTML para definir mejor su **rol**, sus **propiedades** y su **estado**, especialmente en componentes de interfaz que no son nativos de HTML (ej: _sliders_, pestañas, árboles de navegación).

ARIA se compone de tres categorías principales:

#### a) Roles ARIA

El atributo **`role`** define qué es un elemento. Se usa principalmente cuando no puedes usar el elemento HTML nativo apropiado.

- **Ejemplo:** Si tienes que construir un _tab bar_ con `<div>` por motivos de _framework_ (aunque no se recomienda), puedes decirle al navegador que actúe como un menú.
    

```
<div role="button" tabindex="0">Cerrar</div>

<ul role="menubar"> 
    </ul>
```

#### b) Estados y Propiedades ARIA (`aria-*`)

Estos atributos añaden información dinámica o descriptiva sobre el elemento.

|Atributo|Propósito|Ejemplo de Uso|
|---|---|---|
|**`aria-label`**|Proporciona una etiqueta de texto invisible cuando no hay texto visible (ej. un botón de icono).|`<button aria-label="Cerrar ventana">X</button>`|
|**`aria-labelledby`**|Usa el `id` de otro elemento para etiquetar el elemento actual.|`<h2 id="titulo">Carrito</h2> <div role="region" aria-labelledby="titulo">...</div>`|
|**`aria-hidden`**|Oculta un elemento **visualmente** del lector de pantalla.|`<i class="icono" aria-hidden="true"></i>` (Para iconos decorativos)|
|**`aria-expanded`**|Indica si un elemento controlable (ej. un menú desplegable) está abierto o cerrado.|`<button aria-expanded="false">Menú</button>`|
|**`aria-current`**|Indica el elemento actual en una colección (ej. página actual en una paginación).|`<a href="#" aria-current="page">3</a>`|

---

### 3. La Navegación por Teclado: `tabindex`

Las personas que no pueden usar un ratón dependen del teclado para navegar. HTML soporta esto con la tecla `Tab`.

- **Flujo Normal:** Los elementos interactivos nativos (`<a>`, `<button>`, `<input>`) son automáticamente enfocables en el orden en que aparecen en el código (el **orden de tabulación**).
    
- **`tabindex`**: Este atributo controla si y dónde un elemento puede ser enfocado.
    

|Valor de `tabindex`|Función|Uso|
|---|---|---|
|**`tabindex="0"`**|Permite que elementos no interactivos sean enfocados en el orden normal del documento.|Para hacer un `<div>` o `<span>` enfocable.|
|**`tabindex="-1"`**|Permite enfocar un elemento **solo con JavaScript**. No aparece en el orden de tabulación normal.|Útil para mover el foco a un mensaje de error o una ventana modal.|
|**`tabindex=">0"`**|Establece un orden de tabulación **específico** (ej. `1, 2, 3...`).|**EVITAR**. Rompe el flujo natural del código y confunde al usuario. ¡No uses valores positivos!|

---

### 4. Buenas Prácticas de Accesibilidad Clave

1. **Contraste de Color:** Asegúrate de que el texto y el fondo tengan un contraste de color suficiente (esto es técnicamente CSS, pero vital para el HTML).
    
2. **Etiquetas Asociadas:** Siempre usa `<label>` con `for` y `id` para asociar etiquetas a formularios, incluso si las etiquetas están ocultas visualmente. [[07 - Formularios en HTML5]]
    
3. **Encabezados Jerárquicos:** Mantén la jerarquía correcta de `<h1>` a `<h6>`. Los lectores de pantalla usan estos encabezados para moverse rápidamente por el contenido.
    
4. **Enlaces Descriptivos:** Evita enlaces con el texto "Haz clic aquí" o "Más información". En su lugar, usa `Leer más sobre HTML5 semántico` o `Descargar informe financiero`.
    

---

### Práctica recomendada

**Objetivo:** Añadir accesibilidad ARIA y manejo de foco a un componente no nativo.

1. Crea un nuevo archivo llamado `accesibilidad.html`.
    
2. Implementa la siguiente alerta de error que no usa el elemento nativo `alert` del navegador (para simular un componente de interfaz rico):
    

```
<body>
    <h1>Prueba de Accesibilidad ARIA</h1>

    <div id="miAlerta" role="alert" tabindex="-1" style="border: 2px solid red; padding: 10px; margin-bottom: 20px;">
        <strong>Error de Conexión:</strong> No se pudo cargar el archivo de configuración.
    </div>

    <button onclick="document.getElementById('miAlerta').focus()" aria-label="Mostrar mensaje de error de conexión" title="Mostrar error">
        [ ⚠️ ] </button>
    
    <p>Este texto tiene contenido importante.</p>
    <span aria-hidden="true"> --- ESTRELLA DECORATIVA --- </span>
    <p>Este texto también es relevante.</p>
    
</body>
```

3. Visualiza el archivo. Aunque no puedes probar con un lector de pantalla, puedes **Inspeccionar** el código para verificar la presencia y la sintaxis correcta de los atributos ARIA. Observa que el `span` decorativo será ignorado por los _screen readers_.
    

---

### Cierre

Has aprendido que la accesibilidad se basa en la **semántica primero**, y luego en el uso estratégico de **WAI-ARIA** para rellenar los vacíos de información, especialmente para componentes interactivos complejos. Siempre prioriza la etiqueta HTML nativa. Un desarrollador que codifica con accesibilidad en mente es un desarrollador de élite.

---

## Resumen de puntos claves

| Concepto                 | Función                                                        | Uso Crucial                                                             |
| ------------------------ | -------------------------------------------------------------- | ----------------------------------------------------------------------- |
| **A11Y**                 | Accesibilidad web.                                             | Obligación ética y de calidad. Empieza por la semántica.                |
| **WAI-ARIA**             | Atributos para definir roles, estados y propiedades.           | Útil para componentes de interfaz **no nativos** (widgets).             |
| **`role`**               | Define qué es un elemento.                                     | Ej: `role="button"`, `role="alert"`.                                    |
| **`aria-label`**         | Etiqueta un elemento sin texto visible.                        | Imprescindible para botones de icono o imágenes interactivas sin texto. |
| **`aria-hidden="true"`** | Oculta un elemento al lector de pantalla.                      | Para contenido puramente decorativo.                                    |
| **`tabindex="0"`**       | Hace que un elemento no interactivo sea enfocable por teclado. | Crucial para la navegación sin ratón.                                   |
| **`alt`**                | Texto alternativo de la imagen.                                | Debe describir la función o contenido de la imagen (nunca omitir).      |

**👉 Hemos garantizado la inclusividad. Ahora, aseguraremos que nuestro código sea impecable y fácil de mantener. Vamos a la siguiente clase (Clase 11):** [[11 - Buenas prácticas de organización y validación de código]]