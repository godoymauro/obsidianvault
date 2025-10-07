¡Excelente! Ya sabes manejar el texto, los enlaces, la multimedia y los formularios. Es momento de pasar de construir "piezas de contenido" a construir la **arquitectura** completa de una página web moderna.

---

Aquí tienes los apuntes de la clase anterior: [[07 - Formularios en HTML5]]

## Clase 08 - Elementos semánticos (header, nav, main, section, article, aside, footer) 🏗️

### Introducción

Esta es una de las clases más importantes, ya que define el estándar de **HTML5 moderno**. Los elementos semánticos son la mayor mejora sobre versiones anteriores, que dependían del genérico `<div>`. Un elemento **semántico** es aquel cuyo _nombre_ le dice al navegador y al desarrollador cuál es su _propósito_ (ej. `<header>` es para la cabecera, `<nav>` es para la navegación). Usar la etiqueta correcta es crucial para la **accesibilidad**, el **SEO** y el mantenimiento del código.

---

### 1. La Estructura Principal del Documento

Estos elementos se usan para dividir el `<body>` en las áreas funcionales principales de un sitio web.

#### a) `<header>` (Cabecera)

- **Propósito:** Representa el contenido introductorio o un grupo de ayudas de navegación, generalmente ubicado en la parte superior de la página.
    
- **Contenido Típico:** Logotipos, títulos principales (`<h1>`), lemas, y a menudo, el elemento `<nav>`.
    
- **Uso Múltiple:** Puedes tener **varios** `<header>` en una página (uno para el sitio completo y uno para cada `<article>` o `<section>`).
    

```
<body>
    <header>
        <h1>Nombre de Mi Sitio Web</h1>
        <nav aria-label="Navegación principal">...</nav>
    </header>
    </body>
```

#### b) `<nav>` (Navegación)

- **Propósito:** Contiene enlaces de navegación **principales** o bloques de enlaces esenciales para el sitio.
    
- **Uso Semántico:** Solo debe usarse para grupos **significativos** de enlaces (ej. menú principal, tabla de contenidos, migas de pan).
    
- **Mejor Práctica:** Si tienes múltiples elementos `<nav>`, usa el atributo `aria-label` (ver [[10 - Accesibilidad en HTML5]]) para diferenciarlos y mejorar la accesibilidad.
    

#### c) `<main>` (Contenido Principal)

- **Propósito:** Contiene el contenido **único** y central del documento. El contenido principal es la razón de ser de la página.
    
- **Restricción:** **Solo puede haber un elemento `<main>`** por documento.
    
- **Nota:** No debe contener contenido que se repita en varias páginas (como _header_, _footer_ o _sidebar_ de navegación secundaria).
    

#### d) `<footer>` (Pie de Página)

- **Propósito:** Contiene información de cierre o metadatos de su sección o documento contenedor.
    
- **Contenido Típico:** Información de derechos de autor, enlaces a términos y privacidad, información de contacto.
    
- **Uso Múltiple:** Al igual que `<header>`, puedes tener varios `<footer>` (uno para el sitio y uno para cada `<article>` o `<section>`).
    

---

### 2. Estructurando el Contenido de la Página

Estos elementos organizan el contenido dentro del `<main>` y definen el flujo de la narrativa o los datos.

#### a) `<section>` (Sección Genérica)

- **Propósito:** Agrupa contenido temáticamente relacionado. Representa una sección genérica de un documento, a menudo con un encabezado (`<h2>` a `<h6>`).
    
- **Regla de Oro:** Una `<section>` **casi siempre** debe tener un encabezado que describa su contenido. Si la vas a usar solo para aplicar CSS, es mejor usar un `<div>`.
    

```
<main>
    <section>
        <h2>Filosofía de Desarrollo</h2>
        <p>Nuestro enfoque se centra en la semántica y la accesibilidad.</p>
    </section>
    
    <section>
        <h2>Nuestras Herramientas</h2>
        <p>Utilizamos VS Code y la validación en vivo.</p>
    </section>
</main>
```

#### b) `<article>` (Contenido Independiente)

- **Propósito:** Contiene contenido que es independiente y potencialmente **redistribuible** por sí mismo.
    
- **Uso Típico:** Artículos de blog, publicaciones de foro, comentarios de usuarios, _widgets_ individuales.
    
- **Autonomía:** Debe tener sentido por sí mismo sin el resto del contenido de la página. Por lo general, debe contener un elemento de encabezado (e.g., `<h1>`, `<h2>`).
    

```
<article>
    <h3>Título del Post del Blog</h3>
    <p>Publicado por Juanita Pérez el <time datetime="2025-05-10">10 de Mayo</time></p>
    <p>Este es el cuerpo del artículo...</p>
    <footer>
        <p>Etiquetas: HTML5, Semántica</p>
    </footer>
</article>
```

