¡Adelante! Con los gráficos bajo control, volvemos a la multimedia para ver cómo optimizarla, hacerla accesible globalmente y prepararla para la distribución en línea.

---

Aquí tienes los apuntes de la clase anterior: [[13 - Canvas y SVG]]

## Clase 14 - Audio y video avanzado (controles, subtítulos, streaming) 🎬

### Introducción

En [[05 - Imágenes y multimedia]] aprendimos a incrustar `<audio>` y `<video>`. En esta clase, profundizaremos en atributos y elementos que te permiten controlar la reproducción, optimizar el rendimiento y, lo más importante, añadir **subtítulos, leyendas y descripciones**. El dominio de **`<track>`** y los conceptos de **precarga** son vitales para ofrecer una experiencia multimedia profesional, accesible y eficiente.

---

### 1. Optimización y Control de Rendimiento

El rendimiento es crucial cuando se manejan archivos grandes de audio o video. Debemos indicarle al navegador cómo debe preparar el medio.

#### a) Precarga (_Preload_)

El atributo **`preload`** en `<video>` o `<audio>` le indica al navegador cuántos datos debe descargar al cargar la página, antes de que el usuario haga clic en "Play".

|Valor `preload`|Función|Impacto en Rendimiento|
|---|---|---|
|**`none`**|No descarga nada del archivo.|**Máximo ahorro de ancho de banda.** Ideal para videos al final de la página.|
|**`metadata`**|Solo descarga los metadatos (duración, dimensiones, _códecs_).|**Equilibrado.** Permite mostrar la duración y el `poster` rápidamente.|
|**`auto`**|El navegador puede descargar todo el archivo multimedia.|**Mínimo ahorro.** Solo usar para videos muy pequeños o esenciales.|

```
<video controls preload="metadata" poster="miniatura.jpg">
    </video>
```

#### b) Códecs y Compatibilidad

Ya sabes que el elemento **`<source>`** con el atributo `type` nos permite ofrecer múltiples archivos. En un contexto avanzado, es útil especificar también el _códec_ dentro del `type` para que el navegador decida más rápido.

- **Ejemplo:** Especificando códecs VP9 (WebM) y H.264 (MP4).
    

```
<video controls>
    <source src="video.webm" type="video/webm; codecs=vp9"> 
    <source src="video.mp4" type="video/mp4; codecs=avc1.42E01E"> 
    <p>Tu navegador no soporta los formatos de video.</p>
</video>
```

---

### 2. Accesibilidad: El Poder del Elemento `<track>`

El elemento **`<track>`** es la herramienta de accesibilidad más importante en la multimedia de HTML5. Permite incrustar pistas de texto temporizadas (subtítulos, descripciones) a archivos de audio y video. Se coloca _dentro_ de las etiquetas `<video>` o `<audio>`.

#### Atributos Clave de `<track>`

|Atributo|Función|
|---|---|
|**`src`**|Ruta al archivo de texto (debe ser un archivo **WebVTT**).|
|**`kind`**|Define el tipo de pista de texto. **Crucial para la accesibilidad.**|
|**`srclang`**|Código de idioma de la pista (ej: `es`, `en`).|
|**`label`**|El nombre visible que el usuario seleccionará en el menú de controles.|
|**`default`**|Indica que esta pista debe ser seleccionada automáticamente al inicio.|
#### Tipos de Pistas (`kind`)

1. **`subtitles`**: **Traducción** del diálogo (para usuarios que hablan otro idioma).
    
2. **`captions`**: **Transcripción** de todo el audio (diálogo **más** sonidos como `[Música dramática]`). **Es el estándar para personas con problemas auditivos.**
    
3. **`descriptions`**: **Narración de audio** que describe las acciones visuales que ocurren en la pantalla (para personas ciegas o con baja visión).
    
4. **`chapters`**: Metadatos para crear puntos de navegación rápida (_chapters_).
    

