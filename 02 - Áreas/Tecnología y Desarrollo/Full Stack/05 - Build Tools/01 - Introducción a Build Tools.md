# 01 - Introducción a Build Tools

Aquí encontraras todo el indice del curso en el mapa de contexto: [[05 - Build Tools/00 - MOC]]

## 📝 Introducción

¡Bienvenido! Empezaremos por el principio: **¿Qué demonios son las Build Tools (Herramientas de Construcción) y por qué son cruciales para el desarrollo frontend moderno?**

Piensa en un desarrollo web como la construcción de un edificio. Como desarrollador, tú eres el arquitecto. Escribes los planos (código JavaScript, HTML, CSS), pero no puedes entregarle esos planos directamente a los clientes. Necesitas obreros, maquinaria y procesos que transformen esos planos en un edificio sólido, optimizado y habitable. Esas herramientas y procesos son las **Build Tools**.

## 🧠 Teoría: La Evolución de la Construcción Web

### ¿Qué son las Build Tools?

Son programas o librerías que **automatizan tareas** repetitivas y complejas necesarias para tomar el código fuente (el que escribes) y transformarlo en código listo para producción (el que el navegador ejecuta de forma eficiente).

### ¿Para qué sirven?

1. **Manejo de Módulos (Bundling):** Permiten que dividas tu código en muchos archivos pequeños e interdependientes (módulos), para luego unirlos de forma inteligente en unos pocos archivos optimizados.
    
2. **Transpilación:** Convierten código moderno (como el último JavaScript o TypeScript) en código compatible con navegadores más antiguos.
    
3. **Procesamiento de Assets:** Optimizan imágenes, comprimen CSS (Sass/Less) y preparan fuentes.
    
4. **Optimización:** Minifican (reducen el tamaño) y ofuscan el código para que cargue más rápido.
    
5. **Desarrollo Eficiente (HMR):** Permiten recargas rápidas del código en el navegador sin perder el estado (Hot Module Replacement o HMR).
    

---

### Diferencias: Legacy vs. Moderno

|Característica|Enfoque Legacy (Ej: Grunt, Gulp)|Enfoque Moderno (Ej: Vite, Webpack)|
|---|---|---|
|**Manejo de Módulos**|Principalmente **concatenación** de archivos y **"watchers"** de tareas.|**Bundling sofisticado** basado en el árbol de dependencias (AST).|
|**Velocidad de Dev**|Lento. Necesita reconstruir gran parte del proyecto en cada cambio.|**Rápido.** Utiliza el sistema nativo de módulos de JS (`<script type="module">`) y HMR.|
|**Configuración**|Basado en tareas (`tasks`) y mucha configuración manual.|Basado en el **grafo de dependencias**. **Zero-config** o configuración basada en plugins.|
|**Herramientas Clave**|Gulp, Grunt, Browserify.|**Vite, Webpack, esbuild, Parcel.**|

Exportar a Hojas de cálculo

### El Papel del Bundler y el Gestor de Paquetes

Estas son las dos piezas clave que veremos:

1. **Gestor de Paquetes (Package Manager):** (npm, Yarn, pnpm). Se encarga de **descargar, instalar, actualizar y gestionar** todas las librerías de terceros (dependencias) que tu proyecto necesita.
    
2. **Bundler (Empaquetador):** (Vite, Webpack, Parcel). Se encarga de **procesar, transformar y combinar** todo tu código (incluyendo las dependencias) en los _bundles_ de salida que se desplegarán.
    

|Herramienta|Función Principal|Ejemplo de Tarea|
|---|---|---|
|**Gestor de Paquetes**|Obtener las dependencias del proyecto.|Instalar **React** o **Axios**.|
|**Bundler**|Optimizar y juntar el código final.|Convertir un archivo `.jsx` en un archivo `.js` y minificarlo.|

Exportar a Hojas de cálculo

---

## 🛠️ Ejemplos: ¿Qué hace un Bundler?

Imagina que tienes estos tres archivos en desarrollo:

JavaScript

```
// src/utils.js
export const sum = (a, b) => a + b;

// src/component.js
import { sum } from './utils.js';

export const MyComponent = () => {
  console.log(sum(5, 3));
};

// src/main.js
import { MyComponent } from './component.js';
MyComponent();
```

Un navegador solo puede cargar muchos archivos pequeños con la etiqueta `<script>` lo cual es ineficiente.

El **Bundler** lo transforma en un solo archivo optimizado para producción, por ejemplo, `dist/bundle.js`:

JavaScript

```
// dist/bundle.js (Simplificado)
(() => {
  // src/utils.js contenido (puede ser minificado)
  const sum = (a, b) => a + b;
  
  // src/component.js contenido
  const MyComponent = () => {
    console.log(sum(5, 3));
  };

  // src/main.js contenido
  MyComponent();
})(); 
// Y este es el único archivo que se carga en el HTML: <script src="./dist/bundle.js"></script>
```

Además, el bundler maneja la **transpilación** (convertir `const` en `var` si se necesita compatibilidad antigua) y la **minificación** (eliminar espacios y comentarios).

## 🛑 Errores Comunes (Early Stage)

|Error|Descripción|Solución (Conceptual)|
|---|---|---|
|**"Module not found"**|El bundler o el navegador no pueden encontrar un archivo importado.|Verifica las rutas de `import/require`. Asegúrate de que el gestor de paquetes instaló la dependencia.|
|**"Unexpected token"**|Usaste una sintaxis de JavaScript moderna (ej: el operador `?` de encadenamiento opcional) que el navegador o el transpilador no entienden.|Necesitas configurar un transpilador (como Babel) para que convierta esa sintaxis moderna en una más antigua.|
|**"Error: You must install Node.js"**|Intentas usar el gestor de paquetes (`npm`, `yarn`) sin tener instalado el entorno de ejecución.|Instala **Node.js**. Esto instala `npm` por defecto.|

Exportar a Hojas de cálculo

## 📚 Resumen de Puntos Clave

- **Build Tools** automatizan la transformación del código fuente en código de producción.
    
- Las herramientas **modernas** (Vite, esbuild) priorizan la velocidad de desarrollo y el uso de módulos nativos.
    
- El **Gestor de Paquetes** (npm, yarn) trae librerías de terceros.
    
- El **Bundler** (Webpack, Vite) procesa, transforma y junta tu código.
    

---

Nuestra próxima clase se centrará en la herramienta más fundamental para todo este proceso: **Node.js y la instalación de nuestros Gestores de Paquetes**.

[[02 - Instalación y configuración básica]]