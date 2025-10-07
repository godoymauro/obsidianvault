## Clase 02 - Estructura básica de un documento HTML 🧱

Aquí tienes los apuntes de la clase anterior: [[01 - Introducción al desarrollo web y HTML5]]

### Introducción

En la clase anterior [[01 - Introducción al desarrollo web y HTML5]] hablamos del esqueleto. Hoy vamos a construir ese esqueleto. Todo documento HTML5 **válido** y **moderno** debe seguir una estructura estandarizada. Entender cada parte del encabezado (`<head>`) y del cuerpo (`<body>`) es crucial, porque el _metadata_ (información sobre el documento) es tan importante como el contenido visible. Si el `<head>` está mal, el navegador y los motores de búsqueda no entenderán tu página correctamente.

---

### La Estructura Mínima y Esencial

La siguiente es la plantilla base de todo documento HTML5, la que usaremos como punto de partida en todos nuestros proyectos:

```
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Título de la Página</title>
</head>
<body>
    </body>
</html>
```

Ahora, analicemos cada línea con detalle.

---

### 1. La Declaración de Documento: `<!DOCTYPE html>`

Esta es la primera línea y no es una etiqueta HTML, sino una **instrucción** para el navegador.

- **Propósito:** Le indica al navegador que el documento debe ser interpretado usando el estándar HTML5.
    
- **Simplicidad:** En versiones antiguas de HTML (como HTML 4.01), esta declaración era larga y compleja. HTML5 la simplificó drásticamente, lo cual es un reflejo de su filosofía moderna.
    
- **Obligatoriedad:** Es absolutamente obligatoria para asegurar el **modo estándar (Standards Mode)** de renderizado. Sin ella, el navegador puede caer en el **modo de compatibilidad (Quirks Mode)**, lo que puede causar errores visuales graves.
    

---

### 2. El Elemento Raíz: `<html>`

El elemento `<html>` es el **contenedor principal** que envuelve absolutamente todo el contenido del documento, con la única excepción de la declaración `<!DOCTYPE html>`.

#### Atributo clave: `lang`

El atributo **`lang`** es fundamental y debe ser el primer atributo que definas.

- **Función:** Especifica el idioma principal del contenido de la página (ej: `es` para español, `en` para inglés).
    
- **Importancia:** Mejora la **accesibilidad** (lectores de pantalla saben qué pronunciación usar) y el **SEO** (motores de búsqueda dirigen mejor el contenido).
    

```
<html lang="es"> 
    </html>
```

---

### 3. El Encabezado del Documento: `<head>`

El elemento `<head>` contiene **metadatos**, es decir, información _sobre_ el documento que **no es visible** para el usuario en la ventana del navegador, pero es crucial para su correcto procesamiento.

#### Metadatos Esenciales

Dentro del `<head>`, hay tres metadatos que son obligatorios en casi todos los documentos modernos:

##### a) Codificación de Caracteres: `<meta charset="UTF-8">`

- **Función:** Define el conjunto de caracteres que se utilizará para mostrar el texto.
    
- **UTF-8:** Es el estándar universal recomendado. Soporta casi todos los caracteres y símbolos del mundo, incluyendo tildes, la letra "ñ", y emojis. **Siempre** debes usar `UTF-8`.
    
- **Etiqueta Vacía:** Es una etiqueta **vacía** (no tiene cierre).
    

##### b) Título de la Página: `<title>`

- **Función:** Define el título que aparece en la **pestaña** del navegador, en los **marcadores/favoritos**, y como **encabezado en los resultados de búsqueda (SEO)**.
    
- **Importancia:** Es uno de los elementos de SEO más importantes. Debe ser conciso, descriptivo y único.
    

##### c) Configuración de Vista (Viewport): `<meta name="viewport" content="width=device-width, initial-scale=1.0">`

Este es el meta-tag de la **responsividad**. Aunque es técnico, es vital en la web actual.

