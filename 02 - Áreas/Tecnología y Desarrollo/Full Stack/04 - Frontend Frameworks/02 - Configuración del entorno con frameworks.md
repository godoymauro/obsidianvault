El siguiente paso es preparar nuestro "taller" de desarrollo. No podemos construir nada sin las herramientas adecuadas.

---

Aquí tienes los apuntes de la clase anterior: [[01 - Introducción a Frontend Frameworks]]

## 02 - Configuración del Entorno

### Introducción al tema: Contexto y Utilidad

Antes de escribir nuestro primer componente, necesitamos un entorno moderno que haga el trabajo pesado por nosotros: transpilar el código JSX/templates a JS puro, optimizar, refrescar el navegador automáticamente, y gestionar dependencias.

Configurar un entorno de desarrollo puede ser históricamente tedioso (Webpack, Babel, etc.). Afortunadamente, en 2025, herramientas como **Vite** han simplificado esto enormemente, permitiéndonos enfocarnos en el código y no en la configuración.

### Prerrequisitos

- Haber comprendido la importancia de la programación basada en componentes (ver [[01 - Introducción a Frontend Frameworks]]).
    
- Conocimiento básico de la línea de comandos (Terminal/CMD).
    

### Explicación Completa y Extendida

La configuración moderna del Frontend se basa en tres pilares:

1. **Node.js y Gestores de Paquetes:** La base de nuestro _runtime_ de JavaScript fuera del navegador.
    
2. **Editores de Código:** Nuestra interfaz principal (VSCode).
    
3. **Herramientas de Build/Bundlers:** La magia que empaqueta y optimiza nuestro código para el navegador (Vite, Snowpack, Webpack).
    

#### 1. Node.js y Gestores de Paquetes (npm/Yarn/pnpm)

**Node.js** es un entorno de ejecución de JavaScript del lado del servidor. Lo usamos en Frontend para ejecutar nuestras herramientas de _build_ y para gestionar dependencias.

**npm (Node Package Manager)**, **Yarn** o **pnpm** son gestores de paquetes que nos permiten instalar librerías (como React, Vue, Axios) y herramientas de desarrollo (como Vite) de forma organizada.

|Comando Básico|Uso|
|---|---|
|`node -v`|Verifica que Node.js esté instalado.|
|`npm install react`|Instala el paquete `react` en el proyecto.|
|`npm run dev`|Ejecuta el script de desarrollo definido en el `package.json`.|

Exportar a Hojas de cálculo

#### 2. Visual Studio Code (VSCode)

Es el editor estándar de la industria. Su ecosistema de extensiones es vital.

|Extensión Recomendada|Utilidad|
|---|---|
|**ESLint**|Analiza estáticamente el código para encontrar errores y forzar buenas prácticas.|
|**Prettier**|Formatea el código de manera consistente (muy importante en equipos).|
|**React/Redux/Vue/Svelte Snippets**|Atajos de teclado para crear estructuras de código rápido.|

Exportar a Hojas de cálculo

#### 3. Herramientas de Build y Bundlers (Vite: El estándar moderno)

Una aplicación moderna no se ejecuta directamente en el navegador. Necesita ser **transpilada** (convertir JSX/ES6+ en JS compatible) y **empaquetada** (_bundle_).

- **Webpack/Parcel:** Los _bundlers_ tradicionales. Poderosos, pero lentos en grandes proyectos.
    
- **Vite (Recomendado):** El nuevo estándar. Utiliza módulos nativos de ES (ESM) para servir código en desarrollo, lo que resulta en un _Hot Module Replacement (HMR)_ **instantáneo**. Esto significa que los cambios en tu código aparecen en el navegador sin recargar la página, y lo hacen casi al instante.
    

|Característica|Webpack (tradicional)|Vite (moderno)|
|---|---|---|
|**Arranque del Servidor**|Lento (espera a empaquetar todo).|Rápido (sirve módulos nativos).|
|**HMR**|Rápido, pero puede tardar en grandes proyectos.|Instantáneo gracias a ESM nativo.|
|**Configuración**|Compleja (archivos de configuración grandes).|Mínima o nula, configurado por defecto.|

Exportar a Hojas de cálculo

#### Integración con Frameworks

Vite está diseñado para integrarse perfectamente con React, Vue y Svelte. Cuando creamos un proyecto con Vite, le decimos qué framework usar, y él se encarga de la configuración de _build_ necesaria (como Babel para React JSX o el compilador de Svelte).

### Diagrama Conceptual del Flujo de Desarrollo

Fragmento de código

