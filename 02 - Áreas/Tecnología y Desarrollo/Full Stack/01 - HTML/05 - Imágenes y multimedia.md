¡Estupendo! Hemos estructurado el texto y dominado la navegación. Ahora, vamos a la parte que realmente da vida y color a la web: la **multimedia**. HTML5 transformó por completo cómo manejamos imágenes, audio y video, haciéndolos nativos y sin necesidad de _plugins_.

---

Aquí tienes los apuntes de la clase anterior: [[04 - Enlaces y navegación]]

## Clase 05 - Imágenes y multimedia 🖼️

### Introducción

Esta clase te enseñará a integrar contenido no textual de manera **semántica** y **accesible**. El elemento **`<img>`** es quizás la etiqueta más usada después de las de texto. Pero HTML5 va más allá, introduciendo `<audio>` y `<video>` para incrustar medios sin depender de Flash, y dándonos el poder de los gráficos programables con `<canvas>`. Entender el atributo **`alt`** y la gestión de códecs es fundamental para ser un desarrollador moderno.

---

### 1. Imágenes: El elemento `<img>`

El elemento `<img>` es una etiqueta **vacía** (**void element**), lo que significa que no tiene contenido de cierre (`</img>`). Su función es crear un espacio en el documento e insertar una imagen **referenciada** a través de sus atributos.

#### Atributos Esenciales

Dos atributos son obligatorios para que una imagen sea considerada válida y accesible:

1. **`src` (Source):** Define la **ruta** (absoluta o relativa) donde se encuentra el archivo de imagen. Es el corazón de la etiqueta.
    
2. **`alt` (Alternative Text):** Proporciona una **descripción textual** de la imagen.
    

|Atributo|Explicación|Importancia|
|---|---|---|
|**`src`**|Ruta del archivo (ej: `../img/logo.png`).|**Obligatorio.** Sin él, no hay imagen.|
|**`alt`**|**Texto alternativo** de la imagen.|**Obligatorio.** Vital para **accesibilidad** (lectores de pantalla) y **SEO** (motores de búsqueda).|

```
<img src="assets/perfil.jpg" alt="Fotografía de perfil de Juan Pérez sonriendo" width="200" height="200">
```

> **Nota de Accesibilidad:** Si una imagen es **puramente decorativa** (ej. un borde simple), debes usar `alt=""` (texto alternativo vacío) para indicar a los lectores de pantalla que la ignoren. **Nunca** omitas el atributo `alt` por completo.

#### Atributos de Dimensión: `width` y `height`

- Definen el ancho y alto de la imagen en **píxeles** sin usar la unidad (`width="300"`).
    
- **Mejor Práctica:** Aunque el tamaño final debe gestionarse con CSS, es una buena práctica incluirlos. Esto ayuda al navegador a **reservar el espacio** antes de que la imagen cargue completamente, evitando el "salto" de contenido (_Layout Shift_).
    

---

### 2. Multimedia Nativ a en HTML5

Antes de HTML5, para mostrar un video o reproducir audio, se dependía de _plugins_ como Flash. Hoy, `<audio>` y `<video>` son nativos del navegador.

#### a) Video: El elemento `<video>`

El elemento `<video>` incrusta un reproductor de video directamente en la página.

|Atributo|Función|
|---|---|
|**`controls`**|Muestra los controles estándar del reproductor (pausa, volumen, etc.). **Imprescindible**.|
|**`autoplay`**|Inicia la reproducción automáticamente (a menudo bloqueado por navegadores).|
|**`loop`**|Reproduce el video en bucle.|
|**`muted`**|Silencia el video por defecto (requerido para `autoplay` en muchos navegadores).|
|**`poster`**|URL de una imagen que se muestra antes de que el video se reproduzca (una miniatura).|

```
<video width="640" height="360" controls poster="miniatura.jpg">
    <source src="video.mp4" type="video/mp4">
    <source src="video.webm" type="video/webm">
    <p>Tu navegador no soporta el elemento de video.</p>
</video>
```

#### b) Audio: El elemento `<audio>`

Funciona de manera idéntica a `<video>`, pero solo para archivos de audio.

```
<audio controls>
    <source src="musica.mp3" type="audio/mp3">
    <source src="musica.ogg" type="audio/ogg">
    <p>Tu navegador no soporta el elemento de audio.</p>
</audio>
```

