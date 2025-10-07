¡Claro! Hemos cubierto el contenido principal (texto, enlaces, multimedia). Ahora, vamos a la **organización estructurada** de ese contenido, un paso fundamental para la usabilidad y la semántica.

---

Aquí tienes los apuntes de la clase anterior: [[05 - Imágenes y multimedia]]

## Clase 06 - Listas y tablas 📊

### Introducción

Las listas y las tablas son los elementos clave para organizar la información de manera legible y estructurada. Una **lista** es ideal para elementos secuenciales o no secuenciales (como un menú o una lista de compras), mientras que una **tabla** se utiliza para datos **bidimensionales** que requieren encabezados de columna y/o fila (como datos financieros o un horario). Usarlos incorrectamente (por ejemplo, usar tablas para diseño) es un error de semántica clásico que debemos evitar a toda costa.

---

### 1. Las Listas HTML

HTML5 nos proporciona tres tipos de listas, cada una con un propósito semántico distinto.

#### a) Lista No Ordenada (`<ul>`)

- **Significado Semántico:** Utilizada para una colección de elementos donde el **orden no es importante**.
    
- **Apariencia por defecto:** Se muestra con viñetas (_bullets_).
    
- **Estructura:**
    
    - `<ul>` (Unordered List): El contenedor principal.
        
    - `<li>` (List Item): Cada elemento individual de la lista.
        

```
<h2>Tareas Pendientes</h2>
<ul>
    <li>Comprar leche</li>
    <li>Estudiar HTML5</li>
    <li>Enviar correo al profesor</li>
</ul>
```

#### b) Lista Ordenada (`<ol>`)

- **Significado Semántico:** Utilizada para una colección de elementos donde el **orden es importante** (ej. pasos, rankings).
    
- **Apariencia por defecto:** Se muestra con números.
    
- **Atributos clave de `<ol>`:**
    
    - **`type`**: Cambia el estilo de numeración (ej. `a` para letras, `i` para números romanos).
        
    - **`start`**: Define el número con el que comienza la lista (ej. `start="5"`).
        
    - **`reversed`**: Invierte el orden de la numeración.
        

```
<h2>Pasos para Configurar tu Entorno</h2>
<ol start="3">
    <li>Abrir VS Code</li>
    <li>Crear un nuevo archivo .html</li>
    <li>Escribir el DOCTYPE</li>
</ol>
```

#### c) Lista de Descripción o Definición (`<dl>`)

- **Significado Semántico:** Utilizada para mostrar una lista de **términos** y sus **descripciones** (ej. glosarios, pares nombre-valor).
    
- **Estructura:**
    
    - `<dl>` (Description List): El contenedor principal.
        
    - `<dt>` (Description Term): El término o nombre que se describe.
        
    - `<dd>` (Description Description): La descripción o definición del término.
        

```
<dl>
    <dt>HTML5</dt>
    <dd>Lenguaje de marcado para estructurar contenido web.</dd>
    
    <dt>CSS3</dt>
    <dd>Hojas de estilo para dar presentación y diseño.</dd>
</dl>
```

> **Mejor Práctica (Menús de Navegación):** El elemento **`<ul>`** es el estándar de oro para construir menús de navegación (`<nav>`) [[08 - Elementos semánticos]], ya que la lista de enlaces no tiene un orden inherente que requiera números.

---

### 2. Tablas de Datos: El elemento `<table>`

El elemento `<table>` está diseñado **exclusivamente** para mostrar datos tabulares bidimensionales. **Nunca** debe usarse para maquetación o diseño.

#### Estructura Básica de una Tabla

|Etiqueta|Función|Significado Semántico|
|---|---|---|
|**`<table>`**|El contenedor de toda la tabla.|Define el inicio de datos tabulares.|
|**`<caption>`**|Título visible de la tabla.|Mejora la accesibilidad y el contexto.|
|**`<thead>`**|Contenedor para la fila de encabezados.|Indica la cabecera de la tabla (datos no repetitivos).|
|**`<tbody>`**|Contenedor para las filas de datos.|Contenido principal de la tabla.|
|**`<tr>`**|Table Row (Fila).|Define una nueva fila dentro de `<thead>` o `<tbody>`.|
|**`<th>`**|Table Header (Celda de Encabezado).|Etiqueta una celda como **encabezado**. (Importante para accesibilidad).|
|**`<td>`**|Table Data (Celda de Datos).|Una celda de datos estándar.|

#### Ejemplo de Tabla Semántica

