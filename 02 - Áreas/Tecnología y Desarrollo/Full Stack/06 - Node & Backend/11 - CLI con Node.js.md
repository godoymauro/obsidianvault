¡Claro! Hemos cubierto cómo Node.js funciona como servidor, cómo gestiona flujos de datos grandes, y cómo se comunica internamente con eventos. Pero Node.js no es solo para el backend web; es una excelente herramienta para crear utilidades y scripts que corren en tu propia terminal.

Esta última clase del Nivel 2 te enseñará a construir herramientas de línea de comandos (CLI).

Aquí tienes la nota para la clase anterior: **[[10 - Eventos y EventEmitter]]**.

---

# 11 - CLI con Node.js

## 📚 Introducción: ¿Por qué Node.js para CLI?

Un **CLI (Command Line Interface)** es un programa que se ejecuta directamente desde la terminal (ej: `git`, `npm`, `node`). Node.js es ideal para esto porque:

1. **Acceso a NPM:** Puedes usar miles de paquetes NPM que ya conoces.
    
2. **Rendimiento:** Es rápido para tareas de I/O (lectura/escritura de archivos, peticiones a APIs externas).
    
3. **Ubicuidad:** Node.js está instalado en casi todos los entornos de desarrollo moderno.
    

Aprender a crear tu propia herramienta CLI te permite automatizar tareas específicas de tu proyecto, como generar archivos, manipular datos o ejecutar despliegues locales.

---

## 🛠 Paso 1: El Shebang y la Ejecución

Para que tu archivo JavaScript sea ejecutable directamente como un comando, necesita dos cosas:

### 1. El Shebang (Línea Mágica)

Esta es la primera línea del archivo, que le dice al sistema operativo qué intérprete debe usar para ejecutar el script.

**Archivo:** `mi-cli.js`

JavaScript

```
#!/usr/bin/env node 
// ¡Importante! Siempre debe ser la primera línea

console.log("¡Hola desde mi primera herramienta CLI!");
```

### 2. Permisos de Ejecución

Debes darle permisos de ejecución al archivo en Linux/macOS:

Bash

```
chmod +x mi-cli.js
```

Ahora puedes ejecutarlo directamente (si estás en la misma carpeta):

Bash

```
./mi-cli.js
# Salida: ¡Hola desde mi primera herramienta CLI!
```

---

## 👂 Paso 2: Leyendo Argumentos de Entrada

La mayoría de los programas CLI necesitan leer información proporcionada por el usuario (ej: `git commit -m "Mensaje"`). Node.js te da acceso a estos argumentos a través del objeto global **`process`**.

La propiedad clave es **`process.argv`**: un _array_ que contiene los argumentos de la línea de comandos.

|Índice|Contenido|Ejemplo|
|---|---|---|
|**`process.argv[0]`**|Ruta al ejecutable de `node`.|`/usr/bin/node`|
|**`process.argv[1]`**|Ruta al script JS que se está ejecutando.|`/path/to/mi-cli.js`|
|**`process.argv[2]` en adelante**|Los argumentos reales que pasó el usuario.|`generate`, `--file`, `datos.txt`|

### Ejemplo de Lectura de Argumentos

Modifica `mi-cli.js`:

JavaScript

```
#!/usr/bin/env node

const args = process.argv.slice(2); // Ignoramos las dos primeras entradas
const comando = args[0]; 
const nombre = args[1];

if (comando === 'saludar' && nombre) {
    console.log(`¡Hola, ${nombre}! Bienvenido a mi CLI.`);
} else if (comando === 'version') {
    console.log(`Versión: 1.0.0`);
} else {
    console.log(`Uso: ./mi-cli.js saludar <nombre>`);
    console.log(`O:   ./mi-cli.js version`);
}
```

**Ejecución:**

Bash

```
./mi-cli.js saludar Juan
# Salida: ¡Hola, Juan! Bienvenido a mi CLI.

./mi-cli.js version
# Salida: Versión: 1.0.0
```

---

## 📦 Paso 3: Usando Paquetes para Argumentos Avanzados

Leer `process.argv` directamente puede volverse tedioso cuando manejas muchas banderas (`--version`), _flags_ cortos (`-v`) o argumentos con valores.

Para esto, la comunidad ha creado librerías especializadas. La más popular y recomendada es **`yargs`** o la más simple **`minimist`**. Usaremos `yargs` por su robustez.

