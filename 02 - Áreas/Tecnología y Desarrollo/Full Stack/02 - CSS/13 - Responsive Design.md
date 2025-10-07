Con Flexbox y Grid, tenemos las herramientas de maquetación; ahora, necesitamos la estrategia para usarlas de manera efectiva en cualquier dispositivo. Esta clase es el corazón del desarrollo web moderno.

Aquí tienes la decimotercera clase.

---

Aquí tienes los apuntes de la clase anterior: [[12 - Grid Layout en CSS]]

# 13 - Responsive Design

## 📌 Introducción

El **Responsive Design** (Diseño Adaptable o Responsivo) es la práctica de crear sitios web cuyo diseño y _layout_ se ajustan fluidamente para funcionar bien en una variedad de dispositivos y tamaños de pantalla, desde teléfonos móviles pequeños hasta monitores de escritorio grandes. No se trata solo de encoger la página, sino de **reorganizar** y **re-priorizar** el contenido.

El Responsive Design se basa en tres pilares:

1. **Grillas Fluidas:** Usar unidades relativas (`%`, `fr`, `vw`) en lugar de unidades fijas (`px`).
    
2. **Imágenes Flexibles:** Asegurar que las imágenes no desborden sus contenedores.
    
3. **Media Queries:** La herramienta de CSS para aplicar estilos diferentes según las características del dispositivo.
    

## 📱 Mobile-First vs. Desktop-First

La filosofía que adoptes influye en cómo escribes tus Media Queries. Hoy en día, el enfoque más recomendado es **Mobile-First**.

### 1. Mobile-First (Recomendado)

- **Filosofía:** Comienzas diseñando y codificando para la **pantalla más pequeña** (móvil). Los estilos base de tu CSS están optimizados para el móvil.
    
- **Progresión:** Usas Media Queries para **añadir** estilos a medida que la pantalla se hace **más grande**.
    
- **Ventajas:** Mejora el rendimiento en móviles (los estilos pesados de escritorio se cargan condicionalmente) y te obliga a priorizar el contenido.
    
- **Media Query:** Se utiliza `min-width` (ancho mínimo).
    

### 2. Desktop-First

- **Filosofía:** Comienzas diseñando para la pantalla grande (escritorio).
    
- **Progresión:** Usas Media Queries para **sobrescribir** estilos a medida que la pantalla se hace **más pequeña**.
    
- **Media Query:** Se utiliza `max-width` (ancho máximo).
    

---

## 💻 El Viewport Meta Tag

Antes de escribir cualquier CSS responsivo, es **obligatorio** incluir la siguiente meta etiqueta en el `<head>` de tu documento HTML. Esta etiqueta le indica al navegador móvil que el ancho del _viewport_ (la ventana) debe ser igual al ancho del dispositivo.

HTML

```
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

- `width=device-width`: Establece el ancho del _viewport_ al ancho del dispositivo en píxeles.
    
- `initial-scale=1.0`: Establece el nivel de zoom inicial a 1, sin zoom por defecto.
    

---

## ❓ Media Queries (Consultas de Medios)

Las Media Queries son la sintaxis de CSS que te permite aplicar un bloque de estilos solo si se cumplen ciertas condiciones (la consulta).

@media [tipo de medio] and ([caracterıˊstica]:[valor]) {reglas de estilo}

### 1. Sintaxis Básica y Breakpoints

Los **Breakpoints** son los puntos de inflexión donde el diseño necesita ajustarse (ej: 600px, 768px, 1200px).

#### A. Mobile-First (Usando `min-width`)

Recomendado. Los estilos base son para menos de 768px.

CSS

```
/* 1. Estilos Base (Aplicables a TODO, pero optimizados para móvil) */
.contenedor {
    width: 90%; 
}
.tarjeta {
    display: block; /* En móvil, las tarjetas se apilan */
}

/* 2. Breakpoint Tablet (Cuando el ancho MÍNIMO sea 768px o más grande) */
@media screen and (min-width: 768px) {
    .tarjeta {
        /* Sobrescribimos el estilo móvil: de bloque a inline-block */
        display: inline-block; 
        width: 45%; 
    }
}

/* 3. Breakpoint Escritorio (Cuando el ancho MÍNIMO sea 1200px o más grande) */
@media screen and (min-width: 1200px) {
    .contenedor {
        width: 1100px; /* Ancho fijo para pantallas muy grandes */
    }
    .tarjeta {
        width: 30%; /* Ahora caben 3 en fila */
    }
}
```

#### B. Desktop-First (Usando `max-width`)

Menos recomendado, pero útil si tienes que modificar un proyecto antiguo.

CSS

```
/* 1. Estilos Base (Optimizados para Escritorio) */
.tarjeta {
    width: 300px; 
    float: left; 
}

