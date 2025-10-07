Después de los gigantes Vite y Webpack, es hora de explorar las alternativas que buscan simplificar el _bundling_: **Parcel** y las herramientas ultra-veloces como **esbuild**.

---

# 12 - Parcel

## 📝 Introducción

**Parcel** fue una de las primeras herramientas modernas en popularizar el concepto de **Zero-Config** (_Cero Configuración_). Su objetivo es hacer que el _bundling_ sea tan simple como apuntar a tu punto de entrada y dejar que la herramienta haga el resto. Si buscas un _build_ rápido para un proyecto pequeño o mediano sin tocar un archivo de configuración, Parcel es una excelente opción.

## 🧠 Teoría: La Filosofía Zero-Config

### 1. Detección Automática

Parcel funciona analizando tus dependencias **sin necesidad de _Loaders_ o _Plugins_**.

- Si importa un archivo `.scss`, Parcel buscará automáticamente si tienes `sass` instalado y lo usará para procesar el archivo.
    
- Si importa un archivo `.jsx`, buscará las configuraciones de Babel si existen, o usará su transpilador interno para convertirlo.
    

Esta automatización reduce la curva de aprendizaje y acelera el inicio del proyecto. No necesitas un `webpack.config.js` ni un `vite.config.js` para empezar.

### 2. Procesamiento de Activos

Parcel maneja automáticamente muchos tipos de activos:

- **CSS / SASS / Less:** Los procesa y los inyecta.
    
- **Imágenes:** Optimiza las imágenes y les asigna un hash para el _caching_.
    
- **Archivos estáticos:** Mueve y renombra fuentes y otros archivos.
    

### 3. HMR y Multicore

- **HMR Rápido:** Al igual que Vite, Parcel usa Hot Module Replacement (HMR) para recargar solo los módulos que cambian, aunque su arquitectura es diferente a la de ESM nativo de Vite.
    
- **Procesamiento en paralelo:** Parcel aprovecha múltiples núcleos de CPU para ejecutar la transpilación y otras tareas de forma concurrente, lo que contribuye a su velocidad de _build_.
    

|Característica|Webpack|Vite|Parcel|
|---|---|---|---|
|**Configuración**|Explícita y Compleja|Moderada (basado en `vite.config.js`)|**Zero-Config** (Mínima)|
|**Arquitectura Dev**|Bundle-First|No-Bundle (ESM Nativo)|Bundle-First (Rápido por multicore)|
|**Transpilación**|Babel Loader|esbuild|Transpiladores internos|
|**Recomendado**|Proyectos Legacy / Control Máximo|Proyectos Nuevos / Velocidad Dev|Proyectos Simples / Prototipos Rápido|

Exportar a Hojas de cálculo

---

## 🛠️ Ejemplos y Comandos Prácticos

### Paso 1: Instalación

Parcel es fácil de instalar y usar, ya que la CLI y el _bundler_ están en un solo paquete.

Bash

```
$ npm i -D parcel
```

### Paso 2: Ejecución (Zero-Config)

Solo necesitas apuntar a tu archivo HTML principal (el punto de entrada de la aplicación). Parcel es inteligente: si el HTML importa un JS, y ese JS importa un CSS, Parcel bundeará todo.

**`package.json`**

JSON

```
{
  "scripts": {
    "start": "parcel src/index.html", // Inicia el servidor dev y HMR
    "build": "parcel build src/index.html" // Genera el build optimizado
  }
}
```

**Estructura de Archivos:**

```
/src
  - index.html 👈 Apuntamos aquí
  - index.js   👈 Importado en index.html
  - styles.css 👈 Importado en index.js
```

**Ejecución:**

Bash

```
# Inicia el servidor de desarrollo, HMR y build automático
$ npm start 
# Parcel detecta .html, luego .js, luego .css, y configura todo.
```

### Paso 3: Configuración Mínima (Sobrescribiendo)

Aunque Parcel es _zero-config_, puedes añadir configuraciones específicas si lo necesitas (por ejemplo, para configurar Babel). Parcel respeta archivos de configuración estándar como `.babelrc` o `postcss.config.js`.

**Ejemplo de Transpilación (Automática):**

