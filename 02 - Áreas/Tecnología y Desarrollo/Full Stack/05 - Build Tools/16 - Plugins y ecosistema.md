Ya aseguramos la compatibilidad de nuestro código. Ahora, en esta clase, uniremos el código con las herramientas de calidad y testing (Linters, Formateadores) para crear un ecosistema de desarrollo robusto, todo mediante **Plugins y Ecosistema**.

---

# 16 - Plugins y ecosistema

## 📝 Introducción

El _bundler_ es el motor, pero el ecosistema de _plugins_ y _loaders_ es lo que le da su verdadera funcionalidad y flexibilidad. Un desarrollo moderno no solo se enfoca en el _build_ final, sino también en la **calidad del código**. Aquí aprenderemos cómo integrar herramientas como **ESLint** (para calidad) y **Prettier** (para formato) en nuestro proceso de _build_ o como _plugins_ del _bundler_.

## 🧠 Teoría: Integración de Herramientas de Calidad

Los **Linters** y **Formateadores** son cruciales en el desarrollo profesional:

1. **Linter (Ej: ESLint):** Analiza estáticamente el código para encontrar **problemas de calidad** y **errores potenciales** (ej: usar una variable antes de declararla, lógica inalcanzable, o violaciones de reglas de programación).
    
2. **Formateador (Ej: Prettier):** Se enfoca solo en el **estilo** y la **apariencia** del código (espacios, comas, puntos y comas, saltos de línea), asegurando consistencia en todo el equipo.
    

La integración puede ocurrir de tres maneras:

- **En el Editor:** (Recomendado para desarrollo diario) Se ejecuta la herramienta al guardar.
    
- **En un Script Pre-Commit:** (Recomendado para calidad de código) Se ejecuta antes de permitir un _commit_ de Git.
    
- **Como Plugin del Bundler:** (Recomendado para asegurar la limpieza del _build_) Se ejecuta durante el _build_ o desarrollo.
    

### Webpack: Loaders y Plugins para la Calidad

Webpack necesita _plugins_ específicos para integrar linters.

- **`eslint-webpack-plugin`**: Ejecuta ESLint como parte del proceso de _build_.
    

### Vite/Rollup: Plugins Rápidos

Vite es más ligero, por lo que a menudo se usa un _plugin_ que aprovecha la velocidad de **esbuild** para el _linting_ o _type checking_.

- **`vite-plugin-checker`**: Un _plugin_ muy popular que ejecuta procesos externos (como la comprobación de tipos de TypeScript o ESLint) en un proceso separado para no ralentizar el HMR de Vite.
    

---

## 🛠️ Ejemplos Prácticos de Integración

### Ejemplo 1: Integración de ESLint con Webpack

Para forzar la revisión de código en cada _build_, instalamos y configuramos el _plugin_.

1. **Instalar dependencias:**
    
    Bash
    
    ```
    $ npm i -D eslint eslint-webpack-plugin
    ```
    
2. **`webpack.config.js`:**
    
    JavaScript
    
    ```
    const ESLintPlugin = require('eslint-webpack-plugin');
    
    module.exports = {
      // ...
      plugins: [
        new ESLintPlugin({
          // Fija automáticamente errores de formato simples
          fix: true, 
          // Define qué archivos revisar
          files: 'src/**/*.{js,jsx,ts,tsx}', 
        }),
        // ... otros plugins (ej: HtmlWebpackPlugin)
      ],
      // ...
    };
    ```
    

**Resultado:** Al ejecutar `$ npm run build` o `$ npm run dev`, Webpack detendrá el proceso si encuentra errores de ESLint críticos.

### Ejemplo 2: Integración de Type Checking con Vite

Usaremos `vite-plugin-checker` para ejecutar la comprobación de tipos de TypeScript de forma concurrente, mostrando los errores en la terminal y en una superposición en el navegador.

1. **Instalar dependencias:**
    
    Bash
    
    ```
    $ npm i -D vite-plugin-checker typescript
    ```
    
2. **`vite.config.js`:**
    
    JavaScript
    
    ```
    import { defineConfig } from 'vite';
    import checker from 'vite-plugin-checker';
    
    export default defineConfig({
      plugins: [
        // Asegura que Vite haga la comprobación de tipos
        checker({ 
          typescript: true, // Habilita la comprobación de TypeScript
          eslint: {         // Opcional: Habilita ESLint también
            lintCommand: 'eslint "./src/**/*.{ts,tsx}"', 
          }
        }),
        // ... otros plugins
      ],
    });
    ```
    

**Resultado:** Al ejecutar `$ npm run dev`, Vite inicia la comprobación de tipos y el _linting_ en paralelo, sin retrasar el inicio del servidor, pero alertando inmediatamente si hay errores.

### Ejemplo 3: Scripts de Testing (Jest/Vitest)

Las herramientas de testing rara vez se integran directamente como un _plugin_ del _bundler_ (que está centrado en el código que se despliega). En su lugar, se ejecutan a través de **Scripts npm** dedicados.

JSON

```
// package.json
{
  "scripts": {
    "build": "vite build",
    "dev": "vite",
    "test": "vitest",               // Para testing rápido basado en Vite (recomendado)
    "test:coverage": "vitest run --coverage",
    "lint": "eslint src --ext .js,.jsx,.ts,.tsx", // Script dedicado de linting
    "format": "prettier --write src" // Script dedicado de formateo
  }
}
```

**Comandos:**

Bash

```
# Ejecuta los tests
$ npm test 

# Ejecuta solo el linter
$ npm run lint
```

---

## 🛑 Errores Comunes

|Error|Descripción|Solución|
|---|---|---|
|**Linter / Checker es lento**|El _plugin_ del _bundler_ está obligando a una comprobación profunda y lenta en cada cambio.|Si usas Vite, usa _plugins_ como `vite-plugin-checker` que ejecutan la comprobación en un proceso worker separado. En Webpack, asegúrate de limitar el campo `files` del plugin.|
|**`process is not defined`**|Estás intentando usar variables de entorno de Node.js (como `process.env.NODE_ENV`) en el código de frontend.|Necesitas un plugin para inyectar variables de entorno en el _bundle_ (ej: `DefinePlugin` en Webpack o la configuración `env` en Vite).|
|**TS Checker no detecta errores en Vite**|Vite usa esbuild para la transpilación (que no hace _type checking_), y el _plugin checker_ no está habilitado.|Asegúrate de instalar y configurar `vite-plugin-checker` (o similar) para ejecutar la comprobación de tipos por separado.|

Exportar a Hojas de cálculo

## 📚 Resumen de Puntos Clave

- El ecosistema de _plugins_ permite integrar herramientas de **calidad de código** (**ESLint**) y **formato** (**Prettier**) en el proceso de _build_.
    
- **Webpack** utiliza _plugins_ específicos (ej: `eslint-webpack-plugin`) que se ejecutan directamente en su proceso de compilación.
    
- **Vite** prefiere _plugins_ que ejecutan tareas lentas (como _type checking_) en **procesos separados** (ej: `vite-plugin-checker`) para no comprometer la velocidad del HMR.
    
- Las herramientas de **Testing** (Jest, Vitest) se ejecutan mejor a través de **Scripts npm** dedicados, fuera del ciclo directo del _bundler_.
    

---

Tenemos nuestro código compatible y de calidad. El paso final antes de desplegar es asegurarnos de que el _bundle_ de producción sea lo más pequeño y rápido posible.

[[17 - Optimización de producción]]