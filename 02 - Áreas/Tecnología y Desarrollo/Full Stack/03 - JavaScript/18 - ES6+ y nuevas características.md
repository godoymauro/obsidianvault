Has sentado las bases de la **programación moderna** con las buenas prácticas. Ahora, es momento de incorporar las herramientas que hacen que JavaScript sea tan potente, limpio y disfrutable de escribir: las características introducidas a partir de **ECMAScript 2015 (ES6)**.

---

Aquí tienes los apuntes de la clase anterior: [[17 - Buenas prácticas de código]]

# 18 - ES6+ y nuevas características

## ✍️ Introducción: La Evolución de JavaScript

**ES6** (o **ECMAScript 2015**) marcó un punto de inflexión para JavaScript. Fue una actualización masiva que introdujo sintaxis mucho más limpia, manejo más intuitivo de datos y estructuras de control mejoradas. A partir de ES6, las actualizaciones se nombran por el año (ES2016, ES2017, etc.), por eso hablamos de **ES6+**.

Si bien ya hemos usado algunas (como `const`/`let` y las funciones flecha, vistas en [[03 - Sintaxis básica y variables]] y [[07 - Funciones en JavaScript]]), aquí profundizaremos en las que mejoran la gestión y manipulación de datos.

---

## 💻 Explicación Detallada: Características Clave de ES6+

### 1. Template Strings (Cadenas de Plantilla) 📝

