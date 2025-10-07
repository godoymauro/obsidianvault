Has aprendido a hacer que tu diseño sea accesible. Ahora, vamos a la parte que convierte un proyecto personal en código de nivel empresarial: la **organización**. Un código CSS limpio y estructurado es la clave para la colaboración y el mantenimiento a largo plazo.

Aquí tienes la vigesimosegunda clase.

---

Aquí tienes los apuntes de la clase anterior: [[21 - Accesibilidad con CSS (A11y)]]

# 22 - Mantenimiento y Organización de CSS

## 📌 Introducción

A medida que un proyecto web crece, las hojas de estilo pueden volverse inmanejables. La repetición, las dependencias confusas y la falta de un convenio de nombres claro llevan a lo que se llama **"CSS spaghetti"**.

El mantenimiento y la organización de CSS buscan evitar esto, haciendo que tu código sea:

1. **Escalable:** Puede crecer sin romperse o volverse lento.
    
2. **Modular:** Las piezas son independientes y reutilizables.
    
3. **Predictivo:** Mirar una clase en el HTML te dice exactamente lo que hace en el CSS.
    

La herramienta más popular para lograrlo es el modelo **BEM**.

---

## 🏗️ Convenciones de Nombres: BEM

**BEM** significa **Block, Element, Modifier** (Bloque, Elemento, Modificador). Es un enfoque de nomenclatura que ayuda a los desarrolladores a construir componentes aislados y reutilizables.

### 1. Bloque (Block)

Un Bloque es una entidad independiente y reutilizable (un componente). Debe ser capaz de usarse en cualquier lugar de la página.

- **Sintaxis:** Nombre simple, en minúsculas, separado por guiones.
    
- **Ejemplos:** `.header`, `.button`, `.card`, `.form`.
    

### 2. Elemento (Element)

Un Elemento es una parte de un Bloque que no tiene sentido por sí misma fuera del Bloque.

- **Sintaxis:** El nombre del Bloque, seguido de **doble guion bajo** (`__`), y luego el nombre del Elemento.
    
- **Ejemplos (dentro del Bloque `.card`):** `.card__image`, `.card__title`, `.card__footer`.
    

### 3. Modificador (Modifier)

Un Modificador es un indicador (una bandera) que define el aspecto o comportamiento de un Bloque o un Elemento. Se utiliza para representar variaciones (ej: un botón grande, un botón primario).

- **Sintaxis:** El nombre del Bloque o Elemento, seguido de **doble guion medio** (`--`), y luego el nombre del Modificador.
    
- **Ejemplos (para un Bloque `.button`):** `.button--primary`, `.button--large`, `.button--disabled`.
    

### ✍️ Ejemplo Práctico de BEM

|HTML (Usando BEM)|CSS (BEM)|
|---|---|
|`html <div class="card card--featured"> <h2 class="card__title">Título</h2> <p class="card__description">Texto</p> <button class="button button--primary">Comprar</button> </div>`|`css .card { /* Estilos base del bloque */ } .card--featured { /* Modificador: cambia el fondo para destacarlo */ background-color: gold; } .card__title { /* Estilos del elemento título */ font-size: 1.5rem; } .button--primary { /* Modificador: cambia el botón a color azul */ background-color: blue; }`|

Exportar a Hojas de cálculo

**Ventajas Clave de BEM:**

1. **Especificidad Baja:** Al usar selectores de una sola clase (`.card__title`), la especificidad es baja, haciendo que los estilos sean fáciles de sobrescribir sin usar `!important` (ver [[04 - Especificidad y herencia]]).
    
2. **Modularidad:** Cada regla CSS es un bloque o parte de él; sabes que `.card__title` **solo** afectará al `.card`.
    
3. **Búsqueda Fácil:** En un proyecto grande, buscar `.card__title` te lleva directamente al CSS relevante.
    

---

## 📂 Organización de Archivos (Arquitectura Modular)