```
graph TD
    A[Código Fuente: Componentes .jsx / .vue / .svelte] --> B(Servidor de Desarrollo Vite);
    B --> C{Navegador};
    C --> D(Guardar Archivo);
    D --> B;
    B --> E[HMR: Actualización Instantánea];
    E --> C;
    B -- Comando 'vite build' --> F[Bundler: Rollup (Integrado en Vite)];
    F --> G[Código Optimizado para Producción: JS, CSS, HTML];
    G --> H[Despliegue (Hosting)];
```

### Ejercicios Prácticos Paso a Paso (Usando Vite)

Vamos a crear un proyecto base para React, Vue y Svelte. **Te recomiendo encarecidamente instalar Node.js (si no lo tienes) antes de este paso.**

#### **Paso 1: Instalación de Node.js**

1. Ve a [https://nodejs.org/](https://nodejs.org/en/) y descarga la versión LTS (Recomendada para la mayoría de usuarios).
    
2. Abre tu Terminal/CMD y verifica la instalación:
    
    Bash
    
    ```
    node -v   # Debe mostrar algo como v20.x.x
    npm -v    # Debe mostrar algo como 10.x.x
    ```
    

#### **Paso 2: Crear el Proyecto con Vite (Multi-Framework)**

Vite usa un comando simple para iniciar proyectos preconfigurados.

1. **Crea una carpeta de proyectos:**
    
    Bash
    
    ```
    mkdir mi-proyecto-frontend
    cd mi-proyecto-frontend
    ```
    
2. **Ejecuta el comando de creación de Vite:**
    
    Bash
    
    ```
    npm create vite@latest
    ```
    
3. **Sigue el Asistente:**
    
    - **Project name:** `mi-primer-app-react`
        
    - **Select a framework:** `React`
        
    - **Select a variant:** `JavaScript` (o TypeScript si lo prefieres)
        
4. **Instala las dependencias y arranca el servidor:**
    
    Bash
    
    ```
    cd mi-primer-app-react
    npm install
    npm run dev
    ```
    
    Tu aplicación estará visible en `http://localhost:5173/` (o un puerto similar) y con HMR activado.
    

#### **Paso 3 (Opcional): Repetir para Vue y Svelte**

Para ver la uniformidad de la herramienta, puedes crear dos proyectos más en la carpeta principal:

- **Para Vue:**
    
    Bash
    
    ```
    npm create vite@latest
    # Project name: mi-primer-app-vue
    # Select a framework: Vue
    # Select a variant: JavaScript
    ```
    
- **Para Svelte:**
    
    Bash
    
    ```
    npm create vite@latest
    # Project name: mi-primer-app-svelte
    # Select a framework: Svelte
    # Select a variant: JavaScript
    ```
    

### Errores Comunes y Troubleshooting

|Problema Común|Causa más probable|Solución|
|---|---|---|
|`npm command not found`|Node.js no se instaló correctamente o la ruta no está configurada.|Reinicia tu Terminal/CMD. Si persiste, reinstala Node.js.|
|El navegador no se refresca (HMR no funciona).|El código tiene un error de sintaxis irrecuperable o el servidor falló al iniciar.|Revisa la consola de tu Terminal en busca de mensajes de error de compilación. Reinicia `npm run dev`.|
|Error al instalar dependencias (`npm install`).|Conflicto de versiones o problemas de permisos.|Borra la carpeta `node_modules` y el archivo `package-lock.json`, luego intenta `npm install` de nuevo. Usa `npm install --force` si es un error de dependencias.|

Exportar a Hojas de cálculo

### Resumen de Puntos Clave

- **Node.js** y **npm/Yarn/pnpm** son esenciales para ejecutar las herramientas de desarrollo y gestionar las librerías.
    
- **Vite** es la herramienta de _build_ moderna preferida, destacando por su velocidad de arranque y su **Hot Module Replacement (HMR)** instantáneo, gracias a los módulos nativos de ES.
    
- Vite facilita la creación de proyectos para **React**, **Vue** y **Svelte** con una configuración mínima.
    
- **VSCode** es el editor recomendado, potenciado por extensiones clave como **ESLint** y **Prettier**.
    
- El flujo de trabajo moderno implica que el código fuente se **transpila** y **empaqueta** antes de llegar al navegador (en producción).
    

### Enlaces Internos

👉 Ahora que tenemos el entorno, es crucial entender cómo los frameworks usan la sintaxis para describir la interfaz en [[03 - JSX y templates]].

---

**Siguiente paso:** Nuestro entorno está listo. Ahora, vamos a la parte de la sintaxis: la gran diferencia entre el **JSX** de React y los **Templates** de Vue y Svelte. Pasemos a [[03 - JSX y templates]].