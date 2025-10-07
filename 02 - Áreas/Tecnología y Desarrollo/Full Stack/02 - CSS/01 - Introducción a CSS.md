
---

Aquí encontraras todo el indice del curso en el mapa de contexto: [[02 - CSS/00 - MOC]]

# 01 - Introducción a CSS

## 📌 Introducción

¡Bienvenido a CSS! Si ya sabes HTML, tienes la estructura. Ahora, vamos a darle **belleza, estilo y orden**. CSS, que significa **Cascading Style Sheets** (Hojas de Estilo en Cascada), es el lenguaje que utilizamos para describir la presentación de un documento escrito en un lenguaje de marcado como **HTML**.

Piensa en HTML como el esqueleto (la estructura y el contenido) y CSS como la piel, la ropa y el maquillaje (el diseño, los colores, las fuentes, la disposición).

## 🌍 ¿Qué es CSS?

CSS es un lenguaje de **diseño gráfico y maquetación**. Su propósito es separar el contenido de un documento (HTML) de su presentación visual. Esta separación ofrece grandes ventajas:

1. **Mantenimiento Sencillo:** Puedes cambiar el diseño de un sitio web completo modificando un solo archivo CSS.
    
2. **Consistencia:** Asegura que todos los elementos de un tipo (ej: todos los títulos h1) se vean iguales en todo el sitio.
    
3. **Accesibilidad:** Permite que diferentes usuarios accedan al contenido con diferentes estilos (ej: versiones de alto contraste).
    
4. **Menor Tamaño de Archivo:** El HTML se vuelve más limpio y pequeño, lo que acelera el tiempo de carga.
    

## 🕰 Breve Historia

CSS fue propuesto por primera vez en 1994 y se convirtió en una recomendación oficial del W3C (World Wide Web Consortium) en **1996** con CSS Nivel 1. La versión que usamos hoy en día, CSS3, no es realmente una única especificación, sino una colección de módulos. Esto ha permitido que el lenguaje evolucione más rápido, añadiendo características como Flexbox, Grid Layout y Media Queries, fundamentales para el desarrollo moderno.

## 🤝 Relación con HTML

CSS y HTML son inseparables en el desarrollo web moderno:

| HTML                                                                                               | CSS                                                                                                                 |
| -------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| Se enfoca en el **Contenido** y la **Estructura** (ej: _Esto es un párrafo_, _Esto es un título_). | Se enfoca en la **Presentación** y el **Diseño** (ej: _El párrafo debe ser azul_, _El título debe estar centrado_). |
| Usa **Etiquetas** (```<p>, <h1>, <div>)```.                                                        | Usa **Reglas** que apuntan a esas etiquetas (selectores).                                                           |

Exportar a Hojas de cálculo

---

## 🔗 Tres Formas de Aplicar CSS al HTML

Hay tres maneras principales de enlazar el estilo CSS con el documento HTML. La elección correcta es crucial para la organización y el mantenimiento.

### 1. CSS Externo (Recomendado)

Es la forma más profesional y recomendada. Se crea un archivo separado con la extensión `.css` (ej: `styles.css`) que contiene todas las reglas de estilo.

**Ventajas:**

- Separa completamente la estructura del diseño.
    
- Permite aplicar el mismo estilo a **múltiples páginas** HTML.
    
- Mejora la velocidad de carga (el navegador almacena en caché el archivo CSS).
    

**Implementación (dentro de la etiqueta `<head>` del HTML):**

HTML

```
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Mi Primera Web con CSS</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <h1>Hola Mundo</h1>
</body>
</html>
```

CSS

```
/* En tu archivo styles.css */
h1 {
    color: blue;
}
```

### 2. CSS Interno (o Incrustado)

Los estilos se definen dentro de la etiqueta **`<style>`** en la sección `<head>` del documento HTML.

**Ventajas:**

- Útil para estilos específicos de una sola página.
    
- No requiere un archivo externo.
    

**Desventajas:**

- Solo afecta a la página en la que está definido.
    
- Mezcla la estructura (HTML) con el diseño (CSS).
    

**Implementación:**

HTML

```
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Estilo Interno</title>
    <style>
        h1 {
            color: purple;
            text-align: center;
        }
    </style>
</head>
<body>
    <h1>Título con Estilo Interno</h1>
</body>
</html>
```

### 3. CSS en Línea (Inline)

Los estilos se aplican directamente a un elemento HTML específico usando el atributo **`style`**.

**Ventajas:**

- Se utiliza para anular rápidamente estilos de forma puntual.
    
- Tiene la **máxima prioridad** (lo veremos en [[04 - Especificidad y herencia]]).
    

**Desventajas (¡No recomendado para diseño general!):**

- No se puede reutilizar.
    
- Hace que el código HTML sea muy desordenado y difícil de mantener.
    

**Implementación:**

HTML

```
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Estilo en Línea</title>
</head>
<body>
    <p style="color: red; font-size: 20px;">Este texto es rojo y grande.</p>
</body>
</html>
```

---

## ✅ Resumen de Puntos Clave

- **CSS** significa _Cascading Style Sheets_ y controla la **presentación** y el **diseño** del contenido HTML.
    
- HTML es la estructura, CSS es la apariencia.
    
- La forma recomendada de aplicar CSS es el **CSS Externo** (`<link rel="stylesheet" href="...">`), ya que separa código y diseño.
    
- El **CSS Interno** (etiqueta `<style>`) es para estilos a nivel de página.
    
- El **CSS en Línea** (atributo `style`) se usa solo para excepciones puntuales y tiene la mayor prioridad.
    

👉 A continuación, aprenderemos cómo se escriben realmente esas reglas que hemos visto. Continúa con [[02 - Anatomía de una regla CSS]].