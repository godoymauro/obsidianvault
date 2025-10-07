Ya sabes modelar entidades con Objetos. El siguiente paso es aprender a **comunicarte** con el exterior. Cuando envías o recibes datos de un servidor (como cuando inicias sesión o cargas un _feed_ de noticias), esos datos casi siempre viajan en formato **JSON**.

---

Aquí tienes los apuntes de la clase anterior: [[09 - Objetos y POO en JavaScript]]

# 10 - JSON y almacenamiento

## ✍️ Introducción: El Lenguaje Universal de los Datos

**JSON (JavaScript Object Notation)** no es un tipo de dato de JavaScript en sí mismo, sino un **formato de texto** para el intercambio de datos. Es la forma en que los objetos de JavaScript "viajan" por Internet.

Piensa en tus objetos de JavaScript (de la [[09 - Objetos y POO en JavaScript]]) como comida que acabas de cocinar. JSON es el **empaquetado estandarizado** que permite enviar esa comida a través del mundo (Internet) de forma segura y legible, para que otro sistema (otro servidor, otra aplicación) pueda "desempaquetarla" y usarla.

En esta clase, aprenderemos a convertir entre Objetos JS y JSON, y cómo usar el navegador para guardar datos localmente.

---

## 💻 Explicación Detallada: JSON, Serialización y Deserialización

### 1. JSON vs. Objeto de JavaScript

La estructura de JSON es prácticamente idéntica a la de un Objeto de JavaScript, pero con una regla fundamental: **todas las claves (nombres de propiedad) deben estar encerradas en comillas dobles** (`""`).

|Característica|Objeto JS|JSON (Formato de Texto)|
|---|---|---|
|**Definición**|Tipo de dato nativo.|Formato de texto universal.|
|**Claves**|Pueden ir sin comillas (si son válidas).|**Siempre** deben ir entre comillas dobles.|
|**Métodos**|Puede contener funciones (métodos).|**NO** puede contener funciones (solo datos).|

Exportar a Hojas de cálculo

**Ejemplo de un objeto JSON (¡Es un string de texto!):**

JSON

```
{
    "nombre": "Atlas",
    "version": 1.0,
    "activo": true
}
```

### 2. El Objeto Global `JSON`

JavaScript proporciona un objeto global llamado `JSON` con dos métodos estáticos esenciales para trabajar con este formato:

#### A) `JSON.stringify()` (Serialización)

Este método toma un **Objeto de JavaScript** (o un Array) y lo convierte en una **cadena de texto (String) con formato JSON**. Este es el paso que debes hacer antes de enviar datos a un servidor o guardarlos en `localStorage`.

> **Objeto JS → String JSON** (El proceso de **Serialización**)

JavaScript

```
const datosUsuario = {
    id: 101,
    email: "user@mail.com",
    config: { tema: "claro" }
};

// Convertir el objeto a una cadena JSON
const jsonTexto = JSON.stringify(datosUsuario);

console.log(typeof datosUsuario); // Muestra: "object"
console.log(typeof jsonTexto);    // Muestra: "string"
console.log(jsonTexto);           // Muestra: '{"id":101,"email":"user@mail.com","config":{"tema":"claro"}}'
```

#### B) `JSON.parse()` (Deserialización)

Este método toma una **cadena de texto JSON** y la convierte de nuevo en un **Objeto de JavaScript** utilizable. Este es el paso que debes hacer después de recibir datos de un servidor o de `localStorage`.

> **String JSON → Objeto JS** (El proceso de **Deserialización**)

JavaScript

```
const jsonRecibido = '{"ciudad": "Berlín", "poblacion": 3769000}';

// Convertir la cadena JSON de vuelta a un objeto JS
const objetoCiudad = JSON.parse(jsonRecibido);

console.log(typeof objetoCiudad);      // Muestra: "object"
console.log(objetoCiudad.ciudad);      // Muestra: "Berlín"
console.log(objetoCiudad.poblacion);   // Muestra: 3769000
```

---

## 💻 Explicación Detallada: Almacenamiento Local en el Navegador

El navegador nos da la capacidad de guardar información directamente en la computadora del usuario, lo que permite guardar preferencias o el estado de una aplicación. El mecanismo más común para esto es la **Web Storage API**.

