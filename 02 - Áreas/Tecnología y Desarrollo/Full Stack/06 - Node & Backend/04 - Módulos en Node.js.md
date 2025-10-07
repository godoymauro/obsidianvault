¡Fantástico! Has dominado el concepto más vital: el **Event Loop**. Ahora sabes que en Node.js, la modularidad y la organización del código son esenciales para mantener ese hilo único rápido y eficiente.

Nuestra cuarta clase se centra en cómo Node.js organiza el código en piezas reutilizables, un concepto fundamental en cualquier lenguaje moderno.

Aquí tienes la nota para la clase anterior: [[03 - Node.js Runtime]]

---

# 04 - Módulos en Node.js

## 📚 Introducción: Modularidad y Reutilización

Un **módulo** es simplemente un archivo de JavaScript que contiene una unidad de código encapsulada. En el backend, la modularidad es crucial para la **organización**, el **mantenimiento** y la **reutilización**. En lugar de tener miles de líneas de código en un solo archivo, dividimos la aplicación en módulos (por ejemplo, un módulo para la base de datos, otro para las rutas, otro para la autenticación).

Node.js ha lidiado históricamente con **dos sistemas de módulos** principales: **CommonJS** y **ES Modules (ESM)**.

---

## 💾 CommonJS: El Sistema Tradicional de Node.js

**CommonJS** fue el sistema de módulos original y por defecto de Node.js durante años. Es síncrono y se basa en dos palabras clave: `require` para importar y `module.exports` o `exports` para exportar.

### 1. Exportación (CommonJS)

Para exportar una o varias funciones/variables, usamos `module.exports`.

**Archivo:** `calculadora.js`

JavaScript

```
// Exporta el objeto { sumar: funcion, restar: funcion }

const sumar = (a, b) => a + b;
const restar = (a, b) => a - b;

// 💡 module.exports define qué es visible fuera del módulo
module.exports = {
    sumar,
    restar,
    // Podemos exportar cualquier cosa: funciones, variables, objetos, clases
    PI: 3.14159
};
```

### 2. Importación (CommonJS)

Para importar, usamos la función `require()`, que es síncrona (detiene la ejecución hasta que el módulo se carga).

**Archivo:** `app-cjs.js`

JavaScript

```
// Importamos el objeto exportado desde calculadora.js
// La ruta es relativa al archivo actual
const operaciones = require('./calculadora'); 

console.log('Suma de 5 + 3:', operaciones.sumar(5, 3)); // Salida: 8
console.log('Resta de 10 - 4:', operaciones.restar(10, 4)); // Salida: 6
console.log('Valor de PI:', operaciones.PI); // Salida: 3.14159
```

---

## 🆕 ES Modules (ESM): El Estándar Moderno de JavaScript

**ES Modules (ESM)** es el sistema de módulos estándar definido por el lenguaje JavaScript. Es el que se usa universalmente en el frontend (navegadores) y se ha adoptado completamente en Node.js. Es **asíncrono** por diseño, lo que encaja mejor con la filosofía de Node.js.

### ¿Cómo Habilitar ESM en Node.js?

Hay dos maneras:

1. **Renombrar el archivo** con la extensión `.mjs` (más explícito).
    
2. Añadir `"type": "module"` en tu archivo `package.json`. **Esta es la práctica moderna recomendada.**
    

JSON

```
// package.json
{
  "type": "module",
  "main": "app-esm.js",
  // ... resto del package.json
}
```

### 1. Exportación (ESM)

Usamos la palabra clave `export`.

**Archivo:** `utilidades.js` (Asumiendo `"type": "module"` en `package.json`)

JavaScript

```
// Exportación nombrada
export const mensajeBienvenida = '¡Bienvenido al sistema!';

// Exportación por defecto (solo puede haber una por módulo)
const multiplicar = (a, b) => a * b;
export default multiplicar;

// Exportación nombrada de una función
export function dividir(a, b) {
    if (b === 0) throw new Error("División por cero.");
    return a / b;
}
```