1. Inicializa tu proyecto (si no lo has hecho) e instala `yargs`:
    
    Bash
    
    ```
    npm init -y
    npm install yargs
    ```
    
2. Crea el archivo `yargs-cli.js`:
    

JavaScript

```
#!/usr/bin/env node

const yargs = require('yargs/yargs');
const { hideBin } = require('yargs/helpers');

// Usamos el API de yargs para definir comandos y opciones
yargs(hideBin(process.argv))
    .command(
        'generate <type>', // Definición del comando y el argumento obligatorio
        'Genera un archivo o componente.', // Descripción
        (yargs) => {
            // Opciones específicas para este comando
            return yargs
                .positional('type', {
                    describe: 'El tipo de elemento a generar (ej: route, model)',
                    type: 'string'
                })
                .option('n', {
                    alias: 'name',
                    describe: 'Nombre para el archivo generado',
                    type: 'string',
                    demandOption: true // Hace que esta opción sea obligatoria
                });
        },
        (argv) => {
            // Lógica de ejecución del comando 'generate'
            console.log(`Generando ${argv.type} con el nombre: ${argv.name}.js`);
            // Aquí llamarías a fs.writeFile()
        }
    )
    .option('verbose', { // Opción global para toda la CLI
        alias: 'v',
        type: 'boolean',
        description: 'Muestra información detallada de la ejecución'
    })
    .demandCommand(1, 'Debes ingresar al menos un comando.') // Requiere al menos un comando
    .help() // Habilita el comando --help
    .argv;
```

### Ejecución con `yargs`

Bash

```
node yargs-cli.js generate route --name usuarios
# Salida: Generando route con el nombre: usuarios.js

node yargs-cli.js --help
# Salida: Muestra la ayuda generada automáticamente por yargs.
```

---

## 🌐 Publicando tu Herramienta CLI

Si tu herramienta es útil, puedes hacer que se instale globalmente en cualquier máquina usando `npm`.

1. Añade la sección **`bin`** a tu `package.json`:
    
    JSON
    
    ```
    // package.json
    "name": "mi-utilidad-cli",
    "version": "1.0.0",
    "main": "index.js",
    "bin": {
      "miclean": "./mi-cli.js" // Nombre del comando: archivo a ejecutar
    },
    // ...
    ```
    
2. Instala el paquete globalmente (para probarlo):
    
    Bash
    
    ```
    npm install -g .  // El punto significa "el paquete en esta carpeta"
    ```
    
3. Ahora, el comando `miclean` estará disponible en cualquier parte de tu terminal.
    
    Bash
    
    ```
    miclean saludar Pedro
    ```
    

---

## ✅ Buenas Prácticas y Consejos

|Categoría|Buena Práctica|Error Común|
|---|---|---|
|**Argumentos**|Usar librerías como `yargs` o `commander` para manejar argumentos complejos de manera robusta.|Intentar parsear manualmente todos los argumentos y banderas con `process.argv` y _regex_.|
|**I/O**|Usar módulos nativos como `fs` y `path` para manejar archivos locales.|Usar la CLI para tareas de servidor web que deberían ser manejadas por Express/HTTP.|
|**Interactividad**|Usar librerías como `inquirer` para hacer preguntas interactivas al usuario (útil para `npm init`).|Obligar al usuario a pasar 10 argumentos en lugar de guiarlo con preguntas.|
|**Estabilidad**|Usar `try...catch` en la lógica de tu CLI para reportar errores amigables en lugar de mensajes de _stack trace_ de Node.js.|Dejar errores sin manejar, exponiendo el código interno.|

---

## 🔑 Resumen de Puntos Clave

- Node.js es una herramienta poderosa para construir **CLIs (Command Line Interfaces)**.
    
- La línea **`#!/usr/bin/env node` (Shebang)** y los permisos de ejecución hacen que el script sea ejecutable directamente.
    
- **`process.argv`** contiene los argumentos de entrada.
    
- Librerías como **`yargs`** simplifican el parseo de comandos, subcomandos, banderas y opciones.
    
- La sección **`bin`** en `package.json` permite instalar tu herramienta globalmente a través de NPM.
    

---

# 🚀 Nivel 2 Completado

¡Felicidades! Has completado el Nivel 2. Tienes una base sólida en la asincronía y el ecosistema de Node.js.

Ahora, pasamos al **Nivel 3: Express.js y APIs REST**, donde aplicaremos todo este conocimiento para construir la aplicación backend más común: una API.

Tu próxima clase será: **[[12 - Introducción a Express]]**.