- **`width=device-width`:** Le dice al navegador que el ancho del contenido debe ser igual al ancho de la pantalla del dispositivo.
    
- **`initial-scale=1.0`:** Establece el nivel de zoom inicial a 1, asegurando que no haya zoom por defecto.
    

> **Nota:** Sin el viewport tag, los navegadores móviles intentarán renderizar la página como si fuera un escritorio, haciendo que se vea diminuta e inutilizable. **Siempre inclúyelo.**

---

### 4. El Cuerpo del Documento: `<body>`

El elemento `<body>` es el **contenedor de todo el contenido visible** que el usuario verá en la ventana del navegador.

- **Contenido:** Incluye texto, imágenes, enlaces, listas, formularios, tablas, videos, etc.
    
- **Estructura:** Todas las etiquetas de contenido que aprenderemos en las siguientes clases (como `<h1>`, `<p>`, `<img>`, etc.) deben ir dentro del `<body>`.
    

```
<body>
    <h1>Este es un título visible</h1>
    <p>Este es un párrafo de texto.</p>
</body>
```

---

### Práctica recomendada

**Objetivo:** Crear una plantilla HTML5 completa y validar sus elementos esenciales.

1. **Crea un Nuevo Archivo:** En tu carpeta `CursoHTML5`, crea un archivo llamado `plantilla.html`.
    
2. **Escribe la Estructura:** Copia el código base y añade un comentario dentro del `<body>` que describa brevemente el contenido:
    
```
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0"> 
    <title>Mi Plantilla Maestra de HTML5</title>
</head>
<body>
    -->
    <h1>Estructura Lista</h1>
    <p>Hemos definido el DOCTYPE, el lenguaje, el charset y el viewport. ¡Cimientos sólidos!</p>
</body>
</html>
```

3. **Inspecciona:** Abre `plantilla.html` en tu navegador. Haz clic derecho y selecciona **"Inspeccionar"** (o "Herramientas de Desarrollador"). Navega a la pestaña de "Elementos". Observa cómo el navegador reconoce la estructura `<html>`, `<head>` y `<body>`.
    
4. **Cambia el Título:** Modifica el texto dentro de `<title>` y verifica cómo cambia el nombre de la pestaña.
    

---

### Cierre

Has aprendido la plantilla sagrada de HTML5. Recuerda: el `<head>` establece las reglas y la identidad del documento para las máquinas (navegadores, buscadores), mientras que el `<body>` contiene la experiencia para el usuario. Nunca olvides el **`<!DOCTYPE html>`** y el **`viewport`**.

---

## Resumen de puntos claves

| Elemento                     | Propósito                                         | Regla de Oro                                                    |
| ---------------------------- | ------------------------------------------------- | --------------------------------------------------------------- |
| **`<!DOCTYPE html>`**        | Indica el uso del estándar HTML5.                 | Debe ser la primera línea; asegura el Modo Estándar.            |
| **`<html lang="es">`**       | Contenedor raíz; define el idioma.                | **Siempre** incluye el atributo `lang` por accesibilidad y SEO. |
| **`<head>`**                 | Contiene metadatos (información NO visible).      | Es el "cerebro" del documento.                                  |
| **`<meta charset="UTF-8">`** | Define la codificación de caracteres.             | **Siempre** usa `UTF-8` para compatibilidad global.             |
| **`<title>`**                | Título de la pestaña y del resultado de búsqueda. | Debe ser único y descriptivo.                                   |
| **`viewport`**               | Asegura que la página sea responsive.             | **Obligatorio** para el desarrollo web móvil-primero.           |
| **`<body>`**                 | Contiene todo el contenido **visible**.           | Todas las etiquetas de contenido (texto, imágenes) van aquí.    |

**👉 Estamos listos para empezar a llenar ese cuerpo. Vamos a la siguiente clase:** [[03 - Etiquetas de texto y formato]]