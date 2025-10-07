Hemos pasado de la gestión de listas a la estructura más fundamental para modelar la realidad: los **Objetos**. Esta clase es la puerta de entrada a la **Programación Orientada a Objetos (POO)** en JavaScript.

---

Aquí tienes los apuntes de la clase anterior: [[08 - Arrays en profundidad]]

# 09 - Objetos y POO en JavaScript

## ✍️ Introducción: Modelando la Realidad

En la [[04 - Tipos de datos en JavaScript]] vimos que un **Objeto** es un tipo de dato estructurado. Piensa en él como una **ficha o tarjeta de identidad** que describe una cosa o entidad compleja (una persona, un coche, una cuenta bancaria) mediante un conjunto de **pares clave-valor**.

- La **Clave** (o Propiedad) es el nombre de la característica (ej: `color`, `edad`).
    
- El **Valor** es el dato asociado a esa característica (ej: `"rojo"`, `30`).
    

Aquí exploraremos cómo crear, manipular objetos y, de forma introductoria, cómo JS maneja la POO.

---

## 💻 Explicación Detallada: Creación y Manipulación de Objetos

### 1. Creación de Objetos (Literales)

La forma más común y sencilla de crear un objeto es mediante la sintaxis literal, usando llaves `{}`.

JavaScript

```
// Objeto que representa un vehículo
const vehiculo = {
    // Propiedades
    marca: "Toyota",
    modelo: "Corolla",
    anio: 2022,
    
    // Un valor de una propiedad puede ser una función (un método)
    encender: function() {
        console.log("El motor ha arrancado.");
    },
    
    // Forma moderna de definir un método (función flecha)
    obtenerAntiguedad: () => {
        const anioActual = new Date().getFullYear();
        return anioActual - vehiculo.anio;
    }
};
```

### 2. Acceso y Modificación de Propiedades

Hay dos formas principales de acceder o modificar el valor de una propiedad:

|Sintaxis|Uso|Cuándo usarla|
|---|---|---|
|**Punto `.`**|La más común y legible.|Cuando conoces el nombre de la propiedad.|
|**Corchetes `[]`**|Útil si el nombre de la propiedad está guardado en una variable o si tiene espacios.|Cuando la clave es dinámica (viene de una variable).|

Exportar a Hojas de cálculo

JavaScript

```
// Acceso y Lectura
console.log(vehiculo.marca);     // Sintaxis de punto (Muestra: "Toyota")
const clave = "modelo";
console.log(vehiculo[clave]);    // Sintaxis de corchetes (Muestra: "Corolla")

// Modificación (Mutación)
vehiculo.anio = 2023; // El objeto puede ser modificado incluso si fue declarado con 'const'
                      // (Solo se protege la variable 'vehiculo', no su contenido)

// Agregar una nueva propiedad
vehiculo.color = "Gris";

// Eliminar una propiedad
delete vehiculo.modelo;
```

### 3. Métodos: Acciones del Objeto

Un **método** es una función que está almacenada como propiedad de un objeto. Los métodos permiten al objeto realizar acciones.

#### **La Palabra Clave `this`**

Dentro de un método, la palabra clave **`this`** se refiere al **objeto actual** que está ejecutando ese método. Es fundamental para acceder a otras propiedades del mismo objeto.

JavaScript

```
const mascota = {
    nombre: "Fido",
    tipo: "Perro",
    
    // Método que usa 'this' para acceder al 'nombre' y 'tipo'
    emitirSonido: function() {
        // 'this' apunta a 'mascota'
        console.log(`${this.nombre} (${this.tipo}) dice ¡Guau!`);
    }
};

mascota.emitirSonido(); // Muestra: Fido (Perro) dice ¡Guau!
```

> **⚠️ Advertencia sobre `this` en Funciones Flecha:** En los objetos, es mejor usar la sintaxis de función tradicional (`function() { ... }`) para los métodos, ya que las funciones flecha no enlazan su propio `this`, lo que puede llevar a errores inesperados.

---

## 💻 Explicación Detallada: Introducción a POO con Clases (ES6+)

La **Programación Orientada a Objetos (POO)** es una metodología de programación que organiza el código en torno a objetos. En lugar de centrarnos en la lógica, nos centramos en las **entidades** que queremos modelar.

Antes de ES6 (2015), JavaScript usaba prototipos. Hoy, usamos la sintaxis de **`class`** (Clase), que es un "azúcar sintáctico" (una forma más limpia) para simular la POO tradicional.

### 1. Clases: El Molde

Una **clase** es la **plantilla** o el **molde** a partir del cual se crean los objetos.

JavaScript

```
class Empleado {
    // El constructor es una función especial que se ejecuta al crear un nuevo objeto (instancia)
    constructor(nombre, cargo, salario) {
        // Asignamos los parámetros a las propiedades del nuevo objeto (usando 'this')
        this.nombre = nombre;
        this.cargo = cargo;
        this.salario = salario;
    }

    // Método de la clase
    presentarse() {
        return `Hola, soy ${this.nombre} y mi cargo es ${this.cargo}.`;
    }
}
```

