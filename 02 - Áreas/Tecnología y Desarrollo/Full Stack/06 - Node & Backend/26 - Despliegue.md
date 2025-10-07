¡Excelente! Hemos llegado a la etapa final de este nivel. Ya sabes cómo automatizar tu código con CI/CD ( **[[25 - CI-CD]]**), y el siguiente paso es lanzarlo al mundo. Elegir la plataforma de despliegue correcta es crucial para la estabilidad, el costo y la escalabilidad de tu API.

Esta clase te guiará a través de las opciones modernas y profesionales para alojar tu aplicación Node.js.

Aquí tienes la nota para la clase anterior: **[[25 - CI-CD]]**.

---

# 26 - Despliegue

## 📚 Introducción: Del Laptop al Mundo

El **Despliegue (_Deployment_)** es el proceso de poner tu aplicación, probada y empaquetada (idealmente en **[[25 - CI-CD]]**), a disposición de los usuarios en un servidor accesible públicamente.

Para Node.js, las opciones de _hosting_ caen en tres categorías principales, ofreciendo un equilibrio diferente entre facilidad de uso y control.

---

## ☁️ 1. Proveedores "Zero-Config" y PaaS (Platform as a Service)

Estos proveedores se centran en la simplicidad. Solo necesitas conectar tu repositorio de Git, y ellos se encargan de la construcción (instalar `npm`, ejecutar `npm build`) y del despliegue. Ideal para prototipos, MVPs y pequeñas empresas.

|Plataforma|Modelo|Ventajas Clave|Casos de Uso|
|---|---|---|---|
|**Render**|PaaS|Soporte nativo para **Docker** y **Bases de Datos** (PostgreSQL, Redis). Excelente para Node.js y _full-stack_.|APIs REST, Servicios que necesitan bases de datos persistentes.|
|**Railway**|PaaS|Enfocado en _Developer Experience_ (DX). Despliegues ultrarrápidos, genera URLs efímeras para PRs.|Desarrollo ágil, Microservicios pequeños.|
|**Fly.io**|PaaS/IaaS|Enfocado en desplegar cerca del usuario (**Edge Computing**). Ideal para latencia baja.|Aplicaciones globales, WebSockets (ver [[28 - WebSockets y tiempo real]]).|
|**Heroku**|PaaS (Tradicional)|Muy fácil, pero sus planes gratuitos son limitados.|Proyectos personales, apps sencillas.|

### Flujo de Despliegue PaaS (Ejemplo Render/Railway)

1. Conectas tu cuenta de GitHub y seleccionas el repositorio.
    
2. Defines el **comando de inicio** (ej: `node index.js`).
    
3. Defines las **Variables de Entorno** (ej: `JWT_SECRET`, `DB_URI`).
    
4. El proveedor detecta un _push_ a la rama `main` y automáticamente construye y despliega la nueva versión.
    

---

## 🐳 2. Despliegue basado en Contenedores (Docker)

Esta es la forma más profesional y portátil de desplegar, utilizando la imagen **Docker** que construiste en la clase **[[25 - CI-CD]]**.

|Plataforma|Modelo|Descripción|Casos de Uso|
|---|---|---|---|
|**AWS ECS/Fargate**|Contenedor|Servicio de orquestación de contenedores de AWS. **Fargate** abstrae la gestión del servidor.|Escala masiva, integración profunda con otros servicios de AWS.|
|**Google Cloud Run**|Serverless/Contenedor|Ejecuta contenedores en un entorno _serverless_. Solo pagas cuando recibe tráfico.|Microservicios, APIs con tráfico variable.|
|**DigitalOcean/Linode**|Contenedor / IaaS|Plataformas sencillas de IaaS/VPS. Necesitas configurar Docker y PM2 (ver [[31 - Escalabilidad y performance]]) tú mismo.|Control total del servidor, costos más predecibles.|

**Importante:** Usar Docker desacopla tu aplicación del proveedor, permitiéndote migrar de forma sencilla entre AWS, GCP o Azure.

---

## 🧠 3. Estrategias de Despliegue

Un despliegue no es solo copiar archivos. Si tu API tiene mucha carga, necesitas reemplazar la versión antigua por la nueva sin interrumpir a los usuarios.

