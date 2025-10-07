Hemos visto el modelo BEM para estructurar el CSS. Ahora, exploraremos otras filosofías de organización más amplias que buscan resolver el problema del CSS a gran escala, incluyendo el enfoque _Utility-first_ que ya mencionamos.

Aquí tienes la vigesimotercera clase.

---

Aquí tienes los apuntes de la clase anterior: [[22 - Mantenimiento y organización de CSS]]

# 23 - Metodologías Modernas

## 📌 Introducción

Las **Metodologías CSS** son conjuntos de reglas y patrones que guían la escritura y la organización del código. Más que solo convenciones de nombres (como BEM), estas metodologías buscan estandarizar la arquitectura completa de la hoja de estilos, haciendo que el código sea predecible, reutilizable y fácil de escalar.

Si bien no necesitas aplicarlas todas, conocer estas arquitecturas te permitirá entender cómo se diseñan y mantienen las bases de código CSS de las grandes empresas.

---

## 🏗️ OOCSS (Object-Oriented CSS)

**OOCSS** (CSS Orientado a Objetos) fue una de las primeras metodologías en buscar la escalabilidad. Su idea central es aplicar los principios de la programación orientada a objetos al CSS, promoviendo la reutilización del código.

Se basa en dos principios clave:

### 1. Separación de Estructura y Apariencia

- **Estructura:** Propiedades que definen la geometría (ej: `width`, `height`, `margin`, `padding`).
    
- **Apariencia:** Propiedades que definen el estilo visual (ej: `background-color`, `border`, `color`).
    

La idea es crear una clase para la estructura y otra para la apariencia.

**Ejemplo:** En lugar de crear `.tarjeta-azul` y `.tarjeta-roja`, creas:

CSS

```
.media-object { /* Estructura: usada para cualquier tarjeta */
    width: 300px;
    padding: 15px;
    border: 1px solid #ccc;
}

.theme-blue { /* Apariencia: puede usarse en cualquier objeto */
    background-color: blue;
    color: white;
}
.theme-red { /* Apariencia: puede usarse en cualquier objeto */
    background-color: red;
    color: white;
}
```

**HTML:** `<div class="media-object theme-blue">...</div>`

### 2. Separación de Contenedor y Contenido

Los estilos de un elemento deben ser independientes de dónde se usan. Los estilos **nunca** deben depender de selectores descendientes anidados (ej: `.sidebar h3 { ... }`).

- **Mal (Depende del contenedor):** `.sidebar h3 { color: blue; }`
    
- **Bien (Independiente):** `<h3 class="header--blue">` con `.header--blue { color: blue; }`
    

---

## ⚛️ Atomic CSS (ACSS)

**Atomic CSS** o **Functional CSS** lleva al extremo el principio de OOCSS de la reutilización.

- **Principio:** Cada clase CSS debe tener una **única, atómica y explícita declaración**. La apariencia de un elemento se construye puramente a partir de una larga lista de clases utilitarias en el HTML.
    
- **Ejemplo:** En lugar de `.button-primary { background: blue; padding: 10px; }`, se usa:
    
    HTML
    
    ```
    <button class="bg-blue padding-medium">
    ```
    

### 1. Tailwind CSS (La Implementación más Famosa)

**Tailwind CSS** es el _framework_ que popularizó esta metodología (visto en [[15 - Frameworks CSS y librerías]]).

|Clase Tailwind|Declaración CSS Implícita|
|---|---|
|`flex`|`display: flex;`|
|`items-center`|`align-items: center;`|
|`justify-end`|`justify-content: flex-end;`|
|`text-lg`|`font-size: 1.125rem;`|
|`mt-4`|`margin-top: 1rem;`|
|`md:w-1/2`|`@media (min-width: 768px) { width: 50%; }`|

Exportar a Hojas de cálculo

**Ventajas:** Personalización extrema, eliminación de CSS sin usar (purga), velocidad de desarrollo una vez dominado.

**Desventajas:** HTML muy verboso (lleno de clases), curva de aprendizaje.

---

## 💅 CSS-in-JS (La Alternativa JavaScript)

**CSS-in-JS** es una metodología donde los estilos se escriben directamente en archivos JavaScript o TypeScript, generalmente usando plantillas _template literals_ (cadenas de texto multilinea).

- **Propósito:** Elimina la necesidad de archivos CSS separados, ya que los estilos se vinculan directamente al componente de la interfaz de usuario.
    
- **Librerías Populares:** Styled Components, Emotion, JSS.
    

**Ejemplo (Styled Components):**

JavaScript

```
// En tu archivo componente.js
import styled from 'styled-components';

const BotonPrimario = styled.button`
    background: ${props => props.primary ? 'blue' : 'gray'};
    color: white;
    padding: 10px 20px;
    border-radius: 5px;
    /* Los estilos se calculan aquí y se inyectan automáticamente en el <style> del HTML */
`;

// Uso en React/Vue: <BotonPrimario primary={true}>Enviar</BotonPrimario>
```

**Ventajas:**

1. **Aislamiento de Estilos:** Los estilos están encapsulados en el componente y no pueden "filtrarse" y afectar a otros elementos.
    
2. **CSS Dinámico:** Uso de lógica JavaScript para crear estilos dinámicos (variables, propiedades condicionales) sin tocar `@keyframes` o variables CSS puras.
    
3. **Eliminación de Clases Muertas:** Cuando eliminas un componente, sus estilos asociados desaparecen automáticamente.
    

**Desventajas:**

1. **Dependencia de JavaScript:** Requiere JavaScript para que el estilo se cargue.
    
2. **Curva de Aprendizaje:** Es un paradigma de programación diferente al CSS tradicional.
    

---

## ✅ Resumen de Puntos Clave

- **OOCSS** promueve la reutilización mediante la separación de **estructura** (`.media-object`) y **apariencia** (`.theme-blue`).
    
- **Atomic CSS** (ej: **Tailwind CSS**) basa el estilo en el uso de una clase utilitaria para cada declaración CSS (ej: `text-center`, `mt-4`). Esto resulta en HTML muy verboso pero CSS final muy ligero.
    
- **CSS-in-JS** (ej: Styled Components) escribe el CSS directamente dentro de archivos JavaScript, proporcionando un aislamiento de estilos perfecto y la capacidad de usar lógica de programación para definir el diseño.
    
- La metodología elegida debe basarse en el tamaño del proyecto, el _stack_ tecnológico (JavaScript Framework) y las prioridades de **mantenibilidad** vs. **personalización**.
    

👉 Hemos terminado con la organización pura de CSS. Ahora, vamos a la interconexión entre CSS y JavaScript, que es necesaria en la mayoría de las metodologías modernas. Continúa con [[24 - Integración de CSS con JavaScript]].