Es una mala práctica tener todo tu CSS en un solo archivo `styles.css`. Los proyectos escalables dividen el CSS en archivos más pequeños e importan todo a un archivo principal.

### 1. Metodología 7-1 (SASS/Preprocesadores)

Aunque es más común con preprocesadores (que veremos en [[23 - Metodologías modernas]]), este enfoque de estructura de carpetas es útil incluso con CSS puro (importando archivos).

|Carpeta|Contenido|Ejemplo de Archivos|
|---|---|---|
|**`base/`**|Configuración de inicio (resets, tipografía base, variables).|`_reset.css`, `_typography.css`|
|**`components/`**|Bloques BEM reutilizables (tarjetas, botones, navegación).|`_button.css`, `_card.css`, `_navbar.css`|
|**`layout/`**|Estructura de página (header, footer, grid, secciones principales).|`_header.css`, `_grid.css`, `_footer.css`|
|**`pages/`**|Estilos específicos que solo se usan en una página (ej: página de inicio).|`_home.css`, `_about.css`|
|**`themes/`**|Estilos para temas (modo oscuro, temas de color alternativos).|`_dark-mode.css`|

Exportar a Hojas de cálculo

### 2. Archivo Principal (`style.css`)

El archivo CSS principal es el único enlazado al HTML. Utiliza la regla `@import` para ensamblar todos los archivos modulares en el orden correcto.

CSS

```
/* style.css (El archivo único que el navegador carga) */

/* 1. Base y Variables */
@import 'base/_reset.css';
@import 'base/_variables.css';
@import 'base/_typography.css';

/* 2. Layouts */
@import 'layout/_grid.css';
@import 'layout/_header.css';
@import 'layout/_footer.css';

/* 3. Componentes (Bloques BEM) */
@import 'components/_button.css';
@import 'components/_card.css';
@import 'components/_navbar.css';

/* 4. Página específica */
@import 'pages/_home.css';
```

---

## 🧼 Escribir CSS Limpio y Legible

Más allá de la estructura, la forma en que escribes tu código facilita el mantenimiento:

1. **Comentarios de Bloque:** Usa comentarios (`/* ... */`) para separar secciones y componentes (ej: `/* =================== Componente: Card =================== */`).
    
2. **Una Declaración por Línea:** Mejora la legibilidad y la gestión de versiones (ej: git).
    
    CSS
    
    ```
    /* Limpio y legible */
    .button {
        color: white;
        padding: 10px 20px;
        border-radius: 5px;
    }
    ```
    
3. **Agrupación de Selectores:** Si varios selectores tienen las mismas propiedades, agrúpalos en una sola regla para evitar la repetición.
    
    CSS
    
    ```
    /* En lugar de repetir los mismos 5 estilos en h1 y h2, agrúpalos */
    h1, h2, .subtitulo {
        font-family: var(--fuente-principal);
        color: var(--color-primario);
        text-align: center;
    }
    ```
    

---

## ✅ Resumen de Puntos Clave

- Un código CSS **Escalable** y **Modular** es clave para proyectos grandes.
    
- **BEM (Block, Element, Modifier)** es el estándar de nomenclatura para crear componentes reutilizables y aislar estilos.
    
- BEM usa **doble guion bajo** (`__`) para Elementos y **doble guion medio** (`--`) para Modificadores.
    
- La **Arquitectura Modular** (dividir el código en archivos por responsabilidad: `base/`, `components/`, `layout/`) facilita el mantenimiento.
    
- La regla **`@import`** en el archivo CSS principal se usa para ensamblar todos los módulos.
    
- Escribir con **comentarios de bloque**, **una declaración por línea** y **agrupando selectores** mejora la legibilidad y la colaboración.
    

👉 Hemos visto BEM, que es una metodología. Hay otros enfoques que van un paso más allá, combinando la filosofía de BEM con el poder de los _utilities_. Continúa con [[23 - Metodologías modernas]].