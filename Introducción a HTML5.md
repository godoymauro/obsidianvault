¡Por supuesto! Aquí tienes un desarrollo completo para tu nota [[Introducción a HTML5]], abarcando desde los conceptos más básicos hasta sus características más importantes y un ejemplo práctico.

---

# Introducción a HTML5

## ¿Qué es HTML5?

**HTML** son las siglas de *HyperText Markup Language* (Lenguaje de Marcado de Hipertexto). Es el lenguaje estándar utilizado para crear y estructurar el contenido de las páginas web. Piensa en él como el **esqueleto** de un sitio web: define los elementos básicos como títulos, párrafos, imágenes, enlaces y mucho más.

**HTML5** es la quinta y actual versión principal de este estándar. No es solo una actualización, sino una revisión completa que introduce nuevas y potentes características para hacer la web más rica, interactiva y semántica.

### La Trinidad del Desarrollo Web

Para entender el rol de HTML, es útil conocer a sus dos compañeros inseparables:

1.  **HTML (Estructura):** Define el contenido y su organización. Es el "qué".
2.  **CSS (Presentación):** Controla la apariencia visual (colores, fuentes, diseño). Es el "cómo se ve".
3.  **JavaScript (Comportamiento):** Añade interactividad y funcionalidad dinámica. Es el "qué hace".

---

## La Estructura Básica de un Documento HTML5

Todo archivo HTML5 sigue una estructura fundamental. Es una plantilla que le dice al navegador cómo interpretar el contenido.

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Título de la Página</title>
    <!-- Aquí van los metadatos, enlaces a CSS, etc. -->
</head>
<body>
    <!-- Aquí va todo el contenido visible de la página -->
    <h1>Este es un encabezado principal</h1>
    <p>Este es un párrafo de texto.</p>
