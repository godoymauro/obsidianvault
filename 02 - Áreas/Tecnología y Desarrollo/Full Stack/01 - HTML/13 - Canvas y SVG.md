¡Perfecto! Hemos explorado las funcionalidades avanzadas con las APIs de HTML5. Ahora, vamos a la parte más visual y creativa: la generación de gráficos.

---

Aquí tienes los apuntes de la clase anterior: [[12 - APIs de HTML5]]

## Clase 13 - Canvas y SVG (dibujos y gráficos) 🎨

### Introducción

Esta clase te enseñará a manejar dos poderosas tecnologías de HTML5 para crear gráficos en el navegador: **`<canvas>`** y **SVG (Scalable Vector Graphics)**. Aunque ambos permiten dibujar, son fundamentalmente diferentes en su naturaleza y aplicación: **`<canvas>`** es para gráficos **rasterizados (de píxeles)** controlados por JavaScript, y **SVG** es para gráficos **vectoriales** basados en XML/HTML. Elegir el correcto para tu proyecto es clave para la optimización y la calidad de la imagen.

---

### 1. El Elemento `<canvas>`: El Lienzo de Píxeles

El elemento `<canvas>` es una caja rectangular en tu documento HTML que actúa como un lienzo. Para dibujar en él, necesitas utilizar una **API de JavaScript**. Piensa en esto como pintar sobre un lienzo real: una vez que colocas pintura (píxeles), la pintura ya no es un objeto independiente.

#### a) Sintaxis y Preparación

La etiqueta HTML solo define el espacio. Los atributos de ancho y alto son cruciales, ya que definen el número de píxeles del lienzo.

```
<canvas id="miLienzo" width="400" height="200" style="border:1px solid #000;">
    Tu navegador no soporta Canvas. </canvas>
```

#### b) Dibujando con JavaScript (Contexto 2D)

Para empezar a dibujar, debes obtener el **contexto** de renderizado del lienzo, generalmente el contexto 2D. Este objeto contiene todos los métodos de dibujo.

```
const canvas = document.getElementById('miLienzo');
// Obtenemos el contexto 2D, el objeto que nos permite dibujar
const ctx = canvas.getContext('2d');

// 1. Dibujar un rectángulo simple
ctx.fillStyle = 'red'; // Establece el color de relleno
ctx.fillRect(20, 20, 100, 150); // Coordenadas (x, y) y dimensiones

// 2. Dibujar un círculo (ruta)
ctx.strokeStyle = 'blue';
ctx.lineWidth = 5;
ctx.beginPath(); // Inicia una nueva ruta
ctx.arc(200, 100, 50, 0, 2 * Math.PI); // centro(x, y), radio, ángulos
ctx.stroke(); // Dibuja la línea
```

#### c) Naturaleza de `<canvas>` (Gráficos _Raster_)

- **Dependiente de la Resolución:** Los gráficos de Canvas son píxeles. Si el usuario hace zoom, los gráficos se **pixelan** (se ven borrosos).
    
- **Ventaja:** Ideal para manipulación de imágenes a nivel de píxel (filtros), animaciones complejas con muchos objetos, y visualizaciones de datos que cambian constantemente (como gráficos en tiempo real).
    
- **Desventaja:** El contenido dibujado no forma parte del DOM, lo que limita la accesibilidad y el SEO (a menos que añadas descripciones con ARIA).
    

---

### 2. SVG: Gráficos Vectoriales Escalares

**SVG (Scalable Vector Graphics)** es un formato de imagen basado en XML que describe gráficos bidimensionales mediante fórmulas matemáticas. Cuando incrustas SVG en HTML, cada forma se convierte en un **elemento DOM** real.

#### a) Naturaleza de SVG (Gráficos _Vectoriales_)

- **Independiente de la Resolución:** Los SVG **nunca se pixelan**. El navegador recalcula la fórmula matemática para renderizar la forma a cualquier tamaño, garantizando una calidad perfecta.
    
- **Control:** El gráfico se define usando **etiquetas** dentro del contenedor `<svg>` (`<circle>`, `<rect>`, `<path>`). Puedes estilizarlos con CSS y manipularlos fácilmente con JavaScript.
    

#### b) Sintaxis y Estructura

```
<svg width="300" height="150" viewBox="0 0 300 150" xmlns="http://www.w3.org/2000/svg">
    
    <circle cx="75" cy="75" r="50" fill="yellow" stroke="orange" stroke-width="5" />
    
    <rect x="150" y="50" width="100" height="50" fill="purple" />

    </svg>
```