```
<table>
    <caption>Ventas Trimestrales por Producto</caption>
    <thead>
        <tr>
            <th>Producto</th>
            <th>Q1</th>
            <th>Q2</th>
            <th>Q3</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Licencias Pro</td>
            <td>150</td>
            <td>180</td>
            <td>220</td>
        </tr>
        <tr>
            <td>Soporte Básico</td>
            <td>300</td>
            <td>320</td>
            <td>290</td>
        </tr>
    </tbody>
</table>
```

#### Fusión de Celdas (Spanning)

Podemos hacer que una celda se extienda por múltiples filas o columnas.

- **`colspan`**: Indica cuántas columnas debe abarcar una celda.
    
- **`rowspan`**: Indica cuántas filas debe abarcar una celda.
    

```
<table>
    <thead>
        <tr>
            <th colspan="2">Detalles Generales</th> <th>Ventas Totales</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="2">Región A</td> <td>Producto X</td>
            <td>500</td>
        </tr>
        <tr>
            <td>Producto Y</td>
            <td>300</td>
        </tr>
    </tbody>
</table>
```

> **Nota de Accesibilidad:** El uso correcto de `<th>` y los contenedores `<thead>` y `<tbody>` permite a los lectores de pantalla entender qué datos se relacionan con qué encabezados, mejorando drásticamente la experiencia para usuarios con discapacidades visuales.

---

### Práctica recomendada

**Objetivo:** Construir una lista multinivel y una tabla con encabezados y fusión de celdas.

1. Crea un nuevo archivo llamado `estructuras.html`.
    
2. Implementa la siguiente estructura dentro del `<body>`:
    

```
<body>
    <h1>Organización de Contenido</h1>

    <h2>Mi Receta Favorita (Lista Ordenada y Anidada)</h2>
    <ol>
        <li>Preparar la base</li>
        <li>Mezclar ingredientes secos
            <ul>
                <li>Harina</li>
                <li>Azúcar</li>
                <li>Polvo de hornear</li>
            </ul>
        </li>
        <li>Hornear por 45 minutos</li>
    </ol>
    
    <h2>Reporte de Proyecto (Tabla Compleja)</h2>
    <table>
        <caption>Progreso Semanal del Equipo</caption>
        <thead>
            <tr>
                <th rowspan="2">Semana</th>
                <th colspan="3">Tareas Completadas</th>
            </tr>
            <tr>
                <th>Frontend</th>
                <th>Backend</th>
                <th>Testing</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <th>Semana 1</th>
                <td>5</td>
                <td>3</td>
                <td>12</td>
            </tr>
            <tr>
                <th>Semana 2</th>
                <td>7</td>
                <td>5</td>
                <td>9</td>
            </tr>
        </tbody>
    </table>
</body>
```

3. Visualiza el archivo. Observa cómo la lista anidada utiliza viñetas por defecto (aunque esté dentro de una lista numerada) y cómo la tabla usa `rowspan` y `colspan` para una cabecera más informativa.
    

---

### Cierre

Dominar las listas y las tablas te da el poder de presentar datos complejos de manera clara. Recuerda la regla fundamental: **HTML para la estructura y semántica**, nunca para el diseño. `<ul>` y `<ol>` para colecciones, y `<table>` **solo** para datos tabulares.

---

## Resumen de puntos claves

| Elemento                  | Tipo                   | Función Principal                                              | Atributos Clave                          |
| ------------------------- | ---------------------- | -------------------------------------------------------------- | ---------------------------------------- |
| **`<ul>`**                | Lista No Ordenada      | Colecciones sin orden de importancia.                          | Ninguno esencial.                        |
| **`<ol>`**                | Lista Ordenada         | Colecciones donde el orden es crucial (pasos, rankings).       | **`type`**, **`start`**, **`reversed`**. |
| **`<dl>`**                | Lista de Definición    | Pares término-descripción (glosarios).                         | Contiene `<dt>` y `<dd>`.                |
| **`<table>`**             | Contenedor de Datos    | Mostrar datos bidimensionales estructurados.                   | N/A (usa subetiquetas).                  |
| **`<th>`**                | Celda de Encabezado    | Etiqueta las celdas como títulos (crucial para accesibilidad). | **`colspan`**, **`rowspan`**.            |
| **`<tbody>` / `<thead>`** | Contenedores de Partes | Estructura semántica de la tabla (Cuerpo y Cabecera).          | N/A.                                     |

**👉 Hemos aprendido a mostrar y organizar la información. El siguiente paso es la interacción: capturar datos del usuario. Vamos a la siguiente clase:** [[07 - Formularios en HTML5]]