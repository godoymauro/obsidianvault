¡Claro! Hemos estructurado el esqueleto de nuestra página con la semántica de HTML5. Ahora, es el momento de dotar a esos elementos de funcionalidad y control universal.

---

Aquí tienes los apuntes de la clase anterior: [[08 - Elementos semánticos]]

## Clase 09 - Atributos globales y buenas prácticas 🌟

### Introducción

Los **atributos globales** son superpoderes: son un conjunto de atributos que **pueden aplicarse a _cualquier_ elemento HTML** (sí, incluso al `<body>`, al `<h1>`, o al `<a>`). Su objetivo es añadir funcionalidad universal, desde dar una identidad única a un elemento (`id`) hasta permitir que el usuario edite el contenido directamente en el navegador (`contenteditable`). Dominar estos atributos es crucial para el desarrollo moderno, la estilización con CSS y la manipulación con JavaScript.

---

### 1. Los Tres Grandes Atributos Globales

Estos atributos son la base de la interacción entre HTML, CSS y JavaScript.

#### a) `id` (Identificador Único)

- **Propósito:** Define un **identificador único** para un elemento dentro de todo el documento.
    
- **Regla Crucial:** El valor del `id` **DEBE ser único** en toda la página.
    
- **Usos:**
    
    1. Como **ancla de destino** para enlaces de fragmento (visto en [[04 - Enlaces y navegación]]).
        
    2. Como selector rápido en **JavaScript** (`document.getElementById()`).
        
    3. Como selector de **CSS** (aunque las clases son preferidas para la estilización).
        
```
<section id="carrito-compras">
    <h2>Tu Cesta</h2>
    </section>
```

#### b) `class` (Clase de Estilo/Comportamiento)

- **Propósito:** Define uno o más **nombres de clase** que se usan para seleccionar elementos en CSS o JavaScript.
    
- **Flexibilidad:** El mismo nombre de clase **puede repetirse** en múltiples elementos. Un solo elemento puede tener **múltiples clases** separadas por espacios.
    
- **Uso Primario:** El método estándar para aplicar estilos (CSS) y agrupar elementos por comportamiento (JavaScript).
    

```
<h2 class="titulo-seccion">Nuestros Productos</h2>

<button class="btn btn-principal grande">Comprar Ahora</button>
```

#### c) `style` (Estilos en Línea)

- **Propósito:** Permite aplicar reglas de estilo CSS **directamente** al elemento.
    
- **Regla de Oro:** **Evita su uso**. Esto viola el principio de separación de responsabilidades (HTML para estructura, CSS para estilo). Solo úsalo para pruebas rápidas o para estilos generados dinámicamente por JavaScript, **nunca** para la estilización principal.
    

```
<p style="color: red; font-size: 1.2em;">Este texto es rojo.</p>
```

---

### 2. Atributos de Datos y Acciones

#### a) `data-*` (Atributos de Datos Personalizados)

- **Propósito:** Permite almacenar **datos personalizados** privados en el código HTML. Estos datos no tienen ningún efecto en la presentación o el diseño, pero son accesibles a través de JavaScript.
    
- **Formato:** El nombre del atributo debe comenzar con `data-` seguido de cualquier nombre en minúsculas y sin espacios (ej. `data-user-id`).
    
- **Uso:** Es el método estándar para comunicar información del front-end (HTML) a la lógica del back-end o a scripts de JavaScript.
    

```
<button class="agregar-carrito" data-producto-id="73291" data-precio="150.99">
    Añadir
</button>

```

#### b) `hidden`

- **Propósito:** Indica que un elemento **no es relevante** actualmente y debe ser invisible.
    
- **Comportamiento:** El navegador no lo renderiza.
    
- **Diferencia con CSS:** A diferencia de `display: none;` en CSS, el atributo `hidden` tiene un **significado semántico**: el contenido no es relevante para el usuario en este momento.
    

```
<p id="mensaje-error" hidden>Error al enviar el formulario. Revise los datos.</p>
```

---

### 3. Atributos de Contenido y Edición

#### a) `title` (Información Adicional)

- **Propósito:** Proporciona información adicional sobre el elemento. El valor se muestra como un **tooltip** cuando el usuario pasa el cursor sobre el elemento.
    
- **Uso:** Útil para explicaciones cortas o sugerencias.
    