### 2. Instanciación: Creando Objetos

Usamos la palabra clave **`new`** para crear un objeto (una **instancia**) a partir de la clase.

JavaScript

```
// Creamos una nueva instancia de la clase Empleado
const empleado1 = new Empleado("Juan Pérez", "Desarrollador Senior", 60000);
const empleado2 = new Empleado("Ana Gómez", "Diseñadora UX", 55000);

console.log(empleado1.presentarse()); 
// Muestra: Hola, soy Juan Pérez y mi cargo es Desarrollador Senior.
```

### 3. Herencia: Reutilizando Plantillas

La **herencia** permite que una clase (la subclase o hija) herede las propiedades y métodos de otra clase (la superclase o padre), promoviendo la reutilización de código.

- Usamos **`extends`** para heredar.
    
- Usamos **`super()`** dentro del `constructor` de la clase hija para llamar al constructor del padre.
    

JavaScript

```
class Gerente extends Empleado {
    constructor(nombre, salario, departamento) {
        // Llama al constructor de la clase Empleado (padre)
        super(nombre, "Gerente", salario); 
        this.departamento = departamento;
    }

    // Sobrescribimos o extendemos un método del padre
    presentarse() {
        const presentacionPadre = super.presentarse();
        return `${presentacionPadre} Dirijo el departamento de ${this.departamento}.`;
    }
}

const gerente1 = new Gerente("Laura Martínez", 80000, "Marketing");

console.log(gerente1.presentarse()); 
// Muestra: Hola, soy Laura Martínez y mi cargo es Gerente. Dirijo el departamento de Marketing.
```

---

## 🛠️ Ejemplos Prácticos: Objetos y Clases

Crea un archivo `clase09.js` y ejecútalo con Node.js.

JavaScript

```
// ------------------------------------
// PARTE 1: Objeto Literal y Desestructuración
// ------------------------------------
const configuracion = {
    tema: "oscuro",
    notificaciones: true,
    idioma: "es",
    mostrarMensaje: function(msg) {
        console.log(`[Configuración ${this.tema}]: ${msg}`);
    }
};

// Acceso al método
configuracion.mostrarMensaje("Sistema cargado."); // Muestra: [Configuración oscuro]: Sistema cargado.

// Desestructuración (ES6+): Extraer propiedades del objeto en variables separadas
const { tema, idioma } = configuracion; 
console.log(`El tema actual es ${tema} y el idioma es ${idioma}.`);
// Muestra: El tema actual es oscuro y el idioma es es.

console.log("-------------------");

// ------------------------------------
// PARTE 2: Clases e Instanciación
// ------------------------------------
class Tarea {
    constructor(descripcion, fechaLimite) {
        this.id = Date.now(); // ID único basado en el tiempo actual
        this.descripcion = descripcion;
        this.completada = false;
        this.fechaLimite = fechaLimite;
    }

    // Método para cambiar el estado
    marcarComoCompletada() {
        this.completada = true;
    }
}

const tarea1 = new Tarea("Preparar la Clase 10", "2025-01-15");
const tarea2 = new Tarea("Revisar correos", "2025-01-10");

tarea1.marcarComoCompletada(); // Ejecutamos el método

console.log(`Tarea 1: ${tarea1.descripcion} - ¿Completada? ${tarea1.completada}`);
console.log(`Tarea 2: ${tarea2.descripcion} - ¿Completada? ${tarea2.completada}`);
// Tarea 1: Preparar la Clase 10 - ¿Completada? true
// Tarea 2: Revisar correos - ¿Completada? false
```

---

## ✅ Resumen de Puntos Clave

- Un **Objeto** es una colección de **pares clave-valor** (propiedades) encerrados en `{}`.
    
- Los objetos se acceden con la notación de **punto** (`.`) o **corchetes** (`[]`).
    
- Un **Método** es una función que está dentro de un objeto.
    
- La palabra clave **`this`** dentro de un método se refiere al objeto que lo invocó.
    
- **POO** se centra en modelar entidades.
    
- Una **`class`** es una plantilla para crear objetos.
    
- El **`constructor`** es la función que inicializa las propiedades de un nuevo objeto.
    
- La palabra clave **`new`** se usa para crear una **instancia** (objeto) a partir de una clase.
    
- La **herencia** se logra con **`extends`** y llamando a **`super()`** para reutilizar propiedades del padre.
    

---

Has dominado la creación y manipulación de las dos estructuras de datos más importantes (Arrays y Objetos). El siguiente paso es entender cómo se intercambian estos datos, especialmente a través de Internet, usando **JSON**.

👉 Continuar con [[10 - JSON y almacenamiento]]