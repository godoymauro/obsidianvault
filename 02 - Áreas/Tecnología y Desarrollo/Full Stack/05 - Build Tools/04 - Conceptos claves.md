Con la base de tu proyecto inicializada, es crucial entender cómo manejar las librerías externas. Esta clase cubre los conceptos fundamentales que rigen la instalación y actualización de cualquier paquete en JavaScript.

---

Aquí tienes los apuntes de la clase anterior: [[03 - Inicialización de proyectos]]

# 04 - Conceptos claves

## 📝 Introducción

El verdadero poder del ecosistema JavaScript reside en su vasta comunidad y los miles de paquetes que puedes reutilizar. Para gestionar estos paquetes de forma segura y consistente, necesitamos dominar cuatro conceptos clave:

1. **Tipos de Dependencias** (`dependencies`, `devDependencies`, `peerDependencies`).
    
2. **Versionado Semántico (SemVer)**.
    
3. **Archivos Lock** (`lock files`).
    

## 🧠 Teoría: Clasificación y Control de Paquetes

### 1. Tipos de Dependencias

Al instalar un paquete, tu gestor (`npm`, `yarn`, `pnpm`) lo clasifica en una de tres categorías dentro de `package.json`:

#### A. `dependencies` (Dependencias de Producción)

- **Propósito:** Son las librerías **necesarias para que la aplicación funcione en producción** (es decir, cuando el usuario final la ejecuta en el navegador o en el servidor).
    
- **Ejemplos:** React, Vue, Axios (para peticiones HTTP), un componente UI.
    
- **Comando:** `$ npm install <paquete>` (o `$ npm i <paquete>`)
    

#### B. `devDependencies` (Dependencias de Desarrollo)

- **Propósito:** Son las librerías necesarias para **construir, probar, lintar o desarrollar** la aplicación, pero que **no se incluyen** en el _bundle_ final que ve el usuario o que no se ejecutan en producción.
    
- **Ejemplos:** Webpack, Vite, Babel, Jest (para testing), ESLint (para linting).
    
- **Comando:** `$ npm install <paquete> --save-dev` (o `$ npm i -D <paquete>`)
    

#### C. `peerDependencies` (Dependencias "Par")

- **Propósito:** Se usan principalmente cuando **tú estás creando una librería o plugin**, e indicas que el **usuario de tu librería ya debe tener instalado** otro paquete (el _peer_ o par). El gestor de paquetes no instala el _peer dependency_ automáticamente; solo advierte si falta.
    
- **Ejemplo:** Si creas un plugin para React, pondrías `react` y `react-dom` como `peerDependencies`. Así evitas tener múltiples copias de React en el _bundle_ final del usuario.
    

JSON

```
// package.json (Ejemplo)
{
  "dependencies": {
    "react": "^18.2.0",        // Necesario para que la app corra
    "axios": "^1.6.0"          // Necesario para hacer peticiones
  },
  "devDependencies": {
    "webpack": "^5.89.0",      // Necesario para construir la app
    "eslint": "^8.56.0"        // Necesario para el desarrollo y linting
  }
}
```

---

### 2. Versionado Semántico (SemVer)

**SemVer** es una regla de oro en el mundo del software. Asegura que los desarrolladores puedan predecir el impacto que tendrá una actualización de un paquete. Se define como **MAYOR.MENOR.PARCHE**.

**Formato:** `X.Y.Z`

- **X (Mayor):** Cambios que **rompen la compatibilidad** (_breaking changes_). Necesitas migrar código. (Ej: 1.0.0 a **2**.0.0).
    
- **Y (Menor):** Nuevas funcionalidades **compatibles con versiones anteriores**. (Ej: 1.1.0 a 1.**2**.0).
    
- **Z (Parche):** Correcciones de _bugs_ **compatibles con versiones anteriores**. (Ej: 1.1.1 a 1.1.**2**).
    

#### Los Símbolos de Rango (Range Operators)

Cuando instalas un paquete, el gestor añade un símbolo a la versión, indicando qué nivel de actualización es seguro aceptar:

|Símbolo|Significado|Rangos permitidos (Ej. `1.2.3`)|Impacto en la Práctica|
|---|---|---|---|
|**`^` (Caret)**|Acepta actualizaciones **MENORES** o de **PARCHE**.|`>=1.2.3 <2.0.0`|**Recomendado.** Solo acepta actualizaciones compatibles (no rompe la Mayor).|
|**`~` (Tilde)**|Acepta actualizaciones solo de **PARCHE**.|`>=1.2.3 <1.3.0`|Más estricto, generalmente usado en librerías críticas.|
|**`*` o Ninguno**|Acepta **CUALQUIER** versión (Mayor, Menor, Parche).|`>=1.2.3`|**Peligroso.** Nunca uses esto en producción ya que podría romper el build.|

