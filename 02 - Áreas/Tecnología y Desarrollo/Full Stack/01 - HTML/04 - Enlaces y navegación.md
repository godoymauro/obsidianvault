## Clase 04 - Enlaces y navegación 🔗

Aquí tienes los apuntes de la clase anterior: [[03 - Etiquetas de texto y formato]]

### Introducción

El elemento **`<a>`** (del inglés _anchor_) es el corazón de internet. Su función es convertir cualquier contenido (texto, imagen, incluso un elemento de bloque entero) en un **hipervínculo** (o _link_). En esta clase, dominaremos cómo crear enlaces internos (dentro de nuestro sitio), externos (a otros sitios) y cómo controlar su comportamiento con atributos clave como `target` y `rel`.

---

### 1. El Elemento Ancla: `<a>` y el Atributo `href`

El elemento `<a>` necesita un atributo fundamental para funcionar: **`href`** (Hypertext Reference). Este atributo es donde especificamos la **URL de destino** del enlace.

#### Sintaxis Básica

```
<a href="url_de_destino">Texto del Enlace (Contenido)</a>
```

- **`<a>`**: La etiqueta de apertura y cierre.
    
- **`href`**: El atributo que contiene la dirección.
    
- **Texto del Enlace**: El contenido visible que el usuario hará clic. Debe ser **descriptivo** para mejorar la accesibilidad y el SEO.
    

#### Ejemplo de Enlace Externo

```
<p>Para buscar información adicional, visita el sitio oficial del <a href="https://www.w3.org/html/">W3C (Consorcio World Wide Web)</a>.</p>
```

---

### 2. Tipos de Rutas (`href`)

Saber cómo definir la ruta es crucial para que tus enlaces no se rompan. Hay dos categorías principales:

#### a) Rutas Absolutas

Una **ruta absoluta** es una dirección web completa y funcional por sí misma. Se utiliza para enlazar a **sitios externos**.

- **Formato:** Siempre incluye el protocolo (`http://` o `https://`).
    
- **Ejemplo:** `https://www.google.com/`
    

#### b) Rutas Relativas

Una **ruta relativa** es una dirección que se especifica **respecto a la ubicación del archivo HTML actual**. Se utiliza para enlazar a archivos **dentro de nuestro propio sitio web**.

- **Enlazar a un archivo en la misma carpeta:**
    
    ```
    <a href="contacto.html">Contáctanos</a>
    ```
    
- **Enlazar a un archivo en una subcarpeta (ej: `secciones/`):**
    
    ```
    <a href="secciones/servicios.html">Nuestros Servicios</a>
    ```
    
- **Enlazar a un archivo que está un nivel _arriba_ (usando `../`):**
    
    ```
    <a href="../index.html">Ir al Inicio</a> 
    ```
    

> **Nota:** Usar rutas relativas para tu propio sitio es una **mejor práctica**, ya que facilita mover el sitio completo de un servidor a otro sin tener que cambiar cada URL.

---

### 3. Atributos de Control de Navegación

HTML5 nos da control sobre cómo el navegador debe abrir el destino del enlace.

#### a) `target="_blank"`

Este atributo le dice al navegador que debe abrir el enlace en una **nueva pestaña o ventana**.

- **Uso:** Generalmente se utiliza para **enlaces externos**, para que el usuario no abandone tu sitio.
    

```
<a href="https://www.wikipedia.org" target="_blank">Abrir Wikipedia en una nueva pestaña</a>
```

> **¡Advertencia de Seguridad y Rendimiento!** Cuando usas `target="_blank"`, debes **siempre** incluir el atributo `rel="noopener noreferrer"`.
> 
> - **`noopener`**: Evita que la nueva pestaña tenga control sobre la pestaña original (un riesgo de seguridad).
>     
> - **`noreferrer`**: Evita pasar información de referencia (URL de tu página) a la página de destino.
>     

#### b) `rel` (Relación)

El atributo `rel` define la **relación** entre la página actual y la página enlazada. Es muy importante para el SEO y la seguridad.

|Valor `rel`|Significado|Uso Principal|
|---|---|---|
|**`noopener`**|Seguridad (usado con `_blank`).|Previene vulnerabilidades de _phishing_.|
|**`noreferrer`**|Seguridad/Privacidad (usado con `_blank`).|Oculta la página de origen a la de destino.|
|**`nofollow`**|Indica a los motores de búsqueda **que no sigan** ese enlace.|Para enlaces de publicidad, comentarios de usuarios no confiables, o contenido pagado.|
|**`sponsored`**|Indica un enlace patrocinado o publicitario.|Reemplaza parte del uso de `nofollow` en el contexto SEO moderno.|

