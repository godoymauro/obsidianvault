¡Absolutamente! Has explorado las arquitecturas modernas (GraphQL, Serverless) y sabes que Node.js es rápido. Pero la verdadera prueba de fuego llega bajo **alta carga**. Una API exitosa debe ser capaz de manejar un aumento repentino de usuarios sin colapsar.

Esta clase te enseñará cómo escalar Node.js más allá del límite de un solo proceso y cómo optimizar el código para un rendimiento máximo.

Aquí tienes la nota para la clase anterior: **[[30 - Serverless]]**.

---

# 31 - Escalabilidad y performance

## 📚 Introducción: Superando el Límite del Hilo Único

Recordemos el concepto clave de **[[03 - Node.js Runtime]]**: Node.js opera en un **único hilo** principal. Esto es genial para I/O (Input/Output) asíncrono, pero si ese hilo se satura con tareas intensivas de CPU (cálculos pesados, compresión, criptografía síncrona), el rendimiento cae a cero.

La **Escalabilidad** en Node.js no se logra con un servidor más grande (escalamiento vertical), sino con la duplicación de procesos para distribuir la carga (**escalamiento horizontal**).

---

## 🏗 1. Clustering: Escalado Horizontal en un Servidor

El módulo nativo **`cluster`** de Node.js permite que un único proceso maestro (el _master_) distribuya el tráfico de red a múltiples procesos hijos (los _workers_), cada uno ejecutando una copia de tu aplicación. Esto aprovecha todos los núcleos de la CPU de tu servidor.

### Principio de Funcionamiento

- **Master Process:** Escucha el puerto (ej: 3000) y maneja las conexiones entrantes.
    
- **Worker Processes:** Cada uno es un proceso de Node.js independiente y maneja las peticiones.
    
- **Balanceo de Carga:** El sistema operativo o el proceso maestro distribuyen las peticiones entre los _workers_ mediante una estrategia Round-Robin (uno a uno).
    

### Ejemplo con Módulo `cluster` (Conceptual)

JavaScript

```
const cluster = require('node:cluster');
const os = require('node:os');
const express = require('express');

// Contamos los núcleos de la CPU disponibles
const numCPUs = os.cpus().length; 

if (cluster.isMaster) {
    console.log(`[Master ${process.pid}] iniciado.`);

    // 💡 Creamos un worker por cada núcleo de CPU
    for (let i = 0; i < numCPUs; i++) {
        cluster.fork();
    }

    // Manejar la muerte de un worker (esencial para la resiliencia)
    cluster.on('exit', (worker, code, signal) => {
        console.log(`Worker ${worker.process.pid} murió. Iniciando uno nuevo...`);
        cluster.fork(); // ¡Reemplazar el worker caído!
    });
} else {
    // 💡 El código que cada worker ejecuta (tu aplicación Express)
    const app = express();
    app.get('/', (req, res) => {
        res.send(`Hola desde Worker ${process.pid}`);
    });

    app.listen(3000, () => {
        console.log(`[Worker ${process.pid}] escuchando en el puerto 3000.`);
    });
}
```

---

## 🛡 2. PM2: El Gestor de Procesos de Producción

En lugar de escribir y mantener la lógica del módulo `cluster` manualmente, los desarrolladores de Node.js usan **PM2 (Process Manager 2)**.

**PM2** es una herramienta CLI que te permite:

1. **Modo Cluster:** Ejecutar tu aplicación en modo cluster, aprovechando todos los núcleos de CPU automáticamente.
    
2. **Monitorización:** Mostrar el uso de CPU y RAM de cada proceso.
    
3. **Resiliencia:** Reiniciar automáticamente tu aplicación si falla o si un proceso worker muere.
    
4. **Zero Downtime Reload:** Recargar la aplicación sin perder ninguna conexión (ideal para despliegues _rolling_).
    

### Comandos Clave de PM2

|Comando|Propósito|
|---|---|
|`npm install -g pm2`|Instalación global.|
|`pm2 start app.js -i max`|Iniciar la aplicación en modo cluster. `-i max` usa tantos procesos como núcleos de CPU.|
|`pm2 list`|Muestra el estado de todos los procesos gestionados.|
|`pm2 monit`|Muestra el panel de monitoreo en tiempo real (CPU/RAM).|
|`pm2 reload all`|Reinicia todos los procesos sin inactividad.|

