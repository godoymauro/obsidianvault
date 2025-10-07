¡Continuemos! Has llegado a un punto donde tu API es funcional, segura y la puedes monitorear. El problema es que aún está en tu máquina. El siguiente paso profesional es automatizar el proceso de mover tu código desde el desarrollo hasta la producción.

Esta clase se centrará en el flujo de trabajo moderno para la entrega de software: **Integración Continua (CI)** y **Despliegue Continuo (CD)**.

Aquí tienes la nota para la clase anterior: **[[24 - Logs y monitoreo]]**.

---

# 25 - CI/CD

## 📚 Introducción: Automatización y Confianza

**CI/CD** es un conjunto de prácticas de desarrollo que busca automatizar y monitorear todos los pasos en la entrega de software.

- **CI (Continuous Integration - Integración Continua):** Práctica de fusionar los cambios de código al repositorio principal de forma frecuente. Esto dispara una automatización que construye y prueba la aplicación.
    
- **CD (Continuous Delivery/Deployment - Entrega/Despliegue Continuo):** Extensión de CI que automatiza la entrega del código (Entrega Continua) o el despliegue a producción (Despliegue Continuo).
    

El objetivo es detectar errores tempranamente y reducir el riesgo de cada despliegue, permitiéndote liberar nuevas características de forma rápida y confiable.

---

## 🏗 El Pipeline de CI/CD

Un **Pipeline (Tubería)** de CI/CD es una secuencia de pasos automatizados que se ejecuta tras un evento (como un _push_ a Git).

|Etapa|CI (Integración Continua)|CD (Despliegue Continuo)|
|---|---|---|
|**Build**|Instalar dependencias (`npm install`) y compilar (si usas TypeScript/Babel).|Empaquetar la aplicación (ej: crear una imagen **Docker**).|
|**Test**|Ejecutar tests unitarios y de integración (`npm test`).|Crear y etiquetar el artefacto desplegable (imagen Docker, archivo ZIP).|
|**Quality Gate**|Verificar la cobertura de código, _linting_ (ej: ESLint). **Si falla, el Pipeline se detiene.**|Enviar el artefacto a un registro (ej: Docker Hub, AWS ECR).|
|**Deploy**|**N/A** (El CI termina aquí).|Ejecutar el artefacto en el entorno de producción (ej: AWS, Render, Kubernetes).|

---

## 🐙 1. Integración Continua con GitHub Actions

**GitHub Actions** es la herramienta más popular para implementar CI/CD, ya que está integrada directamente en el repositorio de código. Utiliza archivos **YAML** para definir los _workflows_ (flujos de trabajo).

### Estructura de un `workflow` Básico para Node.js

El archivo se guarda en `.github/workflows/ci.yml`.

YAML

```
# .github/workflows/ci.yml
name: Node.js CI

# 1. Cuándo se dispara el workflow
on:
  push:
    branches: [ "main" ] # Disparar en cada push a la rama 'main'
  pull_request:
    branches: [ "main" ] # Disparar en cada Pull Request a 'main'

# 2. Definición del trabajo (job)
jobs:
  build_and_test:
    runs-on: ubuntu-latest # Ejecutar en un entorno Linux (servidor virtual)
    
    steps:
    # a) Configurar el entorno Git
    - uses: actions/checkout@v4
    
    # b) Configurar Node.js (se recomienda usar la LTS)
    - name: Use Node.js 20.x
      uses: actions/setup-node@v4
      with:
        node-version: 20
        cache: 'npm' # Habilitar caché de dependencias para acelerar
    
    # c) Instalar dependencias (BUILD)
    - name: Install Dependencies
      run: npm install
      
    # d) Ejecutar Tests (TEST)
    - name: Run Tests
      run: npm test # Asume que tienes un script 'test' en package.json
      
    # e) (Opcional) Ejecutar Linting o Cobertura de Código
    - name: Check Linting
      run: npm run lint # Si tienes un script de linting
```

**Concepto Clave:** Cada _step_ (paso) en el `job` se ejecuta de forma secuencial. Si un paso falla (`npm test` devuelve un código de error distinto de 0), el _job_ se detiene y el CI falla.