1. Creas un archivo `.babelrc` para especificar _presets_ (ej: para compatibilidad con navegadores antiguos).
    
2. Parcel detectará la presencia de `.babelrc` y automáticamente usará Babel con esa configuración al procesar tus archivos JS/JSX. No necesitas configurarlo en Parcel.
    

---

# 13 - esbuild y Snowpack

## 📝 esbuild: El Foco en la Velocidad

**esbuild** no es un servidor de desarrollo completo como Vite o Parcel, sino un **bundler y transpilador** escrito en **Go**. La principal ventaja de Go sobre JavaScript (incluso en Node.js) es la velocidad: **esbuild es órdenes de magnitud más rápido que Webpack y Rollup**.

### Uso de esbuild

esbuild es ideal para:

1. **Build Scripts Rápidos:** Reemplazar Rollup o Babel para tareas de _bundling_ sencillas en librerías o CI/CD.
    
2. **Dev Servers (detrás de escenas):** Vite usa esbuild para pre-empaquetar dependencias y para una transpilación _on-the-fly_ muy rápida.
    

**Comando de Ejemplo (Build Script puro):**

Bash

```
# Instalar esbuild como binario
$ npm i -D esbuild

# Ejecutar un build simple
$ esbuild src/index.js --bundle --minify --sourcemap --outfile=dist/bundle.js

# Es tan rápido que a menudo se usa directamente en scripts npm para librerías.
```

## 📝 Snowpack (Legacy, Enfoque Precursor)

**Snowpack** fue un precursor crucial de Vite. También adoptó la estrategia de servir módulos ESM nativos sin _bundling_ en desarrollo.

- **Impacto Histórico:** Snowpack demostró que el enfoque **No-Bundle** en desarrollo era viable y rápido.
    
- **Estado Actual:** El proyecto está **descontinuado** a favor de que la comunidad se concentre en **Vite**, que adoptó y mejoró la arquitectura de ESM nativo. No se recomienda su uso en proyectos nuevos.
    

## 📚 Comparación Final de Bundlers Modernos

|Herramienta|Arquitectura Clave|Punto Fuerte|Configuración|
|---|---|---|---|
|**Vite**|No-Bundle (ESM Nativo + Rollup)|**Velocidad de Dev y Build**|Plugin-based|
|**Webpack**|Bundle-First (Grafo de Dependencias)|**Control Granular y Ecosistema**|Configuración Alta|
|**Parcel**|Zero-Config (Multicore + Auto-Detección)|**Simplicidad y Facilidad de Uso**|Zero-Config|
|**esbuild**|Go-Based Transpilation & Bundling|**Velocidad Pura (Motor)**|Usado internamente / Build Scripts|

Exportar a Hojas de cálculo

---

## 🛑 Errores Comunes

|Error|Descripción|Solución|
|---|---|---|
|**Parcel no procesa SASS**|Aunque es _zero-config_, Parcel necesita que el paquete relevante esté instalado.|Asegúrate de instalar `$ npm i -D sass` (o el preprocesador que uses).|
|**esbuild: "Error: Could not resolve..."**|Esbuild necesita rutas relativas y explícitas para los módulos locales (`./file.js`).|Revisa que los `import` estén correctos; esbuild es muy estricto con la resolución.|
|**El build de Parcel es demasiado grande**|Puede que no esté aplicando _Tree Shaking_ o _Code Splitting_ de forma efectiva en un caso complejo.|Considera migrar a Vite/Rollup si el proyecto crece y necesitas optimizaciones más sofisticadas.|

Exportar a Hojas de cálculo

## 📚 Resumen de Puntos Clave

- **Parcel** ofrece _bundling_ **Zero-Config**, detectando y procesando automáticamente los activos con poca o ninguna configuración manual.
    
- **esbuild** es un _bundler/transpilador_ escrito en Go, conocido por ser **el motor más rápido** del ecosistema. Vite lo usa internamente.
    
- **Snowpack** fue clave, pero está descontinuado; **Vite** es el sucesor dominante de la arquitectura No-Bundle.
    

---

Con esto, cerramos la introducción a los _bundlers_. En el siguiente nivel, profundizaremos en la optimización avanzada que estas herramientas nos permiten lograr.

[[14 - Code splitting y lazy loading]]