Hemos terminado con el corazón técnico del CSS. Entramos ahora en el Nivel 5: Buenas Prácticas y Proyectos. Esta clase es crucial para ser un desarrollador profesional, ya que el diseño no es solo para el ojo, sino para **todos** los usuarios.

Aquí tienes la vigesimoprimera clase.

---

Aquí tienes los apuntes de la clase anterior: [[20 - Pseudo-clases y pseudo-elementos avanzados]]

# 21 - Accesibilidad con CSS (A11y)

## 📌 Introducción

La **Accesibilidad Web (A11y)**, que a menudo se abrevia como A11y (porque hay 11 letras entre la 'A' y la 'y'), se refiere a la práctica de diseñar y desarrollar sitios web y herramientas que puedan ser utilizadas por personas con la más amplia gama de discapacidades, incluyendo visuales, auditivas, físicas, de habla, cognitivas y neurológicas.

CSS tiene un papel crucial en la A11y. Un CSS mal implementado puede hacer que un sitio sea imposible de usar para personas con baja visión, dislexia o daltonismo. Un buen CSS, en cambio, garantiza que el diseño **respete las preferencias del usuario** y mantenga la legibilidad en cualquier contexto.

## 👓 Legibilidad y Contraste de Color

El contraste es el requisito de accesibilidad más medible y más fallado. El texto debe tener un contraste de color suficiente con el fondo para que las personas con baja visión o daltonismo puedan leerlo.

### 1. Requisitos de Contraste (WCAG)

Las Pautas de Accesibilidad al Contenido Web (WCAG) establecen dos niveles principales de contraste:

|Nivel|Proporción de Contraste|Aplicación|
|---|---|---|
|**AA (Mínimo)**|**4.5:1** para texto normal.|Estándar de la industria para la mayoría del contenido.|
|**AAA (Mejorado)**|**7:1** para texto normal.|Para contenido muy sensible (ej: sitios gubernamentales, educación).|

Exportar a Hojas de cálculo

**Implicación CSS:** Siempre usa herramientas de contraste para verificar las combinaciones de `color` (texto) y `background-color` (fondo) que elijas.

### 2. Evitar Confiar Solo en el Color

El color nunca debe ser el **único medio** para transmitir información o indicar una acción.

- **Mal Ejemplo:** `input.error { border: 1px solid red; }` (Los usuarios daltónicos podrían no ver la diferencia).
    
- **Mejor Ejemplo:** `input.error { border: 2px solid red; } input.error::after { content: " *Campo Requerido"; }` (Se añade un indicador visual que no depende solo del color).
    

---

## 🔡 Tipografía y Espaciado para la Lectura

El manejo de la tipografía es vital para usuarios con dislexia o problemas de procesamiento visual.

### 1. Tamaños de Fuente y Unidades

- **No Fijas el Tamaño:** **Nunca** uses `px` en el elemento `<body>` o `<html>` para el `font-size`. Esto impide que el usuario pueda escalar el texto usando la configuración de su navegador.
    
- **Usa Unidades Relativas:** Usa **`rem`** para el `font-size` y el espaciado (visto en [[05 - Colores y unidades en CSS]] y [[06 - Tipografía en CSS]]). El `rem` se escala según la configuración base del navegador del usuario.
    

### 2. Longitud de Línea y Altura

- **Longitud de Línea:** Las líneas de texto demasiado largas son difíciles de leer. La longitud óptima es entre 50 y 80 caracteres por línea.
    
    - **Solución CSS:** Usa `max-width` en contenedores de texto, limitándolos a un valor de `ch` (unidades relativas al ancho del carácter '0').
        
    
    CSS
    
    ```
    p {
        max-width: 70ch; /* Limita a unos 70 caracteres por línea */
    }
    ```
    
- **Altura de Línea (`line-height`):** El espaciado entre líneas es crucial.
    
    - **Requisito WCAG:** Usa `line-height` de al menos **1.5** para el cuerpo de texto.
        
    
    CSS
    
    ```
    p {
        line-height: 1.5; /* Recomendado para legibilidad */
    }
    ```
    

---

## ⌨️ Enfoque (Focus) y Navegación por Teclado

Los usuarios que dependen de la navegación por teclado (sin ratón) necesitan ver claramente dónde están en la página. El indicador visual de **enfoque** es lo que les guía.

### 1. No Eliminar el Outline

El navegador muestra un anillo azul o negro alrededor de los elementos interactivos (`<button>`, `<a>`, `<input>`) cuando se navega con la tecla `Tab`. Muchos diseñadores eliminan este anillo por estética, lo que destruye la accesibilidad.

- **Mala Práctica (¡A evitar!):**
    
    CSS
    
    ```
    :focus {
        outline: none; /* ¡NO HACER ESTO! */
    }
    ```
    
- **Buena Práctica (Personalizar, pero mantener):** Si eliminas el `outline` por defecto, debes sustituirlo inmediatamente por una alternativa de contraste alto (ej: un `box-shadow` o un borde).
    
    CSS
    
    ```
    :focus {
        outline: none;
        /* Sustituto visible y con alto contraste */
        box-shadow: 0 0 0 3px var(--color-primario); 
    }
    ```
    

---

## 🎨 Respetar las Preferencias del Usuario

Como vimos en [[14 - Diseño fluido y adaptativo]], CSS nos permite detectar las preferencias del sistema operativo del usuario.

### 1. Reducción de Movimiento

Asegura que las animaciones (`transition`, `@keyframes`) se desactiven para los usuarios que lo soliciten.

CSS

```
@media (prefers-reduced-motion: reduce) {
    /* Desactiva TODAS las animaciones y transiciones por seguridad */
    *, *::before, *::after {
        animation-duration: 0.01ms !important;
        transition-duration: 0.01ms !important;
        animation-iteration-count: 1 !important;
        scroll-behavior: auto !important;
    }
}
```

### 2. Modo Oscuro

Usa la Media Query **`(prefers-color-scheme: dark)`** para cambiar tu esquema de colores por uno de fondo oscuro, respetando la preferencia del sistema.

---

## ✅ Resumen de Puntos Clave

- **A11y (Accesibilidad)** es esencial para que todos los usuarios puedan acceder al contenido.
    
- El contraste de color debe cumplir el estándar **4.5:1 (Nivel AA)**, verificado por herramientas externas.
    
- **Nunca uses `px`** para el `font-size` del texto base; usa **`rem`** para respetar el escalado del navegador.
    
- Usa `max-width: 70ch;` en párrafos para optimizar la **longitud de línea**.
    
- **Nunca elimines el `outline` del `:focus`** a menos que lo sustituyas con un indicador visual personalizado y de alto contraste.
    
- Respeta las preferencias del sistema operativo, como el **modo oscuro** y la **reducción de movimiento**, usando Media Queries.
    

👉 El siguiente paso es organizar el código CSS que hemos estado escribiendo para que sea escalable y fácil de mantener en proyectos de cualquier tamaño. Continúa con [[22 - Mantenimiento y organización de CSS]].