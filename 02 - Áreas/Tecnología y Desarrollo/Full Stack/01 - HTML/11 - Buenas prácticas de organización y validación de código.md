¡Claro que sí! Con la accesibilidad garantizada, pasamos a la disciplina que hace que el código sea duradero: la calidad profesional.

---

Aquí tienes los apuntes de la clase anterior: [[10 - Accesibilidad en HTML5]]

## Clase 11 - Buenas prácticas de organización y validación de código ✅

### Introducción

Esta clase se centra en la **disciplina del desarrollador**. De nada sirve tener la mejor semántica y las APIs más avanzadas si el código es ilegible, inconsistente o, peor aún, inválido. Aprenderás a estructurar tus archivos de manera lógica, a comentar de forma efectiva y, crucialmente, a usar el **Validador del W3C** para asegurar que tu HTML cumple rigurosamente con el estándar. El **código limpio** es el pilar de un proyecto mantenible y colaborativo.

---

### 1. Organización del Proyecto (Estructura de Carpetas)

La organización de archivos es la primera señal de profesionalismo. Un proyecto debe ser fácil de navegar para cualquier persona que trabaje en él.

#### Estructura Estándar

Una estructura lógica y estándar para un proyecto web pequeño a mediano se organiza por **tipo de recurso**:

```
mi-proyecto/
├── index.html            // Página principal (punto de entrada)
├── acerca.html           // Otras páginas HTML (pueden ir en una carpeta 'pages/')
├── assets/               // Contenedor general para todos los recursos estáticos
│   ├── css/              // Hojas de estilo
│   │   └── style.css
│   ├── js/               // Scripts de JavaScript
│   │   └── main.js
│   ├── img/              // Imágenes (logos, fotos, iconos)
│   │   └── logo.png
│   └── media/            // Archivos multimedia (videos, audios)
└── README.md             // Documentación esencial del proyecto
```

#### Reglas de Nomenclatura Clave

- **Minúsculas y Guiones:** Usa siempre **minúsculas** y guiones medios (`-`) para separar palabras en nombres de archivo y carpetas (ej. `mi-imagen.jpg`, `sobre-nosotros.html`).
    
- **Evita:** Espacios, mayúsculas o caracteres especiales. Esto previene problemas de compatibilidad en diferentes sistemas operativos y servidores web, y simplifica las rutas en los enlaces.
    

---

### 2. Estilo de Codificación y Legibilidad

La legibilidad del código es tan importante como su funcionalidad. Un código legible reduce errores y acelera el _debugging_.

#### a) Indentación Consistente

- **Propósito:** La sangría (indentación) visualiza la **jerarquía** de los elementos.
    
- **Estándar:** Mantén una sangría uniforme (ya sea de 2 o 4 espacios, o tabs) para cada nivel de anidamiento. Esto es lo que permite identificar si una etiqueta está mal cerrada o anidada.
    

```
<header>
  <nav>
    <ul class="menu">
      <li><a href="/">Inicio</a></li>
    </ul>
  </nav>
</header>
```

#### b) Citas y Minúsculas

- **Atributos:** Utiliza siempre comillas dobles (`"`) para los valores de los atributos.
    
- **Convención HTML5:** Escribe nombres de etiquetas y atributos en **minúsculas** (ej. `<meta charset="utf-8">` en lugar de `<META CHARSET='UTF-8'>`).
    

#### c) Comentarios Efectivos

Los comentarios deben explicar el **por qué** o el **cómo complejo** de una sección, no simplemente qué hace (eso ya lo dice el código semántico).

- **Comentarios de Bloque:** Para dividir secciones lógicas grandes (Visto en [[08 - Elementos semánticos]]).
    