- **`viewBox`**: Define un sistema de coordenadas interno para que el gráfico pueda escalarse de forma predecible sin distorsionarse.
    

#### c) Accesibilidad y SVG

Esta es una de las grandes ventajas de SVG. Al ser elementos DOM, podemos aplicar la semántica y los atributos ARIA que aprendimos en [[10 - Accesibilidad en HTML5]].

```
<svg role="img" aria-labelledby="titulo-icono">
    <title id="titulo-icono">Icono de configuración</title>
    </svg>
```

---

### 3. Comparativa: Canvas vs. SVG

La decisión se reduce al rendimiento y a la naturaleza del gráfico.

|Característica|`<canvas>`|SVG|
|---|---|---|
|**Tipo de Gráfico**|Rasterizado (mapa de bits/píxeles)|Vectorial (XML/Fórmulas)|
|**Escalabilidad**|Baja (se pixela con zoom)|**Perfecta** (resolución-independiente)|
|**Interacción**|Difícil (debes calcular la posición del ratón en el contexto).|Fácil (cada forma es un elemento DOM con eventos).|
|**Mejor para...**|Juegos, gráficos que se actualizan rápido, manipulación de fotos (filtros).|Iconos, logos, diagramas, ilustraciones, gráficos de datos estáticos.|
|**Accesibilidad**|Limitada (caja negra).|**Excelente** (cada forma es un elemento indexable).|

---

### Práctica recomendada

**Objetivo:** Implementar ambos tipos de gráficos y usar las herramientas de desarrollo para analizar sus diferencias.

1. Crea un archivo llamado `canvas_svg.html`.
    
2. Implementa la siguiente estructura en el `<body>`:
    

```
<body>
    <h1>Demostración de Canvas y SVG</h1>

    <h2>Lienzo de Píxeles (Canvas)</h2>
    <canvas id="lienzoPractica" width="300" height="150" style="border: 1px solid #000;">Gráfico dibujado por JavaScript.</canvas>
    
    <script>
        const canvas = document.getElementById('lienzoPractica');
        const ctx = canvas.getContext('2d');
        
        // Dibujar un texto simple en Canvas
        ctx.font = '24px sans-serif';
        ctx.fillStyle = 'black';
        ctx.fillText('Texto en Canvas', 50, 80);
    </script>

    <hr>

    <h2>Gráfico Vectorial (SVG)</h2>
    <svg width="300" height="150" viewBox="0 0 300 150">
        <text x="50" y="80" font-family="sans-serif" font-size="24" fill="darkgreen">Texto en SVG</text>
        <polygon points="150,10 100,140 200,140" fill="lightblue" stroke="blue" stroke-width="2" />
    </svg>
</body>
```

3. Visualiza el archivo. Luego, usa **Inspeccionar Elemento** en tu navegador:
    
    - **En el Canvas:** Haz clic en el texto "Texto en Canvas". Verás que el inspector solo selecciona la etiqueta `<canvas>`. El texto dibujado no es un elemento.
        
    - **En el SVG:** Haz clic en el texto "Texto en SVG" o en el triángulo. Verás que el inspector selecciona la etiqueta `<text>` o `<polygon>`, demostrando que **cada parte es un elemento DOM**.
        

---

### Cierre

Has aprendido que la elección entre Canvas y SVG depende de la necesidad: si necesitas manipulación intensa de píxeles o alta velocidad de animación, usa **`<canvas>`**. Si priorizas la **escalabilidad, la accesibilidad y la manipulación con CSS**, usa **SVG**.

---

## Resumen de puntos claves

|Concepto|`<canvas>`|SVG|
|---|---|---|
|**Naturaleza**|Basado en píxeles (Rasterizado).|Basado en XML (Vectorial).|
|**Control**|API de JavaScript (`ctx.fillRect`).|Hojas de marcado (elementos `<circle>`, `<rect>`).|
|**Escalabilidad**|Se pixela.|**Perfecta** (ideal para logos e iconos).|
|**Estructura**|Un solo elemento DOM.|**Cada forma es un elemento DOM**.|
|**Accesibilidad**|Baja (requiere texto de respaldo).|Alta (cada forma puede tener atributos ARIA).|

**👉 Hemos dominado los gráficos. Ahora, volvamos a la multimedia para ver sus capacidades más avanzadas, especialmente el manejo de subtítulos y _streaming_. Vamos a la siguiente clase:** [[14 - Audio y video avanzado]]