```
<video controls>
    <source src="clase.mp4" type="video/mp4">
    
    <track kind="captions" src="es-leyendas.vtt" srclang="es" label="Español (Leyendas)" default>
    
    <track kind="descriptions" src="es-descripciones.vtt" srclang="es" label="Descripción de Audio">
</video>
```

---

### 3. Técnicas de Streaming Avanzado

Aunque la infraestructura de _streaming_ (servidores y distribución) es externa a HTML, la etiqueta `<video>` soporta los principios clave para la web moderna.

#### a) Reproducción Adaptativa

Los servicios modernos usan formatos como **HLS** o **DASH** que ofrecen múltiples calidades de video. El navegador, junto con JavaScript y las **Media Source Extensions (MSE)**, puede cambiar dinámicamente el _stream_ (la fuente) basándose en la velocidad de conexión del usuario. `<video>` es el contenedor que hace posible este cambio dinámico.

#### b) Comportamiento en Dispositivos Móviles

- **`playsinline`**: Atributo booleano esencial, especialmente para navegadores móviles (como Safari en iOS). Si está presente, indica que el video debe reproducirse **dentro del área de la página** en lugar de ir automáticamente a pantalla completa.
    
- **`disablepictureinpicture`**: Atributo que evita que el usuario active la función _Picture-in-Picture_ (una ventana flotante) del navegador.
    

```
<video controls playsinline> 
    </video>
```

---

### Práctica recomendada

**Objetivo:** Configurar un video con precarga optimizada y un set de pistas accesibles.

1. Crea un archivo llamado `video_profesional.html`.
    
2. Implementa el siguiente código. Nota que aunque no tenemos los archivos `.vtt`, la sintaxis de `<track>` debe ser correcta para que el navegador los reconozca.
    

```
<body>
    <h1>Reproductor de Video Profesional HTML5</h1>
    
    <video controls width="640" preload="metadata" playsinline poster="miniatura.jpg">
        <source src="trailer.mp4" type="video/mp4">
        
        <track 
            kind="captions" 
            src="trailer-es.vtt" 
            srclang="es" 
            label="Español (Leyendas)" 
            default>
            
        <track 
            kind="subtitles" 
            src="trailer-en.vtt" 
            srclang="en" 
            label="English (Subtitles)">
            
        <p>Tu navegador no soporta el elemento de video.</p>
    </video>
    
    <p>La selección de subtítulos aparecerá en la barra de control del reproductor.</p>
</body>
```

3. Visualiza el archivo. Aunque no se vean los subtítulos, deberías poder ver el ícono de "CC" (Closed Captioning) en la barra de controles del video, que te permite seleccionar entre las dos pistas definidas por las etiquetas `<track>`.
    

---

### Cierre

Has aprendido a profesionalizar la multimedia en HTML5. Recuerda que la **precarga** optimiza la carga inicial, y el uso correcto de **`<track>`** (especialmente con `kind="captions"`) transforma tu contenido en un recurso accesible a nivel mundial.

---

## Resumen de puntos claves

|Elemento/Atributo|Función|Importancia|
|---|---|---|
|**`preload="metadata"`**|Indica al navegador cargar solo metadatos.|**Optimización.** Equilibra rendimiento y usabilidad.|
|**`<source>`**|Ofrece archivos con diferentes códecs.|**Compatibilidad** garantizada entre navegadores.|
|**`<track>`**|Añade pistas de texto temporizadas (VTT).|**Accesibilidad y Globalización.** Crucial para subtítulos/descripciones.|
|**`kind="captions"`**|Transcripción de diálogo + sonidos.|El estándar de accesibilidad para personas sordas.|
|**`playsinline`**|Permite reproducción dentro del _viewport_ en móviles.|**Usabilidad** en dispositivos táctiles.|

**👉 Hemos hecho nuestro contenido accesible. Ahora, asegurémonos de que las máquinas (motores de búsqueda) lo entiendan perfectamente para el SEO. Vamos a la siguiente clase:** [[15 - Microdatos y SEO en HTML5]]