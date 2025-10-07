¡Continuemos! Has llegado a un punto clave: sabes cómo funciona Node.js y cómo crear un servidor, pero no puedes construir una aplicación moderna solo con módulos nativos. Necesitarás librerías externas.

Esta clase se centrará en la herramienta que hace al ecosistema de Node.js tan poderoso: el gestor de paquetes.

Aquí tienes la nota para la clase anterior: [[06 - Módulo HTTP]]

---

# 07 - Gestión de dependencias

## 📚 Introducción: El Poder del Ecosistema

Uno de los mayores atractivos de Node.js es **NPM (Node Package Manager)**. El ecosistema de NPM es el más grande del mundo, con millones de paquetes (librerías) que resuelven prácticamente cualquier problema que puedas imaginar.

La **gestión de dependencias** es el proceso de:

1. Definir qué librerías necesita tu proyecto.
    
2. Instalarlas en la carpeta local `node_modules`.
    
3. Guardar las versiones exactas para asegurar que el proyecto funcione de la misma manera en cualquier máquina.
    

Esto se realiza principalmente a través de un archivo: el **`package.json`**, y de herramientas como `npm`, `yarn` o `pnpm`.

---

## 📄 El Archivo `package.json`: El Manifiesto del Proyecto

Ya lo inicializamos en [[02 - Instalación y configuración]], pero ahora veremos sus secciones cruciales.

### 1. Metadatos del Proyecto

Define la información básica de tu aplicación:

JSON

```
{
  "name": "mi-proyecto-backend",
  "version": "1.0.0",
  "description": "Una API REST sencilla construida con Node.js",
  "main": "index.js", // Punto de entrada principal
  "type": "module", // ¡Recuerda esto para usar ESM!
  "author": "Tu Nombre",
  "license": "ISC"
}
```

### 2. Scripts Personalizados

Esta sección es fundamental para automatizar tareas. En lugar de recordar comandos largos, los defines aquí:

JSON

```
"scripts": {
  // Comando: "npm run start"
  "start": "node index.js", 
  
  // Comando: "npm run dev" (usamos una dependencia de desarrollo, Nodemon)
  "dev": "nodemon index.js", 
  
  // Comando: "npm test"
  "test": "jest" 
}
```

**Para ejecutar:** Simplemente usas `npm run <nombre_del_script>`. Para `start` y `test` puedes omitir `run` (`npm start`, `npm test`).

### 3. Las Dependencias

Aquí se listan todas las librerías necesarias, categorizadas por su rol:

|Sección|Descripción|Ejemplo de Uso|
|---|---|---|
|**`dependencies`**|Librerías que el proyecto necesita para **ejecutarse** correctamente en producción (el _runtime_).|**Express** (Framework), **Mongoose** (ODM para MongoDB).|
|**`devDependencies`**|Librerías solo necesarias durante el **desarrollo, testing o construcción** del proyecto. No se instalan en entornos de producción (si usas `npm install --production`).|**Jest** (Testing), **Nodemon** (Recarga automática), **ESLint** (Linting).|

---

## 📦 Gestores de Paquetes: npm, yarn y pnpm

`npm` es el gestor por defecto, pero han surgido alternativas que mejoran la velocidad y la eficiencia del espacio en disco.

### Comandos de Instalación Esenciales

|Acción|NPM (Node Package Manager)|Yarn|PNPM (Performant NPM)|
|---|---|---|---|
|**Instalar Dependencia**|`npm install express`|`yarn add express`|`pnpm add express`|
|**Instalar Dependencia de Desarrollo**|`npm install -D jest`|`yarn add -D jest`|`pnpm add -D jest`|
|**Instalar Todo (setup)**|`npm install`|`yarn install`|`pnpm install`|
|**Eliminar Dependencia**|`npm uninstall express`|`yarn remove express`|`pnpm remove express`|

**Mi recomendación:** Comienza con **NPM** (ya lo tienes). Si en un futuro quieres optimizar espacio o velocidad, migra a **PNPM**.

---

## 📌 SemVer: El Sistema de Versionado Semántico

Cuando instalas una dependencia, verás símbolos extraños delante de los números de versión en tu `package.json`. Esto es **Semantic Versioning (SemVer)**, y es CRUCIAL para la estabilidad de tu proyecto.