- **Nota de Accesibilidad:** No dependas de él para información crucial, ya que los usuarios de dispositivos táctiles o teclados no lo ven fácilmente.
    

```
<a href="/ayuda" title="Haga clic para ir a la sección de ayuda y preguntas frecuentes.">Ayuda</a>
```

#### b) `contenteditable`

- **Propósito:** Convierte cualquier elemento HTML en un área que el **usuario puede editar directamente** en el navegador.
    
- **Uso:** Ideal para aplicaciones que permiten la edición in-situ del contenido (CMS, editores WYSIWYG).
    

```
<div contenteditable="true" style="border: 1px dashed gray; padding: 10px;">
    Haz clic aquí y edita este texto. ¡Es mágico!
</div>
```

---

### 4. Buenas Prácticas Generales de Código

Para escribir código de calidad profesional, incorpora estos hábitos:

1. **Minúsculas para Etiquetas y Atributos:** HTML **no distingue entre mayúsculas y minúsculas** (es _case-insensitive_), pero el estándar (y las buenas prácticas) exige usar **minúsculas** (ej. `<h1>` en lugar de `<H1>`).
    
2. **Cerrar Etiquetas:** Asegúrate de que cada etiqueta abierta tenga su correspondiente etiqueta de cierre (excepto las etiquetas vacías/void: `<img>`, `<br>`, `<input>`).
    
3. **Indentación:** Usa una sangría consistente (espacios o tabs) para mostrar la jerarquía de los elementos. Esto hace que el código sea legible.
    
4. **Comentarios:** Utiliza la sintaxis `` para explicar secciones complejas o dejar notas. Esto es vital para ti y para otros desarrolladores.
    
5. **Validación:** Siempre busca validar tu código con herramientas como el **W3C Validator**. Un código válido reduce errores y mejora la compatibilidad (esto lo veremos en [[11 - Buenas prácticas de organización y validación de código]]).
    

---

### Práctica recomendada

**Objetivo:** Aplicar los atributos globales para control, estilización e información.

1. Abre el archivo `semantica.html` de la clase anterior.
    
2. Añade los siguientes atributos y elementos al cuerpo del documento:
    

```
<body>
    <header>
        <h1 id="titulo-principal">Mi Blog Semántico</h1>
        <nav class="navegacion-principal">...</nav>
    </header>
    
    <main>
        <section class="contenedor-articulos" data-autor-id="45" data-fecha-creacion="2025-09-30">
            
            <article>
                <h3 class="titulo-articulo" id="post-uno" title="Primer post del semestre.">Primer Post</h3>
                
                <div contenteditable="true" class="editor-texto">
                    <p>Este texto puedes editarlo directamente en el navegador.</p>
                </div>
            </article>
            
        </section>
        
        <p id="error-servidor" hidden>No se pudo conectar con la base de datos.</p>
    </main>
    
    <footer>
        <p class="copyright">&copy; 2025</p>
    </footer>
</body>
```

3. Visualiza el archivo y prueba:
    
    - Pasa el cursor sobre el `<h3>` y ve el **tooltip** (`title`).
        
    - Haz clic en el `<div>` con `contenteditable` y escribe.
        

---

### Cierre

Los atributos globales son el pegamento funcional del HTML. Ahora tienes el control para nombrar elementos (`id`), agruparlos (`class`), pasar datos personalizados (`data-*`) y controlar su visibilidad o editabilidad. Esta capa de control es la que te prepara para interactuar con JavaScript y CSS a nivel avanzado.

---

## Resumen de puntos claves

|Atributo Global|Propósito|Regla de Oro|
|---|---|---|
|**`id`**|Identificador único del elemento.|**DEBE** ser único en la página.|
|**`class`**|Agrupador para CSS y JavaScript.|El método preferido para estilización.|
|**`data-*`**|Almacenamiento de datos personalizados.|Estándar para pasar datos del HTML a JavaScript.|
|**`title`**|Tooltip informativo.|No debe usarse para información crucial de accesibilidad.|
|**`hidden`**|Oculta el elemento por no ser relevante.|Preferido sobre CSS para ocultar semánticamente.|
|**`contenteditable`**|Permite al usuario editar el contenido directamente.|Debe usarse con precaución y control de seguridad.|

**👉 Hemos pulido el código y los atributos. El siguiente nivel es la web inclusiva. Vamos a la clase más vital de todas: Accesibilidad:** [[10 - Accesibilidad en HTML5]]