¡Perfecto! El inicio fue excelente. Ahora que entendemos **qué es Node.js** y su filosofía, el siguiente paso lógico es prepararnos para trabajar con él.

Esta clase es crucial: si el entorno no está bien configurado, tendremos problemas más adelante.

Aquí tienes la nota para la clase anterior: [[01 - Introducción a Node.js]]

---

# 02 - Instalación y configuración

## 📚 Introducción: Un Entorno Limpio

Antes de escribir código, debemos instalar las herramientas esenciales. En el desarrollo backend, es común trabajar con diferentes proyectos que requieren distintas **versiones de Node.js**. Si instalamos una versión globalmente, podríamos romper la compatibilidad de un proyecto antiguo o uno nuevo.

La solución es usar un **Gestor de Versiones de Node (NVM)**.

---

## 🛠 Herramientas Esenciales

|Herramienta|Propósito|Por Qué la Usamos|
|---|---|---|
|**Node.js**|El entorno de ejecución.|Necesario para correr nuestro código JavaScript en el servidor.|
|**npm**|Gestor de paquetes de Node.js.|Instala librerías (_paquetes_ o _dependencias_) y gestiona el proyecto. **Viene incluido con Node.js.**|
|**nvm** (Node Version Manager)|Gestiona múltiples versiones de Node.js.|Nos permite cambiar de versión rápidamente entre proyectos, evitando conflictos de compatibilidad.|

Exportar a Hojas de cálculo

---

## 💻 Paso 1: Instalación de nvm (Recomendado)

Recomiendo encarecidamente usar **nvm** si estás en **Linux o macOS**. Para **Windows**, la alternativa más popular es **nvm-windows**.

### 🍏🐧 Instalación en macOS/Linux (Usando `nvm`)

1. **Abre tu Terminal** (Bash o Zsh).
    
