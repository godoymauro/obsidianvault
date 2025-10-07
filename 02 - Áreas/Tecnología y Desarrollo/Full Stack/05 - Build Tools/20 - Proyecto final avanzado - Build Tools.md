Después de construir y optimizar nuestra aplicación, la clase final se centra en cómo llevar ese _build_ al mundo real. Cubriremos el despliegue a producción, la configuración de CI/CD (Integración y Despliegue Continuos) y las métricas finales de calidad.

---

# 20 - Proyecto final avanzado

## 📝 Introducción

El **despliegue (deployment)** es el último paso del ciclo de vida del desarrollo. Usamos los _build tools_ no solo para crear los archivos, sino para asegurar que ese proceso sea reproducible, verificable y automatizado. En esta clase, enfocamos el _build_ de nuestra SPA moderna hacia la automatización y el análisis post-despliegue.

## 🧠 Teoría: CI/CD y Testing de Bundles

### 1. El Rol de CI/CD

**CI/CD** (Continuous Integration / Continuous Deployment) es el proceso automatizado que se ejecuta cada vez que el código se actualiza (ej: se sube un _commit_ a la rama principal de Git).

|Etapa|Descripción|Comandos Típicos de Build Tools|
|---|---|---|
|**Integración Continua (CI)**|El servidor verifica la calidad, compatibilidad y funcionalidad del código.|`$ npm install`, `$ npm run lint`, `$ npm test`, `$ npm run build` (para verificar que no haya errores de _build_).|
|**Despliegue Continuo (CD)**|El servidor toma los archivos de la carpeta `dist/` generados por el _build_ y los sube al entorno de _hosting_ (Vercel, Netlify, AWS S3, etc.).|`$ npm run deploy` (un script que sube el contenido de `dist`).|

Exportar a Hojas de cálculo

### 2. Testing de Bundles y Regresión

Una vez que el _build_ se completa, ¿cómo sabemos que funciona? El **Testing de Bundles** verifica que la aplicación _construida_ se comporta como se espera.

- **Smoke Tests:** Pruebas rápidas y superficiales para asegurar que las funciones críticas (login, navegación) funcionan en el _bundle_ de producción.
    
- **Bundle Size Budget:** Usar _plugins_ (como en Webpack o Rollup) para **fallar el build** si el tamaño del _bundle_ excede un límite predefinido. Esto previene la regresión de rendimiento.
    

### 3. Análisis de Performance (Post-Build)

Después del despliegue, usamos métricas reales para evaluar el éxito de nuestras optimizaciones.

- **Core Web Vitals:** Métricas clave de Google que miden la experiencia del usuario (ej: **LCP** - Largest Contentful Paint, el tiempo que tarda el contenido principal en cargarse).
    
- **Source Maps:** Asegurar que los **Source Maps** se generen y se almacenen de forma segura. Estos son vitales para _debugging_ en producción, ya que mapean el código minificado de vuelta al código fuente legible que escribiste.
    

---

## 🛠️ Ejemplos: Automatización y Métricas

### Paso 1: Scripts para Despliegue y Pruebas CI/CD

Añadimos scripts específicos para la etapa de _deployment_ en nuestro `package.json`.

JSON

```
// package.json (Final)
{
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build",
    "preview": "vite preview",
    "test": "vitest run",
    
    // El script que el servidor CI/CD ejecutará
    "ci": "npm install --frozen-lockfile && npm run lint && npm run test && npm run build", 
    
    // Script de despliegue simple (ejemplo para Netlify/Vercel CLI)
    "deploy": "npm run build && netlify deploy --prod --dir=dist" 
  }
}
```

> **`--frozen-lockfile` (pnpm/Yarn):** Se usa en entornos de CI para garantizar que el `lock file` se respete y que **no se instale ninguna versión nueva** de dependencias, asegurando reproducibilidad.

### Paso 2: Generación de Source Maps Seguros

Es crucial que el código minificado en producción genere **Source Maps** para el _debugging_.

**Vite (`vite.config.ts`):**

TypeScript