Exportar a Hojas de cálculo

---

### 3. Archivos Lock (`lock files`)

El `package.json` define **rangos** de versiones (ej: `^1.2.3`). Cuando tu equipo o un servidor de CI/CD ejecuta `$ npm install`, el gestor podría instalar diferentes versiones (ej: `1.2.5` un día, y `1.2.8` otro). Esto conduce a inconsistencia ("Funciona en mi máquina, pero no en la tuya").

**La Solución: El Lock File**

- **Propósito:** El _lock file_ registra la **versión exacta y el hash criptográfico** de **cada paquete instalado** (incluyendo sus sub-dependencias).
    
- **Resultado:** Garantiza que cada vez que alguien ejecuta `install`, se instala **exactamente el mismo árbol de dependencias**, asegurando builds reproducibles.
    
- **Comandos:** Se generan y actualizan automáticamente con cada `install` o `update`.
    
- **Archivos:**
    
    - **npm:** `package-lock.json`
        
    - **Yarn:** `yarn.lock`
        
    - **pnpm:** `pnpm-lock.yaml`
        

> **REGLA DE ORO:** Siempre incluye el _lock file_ en el control de versiones (`git`).

## 🛠️ Ejemplos y Comandos Prácticos

### Comandos de Instalación con Flags

|Gestor|Instalar para Producción (`dependencies`)|Instalar para Desarrollo (`devDependencies`)|
|---|---|---|
|**npm**|`$ npm i axios`|`$ npm i -D webpack` o `$ npm i --save-dev webpack`|
|**Yarn**|`$ yarn add axios`|`$ yarn add -D webpack` o `$ yarn add --dev webpack`|
|**pnpm**|`$ pnpm add axios`|`$ pnpm add -D webpack`|

Exportar a Hojas de cálculo

### Ejemplo de Instalación

1. Instalas una librería para desarrollo y una para producción:
    
    Bash
    
    ```
    $ npm i react
    $ npm i -D vite
    ```
    
2. Tu `package.json` se actualiza:
    
    JSON
    
    ```
    {
      "dependencies": {
        "react": "^18.2.0" 👈 Por defecto, npm usa ^
      },
      "devDependencies": {
        "vite": "^5.0.10"  👈 En desarrollo
      }
    }
    ```
    
3. Automáticamente, el archivo `package-lock.json` se crea con las versiones exactas (`18.2.0` y `5.0.10`) y con el árbol completo de todas las sub-dependencias de `react` y `vite`.
    

---

## 🛑 Errores Comunes

|Error|Descripción|Solución|
|---|---|---|
|**"Error: EPERM: operation not permitted, rename..."**|Suele ocurrir cuando un proceso o antivirus bloquea la carpeta `node_modules` durante la instalación en Windows.|Intenta ejecutar la terminal como administrador o reinicia la máquina.|
|**"Dependencia fantasma"**|Tu código usa una librería que funciona en tu máquina, pero no la instalaste, sino que es una sub-dependencia de otra.|Instala explícitamente la librería usando `$ npm i <paquete>` y agrégala a `package.json`. **pnpm** ayuda a prevenir esto.|
|**Inconsistencias de Build**|El build funciona en tu máquina, pero falla en el servidor de CI/CD.|El **lock file** no se incluyó en el repositorio o está desactualizado. Elimina `node_modules` y `lock file` y vuelve a ejecutar `install`.|

Exportar a Hojas de cálculo

## 📚 Resumen de Puntos Clave

- **`dependencies`**: Necesarias para que la app corra en producción.
    
- **`devDependencies`**: Solo para tareas de desarrollo (build, test, lint).
    
- **SemVer** (`MAYOR.MENOR.PARCHE`): Define reglas para actualizaciones. El símbolo `^` es el más común y seguro.
    
- Los **Lock Files** (`package-lock.json`, `yarn.lock`, `pnpm-lock.yaml`) garantizan que las instalaciones sean 100% reproducibles y consistentes en todos los entornos.
    

---

Ahora que entendemos cómo se gestionan los paquetes, es hora de poner el gestor a trabajar con la automatización de tareas.

[[05 - Scripts npm]]