/* 2. Breakpoint Tablet (Cuando el ancho MÁXIMO sea 1024px o menor) */
@media screen and (max-width: 1024px) {
    .tarjeta {
        width: 45%; /* Se hacen más grandes para ajustarse a la pantalla */
    }
}

/* 3. Breakpoint Móvil (Cuando el ancho MÁXIMO sea 600px o menor) */
@media screen and (max-width: 600px) {
    .tarjeta {
        width: 95%; /* Se apilan */
        float: none;
    }
}
```

### 2. Características de Medios Comunes

|Característica|Descripción|Uso Común|
|---|---|---|
|**`min-width` / `max-width`**|El ancho del _viewport_ (la ventana).|El 90% del _responsive design_.|
|**`orientation`**|La orientación del dispositivo (`portrait` o `landscape`).|Ajustar la altura en modo horizontal.|
|**`min-resolution`**|La densidad de píxeles (útil para pantallas Retina).|Cargar imágenes de mayor calidad.|
|**`prefers-color-scheme`**|Si el usuario prefiere el modo claro u oscuro.|`(prefers-color-scheme: dark)`|

Exportar a Hojas de cálculo

---

## 🖼️ Imágenes Flexibles

Un error común es que una imagen con un tamaño fijo (`width: 500px;`) se saldrá de su contenedor en un móvil (que solo tiene 320px de ancho). Para evitar esto, las imágenes deben ser fluidas.

CSS

```
/* Regla de oro para imágenes flexibles */
img, video, canvas {
    /* No importa cuán grande sea la imagen original, nunca desbordará el 100% de su contenedor */
    max-width: 100%; 
    height: auto; /* Mantiene la proporción de la imagen */
}
```

## 📐 Unidades Relativas para un Diseño Fluido

Como vimos en [[05 - Colores y unidades en CSS]], la clave para la fluidez es alejarse de los `px` para las dimensiones generales:

|Unidad|Uso Responsive|
|---|---|
|**`rem`**|Dimensiones de fuentes y espaciado (garantiza que el texto se escale de forma accesible).|
|**`%`**|Anchos de contenedores (ej: `width: 80%;`).|
|**`vw` / `vh`**|Dimensiones que deben ajustarse con el tamaño de la ventana (ej: títulos muy grandes, `font-size: 5vw;`).|
|**`fr`**|Distribución flexible de columnas en Grid Layout.|

Exportar a Hojas de cálculo

---

## ✍️ Ejemplo de Código: Grid y Responsive (Mobile-First)

HTML

```
<div class="galeria-fotos">
    <img src="..." alt="Foto 1">
    <img src="..." alt="Foto 2">
    <img src="..." alt="Foto 3">
    <img src="..." alt="Foto 4">
</div>
```

CSS

```
/* 1. Estilos Base (Móvil: 1 columna, apiladas) */
.galeria-fotos {
    display: grid;
    /* Una columna que ocupa el total del espacio */
    grid-template-columns: 1fr; 
    gap: 10px;
}

/* 2. Breakpoint Tablet (Dos columnas) */
@media screen and (min-width: 700px) {
    .galeria-fotos {
        /* Sobrescribe: ahora dos columnas de igual tamaño */
        grid-template-columns: 1fr 1fr; 
    }
}

/* 3. Breakpoint Escritorio (Cuatro columnas) */
@media screen and (min-width: 1200px) {
    .galeria-fotos {
        /* Sobrescribe: cuatro columnas de igual tamaño */
        grid-template-columns: repeat(4, 1fr); 
    }
}
/* Las imágenes dentro de la galería están protegidas con la regla max-width: 100% */
.galeria-fotos img {
    max-width: 100%;
    height: auto;
}
```

---

## ✅ Resumen de Puntos Clave

- **Responsive Design** es la clave para que un sitio funcione en cualquier dispositivo.
    
- La etiqueta **`<meta name="viewport">`** es obligatoria en el HTML.
    
- El enfoque **Mobile-First** (diseñar para móvil primero) es el estándar de la industria.
    
- **Media Queries** (`@media`) son la herramienta para aplicar estilos basados en las características del dispositivo.
    
- Las consultas **`min-width`** se usan para Mobile-First (aplicar estilos **a partir de** cierto ancho).
    
- Las **imágenes flexibles** se logran con `max-width: 100%; height: auto;`.
    
- El uso constante de unidades relativas (`rem`, `%`, `vw`, `fr`) asegura que el diseño sea fluido.
    

👉 Ahora que sabemos cómo hacer que el diseño sea reactivo, profundizaremos en las técnicas específicas para que el contenido se sienta fluido y se adapte en tiempo real. Continúa con [[14 - Diseño fluido y adaptativo]].