### 2. Importación (ESM)

Usamos la palabra clave `import`.

**Archivo:** `app-esm.js`

JavaScript

```
// Importación de la exportación por defecto (se le puede dar cualquier nombre)
import multiplicar from './utilidades.js';

// Importación de exportaciones nombradas (deben coincidir con el nombre exportado)
import { mensajeBienvenida, dividir } from './utilidades.js';

console.log(mensajeBienvenida); // Salida: ¡Bienvenido al sistema!
console.log('Multiplicación de 6 * 7:', multiplicar(6, 7)); // Salida: 42
console.log('División de 10 / 2:', dividir(10, 2)); // Salida: 5
```

---

## 🔀 CommonJS vs ES Modules: Una Comparativa

Como desarrollador Node.js moderno, **deberías priorizar ES Modules** (`import/export`). Sin embargo, es vital conocer CommonJS, ya que muchas librerías antiguas aún lo usan.

|Característica|CommonJS (CJS)|ES Modules (ESM)|
|---|---|---|
|**Sintaxis de Exportación**|`module.exports = ...`|`export ...`|
|**Sintaxis de Importación**|`const x = require('ruta')`|`import x from 'ruta'`|
|**Naturaleza**|**Síncrona.** Carga el archivo inmediatamente.|**Asíncrona.** Se ajusta mejor a Node.js.|
|**Extensión de Archivo**|`.js` (por defecto)|`.mjs` o `.js` con `"type": "module"`|
|**Uso de `__dirname`**|Disponible (ruta actual del archivo).|No disponible por defecto (debe ser simulado).|

### 🛑 Importando CJS dentro de ESM

A veces, tienes que importar una librería antigua de CommonJS desde un proyecto moderno de ESM. Puedes hacerlo con una importación por defecto:

JavaScript

```
// En un archivo .mjs o con "type": "module"
import http from 'http'; // El módulo 'http' es CJS nativo, pero Node.js lo maneja.

// Si es una librería CJS externa:
import * as paqueteCJS from 'some-cjs-package';
```

---

## ✅ Buenas Prácticas y Consejos

|Categoría|Buena Práctica|Error Común|
|---|---|---|
|**Estándar**|**Usar ES Modules (`import`/`export`)** en todos los proyectos nuevos y modernos.|Mezclar CJS y ESM en el mismo archivo o usar `require` sin necesidad.|
|**Exportación**|Usar **exportaciones nombradas** (`export const func...`) para funciones de utilidad. Reservar la **exportación por defecto** (`export default`) para la entidad principal del módulo (ej: la clase principal de un servicio).|Usar solo exportaciones por defecto, lo que dificulta la importación de múltiples utilidades.|
|**Módulos Nativos**|Entender que los módulos nativos de Node.js (como `fs`, `http`, `path`) se cargan siempre primero.|Intentar crear un módulo llamado `http.js` en tu proyecto, lo que podría generar confusión.|

---

## 🔑 Resumen de Puntos Clave

- **Módulos** son archivos JS que encapsulan y reutilizan código, siendo clave para la organización.
    
- **CommonJS (CJS)** usa `require()` para importar y `module.exports` para exportar. Es el sistema histórico de Node.js.
    
- **ES Modules (ESM)** usa `import` y `export`. Es el estándar moderno de JavaScript y el recomendado.
    
- Para usar ESM, debes configurar `"type": "module"` en el `package.json` de tu proyecto o usar la extensión `.mjs`.
    
- La modularidad te permite mantener el código limpio y el **hilo principal** libre de lógica desordenada y compleja.
    

Hemos aprendido a estructurar nuestro código. Ahora podemos pasar a los módulos nativos de Node.js que nos permiten interactuar con el sistema operativo.

Tu siguiente clase te enseñará a manejar archivos: **[[05 - File System (fs)]]**.