</body>
</html>
```

Desglosemos las partes clave:

*   `<!DOCTYPE html>`: Es la primera línea y declara que el documento es de tipo HTML5. Es crucial para que los navegadores rendericen la página en modo estándar.
*   `<html lang="es">`: Es el elemento raíz que envuelve todo el contenido. El atributo `lang="es"` indica que el idioma principal de la página es el español, lo cual es importante para la accesibilidad y el SEO.
*   `<head>`: Contiene "metadatos" sobre la página que no son visibles directamente para el usuario, como el título, la codificación de caracteres, y enlaces a archivos CSS y JavaScript.
    *   `<meta charset="UTF-8">`: Asegura que el navegador interprete correctamente caracteres especiales, como acentos y la "ñ".
    *   `<meta name="viewport"...>`: Esencial para el diseño responsivo, asegura que la página se vea bien en dispositivos móviles.
    *   `<title>`: Define el título que aparece en la pestaña del navegador.
*   `<body>`: Contiene todo el contenido visible de la página: texto, imágenes, videos, enlaces, etc.

---

## Elementos Semánticos: Dando Significado a la Estructura

Una de las mayores mejoras de HTML5 es la introducción de **etiquetas semánticas**. Antes, los desarrolladores usaban `<div>` para todo, lo que hacía el código menos legible y accesible. Las nuevas etiquetas describen su contenido tanto para el navegador como para el desarrollador.

Principales etiquetas semánticas:

*   `<header>`: Define la cabecera de una página o de una sección. Suele contener el logo, el título principal y el menú de navegación.
*   `<nav>`: Se usa para agrupar los enlaces de navegación principales.
*   `<main>`: Representa el contenido principal y único de la página. Solo debe haber uno por página.
*   `<section>`: Agrupa contenido temáticamente relacionado, como un capítulo de un libro o una sección en una página de inicio.
*   `<article>`: Contiene una pieza de contenido independiente y autocontenida, como una entrada de blog, un artículo de noticias o un comentario en un foro.
*   `<aside>`: Para contenido relacionado tangencialmente con el contenido principal, como una barra lateral (sidebar) con enlaces relacionados o publicidad.
*   `<footer>`: Define el pie de página de un documento o sección. Suele contener información de copyright, enlaces a redes sociales o datos de contacto.
*   `<figure>` y `<figcaption>`: Se usan para agrupar contenido visual (como imágenes, diagramas o código) con su respectiva leyenda.

---

## Novedades Clave en HTML5

Además de la semántica, HTML5 introdujo un conjunto de tecnologías que revolucionaron lo que se puede hacer en un navegador.

### 1. Multimedia Nativa
Antes de HTML5, se necesitaban plugins como Flash para reproducir audio y video. Ahora es nativo.
*   `<video>`: Incrusta un reproductor de video.
    ```html
    <video src="mi-video.mp4" controls width="640" height="360">
        Tu navegador no soporta el elemento de video.
    </video>
    ```
*   `<audio>`: Incrusta un reproductor de audio.
    ```html
    <audio src="mi-cancion.mp3" controls>
        Tu navegador no soporta el elemento de audio.
    </audio>
    ```

### 2. Formularios Mejorados
HTML5 añadió nuevos tipos de `input` que mejoran la experiencia de usuario y la validación de datos, especialmente en móviles.
*   `type="email"`: Valida que el texto tenga formato de correo electrónico.
*   `type="date"`: Muestra un selector de fechas.
*   `type="number"`: Permite solo la entrada de números.
*   `type="tel"`: Optimizado para números de teléfono.
*   `type="url"`: Valida que el texto sea una URL.
*   Atributos como `required` (campo obligatorio) y `placeholder` (texto de ayuda).

### 3. Gráficos
*   `<canvas>`: Proporciona un "lienzo" para dibujar gráficos 2D y 3D dinámicamente usando JavaScript. Ideal para animaciones, juegos y visualización de datos.
*   `<svg>` (Scalable Vector Graphics): Aunque SVG existe desde antes, su integración en HTML5 es total. Permite definir gráficos vectoriales que escalan perfectamente sin perder calidad. Ideal para logos e iconos.

### 4. APIs de Navegador
HTML5 no es solo un lenguaje de marcado, es una plataforma de aplicaciones. Introdujo APIs (Interfaces de Programación de Aplicaciones) que dan acceso a funcionalidades del navegador y del sistema:
*   **API de Geolocalización:** Para obtener la ubicación del usuario.
*   **Web Storage (LocalStorage y SessionStorage):** Para almacenar datos en el navegador de forma persistente.
*   **Web Workers:** Para ejecutar scripts en segundo plano sin congelar la interfaz de usuario.
*   **Drag and Drop:** Para arrastrar y soltar elementos de forma nativa.

---

## Ejemplo Práctico de una Página Sencilla

Aquí tienes un ejemplo que une muchos de estos conceptos en una estructura de página de blog simple.

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mi Blog sobre HTML5</title>
</head>
<body>

    <header>
        <h1>Mi Blog de Tecnología</h1>
        <nav>
            <ul>
                <li><a href="#">Inicio</a></li>
                <li><a href="#">Artículos</a></li>
                <li><a href="#">Contacto</a></li>
            </ul>
        </nav>
    </header>

    <main>
        <article>
            <h2>Introducción a HTML5</h2>
            <p>Publicado el 3 de Octubre de 2025</p>
            <p>HTML5 ha transformado la web. En este artículo, exploramos sus características más importantes...</p>
            
            <section>
                <h3>Elementos Semánticos</h3>
                <p>Una de las mejoras clave son las etiquetas semánticas como header, footer, article y section.</p>
            </section>

            <section>
                <h3>Multimedia</h3>
                <figure>
                    <img src="html5-logo.png" alt="Logo oficial de HTML5" width="128">
                    <figcaption>El logo de HTML5 representa su poder y versatilidad.</figcaption>
                </figure>
            </section>
        </article>
    </main>

    <aside>
        <h3>Artículos Relacionados</h3>
        <ul>
            <li><a href="#">Guía de CSS Grid</a></li>
            <li><a href="#">Primeros pasos con JavaScript</a></li>
        </ul>
    </aside>

    <footer>
        <p>&copy; 2025 Mi Blog de Tecnología. Todos los derechos reservados.</p>
    </footer>

</body>
</html>
```

---

## Próximos Pasos

Una vez que te sientas cómodo con la estructura de HTML, el siguiente paso natural es aprender:
1.  **CSS:** Para dar estilo y diseño a tus páginas.
2.  **JavaScript:** Para añadir interactividad y crear aplicaciones web completas.