Un número de versión se escribe como `MAYOR.MENOR.PARCHE` (ej: `4.18.2`).

|Símbolo|Significado|¿Qué instala?|
|---|---|---|
|**`^` (Caret)**|**Recomendado.** Permite la instalación de nuevas versiones **MENORES** y de **PARCHE**.|`^4.18.2` instalaría `4.19.0` o `4.18.3`, pero _nunca_ `5.0.0`.|
|**`~` (Tilde)**|Permite solo la instalación de nuevas versiones de **PARCHE**. Más estricto.|`~4.18.2` instalaría `4.18.3`, pero _nunca_ `4.19.0`.|
|**Ninguno**|**Exacta.** Instala solo la versión exacta.|`4.18.2` instalaría solo `4.18.2`.|

### 🚨 La Regla de SemVer

- **MAYOR (4.x.x):** Contiene **cambios que rompen la compatibilidad** (_breaking changes_). Necesitas revisar tu código al actualizar.
    
- **MENOR (x.18.x):** Contiene nuevas funcionalidades, pero **es compatible** hacia atrás.
    
- **PARCHE (x.x.2):** Contiene correcciones de bugs y seguridad, pero **es compatible** hacia atrás.
    

**Buena Práctica:** Usar `^` es la opción estándar, ya que te da acceso a mejoras y correcciones de seguridad sin introducir _breaking changes_ (en teoría).

---

## 🔒 `package-lock.json` y `node_modules`

Cuando ejecutas `npm install`, se generan dos cosas:

1. **`node_modules/`:** La carpeta donde se descarga físicamente todo el código de las dependencias.
    
2. **`package-lock.json`:** Un archivo generado automáticamente que **bloquea (lock)** las versiones exactas y anidadas de _cada_ dependencia instalada.
    

**El Flujo:**

1. `package.json` dice: "Quiero Express `^4.0.0`".
    
2. `package-lock.json` dice: "Instalé exactamente `express@4.18.2` y sus 5 dependencias en la versión **exacta** 1.2.3, 4.5.6..."
    

El archivo **`package-lock.json`** garantiza que la aplicación se construya de forma **idéntica** en tu máquina, en la de un compañero, y en el servidor de producción. **Siempre debes incluir `package-lock.json` en tu repositorio de Git.**

---

## ✅ Buenas Prácticas y Errores Comunes

|Categoría|Buena Práctica|Error Común|
|---|---|---|
|**`node_modules`**|**Ignorar la carpeta `node_modules`** en Git (`.gitignore`). Es gigantesca y es reproducible con `npm install`.|Subir `node_modules` al repositorio, llenando el historial de Git innecesariamente.|
|**`package-lock`**|Incluir **`package-lock.json`** en el repositorio (Commit y Push).|Olvidar el _lock file_, causando inconsistencias de versiones entre entornos.|
|**Instalación**|Usar el flag `-D` (`--save-dev`) para herramientas de desarrollo.|Instalar librerías de testing o _linting_ como dependencias de producción, inflando la aplicación final.|
|**Scripts**|Definir un script `start` que apunte al archivo principal de tu aplicación (ej: `"start": "node index.js"`).|No usar scripts y obligar a otros a recordar comandos largos o rutas de archivo.|

---

## 🔑 Resumen de Puntos Clave

- **NPM** es el gestor de paquetes y el repositorio central de librerías para Node.js.
    
- El archivo **`package.json`** es el manifiesto que define metadatos, scripts y dependencias.
    
- **`dependencies`** son para producción, **`devDependencies`** son para desarrollo.
    
- **SemVer** (Mayor.Menor.Parche) y el símbolo **`^`** te ayudan a mantener la estabilidad.
    
- **`package-lock.json`** garantiza la reproducibilidad exacta de la instalación de todas las dependencias.
    

¡Con esto, has cubierto el primer nivel de fundamentos! Sabes qué es Node, cómo funciona, y cómo manejar sus herramientas clave.

Ahora, pasamos al **Nivel 2: JavaScript avanzado en Node**, donde dominarás el concepto más importante de su arquitectura. Tu próxima clase: **[[08 - Asincronía en Node.js]]**.