#### c) `<aside>` (Contenido Relacionado, pero Separado)

- **Propósito:** Contenido que está relacionado tangencialmente con el contenido principal, pero podría ser considerado aparte.
    
- **Uso Típico:** Barras laterales (_sidebars_), bloques de citas al margen, publicidad, grupos de enlaces relacionados.
    
- **Ubicación Semántica:** Puede ir dentro del `<main>` si es relevante a ese contenido, o fuera si es una barra lateral general del sitio.
    

```
<main>
    <article>
        </article>
    
    <aside>
        <h4>Artículos Relacionados</h4>
        <ul>
            <li><a href="#">Lo más nuevo de CSS3</a></li>
            <li><a href="#">Introducción a JavaScript</a></li>
        </ul>
    </aside>
</main>
```

---

### 3. La Decisión Crucial: `<div>` vs. Elementos Semánticos

El elemento **`<div>`** sigue existiendo, pero ahora tiene un propósito muy específico:

- **`<div>`:** Debe usarse **solo** cuando no existe un elemento semántico adecuado para la tarea, o cuando se necesita un contenedor genérico para propósitos de _estilo_ o _scripting_ (principalmente para flexbox o grid con CSS).
    
- **Regla de Oro Semántica:** Si puedes nombrar la sección con una etiqueta de HTML5 (e.g., "Esto es la navegación", "Esto es un artículo"), ¡úsala! **No uses** `<div class="header">`. Usa `<header>`.
    

### Estructura de un sitio web semántico (Esquema)

```
<!DOCTYPE html>
<html lang="es">
<head>
    </head>
<body>
    <header>
        <nav></nav>
    </header>
    
    <main>
        <section>
            <h2>Sobre la Semántica</h2>
            <article>
                <h3>Post principal</h3>
                </article>
            
            <aside>
                </aside>
        </section>
        </main>
    
    <footer>
        </footer>
</body>
</html>
```

---

### Práctica recomendada

**Objetivo:** Reestructurar una página antigua basada en `divs` a una moderna y semántica.

1. Crea un archivo llamado `semantica.html`.
    
2. A continuación, tienes un código de la "vieja web". Transfórmalo en el código semántico:
    

```
<div id="cabeza">
    <div class="logo">Logo</div>
    <div class="menu">Menu</div>
</div>
<div class="contenido-principal">
    <div class="articulo-blog">Post</div>
    <div class="sidebar">Ads</div>
</div>
<div id="pie-de-pagina">Copyright</div>

```

**Solución Esperada (Escribe este código en tu archivo):**

```
<body>
    <header>
        <h1>Mi Blog Semántico</h1>
        <nav>
            </nav>
    </header>
    
    <main>
        <section>
            <h2>Últimas Publicaciones</h2>
            <article>
                <h3>Título de la Publicación</h3>
                <p>Cuerpo del post...</p>
            </article>
        </section>
        
        <aside>
            <h4>Publicidad</h4>
        </aside>
    </main>
    
    <footer>
        <p>&copy; 2025 Mi Sitio Semántico</p>
    </footer>
</body>
```

3. Visualiza el archivo. Aunque visualmente se vea igual, el **significado** para los navegadores, motores de búsqueda y lectores de pantalla es ahora perfectamente claro.
    

---

### Cierre

Has dado el salto más importante hacia el desarrollo web profesional. La **semántica** es tu nueva regla de oro. Recuerda: `<h1>` dentro de `<header>`, el contenido vital dentro de `<main>`, y usar `<article>` vs. `<section>` según la **independencia** del contenido.

---

## Resumen de puntos claves

|Elemento|Significado|Regla de Uso|
|---|---|---|
|**`<header>`**|Contenido introductorio o de ayuda de navegación.|Puede haber varios, pero el principal suele contener el `<h1>` y `<nav>`.|
|**`<nav>`**|Grupo de enlaces de navegación principal.|Solo para enlaces **esenciales** del sitio/sección.|
|**`<main>`**|Contenido único y principal del documento.|**Solo se permite uno** por documento.|
|**`<footer>`**|Información de cierre (copyright, contacto).|Se usa al final del documento o de un `article`/`section`.|
|**`<section>`**|Agrupación temática.|Debe tener un encabezado que lo identifique.|
|**`<article>`**|Contenido independiente (blog post, comentario, etc.).|Debe tener sentido por sí mismo.|
|**`<aside>`**|Contenido tangencialmente relacionado (sidebar).|Contenido secundario, pero relevante al contexto.|
|**`<div>`**|Contenedor genérico.|Usar solo si **no** hay una etiqueta semántica más apropiada.|

**👉 Hemos estructurado el sitio. El siguiente paso es pulir cada elemento con atributos globales y buenas prácticas. Vamos a la siguiente clase:** [[09 - Atributos globales y buenas prácticas]]