### 1. `localStorage`

`localStorage` (Almacenamiento Local) permite guardar datos que **permanecen** incluso si el usuario cierra el navegador o apaga la computadora.

|Método|Descripción|Uso|
|---|---|---|
|**`setItem(clave, valor)`**|Guarda un par clave-valor.|`localStorage.setItem('usuario', jsonTexto)`|
|**`getItem(clave)`**|Recupera el valor asociado a la clave.|`localStorage.getItem('usuario')`|
|**`removeItem(clave)`**|Elimina un par clave-valor.|`localStorage.removeItem('usuario')`|
|**`clear()`**|Elimina **todo** lo guardado por ese sitio web.|`localStorage.clear()`|

Exportar a Hojas de cálculo

> **⚠️ Importante:** `localStorage` **SOLO almacena cadenas de texto (Strings)**. Por eso, si quieres guardar un Objeto, ¡siempre debes usar `JSON.stringify()` primero!

### 2. `sessionStorage`

`sessionStorage` (Almacenamiento de Sesión) funciona exactamente igual que `localStorage`, pero los datos se **eliminan automáticamente** cuando el usuario cierra la pestaña o la ventana del navegador. Es ideal para información temporal de una sesión, como el contenido de un carrito de compras.

---

## 🛠️ Ejemplos Prácticos: Trabajando con JSON y Storage

Este código es ideal para probarlo directamente en la **Consola del Navegador** (ver [[02 - Configuración del entorno]]), ya que `localStorage` solo existe en ese entorno, no en Node.js.

JavaScript

```
// Paso 1: Definir el objeto JS que queremos guardar
const producto = {
    nombre: "Teclado Mecánico",
    precio: 95.50,
    enOferta: true
};

console.log("Objeto original:", producto);
console.log("-------------------");

// Paso 2: Convertir el Objeto a JSON (String) para guardarlo
const productoJSON = JSON.stringify(producto);
console.log("JSON String para guardar:", productoJSON); 
console.log("Tipo:", typeof productoJSON);
console.log("-------------------");


// Paso 3: Guardar el JSON string en localStorage
// localStorage.setItem(clave, valor_string)
localStorage.setItem('ultimo_producto', productoJSON); 
console.log("Producto guardado en localStorage.");
console.log("-------------------");


// Paso 4: Recuperar el JSON String desde localStorage
const jsonRecuperado = localStorage.getItem('ultimo_producto');
console.log("JSON String recuperado:", jsonRecuperado);
console.log("-------------------");


// Paso 5: Convertir el JSON String de vuelta a un Objeto JS utilizable
const productoRecuperado = JSON.parse(jsonRecuperado);

console.log("Objeto JS recuperado:", productoRecuperado);
console.log("¡Podemos usarlo! Precio:", productoRecuperado.precio);

// Limpiamos el almacenamiento para la próxima prueba
localStorage.removeItem('ultimo_producto');
console.log("-------------------");
console.log("Elemento eliminado de localStorage.");
```

---

## ✅ Resumen de Puntos Clave

- **JSON** es un formato de texto universal para el intercambio de datos. Es idéntico a un Objeto JS, pero sus **claves deben usar comillas dobles**.
    
- **Serialización** es el proceso de convertir un Objeto JS a un String JSON. Se usa **`JSON.stringify()`**.
    
- **Deserialización** es el proceso de convertir un String JSON a un Objeto JS. Se usa **`JSON.parse()`**.
    
- **`localStorage`** almacena datos de forma persistente en el navegador (permanecen tras cerrar el navegador).
    
- **`sessionStorage`** almacena datos que se borran al cerrar la pestaña o el navegador.
    
- Ambos métodos de almacenamiento **solo guardan Strings**, por lo que siempre debes usar `JSON.stringify()` antes de guardar Objetos y `JSON.parse()` después de recuperarlos.
    

---

¡Felicidades! Has completado el Nivel 2. Dominas las funciones, los arrays (y sus métodos potentes) y la gestión de datos con Objetos y JSON.

Ahora, pasaremos al Nivel 3, el más excitante, donde conectaremos JavaScript con la página web que el usuario ve: el **DOM** y la **Asincronía**.

👉 Continuar con [[11 - El DOM (Document Object Model)]]