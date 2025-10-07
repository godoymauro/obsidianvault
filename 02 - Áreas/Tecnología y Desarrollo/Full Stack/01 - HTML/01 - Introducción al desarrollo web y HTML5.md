## Clase 01 - Introducción al desarrollo web y HTML5 🌐

### Introducción

¡Bienvenido a tu primera clase! Hoy vamos a desmitificar el desarrollo web y entender el papel fundamental que juega **HTML5**. Aprenderás que HTML no es un lenguaje de programación, sino el **lenguaje de marcado** esencial que da estructura y significado (semántica) a cualquier contenido en Internet. Es la base, el esqueleto sobre el cual se construyen los diseños con CSS y las interacciones con JavaScript. Dominar HTML5 significa construir cimientos web sólidos, modernos y, lo más importante, accesibles.

Aquí encontraras todo el indice del curso en el mapa de contexto: [[01 - HTML/00 - MOC]]

---

### ¿Qué es HTML y por qué HTML5?

#### 1. Definición de HTML

**HTML** significa **HyperText Markup Language** (Lenguaje de Marcado de Hipertexto).

- **Lenguaje de Marcado:** En lugar de decirle a una computadora _qué hacer_ (programación), le decimos _qué es_ un contenido (un título, un párrafo, una lista, un enlace). HTML utiliza **etiquetas** (`<>`) para marcar y describir la estructura del documento.
    
- **Hipertexto:** Se refiere a la capacidad de enlazar documentos entre sí, la función esencial de la World Wide Web.
    

#### 2. La Evolución a HTML5

HTML5 es la quinta revisión principal del estándar HTML (oficializada por el W3C). No es solo una actualización de etiquetas, sino una **plataforma completa** que incluye nuevas funcionalidades y APIs para trabajar directamente en el navegador.

|Característica|HTML (versión anterior: 4.01)|HTML5 (Actual)|
|---|---|---|
|**Declaración**|Larga y compleja (`<!DOCTYPE HTML PUBLIC...`)|Súper simple: `<!DOCTYPE html>`|
|**Semántica**|Uso excesivo de `<div>` con clases (ej: `<div class="header">`)|Introducción de etiquetas semánticas (ej: `<header>`, `<nav>`)|
|**Multimedia**|Requería _plugins_ de terceros (Flash, Silverlight)|Soporte nativo con etiquetas `<audio>`, `<video>` y `<canvas>`.|
|**Formularios**|Tipos de _input_ básicos (texto, contraseña)|Nuevos tipos (date, email, url, number, range) y validación nativa.|
|**APIs**|Muy limitado.|Incluye APIs potentes (Geolocalización, Web Storage, Web Workers).|

> **Nota:** La razón principal de la migración a HTML5 fue la necesidad de construir webs **más semánticas** (con significado claro para máquinas y personas) y **libres de _plugins_** para la multimedia.

---

### Anatomía de una Etiqueta HTML

Toda la estructura de un documento HTML se basa en **elementos**, compuestos por etiquetas.

#### 1. Partes del Elemento

Un elemento típico tiene la siguiente estructura:

HTML

```
<etiqueta atributo="valor"> Contenido </etiqueta>
```

|Parte|Explicación|Ejemplo|
|---|---|---|
|**Etiqueta de apertura**|Indica dónde comienza el elemento.|`<h1>`|
|**Atributo**|Información adicional sobre el elemento (opcional).|`class="titulo-principal"`|
|**Valor**|El valor asignado al atributo.|`"titulo-principal"`|
|**Contenido**|Lo que ve el usuario (texto, otra etiqueta, etc.).|`Mi Web Semántica`|
|**Etiqueta de cierre**|Indica dónde termina el elemento.|`</h1>`|

#### 2. Etiquetas Vacías (Void Elements)

Algunas etiquetas no tienen contenido interno y se cierran a sí mismas. **No** requieren una etiqueta de cierre.

**Ejemplos:**

- `<img>` (para imágenes)
    
