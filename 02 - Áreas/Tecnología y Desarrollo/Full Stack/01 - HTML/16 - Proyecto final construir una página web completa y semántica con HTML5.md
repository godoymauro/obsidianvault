¡El gran momento ha llegado! Has recorrido todas las fases de nuestro curso, desde la estructura básica hasta el SEO y las APIs avanzadas. La **Clase 16** no es una clase teórica, sino la culminación de todo lo que has aprendido: tu **Proyecto Final**.

---

Aquí tienes los apuntes de la clase anterior: [[15 - Microdatos y SEO en HTML5]]

## Clase 16 - Proyecto final: construir una página web completa y semántica con HTML5 🏆

### Introducción

Esta clase final es tu examen práctico. El objetivo es construir una **página web completa, profesional, semántica y accesible**, aplicando los conocimientos de las 15 clases anteriores. El proyecto será una **"Página de Portafolio de Desarrollador"** que te servirá no solo para practicar, sino como tu carta de presentación profesional. No utilizaremos CSS ni JavaScript (excepto donde las APIs de HTML5 lo requieran, como en Web Storage), centrándonos puramente en la perfección del HTML5.

---

### 1. Requisitos del Proyecto

Tu proyecto debe ser un único archivo `portafolio.html` que demuestre el uso **semántico y funcional** de las siguientes áreas:

1. **Estructura Semántica (Clase 08):** Uso correcto de `<header>`, `<nav>`, `<main>`, `<section>`, `<article>`, y `<footer>`.
    
2. **Contenido y Formato (Clase 03, 06):** Uso de jerarquía `<h1>` a `<h6>`, listas ordenadas (`<ol>`) y no ordenadas (`<ul>`), y la etiqueta de citas (`<blockquote>`).
    
3. **Navegación y Multimedia (Clase 04, 14):**
    
    - Enlaces absolutos y relativos.
        
    - Enlaces de fragmento (`#`) para la navegación interna.
        
    - Uso de `<img>` con el atributo **`alt`** descriptivo.
        
    - Inclusión simulada de `<video>` con el elemento **`<track>`** (subtítulos).
        
4. **Interacción (Clase 07):** Un formulario de contacto simple con los atributos de validación nativa (`required`, `type="email"`).
    
5. **Calidad y Accesibilidad (Clase 10, 11):**
    
    - Uso de atributos **`aria-label`** en la navegación.
        
    - Uso de **`tabindex="0"`** en un elemento no nativo (si lo incluyes).
        
    - Verificación de la estructura de anidamiento.
        
6. **Especialización (Clase 12, 15):**
    
    - Uso de un elemento con atributo **`data-*`** para un proyecto.
        
    - Marcado de al menos una `<section>` con Microdatos de Schema.org (ej. `itemtype="https://schema.org/Person"`).
        

---

### 2. Estructura del Documento Final

Aquí tienes el esquema que debes seguir para tu `portafolio.html`.

```
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Portafolio de [Tu Nombre] - Desarrollador HTML5</title>
</head>
<body>
    <header>
        <nav aria-label="Navegación principal del portafolio">
            <ul>
                <li><a href="#perfil">Mi Perfil</a></li>
                <li><a href="#proyectos">Proyectos</a></li>
                <li><a href="#contacto">Contacto</a></li>
            </ul>
        </nav>
    </header>
    
    <main>
        
        <section id="perfil" itemscope itemtype="https://schema.org/Person">
            <h1 itemprop="name">Tu Nombre Completo</h1>
            <p>Puesto: <span itemprop="jobTitle">Especialista en HTML5 Semántico y Accesibilidad</span></p>
            <p>Contacto: <a href="mailto:tu@correo.com" itemprop="email">tu@correo.com</a></p>
            
            <h2>Mi Filosofía</h2>
            <blockquote>
                "Construir la web de manera accesible y orientada al futuro es la única forma de desarrollar."
            </blockquote>
        </section>
        
        <section id="proyectos">
            <h2>Proyectos Destacados</h2>
            
            <article data-proyecto-id="001" data-estado="finalizado">
                <h3>Proyecto Semántico - [Nombre del Proyecto]</h3>
                <p>Descripción del proyecto y tecnologías usadas.</p>
            </article>

            <article>
                <h3>Demo de Video Accesible</h3>
                <video controls width="400" poster="miniatura.jpg">
                    <source src="demo.mp4" type="video/mp4">
                    <track kind="captions" src="demo-es.vtt" srclang="es" label="Español (Captions)" default> 
                </video>
                <p>Este demo muestra cómo aplico la accesibilidad al video.</p>
            </article>
            
        </section>
        
        <section id="contacto">
            <h2>Contáctame</h2>
            <form action="/enviar-portafolio" method="POST">
                <p>
                    <label for="email">Correo:</label>
                    <input type="email" id="email" name="correo" required> </p>
                <p>
                    <label for="mensaje">Mensaje:</label>
                    <textarea id="mensaje" name="mensaje" required></textarea>
                </p>
                <button type="submit">Enviar Mensaje</button>
            </form>
        </section>
        
    </main>
    
    <footer>
        <p>&copy; 2025 - Portafolio Semántico | Desarrollado con puro HTML5.</p>
    </footer>
</body>
</html>
```

---

### 3. Pasos de Evaluación y Cierre

1. **Codificación:** Crea el archivo `portafolio.html` y rellena todos los campos.
    
2. **Revisión Final:** Repasa el código y asegúrate de que no haya `<div>`s donde deberías usar elementos semánticos (Regla de oro de la semántica).
    
3. **Validación W3C (Clase 11):** Copia tu código final y pégalo en el **W3C Markup Validation Service**. La meta es obtener el mensaje: **"Document checking complete. No errors or warnings to show."**
    
4. **Validación Semántica (Clase 15):** Usa la **Prueba de Resultados Enriquecidos de Google** para verificar que tu marcado `itemtype="https://schema.org/Person"` sea reconocido correctamente.
    

### Cierre de Curso

¡Felicidades! Al completar este proyecto, no solo has terminado el curso, sino que has demostrado tu dominio sobre **HTML5** en todos sus aspectos: **estructura, semántica, multimedia, interacción, accesibilidad y calidad profesional**. Estás listo para usar HTML5 de forma profesional en cualquier entorno de desarrollo moderno.

**¡Tu viaje en HTML5 ha sido un éxito!** Ahora puedes continuar tu aprendizaje con CSS y JavaScript, construyendo sobre la base semántica impecable que acabas de dominar.

Ir a los apuntes de CSS: 02 - [[01 - Introducción a CSS]]