---

### 4. Enlaces Internos (Anclas o Fragmentos)

Podemos enlazar a una **sección específica** dentro de la _misma_ página o de _otra_ página. Esto se llama enlace de **fragmento** o **ancla**.

1. **Marcar el Destino:** Usa el atributo **`id`** en el elemento de destino. Los `id` deben ser **únicos** en todo el documento.
    
    ```
    <h2 id="seccion-contacto">Información de Contacto</h2>
    ```
    
2. **Crear el Enlace:** Usa el símbolo de almohadilla (`#`) seguido del valor del `id` en el `href`.
    
    - **Enlace dentro de la misma página:**
        
        ```
        <a href="#seccion-contacto">Ir a Contacto</a> 
        ```
        
    - **Enlace a una sección de OTRA página:**
        
        ```
        <a href="servicios.html#precios">Ver Precios de Servicios</a>
        ```
        

---

### 5. Enlaces de Funcionalidad

El atributo `href` también puede iniciar acciones en el cliente:

- **Correo Electrónico (`mailto`):** Abre el programa de correo predeterminado del usuario.
    
    ```
    <a href="mailto:tunombre@ejemplo.com?subject=Consulta%20sobre%20HTML5">Enviar Correo</a>
    ```
    
    (Nota: `%20` es la codificación URL para el espacio).
    
- **Teléfono (`tel`):** En dispositivos móviles, inicia una llamada.
    
    ```
    <a href="tel:+34123456789">Llamar Ahora</a>
    ```
    

---

### Práctica recomendada

**Objetivo:** Construir una barra de navegación interna y un enlace externo seguro.

1. Abre un nuevo archivo en tu entorno llamado `navegacion.html` y usa la estructura básica.
    
2. Dentro del `<body>`, crea una estructura con enlaces a secciones y un enlace externo:
    

```
<body>
    <h1>Página de Pruebas de Navegación</h1>

    <nav>
        <p>Índice rápido: 
            <a href="#inicio">Introducción</a> | 
            <a href="#detalles">Detalles Técnicos</a> | 
            <a href="#externo">Recursos Externos</a>
        </p>
    </nav>
    <hr>

    <h2 id="inicio">Introducción</h2>
    <p>Aquí va el texto de inicio. Es importante entender que la navegación es la base del hipertexto.</p>
    
    <br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>

    <h2 id="detalles">Detalles Técnicos</h2>
    <p>Estamos usando rutas relativas para el contenido interno. Esto mantiene nuestro sitio portable.</p>
    
    <h2 id="externo">Recursos Externos</h2>
    <p>Revisa la <a href="https://developer.mozilla.org" target="_blank" rel="noopener noreferrer">Documentación de MDN</a> para más referencias.</p>
</body>
```

3. Abre `navegacion.html` en tu navegador. Haz clic en los enlaces del índice y observa cómo la página se desplaza instantáneamente a la sección con el `id` correspondiente.
    

---

### Cierre

El elemento `<a>` es simple en su forma, pero poderoso en su función. Has aprendido a diferenciar entre rutas absolutas y relativas, a controlar la apertura de pestañas con `target="_blank"`, y a asegurar tus enlaces con `rel="noopener noreferrer"`. La próxima clase nos llevará al mundo de lo visual y sonoro.

---

## Resumen de puntos claves

| Elemento/Atributo     | Función                                                 | Uso Crucial                                                     |
| --------------------- | ------------------------------------------------------- | --------------------------------------------------------------- |
| **`<a>`**             | Define un hipervínculo.                                 | El elemento fundamental de la navegación web.                   |
| **`href`**            | Especifica la URL de destino.                           | Define la **ruta** (absoluta o relativa).                       |
| **Rutas Relativas**   | Direcciones basadas en la ubicación del archivo actual. | **Mejor práctica** para enlaces internos del sitio.             |
| **Rutas Absolutas**   | Direcciones completas, incluyendo `http/https`.         | Uso exclusivo para enlaces a **sitios externos**.               |
| **`target="_blank"`** | Abre el enlace en una nueva pestaña.                    | **Siempre** debe ir acompañado de `rel="noopener noreferrer"`.  |
| **`id`**              | Atributo global para marcar un destino.                 | Se usa con el `#` en el `href` para crear enlaces de fragmento. |
|                       |                                                         |                                                                 |

**👉 Es hora de traer la vida a nuestra web. Vamos a la siguiente clase:** [[05 - Imágenes y multimedia]]