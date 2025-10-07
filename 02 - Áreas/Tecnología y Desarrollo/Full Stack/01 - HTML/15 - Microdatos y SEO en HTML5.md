¡Absolutamente! Hemos llegado a un punto donde la calidad de nuestro código se mide no solo por lo que el usuario ve, sino por lo que las máquinas entienden. Vamos a convertir nuestros datos en información estructurada para los motores de búsqueda.

---

Aquí tienes los apuntes de la clase anterior: [[14 - Audio y video avanzado]]

## Clase 15 - Microdatos y SEO en HTML5 🤖

### Introducción

Ya sabes que el HTML semántico ([[08 - Elementos semánticos]]) ayuda al SEO (Search Engine Optimization). Sin embargo, hay un nivel superior de semántica: los **Datos Estructurados** (a veces implementados mediante **Microdatos**). Los Datos Estructurados son pequeñas etiquetas que insertamos en el HTML para describir explícitamente qué significa un dato. Por ejemplo, le decimos a Google que un texto no es solo un `<h3>`, sino el **nombre** de una receta, y un número no es solo un `<span>`, sino su **calificación** promedio. Esto genera los famosos **Fragmentos Enriquecidos** (_Rich Snippets_) en los resultados de búsqueda.

---

### 1. Conceptos Fundamentales de Datos Estructurados

El objetivo es pasar de datos textuales a datos comprensibles por máquinas, utilizando un vocabulario estandarizado conocido como **Schema.org**.

#### El Problema sin Marcado

```
<p>Valoración: 4.5 estrellas</p>
```

Para un motor de búsqueda, eso es solo un párrafo. El número 4.5 no tiene un significado especial.

#### La Solución con Microdatos (Vocabulario de Schema.org)

Microdatos es una sintaxis específica que utiliza tres atributos globales que podemos añadir a **cualquier** elemento HTML.

|Atributo|Función|Explicación|
|---|---|---|
|**`itemscope`**|**Alcance del Ítem.** Crea el contenedor o **contexto** de una entidad. Le dice al buscador: "Todo lo que está aquí dentro es un solo _ítem_."|Siempre se coloca en el elemento contenedor principal.|
|**`itemtype`**|**Tipo de Ítem.** Define el **tipo de esquema** (entidad) que estamos describiendo, usando el vocabulario de Schema.org.|Siempre va junto a `itemscope`. Ej: `https://schema.org/Recipe`.|
|**`itemprop`**|**Propiedad del Ítem.** Define una **propiedad** específica de la entidad.|Se aplica a los elementos que contienen el valor de la propiedad.|

### Ejemplo Práctico: Marcado de una Reseña

Vamos a marcar un bloque de reseña para que Google entienda la calificación:

```
<div itemscope itemtype="https://schema.org/Review">
    
    <p>Por: <span itemprop="author">Andrea B.</span></p>
    
    <div itemprop="reviewRating" itemscope itemtype="https://schema.org/Rating">
        <p>Calificación: <span itemprop="ratingValue">5</span> / <span itemprop="bestRating">5</span> estrellas.</p>
        
        <meta itemprop="worstRating" content="1"> 
    </div>
    
    <p itemprop="reviewBody">El servicio fue impecable, totalmente recomendado.</p>
</div>
```

---

### 2. Microdatos para Contenido NO Visible

A veces, la información que necesita el motor de búsqueda no debe ser visible para el usuario (ej. una ID interna, o un valor numérico que ya se muestra como iconos). Para esto, usamos etiquetas **vacías**:

- **`<meta>`:** Utilizada para propiedades cuyo valor es un texto o un número (ej. el `ratingValue` si solo mostramos estrellas).
    
- **`<link>`:** Utilizada para propiedades cuyo valor es una URL (ej. la URL del logo de la organización).
    

```
<div itemscope itemtype="https://schema.org/Organization">
    <h1 itemprop="name">Mi Empresa S.A.</h1> 
    
    <link itemprop="url" href="https://miempresa.com/">
    <meta itemprop="logo" content="https://miempresa.com/logo.png">
</div>
```