|Estrategia|Proceso|Ventaja|Desventaja|
|---|---|---|---|
|**Recreación (_Recreate_)**|La versión antigua se detiene, la nueva se despliega.|Simple y rápido de implementar.|**Tiempo de inactividad** (_Downtime_) inevitable.|
|**Rolling Update**|Los servidores se actualizan uno a uno. El balanceador de carga dirige el tráfico a los servidores actualizados.|**Cero Downtime** (mantiene el servicio activo).|Proceso más lento; el tráfico se dirige temporalmente a versiones mezcladas.|
|**Blue/Green**|Se crea un entorno _Green_ (nuevo) idéntico al _Blue_ (actual). El tráfico se cambia del _Blue_ al _Green_ de golpe.|**Cero Downtime**, reversión instantánea si falla el _Green_.|Duplica los recursos (y el costo) temporalmente.|
|**Canary**|Se despliega la nueva versión a un **pequeño subconjunto** de usuarios (ej: 5%). Si funciona bien, se escala al 100%.|Mínimo riesgo; se prueba en producción antes del _full rollout_.|Requiere lógica de _testing_ compleja y balanceo avanzado.|

Para empezar, un **Rolling Update** (que muchos PaaS hacen por defecto) es suficiente para garantizar cero inactividad.

---

## 🛠 Variables de Entorno y Configuraciones

Nunca olvides configurar las variables de entorno en tu plataforma de despliegue.

|Variable|Uso|Ejemplos de Plataformas|
|---|---|---|
|`NODE_ENV`|Debe ser `production` para optimizaciones.|Todas las plataformas lo requieren.|
|`PORT`|El puerto que tu aplicación escucha (Express usa `process.env.PORT` por defecto).|El proveedor lo inyecta (ej: Render usa el puerto 10000).|
|`JWT_SECRET`|Claves secretas de autenticación.|Render, Railway, AWS Secrets Manager.|
|`DB_URI`|Cadena de conexión a tu base de datos (con usuario y contraseña).|Todas las plataformas.|

**Importante:** En producción, **nunca** uses un archivo `.env`. Las variables deben inyectarse de forma segura a través del panel de control o un servicio de secretos dedicado.

---

## ✅ Buenas Prácticas y Errores Comunes

|Categoría|Buena Práctica|Error Común|
|---|---|---|
|**Configuración**|Asegúrate de que tu `start` script en `package.json` sea el comando de inicio correcto para producción (ej: `node index.js`).|Usar el script de desarrollo (ej: `nodemon index.js`) en producción.|
|**Costo**|Comienza con plataformas PaaS de pago por uso (Render, Railway, Cloud Run).|Comenzar directamente en AWS EC2 o Kubernetes sin experiencia, incurriendo en costos inesperados.|
|**`node_modules`**|Usar **Docker** o el _build process_ del PaaS para instalar dependencias _frescas_ de producción en el servidor.|Intentar copiar la carpeta `node_modules` local al servidor (falla por diferencias de SO).|
|**Health Check**|Configurar un _endpoint_ simple (`GET /health`) que el proveedor pueda consultar para verificar que el servidor esté vivo y sano.|No tener un _health check_, haciendo que el proveedor no sepa si el servidor realmente inició correctamente.|

---

## 🔑 Resumen de Puntos Clave

- Los **PaaS (Platform as a Service)** como Render y Railway ofrecen el despliegue más rápido y sencillo para Node.js.
    
- El **Despliegue Basado en Contenedores (Docker)** con AWS Fargate o Cloud Run ofrece la máxima portabilidad y escalabilidad.
    
- El uso de **Variables de Entorno** es la única forma segura de manejar secretos en producción.
    
- Las estrategias de despliegue como **Rolling Update** garantizan **cero tiempo de inactividad** para los usuarios.
    

---

# 🚀 Nivel 5 Completado

¡Felicidades! Has completado el **Nivel 5: Autenticación, seguridad y despliegue**. ¡Tu API está lista para funcionar en producción!

Es hora de pasar a técnicas avanzadas para construir servicios que superen los límites de las APIs REST tradicionales.

Entramos al **Nivel 6: Avanzado y arquitecturas modernas**. Tu próxima clase será la introducción al sucesor conceptual de REST: **[[27 - GraphQL con Node.js]]**.