**PM2 es la herramienta de elección para ejecutar Node.js directamente en VPS/IaaS (ej: DigitalOcean, AWS EC2).**

---

## ⚡ 3. Optimización y Performance (Código)

El escalamiento horizontal (PM2, Docker) resuelve la saturación de CPU, pero siempre debes asegurarte de que tu código sea rápido.

### A. Almacenamiento en Caché (Caching)

El _caching_ es la herramienta más efectiva para mejorar la _performance_ en el backend.

|Tipo de Caché|Dónde se Guarda|Cuándo usar|Herramienta Node.js|
|---|---|---|---|
|**In-Memory Cache**|En la RAM del proceso de Node.js.|Datos pequeños y muy consultados (ej: tokens de configuración).|`node-cache`, `lru-cache`|
|**Distributed Cache**|En un servidor de caché dedicado (Redis o Memcached).|Datos que necesitan ser compartidos entre _todos_ los procesos (`cluster`) y microservicios.|`ioredis` (Cliente de Redis)|

**Regla de Oro:** Si una consulta a la BD es lenta y los datos cambian poco, **guárdala en Redis** con un tiempo de expiración corto (ej: 60 segundos).

### B. Workers Threads (Tareas Pesadas de CPU)

Para tareas **intensivas de CPU** (ej: minería de datos, compresión de imágenes pesadas) que _no puedes_ evitar, Node.js ofrece el módulo **`worker_threads`**.

- Permite delegar la tarea de CPU a un **hilo separado** (un _worker_) que no es parte del Event Loop.
    
- El **hilo principal** permanece libre para atender peticiones, mientras el _worker_ procesa la tarea pesada.
    

JavaScript

```
const { Worker, isMainThread } = require('node:worker_threads');

if (isMainThread) {
    // El hilo principal sigue atendiendo peticiones
    // Delega la tarea pesada al worker
    const worker = new Worker('./worker.js'); 
    worker.on('message', (msg) => {
        console.log('Resultado del cálculo:', msg);
    });
} else {
    // worker.js: Lógica de cálculo intensivo
    parentPort.postMessage(resultado_del_calculo_pesado);
}
```

---

## ✅ Buenas Prácticas y Errores Comunes

|Categoría|Buena Práctica|Error Común|
|---|---|---|
|**Balanceo**|Usar PM2 en modo **cluster** en VPS/IaaS, o el auto-escalado de contenedores (Kubernetes/ECS).|Ejecutar Node.js con `node app.js` en producción, usando solo un núcleo de CPU.|
|**I/O**|Asegurar que **todas** las operaciones I/O (BD, archivos, red) sean **asíncronas** y se basen en Promesas/`async/await`.|Usar funciones síncronas (`fs.readFileSync`, bucles muy largos) que bloquean el Event Loop y, por ende, a todos los _workers_ en el cluster.|
|**Caché**|Usar **Redis** (Distributed Cache) para datos compartidos y la invalidación de caché.|Reimplementar soluciones de caché caseras en memoria que solo funcionan en un proceso y fallan en modo cluster.|
|**Mantenimiento**|Monitorear continuamente el uso de CPU y la latencia para identificar cuellos de botella y optimizar el código.|Optimizar el código _antes_ de identificar dónde está el problema de rendimiento (optimización prematura).|

---

## 🔑 Resumen de Puntos Clave

- La **Escalabilidad** en Node.js se logra mediante el **escalamiento horizontal** (duplicando procesos), no vertical.
    
- El módulo nativo **`cluster`** permite aprovechar múltiples núcleos de CPU.
    
- **PM2** es el gestor de procesos de producción que automatiza el modo cluster, la monitorización y la resiliencia (_auto-healing_).
    
- El **Caching (Redis)** es esencial para reducir la carga de la base de datos y la latencia.
    
- Para tareas **intensivas de CPU**, usa **Workers Threads** para no bloquear el Event Loop.
    

---

# 🚀 Nivel 6 Completado

¡Felicidades! Has completado el **Nivel 6: Avanzado y arquitecturas modernas**. Ahora tienes las herramientas para construir APIs, microservicios y soluciones _serverless_ de alto rendimiento y escalabilidad.

Hemos cubierto la teoría y las herramientas. Es hora de sintetizar todo en proyectos reales.

Entramos al **Nivel 7: Proyecto final**. Tu próxima clase será la guía para construir una API RESTful completa desde cero: **[[32 - Proyecto guiado]]**.