- `<br>` (para salto de línea)
    
- `<input>` (para campos de formulario)
    

```
<img src="logo.png" alt="Logotipo de la empresa"> 
```

---

### La Tríada del Desarrollo Web

HTML5 nunca trabaja solo. Forma un equipo indispensable con otros dos lenguajes.

1. **HTML (Estructura):** Define el **esqueleto** y el **significado** del contenido. _("Esto es un título, esto es un párrafo")_
    
2. **CSS (Presentación):** Define la **apariencia** y el **estilo**. _("El título será azul, la fuente grande, y el párrafo tendrá un margen")_
    
3. **JavaScript (Comportamiento):** Define la **interacción** y la **lógica**. _("Al hacer clic en este botón, se mostrará un menú o se validará un dato")_
    

**El enfoque moderno:** Como desarrolladores, debemos usar HTML **solo para estructura**, delegando la presentación a CSS y la lógica a JavaScript.

---

### Herramientas Esenciales

No necesitas un software costoso para empezar. Las herramientas clave son:

1. **Editor de Código:** Te ayuda a escribir código con resaltado de sintaxis y autocompletado.
    
    - **Recomendación:** **VS Code (Visual Studio Code)**. Es gratuito, potente y tiene extensiones geniales.
        
2. **Navegador Web:** Para visualizar tu trabajo e inspeccionar el código.
    
    - **Recomendación:** **Chrome, Firefox o Edge**, ya que tienen excelentes _Developer Tools_ (Herramientas de Desarrollador).
        
3. **Sistema de Archivos:** Para organizar tus archivos. ¡Aquí es donde brilla **Obsidian** para tus notas!
    

---

### Práctica recomendada

**Objetivo:** Configurar tu entorno de trabajo y crear tu primer archivo HTML.

1. **Configuración del Entorno:** Si aún no lo tienes, instala **VS Code** o tu editor preferido.
    
2. **Crear Carpeta:** Crea una carpeta en tu escritorio o documentos llamada `CursoHTML5`.
    
3. **Primer Archivo:** Dentro de esa carpeta, crea un archivo llamado `index.html`. La extensión `.html` es crucial.
    
4. **Escribir Código:** Abre el archivo `index.html` en tu editor de código e ingresa la siguiente estructura básica. **No te preocupes por entender todo aún**, solo cópialo.
    

```
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Mi Primer Documento HTML5</title>
</head>
<body>
    <h1>¡Hola, Universo HTML5!</h1>
    <p>Este es el primer paso de nuestro viaje. ¡Mi entorno está listo!</p>
</body>
</html>
```

5. **Visualizar:** Haz doble clic en el archivo `index.html` desde tu carpeta para abrirlo en el navegador. Deberías ver tu título y tu párrafo.
    

---

### Cierre

Has completado la primera clase. Entiendes que HTML5 es el lenguaje de marcado que estructura el contenido web, es más simple, más potente y se centra en la **semántica** para ser compatible con el ecosistema de la web moderna. Estás listo para empezar a construir.

---

## Resumen de puntos claves

| Concepto          | Descripción                                                        | Clave de Aprendizaje                                                |
| ----------------- | ------------------------------------------------------------------ | ------------------------------------------------------------------- |
| **HTML**          | Lenguaje de **marcado** para la estructura y el hipertexto.        | Es el esqueleto, no el músculo (JS) ni la piel (CSS).               |
| **HTML5**         | La versión moderna y estándar.                                     | Eliminó la necesidad de _plugins_ (Flash) e introdujo la semántica. |
| **Elemento HTML** | Compuesto por `<etiqueta atributo="valor"> Contenido </etiqueta>`. | Conocer la estructura es vital para escribir código válido.         |
| **Tríada Web**    | HTML (Estructura) + CSS (Estilo) + JS (Comportamiento).            | Nunca mezcles responsabilidades; usa HTML solo para el significado. |

**👉 Vamos a la siguiente clase:** [[02 - Estructura básica de un documento HTML]]