---

## 🐳 2. Despliegue con Docker

Para el CD, la mejor práctica es empaquetar tu aplicación y su entorno en un contenedor **Docker**. Un contenedor es una unidad ligera y autónoma que incluye el código, el _runtime_ (Node.js), las librerías y las configuraciones.

**Ventaja:** **"Funciona en mi máquina" = "Funciona en el servidor"**. Docker elimina las inconsistencias de entorno.

### El Archivo `Dockerfile`

Define los pasos para construir la imagen de tu aplicación.

Dockerfile

```
# Dockerfile

# 1. BUILD STAGE: Usamos una imagen base con Node.js LTS
FROM node:20-slim AS builder

# Establecemos el directorio de trabajo dentro del contenedor
WORKDIR /app

# Copiamos package.json y package-lock.json para instalar
COPY package*.json ./
# Instalamos solo las dependencias de producción (más ligero)
RUN npm install --only=production

# 2. PRODUCTION STAGE: La imagen final que correrá
# Usamos una imagen base más pequeña y segura
FROM node:20-alpine

WORKDIR /app

# Copiamos la aplicación desde el stage 'builder'
COPY --from=builder /app/node_modules ./node_modules
COPY . .

# Exponer el puerto por el que escucha Express (ej: 3000)
EXPOSE 3000

# 3. Comando de inicio (entrypoint)
# Inicia la aplicación (usando PM2 o solo node, ver clase avanzada)
CMD [ "node", "index.js" ]
```

### Flujo de Despliegue Contínuo (CD)

1. El Pipeline (GitHub Actions) termina la etapa de CI con éxito.
    
2. Un nuevo _Job_ de CD toma el código.
    
3. Ejecuta `docker build -t mi-api-node .` para crear la imagen.
    
4. Ejecuta `docker push` para enviar la imagen a un **Registro de Contenedores** (ej: Docker Hub, AWS ECR).
    
5. Notifica al servicio de alojamiento (ej: Render, Railway, Kubernetes) para que **tire** (pull) la nueva imagen del registro y la inicie, reemplazando la versión antigua.
    

---

## ✅ Buenas Prácticas y Errores Comunes

|Categoría|Buena Práctica|Error Común|
|---|---|---|
|**Versiones**|**Fijar la versión de Node.js** en el pipeline de CI (`actions/setup-node@v4` con `node-version`).|Usar la versión más reciente sin fijarla, lo que puede introducir errores inesperados si Node.js lanza una nueva versión.|
|**Separación**|Tener un Pipeline de **CI** que solo construye/prueba, y uno de **CD** que solo despliega.|Intentar desplegar a producción sin que los tests del CI hayan pasado primero.|
|**Secretos**|Usar **Secretos de GitHub Actions** (variables de entorno encriptadas) para variables sensibles (credenciales de BD, JWT Secret).|Hardcodear secretos en el archivo `.yml`.|
|**Docker Build**|Usar el _flag_ `--only=production` en `npm install` dentro del _Dockerfile_ y usar **Build Multi-Stage** para reducir el tamaño final de la imagen.|Copiar la carpeta `node_modules` local (que puede contener _dev dependencies_ y binarios específicos del SO) al contenedor.|

---

## 🔑 Resumen de Puntos Clave

- **CI/CD** automatiza la construcción, prueba y despliegue del código.
    
- **GitHub Actions** es la herramienta de elección para definir **Pipelines** de CI/CD con archivos **YAML**.
    
- Los _Pipelines_ se ejecutan de forma secuencial, deteniéndose si alguna etapa (ej: `npm test`) falla.
    
- **Docker** es la forma estándar de empaquetar aplicaciones de Node.js para despliegue, asegurando la consistencia del entorno.
    
- Los **Builds Multi-Stage** en Docker reducen el tamaño final de la imagen al descartar las herramientas de construcción y los _dev dependencies_.
    

Todo está listo para el despliegue final. Ahora veamos las opciones reales para alojar tu API en la nube.

Tu próxima clase te guiará a través de las plataformas de alojamiento: **[[26 - Despliegue]]**.