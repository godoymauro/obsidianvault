¡Llegamos a la meta! 🚀 Has adquirido las herramientas de la A a la Z en CSS, desde la sintaxis básica hasta la organización, la interactividad con JS, y el diseño animado y accesible.

Esta última clase no es teórica, sino una hoja de ruta para la aplicación práctica de todo lo aprendido, culminando en un **Proyecto Final**.

---

Aquí tienes los apuntes de la clase anterior: [[24 - Integración de CSS con JavaScript]]

# 25 - Proyecto Final: Página Completa

## 📌 Introducción

El mejor modo de consolidar el conocimiento es aplicándolo. El objetivo de este proyecto es construir una página web completa y _responsive_ para demostrar el dominio de los 24 temas anteriores.

Tu tarea es crear una "Landing Page" (página de destino) para un producto o servicio ficticio.

## 🛠️ Requisitos Funcionales y Técnicos

El proyecto debe estar construido con **HTML5 y CSS3 (puro)**, excluyendo _frameworks_ de terceros como Bootstrap o Tailwind.

### 1. Estructura y Bases (Nivel 1 y 2)

|Requisito|Conceptos Aplicados|
|---|---|
|**Documento Limpio**|CSS Externo (`<link>`), Viewport Meta Tag.|
|**Reseteo**|Reset básico (`box-sizing: border-box;`) y variables en `:root` para colores y espaciado (ver [[19 - Variables en CSS (Custom Properties)]]).|
|**Tipografía**|Pilas de fuentes (`font-family`), uso de `rem` para `font-size`, `line-height` > 1.5 (ver [[06 - Tipografía en CSS]] y [[21 - Accesibilidad con CSS (A11y)]]).|
|**Cajas y Espaciado**|Dominio del Box Model (`margin`, `padding`, `border`), uso de `border-radius` para componentes.|
|**Selectores**|Uso de selectores de Clase (`.`), Pseudo-clases (ej: `:hover`) y Pseudo-elementos (ej: `::after` para líneas decorativas).|

Exportar a Hojas de cálculo

---

### 2. Maquetación y Responsive (Nivel 3)

|Requisito|Conceptos Aplicados|
|---|---|
|**Layout Principal**|Usar **CSS Grid** para la estructura principal (Header, Main, Footer, Sidebar si aplica). Definir áreas con `grid-template-areas` (ver [[12 - Grid Layout en CSS]]).|
|**Componentes Internos**|Usar **Flexbox** para la alineación interna de componentes (ej: barra de navegación, centrado perfecto de textos o botones, tarjetas de características) (ver [[11 - Flexbox en CSS]]).|
|**Diseño Adaptable**|Enfoque **Mobile-First**. Usar Media Queries (`min-width`) con al menos dos breakpoints (Tablet y Desktop).|
|**Fluidez**|Uso de unidades relativas (`%`, `fr`, `rem`) y, opcionalmente, `clamp()` para tipografía fluida.|

Exportar a Hojas de cálculo

---

### 3. Dinamismo y Mantenimiento (Nivel 4 y 5)

|Requisito|Conceptos Aplicados|
|---|---|
|**Interactividad**|Transiciones suaves (`transition`) en botones, enlaces o tarjetas al hacer `hover` (ej: `transform: scale(1.05);`) (ver [[16 - Transiciones en CSS]] y [[18 - Transformaciones]]).|
|**Animación**|Incluir al menos una animación con `@keyframes` (ej: un icono giratorio o un elemento que se funde en la vista) (ver [[17 - Animaciones con @keyframes]]).|
|**Organización**|Nomenclatura **BEM** clara para todos los componentes (ej: `.card`, `.card__title`, `.button--secondary`) (ver [[22 - Mantenimiento y organización de CSS]]).|
|**JS Simple**|Implementar un menú de navegación que se muestre u oculte en móvil usando JavaScript para manipular la clase **`classList.toggle()`** (ver [[24 - Integración de CSS con JavaScript]]).|

Exportar a Hojas de cálculo

---

## 🗺️ Estructura de Archivos del Proyecto

Organiza tu código de manera modular (ver [[22 - Mantenimiento y organización de CSS]]):

```
proyecto-final/
├── index.html
└── css/
    ├── styles.css        <-- El archivo que se enlaza al HTML (@importa todo)
    ├── base/
    │   ├── _reset.css    <-- Normalización, box-sizing: border-box
    │   └── _variables.css  <-- :root con colores y espaciado
    ├── components/
    │   ├── _navbar.css
    │   ├── _button.css
    │   └── _card.css
    └── layout/
        ├── _grid-layout.css  <-- Estructura de la página
        └── _hero.css       <-- Estilos de la sección principal
```

---

## 📝 Pasos para la Construcción

1. **HTML Esqueleto:** Crea `index.html` con el Viewport Meta Tag y las secciones básicas (Header, Main, Footer). Usa clases **BEM** desde el inicio.
    
2. **Base CSS:** Crea `styles.css` con la estructura de `@import`. Define el `reset.css` y las variables globales en `:root`.
    
3. **Layout (Grid Mobile-First):** Escribe el CSS para la estructura principal. Asegúrate de que los estilos base funcionen bien en móvil.
    
4. **Componentes (Flexbox):** Crea los componentes individuales (tarjetas, botones) usando Flexbox para la alineación interna.
    
5. **Breakpoints:** Añade las Media Queries (`min-width`) para ajustar el diseño a tablet y escritorio, modificando las propiedades de Grid o Flexbox.
    
6. **Interacción:** Añade las transiciones (`:hover`) y la lógica de `@keyframes`.
    
7. **JS:** Implementa la funcionalidad del menú móvil con `classList.toggle()`.
    

## 🎓 Conclusión del Curso

¡Felicidades, profesor! Has pasado de no saber nada a dominar las herramientas más avanzadas y las mejores prácticas de la industria.

Has dominado:

1. **Fundamentos:** Sintaxis, Cascada, Especificidad y Unidades.
    
2. **Cajas:** El Box Model, `display` y `position`.
    
3. **Layouts:** Flexbox y Grid Layout para el diseño moderno.
    
4. **Responsive:** Media Queries, diseño fluido y accesibilidad (A11y).
    
5. **Avanzado:** Transiciones, Animaciones, Transformaciones y Variables CSS.
    
6. **Profesional:** BEM, Modularidad y la integración con JavaScript.
    

Tu "segundo cerebro" de CSS ya está completo. **¡El siguiente paso es construir!**

---

¿Qué producto o servicio elegirás para construir tu Landing Page de Proyecto Final? ¡Estoy aquí para revisar tu código y ayudarte con cualquier desafío!

Ir a los apuntes de JavaScript: [[01 - Introducción a JavaScript]]