```
// vite.config.ts
export default defineConfig({
  // ...
  build: {
    sourcemap: true, // 👈 Generar Source Maps
    // outDir: 'dist',
    // ...
  }
});
```

> **Seguridad:** Los Source Maps no deben ser públicos. En producción, deben subirse a un servicio de monitoreo de errores (como Sentry) para que solo el equipo de desarrollo pueda acceder a ellos al recibir un error.

### Paso 3: Configurando un Budget de Tamaño (Webpack / Rollup)

Para prevenir regresiones de rendimiento, configuramos el _build tool_ para que falle si el _bundle_ supera un tamaño límite.

**Vite/Rollup (`vite.config.ts` - usando `rollupOptions`)**

TypeScript

```
// vite.config.ts (Simulación de Budget con Rollup Warnings)
export default defineConfig({
  // ...
  build: {
    // ...
    rollupOptions: {
      output: {
        // ...
      },
      // Configuración de límites de tamaño
      maxParallelFileOps: 5, 
    },
    // Alerta al superar 500kb de bundle total (ejemplo)
    chunkSizeWarningLimit: 500, 
  }
});

// En Webpack se usa la configuración 'performance' para el mismo propósito.
```

### Paso 4: Análisis y Core Web Vitals

Después del despliegue, herramientas como **Lighthouse** (integrada en Chrome DevTools) o servicios como PageSpeed Insights analizan tu URL de producción.

**Métricas Clave a Monitorear (Gracias a nuestros Build Tools):**

1. **LCP (Largest Contentful Paint):** Nuestro **Code Splitting** (lazy loading) reduce el tamaño inicial del _bundle_, impactando directamente en un LCP más rápido.
    
2. **FID (First Input Delay) / INP (Interaction to Next Paint):** La **minificación** y el **Tree Shaking** reducen la cantidad de código JS que el navegador debe analizar y ejecutar, liberando el hilo principal y mejorando la interactividad.
    

---

## 🛑 Errores Comunes de Despliegue

|Error|Descripción|Solución|
|---|---|---|
|**"Cannot GET /mi-ruta"**|Común en SPAs. El servidor de _hosting_ (Apache, Nginx, o CDN) no encuentra la ruta interna de la SPA y devuelve 404.|Configura el _hosting_ para que todas las peticiones (excepto activos estáticos) se redirijan a **`index.html`** (esto permite que el _router_ de React/Vue tome el control).|
|**Fallo en CI: "package-lock.json outdated"**|El `lock file` se subió desactualizado, o el servidor de CI está forzando una instalación estricta.|Antes de hacer _commit_, ejecuta `$ npm install` (o `$ pnpm install`) y asegúrate de que el _lock file_ esté actualizado y subido a Git.|
|**LCP lento después de la optimización**|El problema no es el _bundle_ JS, sino la carga de otros activos (imágenes, fuentes) o el renderizado inicial.|Optimiza el tamaño de las imágenes. Asegura que los activos críticos (CSS, fuentes) se carguen al principio.|

Exportar a Hojas de cálculo

## 📚 Resumen de Puntos Clave

- **CI/CD** automatiza la integración, el testing (`npm run test`) y la construcción (`npm run build`).
    
- Se debe usar el _flag_ **`--frozen-lockfile`** en CI para garantizar la reproducibilidad y estabilidad del _build_.
    
- Los **Source Maps** son vitales para el _debugging_ en producción, pero deben ser gestionados de forma segura (no públicos).
    
- Monitorear **Core Web Vitals** (LCP, FID) es la forma de medir el impacto real de nuestras optimizaciones de _build_ (Minificación, Tree Shaking, Code Splitting).
    
- El error de despliegue más común en SPAs es la necesidad de configurar el servidor para que redirija todas las rutas a **`index.html`**.
    

---

¡Felicidades! 🎉 Has completado el curso de **Build Tools**. Ahora estás equipado con el conocimiento experto para configurar, optimizar y automatizar el proceso de _build_ de cualquier proyecto frontend moderno.

¿Te gustaría que profundizáramos en un módulo específico (ej: la configuración avanzada de Webpack) o prefieres explorar cómo aplicar esto a un **monorepo**?