Vistas brevemente en la [[04 - Tipos de datos en JavaScript]], las _Template Strings_ son la forma estándar y más legible de trabajar con texto, especialmente cuando se incluyen variables. Se definen con **comillas invertidas (`)**.

|Característica|Propósito|Ejemplo|
|---|---|---|
|**Interpolación**|Permite incrustar variables o expresiones directamente en el texto.|`` `Hola ${nombre} tienes ${2025 - anioNacimiento} años.` ``|
|**Multilínea**|Permite escribir strings que ocupan varias líneas sin usar el carácter `\n`.|`` `Línea 1 <br> Línea 2` ``|

Exportar a Hojas de cálculo

JavaScript

```
const producto = "Teclado";
const precio = 150;

// Antes (Concatenación tediosa)
const mensajeClasico = "El " + producto + " tiene un precio de $" + precio + ".";

// Ahora (Template String, más limpio)
const mensajeModerno = `El ${producto} tiene un precio de $${precio}.`; 
```

---

### 2. Desestructuración (Destructuring) ✨

La **desestructuración** es una sintaxis concisa que permite "desempaquetar" valores de **Arrays** o **Objetos** en variables separadas. Esto es muy común en el código moderno.

#### **A) Desestructuración de Objetos**

Extrae propiedades por su nombre.

JavaScript

```
const usuario = {
    id: 42,
    nombre: 'Alice',
    pais: 'España',
    email: 'a@mail.com'
};

// Desestructuración: Crea las variables 'nombre' y 'pais'
const { nombre, pais } = usuario; 

console.log(nombre); // Muestra: 'Alice'
console.log(pais);   // Muestra: 'España'

// También permite renombrar la variable si el nombre de la propiedad es largo
const { email: correoElectronico } = usuario; 
console.log(correoElectronico); // Muestra: 'a@mail.com'
```

👉 Ver también [[09 - Objetos y POO en JavaScript]]

#### **B) Desestructuración de Arrays**

Extrae valores por su posición.

JavaScript

```
const colores = ['rojo', 'verde', 'azul'];

// Desestructuración: El orden importa
const [primero, segundo, tercero] = colores; 

console.log(primero); // Muestra: 'rojo'
```

---

### 3. Operadores Spread (`...`) y Rest (`...`)

Aunque usan la misma sintaxis de puntos suspensivos (`...`), cumplen funciones opuestas según dónde se utilicen.

#### **A) Spread Operator (Expansión) ...**

Se usa para **expandir** los elementos de un iterable (Array u Objeto) donde se esperan cero o más argumentos/elementos. Es ideal para crear copias o combinar colecciones.

JavaScript

```
// Combinación de Arrays (Creación de un nuevo Array SIN mutar los originales)
const frutas = ['manzana', 'pera'];
const verduras = ['zanahoria', 'papa'];

const comestibles = [...frutas, 'pan', ...verduras]; 
console.log(comestibles); // Muestra: ['manzana', 'pera', 'pan', 'zanahoria', 'papa']

// Copia de Objetos (¡Crea una nueva referencia!)
const configuracionOriginal = { tema: 'claro', notificaciones: true };
const nuevaConfig = { ...configuracionOriginal, tema: 'oscuro', version: 2 };

// nuevaConfig ahora tiene un nuevo objeto con el tema sobrescrito
console.log(nuevaConfig); // Muestra: { tema: 'oscuro', notificaciones: true, version: 2 }
```

#### **B) Rest Parameters (Parámetros Restantes) ...**

Se usa en la **definición de una función** para capturar un número indefinido de argumentos restantes y agruparlos en un único **Array**.

JavaScript

```
// Captura cualquier argumento adicional en el array 'otros'
function sumarTodo(base, ...otrosNumeros) { 
    // 'otrosNumeros' es un Array: [2, 3, 4]
    console.log(otrosNumeros);
    
    // Usamos reduce (ver Clase 08) para sumar todos los elementos
    const sumaRestante = otrosNumeros.reduce((a, b) => a + b, 0);
    return base + sumaRestante;
}

console.log(sumarTodo(1, 2, 3, 4)); // Muestra: 10 (1 + 2 + 3 + 4)
```

---

### 4. Módulos (Imports y Exports) 📦

Antes de ES6, la gestión de archivos JS grandes era caótica. El sistema de **Módulos** estandarizó cómo se divide el código en archivos separados para hacerlo modular (ver [[17 - Buenas prácticas de código]]).

- **`export`:** Se usa para hacer que una función, clase o variable esté disponible fuera de su archivo.
    
- **`import`:** Se usa para traer un recurso exportado desde otro archivo.
    

|Tipo de Exportación|Sintaxis (Archivo A)|Sintaxis (Archivo B)|Uso|
|---|---|---|---|
|**Nombrada**|`export const IVA = 0.21;`|`import { IVA } from './a.js';`|Varias exportaciones por archivo.|
|**Por Defecto**|`export default function algo() {}`|`import MiFuncion from './a.js';`|Solo una por archivo. Se importa sin llaves.|

Exportar a Hojas de cálculo

---

## 🛠️ Ejemplos Prácticos: Aplicando ES6+

Crea un archivo `clase18.js` y ejecútalo con Node.js.

JavaScript

```
const perfil = {
    usuarioId: 77,
    nombreCompleto: "Elena Ríos",
    configuracion: {
        tema: 'oscuro',
        notificaciones: true
    }
};

// 1. Desestructuración anidada y renombre
// Extraemos la propiedad 'nombreCompleto' y 'tema' del objeto anidado
const { 
    nombreCompleto: nombre, // Renombramos
    configuracion: { tema } // Desestructuración anidada
} = perfil;

console.log(`Usuario: ${nombre}. Tema seleccionado: ${tema}`);
console.log("-------------------");

// 2. Spread Operator para Arrays y Objetos
const lista1 = [1, 2];
const lista2 = [4, 5];

// 2a. Array Spread: Combinamos listas
const listaCombinada = [0, ...lista1, 3, ...lista2, 6]; 
console.log("Lista Combinada:", listaCombinada); // Muestra: [0, 1, 2, 3, 4, 5, 6]

// 2b. Objeto Spread: Actualizamos y copiamos el perfil
const perfilActualizado = { 
    ...perfil, 
    nombreCompleto: "Elena Ríos V.", 
    fechaActualizacion: new Date()
};

console.log("Antiguo nombre:", perfil.nombreCompleto);
console.log("Nuevo nombre:", perfilActualizado.nombreCompleto);
console.log("-------------------");

// 3. Rest Parameters
function configurarApp(urlBase, puerto, ...opciones) {
    console.log(`Base: ${urlBase}:${puerto}`);
    // 'opciones' es un Array que contiene ['gzip', true, 1000]
    console.log("Opciones adicionales:", opciones); 
}

configurarApp("http://servidor.com", 8080, "gzip", true, 1000); 
```

---

## ✅ Resumen de Puntos Clave

- **ES6 (ECMAScript 2015)** y sus sucesores (ES6+) introdujeron mejoras fundamentales en la sintaxis de JavaScript.
    
- **Template Strings** (con `` ` ``, `$ {variable}`) permiten interpolación de variables y texto multilínea de forma limpia.
    
- La **Desestructuración** permite extraer valores de Arrays (por posición) y Objetos (por nombre) en variables individuales.
    
- El **Spread Operator (`...`)** se usa para **expandir** y combinar Arrays u Objetos, creando nuevas copias inmutables.
    
- Los **Rest Parameters (`...`)** se usan en la definición de funciones para **agrupar** argumentos restantes en un Array.
    
- Los **Módulos (`export`/`import`)** son el sistema estándar para organizar el código en archivos separados y mantener la modularidad.
    

---

¡Excelente trabajo! Ya tienes todas las herramientas sintácticas modernas. Solo nos queda el último paso técnico: asegurar la calidad de tu código mediante las pruebas automáticas.

👉 Continuar con [[19 - Testing en JavaScript]]