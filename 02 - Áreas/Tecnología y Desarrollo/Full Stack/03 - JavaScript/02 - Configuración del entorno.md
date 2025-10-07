Ya tienes el contexto, ahora vamos a preparar las herramientas. Para escribir código, no necesitas un lápiz y papel, sino un **entorno de desarrollo** adecuado.

---

Aquí tienes los apuntes de la clase anterior: [[01 - Introducción a JavaScript]]

# 02 - Configuración del entorno

## ✍️ Introducción: ¿Por qué necesitamos herramientas?

Piensa en un carpintero: no puede construir una mesa solo con sus manos; necesita sierras, martillos y un banco de trabajo. Como programadores, necesitamos herramientas para **escribir**, **ejecutar** y **depurar** (encontrar errores en) nuestro código JavaScript.

En esta clase, instalaremos las tres herramientas esenciales que usaremos a lo largo del curso:

1. **VSCode (Visual Studio Code):** Nuestro editor de código, el "banco de trabajo".
    
2. **Node.js:** El motor que nos permite ejecutar JavaScript fuera del navegador.
    
3. **Consola del navegador:** La herramienta que nos ayuda a probar JS directamente en la web.
    

---

## 💻 Explicación Detallada: Las Herramientas del Desarrollador

### 1. Visual Studio Code (VSCode)

**VSCode** es nuestro **Editor de Código Fuente** (Source Code Editor). Es como un procesador de texto ultra-potente diseñado específicamente para programar.

#### **¿Por qué VSCode?**

- **Gratuito y Multiplataforma:** Funciona en Windows, Mac y Linux.
    
- **IntelliSense:** Nos ayuda a escribir código más rápido sugiriendo funciones y variables.
    
- **Extensiones:** Permite añadir funcionalidades extra (como autocompletado avanzado o integración con otras herramientas).
    
- **Terminal Integrada:** Podemos ejecutar comandos sin salir del editor.
    

#### **Pasos de Instalación (Autónomos):**

1. Ve a la página oficial de VSCode.
    
2. Descarga el instalador para tu sistema operativo.
    
3. Sigue los pasos de instalación (puedes dejar las opciones predeterminadas).
    

### 2. Node.js: Ejecutando JS fuera del Navegador

Inicialmente, JavaScript solo se ejecutaba dentro de un navegador web. **Node.js** es un "ambiente de ejecución" que toma el motor de JavaScript (llamado **V8**, creado por Google) y lo pone a disposición de tu sistema operativo.

#### **¿Para qué sirve Node.js?**

- **Backend:** Permite crear servidores web y APIs usando JavaScript (una aplicación del mundo real).
    
- **Herramientas:** Nos permite ejecutar archivos JS directamente desde nuestra computadora, lo que es ideal para practicar.
    
- **NPM (Node Package Manager):** Es un gestor de paquetes que viene con Node.js. Piensa en él como una App Store gigante donde los desarrolladores comparten código listo para usar.
    

#### **Pasos de Instalación:**

1. Ve a la página oficial de Node.js.
    
2. **Importante:** Descarga la versión **LTS (Long Term Support)**. Es la versión más estable y recomendada para la mayoría de los usuarios.
    
3. Ejecuta el instalador y acepta las configuraciones por defecto.
    

#### **Verificación de Instalación (¡Crucial!):**

Una vez instalado, abre tu **Terminal** (Mac/Linux) o **Símbolo del Sistema/PowerShell** (Windows) y escribe estos comandos:

Bash

```
# Verifica si Node se instaló correctamente
node -v 

# Verifica si NPM se instaló correctamente
npm -v 
```

Si ves un número de versión (ej: `v18.17.1` y `9.6.7`), ¡felicidades! Node.js está listo.

### 3. La Consola del Navegador (Developer Tools)

La **Consola** es la herramienta de depuración y prueba más inmediata. Está integrada en tu navegador (Chrome, Firefox, Edge). Es nuestro laboratorio de pruebas rápido.

#### **¿Cómo acceder a ella?**

1. Abre tu navegador.
    
2. Haz clic derecho en cualquier parte de la página y selecciona **"Inspeccionar"** o **"Inspect Element"**.
    
3. Busca la pestaña que dice **"Console"** (Consola).
    

#### **Uso rápido de la Consola:**

Aquí puedes escribir y ejecutar líneas de JavaScript en tiempo real y ver los resultados de inmediato.

- Puedes usar el comando `console.log()` para mostrar mensajes (como hicimos en la clase anterior).
    
- Si escribes `5 + 5` y presionas Enter, la consola te dará el resultado `10`.
    

|Comando de Consola|Descripción|
|---|---|
|`console.log(variable)`|Muestra el valor de una variable o un mensaje. ¡Lo usaremos constantemente!|
|`console.error(mensaje)`|Muestra un mensaje de error, útil para depuración.|
|`console.clear()`|Limpia todos los mensajes de la consola.|

Exportar a Hojas de cálculo

---

## 🛠️ Ejemplos Prácticos: Usando las Herramientas

### 1. Prueba en el Navegador (Consola)

1. Abre la Consola de tu navegador.
    
2. Escribe y presiona Enter después de cada línea:
    

JavaScript

```
// Definimos nuestra primera variable (veremos esto en detalle en la Clase 03)
var miNombre = "Profesor Experto"; 

// Usamos el comando de registro para ver el valor
console.log(miNombre); 

// Hacemos una operación matemática
console.log(100 / 5); 

// La consola imprime el resultado inmediatamente (20)
```

### 2. Prueba con Node.js (Archivos)

Vamos a crear nuestro primer archivo ejecutable con Node.js:

1. **Crea un archivo:** En tu escritorio o una carpeta de trabajo, crea un nuevo archivo de texto llamado `hola.js`.
    
2. **Escribe el código:** Abre `hola.js` con VSCode y escribe lo siguiente:
    

JavaScript

```
// hola.js
// Este código se ejecutará con Node.js, no en el navegador.

var mensaje = "¡Hola desde Node.js! ¡Mi primer programa fuera de la web!";

console.log(mensaje);

// Una pequeña operación
console.log("El resultado es: " + (5 * 8)); 
```

3. **Ejecuta el código:**
    
    - Abre tu Terminal (o Símbolo del Sistema).
        
    - Navega a la carpeta donde guardaste `hola.js` (ej: `cd Desktop`).
        
    - Ejecuta el archivo con el comando `node`:
        

Bash

```
node hola.js
```

**Resultado en la Terminal:**

```
¡Hola desde Node.js! ¡Mi primer programa fuera de la web!
El resultado es: 40
```

¡Acabas de ejecutar código JavaScript puro en tu computadora!

---

## ✅ Resumen de Puntos Clave

- **VSCode** es nuestro editor de código esencial, que nos facilita la escritura y organización.
    
- **Node.js** es el entorno que permite a JavaScript ejecutarse fuera del navegador (en tu computadora o un servidor).
    
- La versión recomendada para Node.js es la **LTS (Long Term Support)**.
    
- La **Consola del navegador** es nuestra herramienta principal para pruebas rápidas y depuración en el contexto de una página web.
    
- Para ejecutar un archivo JavaScript (`.js`) con Node, usamos el comando `node nombreArchivo.js` en la terminal.
    

---

Ya tienes el hardware y el software listos. Ahora sí, en la próxima clase, vamos a conocer las reglas del juego: la sintaxis básica y cómo almacenar información.

👉 Continuar con [[03 - Sintaxis básica y variables]]