> **Nota sobre `<source>` y Códecs:** No todos los navegadores soportan los mismos formatos de audio/video (códecs). Usar múltiples etiquetas `<source>` (como MP4 y WebM para video, o MP3 y Ogg para audio) permite que el navegador elija el primer formato que puede reproducir, garantizando máxima compatibilidad.

---

### 3. Gráficos y Animación: `<canvas>` y SVG

HTML5 también introdujo herramientas para trabajar con gráficos dinámicos:

#### a) `<canvas>`

- **Uso:** Crea una superficie rectangular donde se pueden dibujar gráficos, animaciones o juegos usando **JavaScript**.
    
- **Naturaleza:** Es solo un contenedor. Si no usas JS para dibujar en él, estará en blanco.
    
- **Accesibilidad:** Debe incluir contenido de respaldo para navegadores antiguos o lectores de pantalla.
    
```
<canvas id="miLienzo" width="300" height="150">
    Texto de respaldo: Aquí debería haber un gráfico interactivo.
</canvas>
```

#### b) SVG (Scalable Vector Graphics)

- **Naturaleza:** Aunque no es una etiqueta puramente HTML, el estándar HTML5 permite **incrustar** código SVG directamente. SVG es un formato de imagen **vectorial** basado en XML.
    
- **Ventaja:** Las imágenes vectoriales (como logos o iconos) no pierden calidad al ampliarse y suelen tener un tamaño de archivo pequeño.
    
```
<svg width="100" height="100">
    <circle cx="50" cy="50" r="40" stroke="green" stroke-width="4" fill="yellow" />
</svg>
```

---

### Práctica recomendada

**Objetivo:** Integrar una imagen con su atributo `alt` correcto y un video con controles.

1. Crea un nuevo archivo llamado `multimedia.html`.
    
2. Asegúrate de tener una imagen (ej. `paisaje.jpg`) y, si puedes, un video corto (ej. `trailer.mp4`) en tu carpeta de proyecto.
    
3. Implementa el siguiente código dentro del `<body>`:
    
```
<body>
    <h1>Galería HTML5</h1>

    <h2>Una imagen esencial</h2>
    <figure>
        <img src="paisaje.jpg" 
             alt="Un hermoso atardecer con colores naranjas y morados sobre una cordillera nevada." 
             width="600" 
             height="400">
        <figcaption>Paisaje montañoso al atardecer (Figura 1).</figcaption>
    </figure>

    <h2>Video Nativo HTML5</h2>
    <video controls poster="miniatura-video.jpg" width="400">
        <source src="trailer.mp4" type="video/mp4">
        <source src="trailer.webm" type="video/webm">
        <p>Lo sentimos, tu navegador no es compatible con los códecs de video.</p>
    </video>
    
    <h2>Un simple Canvas</h2>
    <canvas id="ejemploCanvas" width="200" height="100" style="border:1px solid #000000;">
        Contenido de respaldo para navegadores que no soportan Canvas.
    </canvas>
    
    <p><em>Nota: El Canvas requiere JavaScript para dibujar el contenido.</em></p>
</body>
```

4. Visualiza el archivo y comprueba que los controles del video funcionen. Si mueves el cursor sobre el video, deberías ver los botones.
    
---

### Cierre

Has aprendido a incrustar imágenes con la obligatoria accesibilidad del atributo `alt`, y a usar los potentes elementos `<audio>` y `<video>` para la multimedia nativa. Además, tienes una introducción a `<canvas>` y SVG para gráficos. Recuerda: **la semántica y la compatibilidad** son tus prioridades en multimedia (por eso usamos el `alt` y los múltiples `<source>`).

---

## Resumen de puntos claves

| Elemento                | Función                                                          | Atributos Clave                                             |
| ----------------------- | ---------------------------------------------------------------- | ----------------------------------------------------------- |
| **`<img>`**             | Incrusta una imagen.                                             | **`src`** (ruta) y **`alt`** (texto alternativo - vital).   |
| **`<video>`/`<audio>`** | Incrusta reproductores multimedia nativos.                       | **`controls`**, **`autoplay`**, **`loop`**, **`muted`**.    |
| **`<source>`**          | Define múltiples rutas de archivo dentro de `<video>`/`<audio>`. | Garantiza la **compatibilidad** entre navegadores y códecs. |
| **`<canvas>`**          | Superficie de dibujo para gráficos y animaciones.                | Requiere **JavaScript** para funcionar.                     |

**👉 Hemos cubierto el contenido principal. Ahora organizaremos ese contenido. Vamos a la siguiente clase:** [[06 - Listas y tablas]]