- **Comentarios de Línea:** Para explicaciones específicas o advertencias.
    
    ```
    <input type="email" name="correo" required> ```
    
    ```
    

#### d) El Atributo `type` (Omisión en HTML5)

En HTML5, se asume que los archivos vinculados son CSS y JavaScript.

- **Buena Práctica:** Omite el atributo `type` (`type="text/css"` o `type="text/javascript"`) para un código más limpio y moderno.
    

|Elemento|HTML4 (Obsoleto)|HTML5 (Limpio)|
|---|---|---|
|**CSS**|`<link rel="stylesheet" type="text/css" href="style.css">`|`<link rel="stylesheet" href="style.css">`|
|**JavaScript**|`<script type="text/javascript" src="main.js"></script>`|`<script src="main.js"></script>`|

---

### 3. La Herramienta Crucial: El Validador del W3C

La validación es el proceso de verificar que tu código HTML cumple rigurosamente con las reglas gramaticales del estándar. Un código no válido puede ser interpretado de forma inconsistente por diferentes navegadores, causando _bugs_ visuales o de comportamiento.

#### a) ¿Por qué validar?

1. **Compatibilidad entre Navegadores:** Asegura que todos los navegadores interpreten el código de la misma manera (evitando el modo _Quirks_).
    
2. **Debugging Rápido:** El validador detecta errores de sintaxis difíciles de ver (etiquetas no cerradas, anidamiento incorrecto, IDs duplicados).
    
3. **Garantía de Calidad:** Muestra profesionalismo y que el proyecto se adhiere a las especificaciones oficiales.
    

#### b) ¿Cómo usarlo?

1. **Herramienta Oficial:** Utiliza el **W3C Markup Validation Service** (puedes buscarlo como _W3C validator_).
    
2. **Proceso:** Puedes ingresar la URL de tu página, subir el archivo HTML, o simplemente pegar el código.
    
3. **El Objetivo:** Siempre apunta a tener cero errores y cero advertencias. Las advertencias no son errores fatales, pero indican áreas de mejora.
    

> **Ejemplos de errores comunes que el validador atrapa:**
> 
> - `ID` duplicados (visto en [[09 - Atributos globales y buenas prácticas]]).
>     
> - Etiquetas de bloque dentro de etiquetas de línea.
>     
> - Falta del atributo `alt` en la etiqueta `<img>`.
>     
> - Uso de atributos obsoletos.
>     

---

### Práctica recomendada

**Objetivo:** Crear un código que parezca correcto pero contenga errores de validación, y luego corregirlos.

1. Crea un nuevo archivo llamado `codigo_valido.html`.
    
2. Escribe el siguiente fragmento, que tiene algunos errores sutiles:
    

```
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>VALIDACION DE CÓDIGO</title>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
    <H1>Mi Página de Prueba</H1>
    <section>
        <a id="enlace-prueba" href="#">
            <p>Este párrafo está anidado incorrectamente en el enlace (ERROR)</p>
        </a>
    </section>
    <img src="foto.jpg" height="200"> </body>
</html>
```

3. **Corrige el código:**
    
    - Cambia `<H1>` a `<h1>` y `<TITLE>` a `<title>`.
        
    - Elimina `type="text/css"` de `<link>`.
        
    - Corrige la anidación: un `<p>` (elemento de bloque) no puede estar dentro de un `<a>` (elemento de línea). Debes poner el `<a>` dentro del `<p>`.
        
    - Añade el atributo `alt=""` a la etiqueta `<img>`.
        
4. **Valida:** Copia y pega el código **corregido** en el W3C Validator hasta que el resultado sea "No errors or warnings to show."
    

---

### Cierre

Has cubierto las prácticas que elevan tu código a un estándar profesional. Recuerda que la organización de archivos, la consistencia en el estilo y, sobre todo, la **validación** con el W3C, son tan importantes como la semántica misma. Estás listo para el último desafío, la última fase de nuestro curso.

---

## Resumen de puntos claves

|Concepto|Regla de Oro|Importancia|
|---|---|---|
|**Organización**|Estructurar por tipo de recurso (`assets/css`, `assets/js`, etc.).|Facilita la colaboración y el mantenimiento del proyecto.|
|**Consistencia**|Usar minúsculas y sangría uniforme (2 o 4 espacios).|Mejora drásticamente la legibilidad y reduce errores.|
|**Validación W3C**|Usar la herramienta oficial para verificar la sintaxis.|**Asegura la compatibilidad** y la calidad del código.|
|**`type` (omisión)**|Omitir `type` en `<script>` y `<link>`.|Mantiene el código limpio y moderno (práctica de HTML5).|

**👉 Hemos pulido la calidad. Es hora de darle superpoderes a nuestra página con la tecnología del navegador, iniciando la Fase IV: ** [[12 - APIs de HTML5]]