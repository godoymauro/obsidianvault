¡Perfecto! Has dominado los selectores, el arte de apuntar. Ahora, abordaremos el desafío más común para los novatos en CSS: la **Cascada**. ¿Qué sucede cuando le dices a un elemento que sea azul, y otra regla le dice que sea rojo?

Aquí tienes la cuarta clase, el corazón del funcionamiento interno de CSS.

---

Aquí tienes los apuntes de la clase anterior: [[03 - Selectores en CSS]]

# 04 - Especificidad y Herencia

## 📌 Introducción

El acrónimo CSS significa **C**ascading **S**tyle **S**heets (Hojas de Estilo en **Cascada**). El término "Cascada" hace referencia a cómo los estilos fluyen y se aplican a los elementos HTML.

Cuando múltiples reglas CSS apuntan al mismo elemento y propiedad (ej: color), el navegador debe decidir cuál aplicar. Esta decisión se toma mediante dos mecanismos principales: la **Especificidad** y la **Herencia**. Entender esto es la clave para depurar por qué un estilo _no se aplica_ o por qué se aplica uno _inesperado_.

## 💧 El Flujo de la Cascada

La Cascada es un proceso de tres pasos que el navegador sigue para resolver conflictos de estilo:

1. **Origen y Orden del Código:** Los estilos que aparecen más tarde en el código (ya sean internos o externos) prevalecen sobre los que aparecieron antes.
    
2. **Especificidad:** Cuán específico es el selector. Un selector más específico anula a uno menos específico, **incluso si este último aparece después**.
    
3. **Importancia:** Las reglas marcadas como `!important` anulan cualquier otra regla de especificidad, aunque su uso es generalmente desaconsejado.
    

---

## ⚖️ Especificidad: El Sistema de Puntuación

La Especificidad es un cálculo que asigna un valor a cada selector. Es como un sistema de puntos de cuatro columnas (A, B, C, D) donde las columnas de la izquierda tienen un valor mucho mayor que las de la derecha.

|Columna|Tipo de Selectores|Descripción|
|---|---|---|
|**A**|**Estilos en Línea**|Siempre vale **1, 0, 0, 0**. Otorga 1000 puntos al selector.|
|**B**|**ID's** (`#`)|Suma 1 por cada selector de ID. (ej: 0, **1**, 0, 0).|
|**C**|**Clases** (`.`), **Pseudo-clases** (`:hover`), **Selectores de Atributos** (`[type="text"]`)|Suma 1 por cada uno. (ej: 0, 0, **1**, 0).|
|**D**|**Elementos** (`p`), **Pseudo-elementos** (`::before`)|Suma 1 por cada etiqueta o pseudo-elemento. (ej: 0, 0, 0, **1**).|

Exportar a Hojas de cálculo

> 💡 **Regla de Oro:** Los ID (columna B) son exponencialmente más importantes que las Clases (columna C). 1 ID (`1,0,0,0`) siempre gana a 1000 Clases juntas.

### Cálculo de Especificidad: Ejemplo

|Selector|Cálculo (A, B, C, D)|Puntuación|Resultado|
|---|---|---|---|
|`p`|(0, 0, 0, 1)|1|Bajo|
|`.card`|(0, 0, 1, 0)|10|Medio|
|`p.card`|(0, 0, 1, 1)|11|Medio|
|`#menu a`|(0, 1, 0, 1)|101|Alto|
|`style="..."`|(1, 0, 0, 0)|1000|Máximo|

Exportar a Hojas de cálculo

### El caso del `!important`

Si bien no se incluye en el cálculo formal, la palabra clave `!important` anula la especificidad y el orden.

CSS

```
p {
    color: red !important; /* ¡Gana! */
}

/* El siguiente estilo aparecerá DEBAJO, pero debido al !important, es ignorado. */
p#parrafo-unico {
    color: blue; 
} 
```

⚠️ **Advertencia:** Usa `!important` solo como último recurso (ej: para anular estilos de _frameworks_ de terceros). Su abuso destruye la cascada y hace el CSS imposible de mantener.

---

## 🌳 Herencia Natural

La Herencia es el mecanismo por el cual ciertas propiedades CSS, si no se definen explícitamente en un elemento, se transmiten **automáticamente** del elemento **padre** a sus elementos **hijo** y descendientes.

### Propiedades que Heredan

Generalmente, las propiedades relacionadas con el **texto** heredan, porque es lógico que si defines el color de fuente del cuerpo de la página, todos los textos dentro lo tengan.

|Heredan|No Heredan (Necesitan ser definidas)|
|---|---|
|`color` (color de texto)|`background-color` (fondo)|
|`font-family`, `font-size`, `font-weight` (tipografía)|`margin`, `padding`, `border` (modelo de caja)|
|`line-height`, `text-align`|`width`, `height` (dimensiones)|
|`cursor`|`display`, `position`|

Exportar a Hojas de cálculo

### Ejemplo de Herencia

Si aplicas la fuente al cuerpo (`body`), todos los elementos dentro del cuerpo la heredarán:

HTML

```
<body>
    <h1>Título</h1>
    <p>Párrafo de texto.</p>
</body>
```

CSS

```
/* El cuerpo define la fuente y el color */
body {
    font-family: Arial, sans-serif;
    color: gray;
}

/* No necesitamos definir la fuente o el color aquí */
h1 {
    font-weight: bold; /* Pero sí podemos anular lo que queramos */
}
```

En este caso, tanto el h1 como el p tendrán la fuente `Arial` y el color `gray`, a menos que se les aplique una regla más específica.

### Controlando la Herencia con Propiedades Específicas

CSS proporciona valores especiales para controlar explícitamente la herencia:

|Valor|Descripción|
|---|---|
|`inherit`|Obliga a la propiedad a tomar el valor de su elemento padre.|
|`initial`|Establece el valor de la propiedad a su valor predeterminado por el navegador.|
|`unset`|Restablece el valor. Si la propiedad hereda, actúa como `inherit`; si no hereda, actúa como `initial`.|

Exportar a Hojas de cálculo

---

## ✅ Resumen de Puntos Clave

- La **Cascada** es el proceso por el cual el navegador decide qué estilos aplicar cuando hay conflictos.
    
- La **Especificidad** es un sistema de puntuación para selectores. Los estilos en línea tienen la puntuación más alta, seguidos por los ID's, Clases/Pseudo-clases, y finalmente los Elementos.
    
- La **Regla del Empate:** Si las puntuaciones de Especificidad son iguales, gana la regla que está **última** en la hoja de estilos.
    
- La **Herencia** permite que propiedades como el color y la tipografía se pasen automáticamente del padre a los hijos.
    
- Las propiedades relacionadas con el Modelo de Caja (`margin`, `padding`, `border`) **no heredan**.
    

👉 Entender la Cascada a menudo implica lidiar con valores y unidades. Nuestro próximo paso es explorar las diferentes formas en que le decimos a CSS cuán grande, largo o de qué color queremos que sean los elementos. Continúa con [[05 - Colores y unidades en CSS]].