---

### 3. Buenas Prácticas y Validación SEO

La eficacia de los Microdatos depende totalmente de que tu marcado sea **sintácticamente correcto** y use el vocabulario oficial.

1. **Enlace a Schema.org:** Siempre usa la URL completa (`https://schema.org/Tipo`) para el `itemtype`.
    
2. **Mantenimiento:** La estructura de Microdatos debe coincidir con el contenido visible. Si cambias la calificación en el HTML, debes cambiar el valor en el `itemprop`.
    
3. **Herramienta de Validación:** Google proporciona una herramienta crucial llamada **"Prueba de Resultados Enriquecidos"** (busca _Google Rich Results Test_). **Siempre** pasa tu código por esta herramienta. Es la única forma de garantizar que Google interpreta tu esquema correctamente y que tu contenido es elegible para aparecer como Fragmento Enriquecido.
    
4. **Prioridad:** Enfócate en marcar tipos de contenido que beneficien al usuario y que Google prioriza, como reseñas, recetas, productos y eventos.
    

---

### Práctica recomendada

**Objetivo:** Marcar una publicación de blog con metadatos de **Article**.

1. Crea un archivo llamado `microdatos.html`.
    
2. Implementa la estructura de un artículo (que contiene autor y fecha) usando los Microdatos:
    

```
<body>
    <h1>Microdatos: Marcado de Contenido</h1>

    <article itemscope itemtype="https://schema.org/Article">
        
        <h2 itemprop="headline">Los 5 Secretos de un HTML5 Bien Estructurado</h2>
        
        <div itemprop="author" itemscope itemtype="https://schema.org/Person">
            <p>Por: <span itemprop="name">Profesor Carlos</span></p>
        </div>
        
        <p>Publicado el: 
            <time itemprop="datePublished" datetime="2025-10-01">1 de octubre de 2025</time>
        </p>
        
        <meta itemprop="image" content="https://tusitio.com/img/html-seo.jpg">
        
        <div itemprop="articleBody">
            <p>La semántica es el secreto mejor guardado del SEO moderno...</p>
        </div>
        
    </article>
</body>
```

3. **Validación:** Copia y pega el código del `<article>` en la Prueba de Resultados Enriquecidos de Google. Verifica que la herramienta reconozca el `Article`, el `headline`, el `author` y la `datePublished`.
    

---

### Cierre

Dominar los Microdatos te posiciona como un desarrollador que piensa en la **visibilidad** y la **indexación avanzada**. Los Fragmentos Enriquecidos que generas con `itemscope`, `itemtype` e `itemprop` pueden aumentar drásticamente la tasa de clics (CTR) de tu sitio. Recuerda siempre verificar tu trabajo con las herramientas de Google.

---

## Resumen de puntos claves

| Concepto              | Función                                                      | SEO Importancia                                                          |
| --------------------- | ------------------------------------------------------------ | ------------------------------------------------------------------------ |
| **Microdatos**        | Etiquetas para definir el **significado** de los datos.      | Generan Fragmentos Enriquecidos y mejoran la visibilidad.                |
| **`itemscope`**       | Crea el contenedor de la entidad.                            | Inicia el marcado de un _ítem_ de Schema.                                |
| **`itemtype`**        | Define el tipo de esquema (ej. `Article`, `Product`).        | Especifica la naturaleza de la información.                              |
| **`itemprop`**        | Define una propiedad específica (ej. `headline`, `author`).  | Asigna el valor del dato dentro de la entidad.                           |
| **Schema.org**        | Vocabulario estandarizado y universal para Microdatos.       | El estándar global para motores de búsqueda.                             |
| **`<meta>`/`<link>`** | Usado para marcar datos que no son visibles para el usuario. | Permite incluir datos esenciales para el esquema, sin alterar el diseño. |

**👉 Hemos pulido cada detalle técnico y de calidad. ¡El gran momento ha llegado! Vamos a la clase final, donde construirás tu proyecto integrador:** [[16 - Proyecto final construir una página web completa y semántica con HTML5]]