2. Ejecuta el siguiente comando para descargar e instalar `nvm`. Este comando puede cambiar, siempre es bueno revisar el [repositorio oficial de nvm](https://github.com/nvm-sh/nvm).
    
    Bash
    
    ```
    # Descargar y ejecutar el script de instalación:
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
    ```
    
3. **Reinicia tu terminal** o ejecuta el comando `source ~/.bashrc` (o `~/.zshrc`, dependiendo de tu shell) para que los cambios surtan efecto.
    
4. Verifica la instalación:
    
    Bash
    
    ```
    nvm --version
    ```
    

### 🪟 Instalación en Windows (Usando `nvm-windows`)

1. Ve al [repositorio oficial de nvm-windows](https://github.com/coreybutler/nvm-windows).
    
2. Descarga el archivo **`nvm-setup.zip`** desde la sección de _releases_.
    
3. Descomprime y ejecuta el instalador. Sigue las instrucciones.
    
4. Verifica la instalación abriendo una **Terminal nueva (CMD o PowerShell)** y ejecutando:
    
    Bash
    
    ```
    nvm version
    ```
    

---

## 🚀 Paso 2: Instalación de Node.js y npm

Una vez que tenemos nuestro gestor de versiones, instalar Node.js es trivial.

### 1. Instalar la Versión LTS

Siempre usaremos la versión **LTS (Long Term Support)**, ya que es la más estable y recomendada para producción.

Bash

```
# En terminal (Linux/macOS/Windows)
nvm install --lts

# Este comando instalará la última versión LTS disponible, por ejemplo:
# nvm install 20
```

### 2. Establecer la Versión por Defecto

Le decimos a `nvm` que use esta versión para todas las nuevas terminales:

Bash

```
nvm alias default lts/gallium # (o la versión que se haya instalado, por ejemplo, 20)
nvm use default
```

### 3. Verificar las Versiones

Verifica que Node.js y npm están instalados correctamente:

Bash

```
# Debería mostrar la versión LTS instalada
node -v

# Debería mostrar la versión de npm (viene con Node.js)
npm -v
```

### Tabla de Comandos Clave de `nvm`

|Comando|Descripción|Ejemplo|
|---|---|---|
|`nvm install <version>`|Instala una versión específica.|`nvm install 18.17.0`|
|`nvm use <version>`|Cambia a una versión ya instalada.|`nvm use 18`|
|`nvm ls`|Lista todas las versiones instaladas.|`nvm ls`|
|`nvm alias default <version>`|Establece la versión predeterminada.|`nvm alias default 20`|

Exportar a Hojas de cálculo

---

## 📦 Paso 3: Entendiendo npm (Node Package Manager)

**npm** es la herramienta más importante de nuestro ecosistema. Cuando decimos "npm", hablamos de dos cosas:

1. **La Herramienta CLI:** El programa que ejecutamos en la terminal (`npm install`, `npm start`, etc.).
    
2. **El Registro:** La base de datos más grande del mundo de paquetes de código reutilizable (librerías).
    

### Inicialización de un Proyecto

Cada proyecto Node.js debe tener un archivo `package.json`. Este archivo es el **manifiesto** de tu proyecto.

1. Crea una carpeta de proyecto:
    
    Bash
    
    ```
    mkdir mi-proyecto-backend
    cd mi-proyecto-backend
    ```
    
2. Inicializa el proyecto:
    
    Bash
    
    ```
    # El flag -y (yes) acepta las opciones por defecto
    npm init -y
    ```
    

El archivo `package.json` se verá así:

JSON

```
{
  "name": "mi-proyecto-backend",
  "version": "1.0.0",
  "description": "",
  "main": "index.js", // El punto de entrada principal
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

### Gestión de Dependencias

Aquí es donde `npm` brilla.

|Comando|Propósito|¿Qué hace?|
|---|---|---|
|`npm install <package>`|Instala una dependencia principal (para el código en ejecución).|Agrega el paquete a la sección `"dependencies"` de `package.json`.|
|`npm install -D <package>`|Instala una dependencia de desarrollo.|Agrega el paquete a la sección `"devDependencies"` (para _testing_, _linting_, etc.).|
|`npm install`|Instala todas las dependencias.|Lee el `package.json` e instala todo en la carpeta `node_modules`.|

Exportar a Hojas de cálculo

**Importante:** Nunca subas la carpeta **`node_modules`** a Git. Es inmensa y `package.json` es suficiente para recrearla en cualquier máquina.

---

## 🛡 Buenas Prácticas y Errores Comunes

|Categoría|Buena Práctica|Error Común|
|---|---|---|
|**Versiones**|**Usar `nvm`**. Siempre trabajar con la versión LTS para entornos de producción.|Instalar Node.js directamente desde el sitio web sin `nvm`. Esto dificulta el cambio de versiones.|
|**`npm install`**|Instalar dependencias sin flags (`npm install <package>`) para dependencias de _runtime_.|Olvidar el flag `-D` o `--save-dev` para herramientas de desarrollo (como Jest para _testing_).|
|**`package.json`**|Mantener el `package.json` actualizado y bien documentado. Definir _scripts_ claros (`"start"`, `"dev"`).|No tener un `package.json` o modificarlo manualmente sin usar los comandos de `npm`.|

Exportar a Hojas de cálculo

---

## 🔑 Resumen de Puntos Clave

- **nvm** es tu mejor amigo para gestionar versiones de Node.js.
    
- La versión **LTS** es la estándar para proyectos estables.
    
- **npm** es el gestor de paquetes que usamos para descargar librerías externas.
    
- **`package.json`** es el corazón de tu proyecto, listando todas las dependencias y _scripts_.
    
- La carpeta **`node_modules`** contiene el código de todas las dependencias y debe ser ignorada por Git.
    

Con tu entorno listo, podemos sumergirnos en la arquitectura de Node.js. El corazón de por qué es tan rápido y eficiente está en su modelo de ejecución.

Tu siguiente clase te revelará el secreto: **[[03 - Node.js Runtime]]**.