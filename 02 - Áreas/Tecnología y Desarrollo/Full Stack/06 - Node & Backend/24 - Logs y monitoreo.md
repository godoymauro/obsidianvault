¡Absolutamente! Con tu API probada y asegurada, el siguiente paso es la **observabilidad**. Cuando tu aplicación esté en producción, necesitas saber qué está haciendo, si está fallando, y dónde está el cuello de botella. Los **Logs y el Monitoreo** son tus ojos y oídos en el servidor.

Esta clase se centrará en cómo registrar eventos importantes y cómo medir el rendimiento de tu aplicación.

Aquí tienes la nota para la clase anterior: **[[23 - Testing]]**.

---

# 24 - Logs y monitoreo

## 📚 Introducción: Observabilidad en el Backend

El uso de `console.log()` es suficiente para el desarrollo local, pero es totalmente insuficiente para la producción. Un sistema de _logging_ profesional debe ser estructurado, filtrable y capaz de almacenar grandes volúmenes de datos.

La **Observabilidad** es la capacidad de entender el estado interno de un sistema desde sus salidas externas. Se basa en tres pilares:

1. **Logs:** Registros de eventos discretos (ej: "Usuario Alice se autenticó").
    
2. **Métricas:** Valores numéricos agregados a lo largo del tiempo (ej: Peticiones por segundo, uso de CPU).
    
3. **Traces:** Seguimiento del camino de una sola petición a través de múltiples servicios.
    

---

## 📝 1. Logs Estructurados con Winston

**Winston** es la librería de _logging_ más popular en Node.js. Permite crear **Logs Estructurados** (JSON), configurar diferentes **Niveles de Log** y definir múltiples **Transportes** (destinos).

### Niveles de Log Estándar (RFC 5424)

|Nivel|Propósito|Ejemplo|
|---|---|---|
|**error**|Fallos críticos y errores de la aplicación (ej: `next(err)`).|Conexión a BD fallida, error 500.|
|**warn**|Problemas potenciales o situaciones inesperadas.|Uso de API deprecada, _rate limiting_ activado.|
|**info**|Eventos generales de la aplicación (ambiente, inicio de servidor).|Servidor iniciado, nueva versión desplegada.|
|**http**|Registros de peticiones HTTP (generalmente manejado por Morgan).|`GET /usuarios 200 5ms`.|
|**debug**|Información detallada para la depuración durante el desarrollo.|Valor de variable en un bucle.|

### Configuración Práctica de Winston

JavaScript

```
// logger/winston.js
// Necesitarás instalar winston (npm install winston)
const winston = require('winston');

// 1. Definición del Formato de Log (JSON es el estándar estructurado)
const logFormat = winston.format.combine(
    winston.format.timestamp(),
    winston.format.errors({ stack: true }), // Incluye el stack trace en los errores
    winston.format.json() // Exporta el log como JSON
);

// 2. Creación de la instancia del Logger
const logger = winston.createLogger({
    level: process.env.NODE_ENV === 'production' ? 'info' : 'debug', // Nivel mínimo a registrar
    format: logFormat,
    defaultMeta: { service: 'api-backend-service' }, // Metadatos por defecto

    // 3. Definición de Transportes (Destinos)
    transports: [
        // a) Console Transport (para desarrollo/terminal)
        new winston.transports.Console({
            format: winston.format.simple() // Usar formato simple para la terminal
        }),
        // b) File Transport (para producción/histórico)
        new winston.transports.File({ 
            filename: 'logs/app-errors.log', 
            level: 'error' // Solo guarda errores en este archivo
        }),
    ],
});

module.exports = logger;
```

**Uso en la API:**

JavaScript

```
const logger = require('./logger/winston');

// En una ruta de login exitosa:
logger.info(`Usuario ID: ${req.user.id} ha iniciado sesión con éxito.`);

// En el manejador de errores:
logger.error('Error de BD capturado:', { 
    url: req.originalUrl, 
    method: req.method, 
    message: err.message, 
    stack: err.stack 
});
```

---

## 💻 2. Morgan: Logging de Peticiones HTTP

**Morgan** es un _middleware_ simple de Express, diseñado exclusivamente para registrar logs de las peticiones HTTP (la actividad del servidor web).

JavaScript

```
// app.js
// Necesitarás instalar morgan (npm install morgan)
const morgan = require('morgan');
const express = require('express');
const app = express();

// Usamos el formato 'combined' (estándar Apache) o 'dev' (colorido)
// Usamos el transporte 'stream' para integrar Morgan con Winston
const winstonLogger = require('./logger/winston'); 
const morganStream = {
    write: (message) => {
        winstonLogger.http(message.trim()); // Morgan registra como HTTP en Winston
    },
};

// 💡 Aplica Morgan como middleware global
app.use(morgan('combined', { stream: morganStream }));

// app.listen(...)
```

Ahora, cada vez que llega una petición, Morgan la registra automáticamente y la pasa a Winston para su formato y almacenamiento.

---

## 📈 3. Monitoreo y Métricas

El _logging_ te dice **qué pasó**. Las **Métricas** te dicen **qué tan bien funciona** tu sistema. Las métricas esenciales para un backend Node.js incluyen:

|Métrica|Propósito|Herramienta|
|---|---|---|
|**Tiempo de Respuesta (Latencia)**|Cuánto tarda la API en responder a una petición.|Prometeheus + Cliente `prom-client`|
|**Uso de Recursos**|Uso de CPU, Memoria (RAM) y recolección de basura (_Garbage Collection_).|PM2 (ver [[31 - Escalabilidad y performance]]), New Relic, Datadog.|
|**Tasa de Errores**|Porcentaje de peticiones que terminan en error (códigos 5xx).|Monitoreo APM (Application Performance Monitoring).|

### Estrategia de Monitoreo (Prometheus)

1. Instalas una librería como **`prom-client`** en Node.js.
    
2. Expones un _endpoint_ especial (ej: `/metrics`) que devuelve las métricas en un formato legible por Prometheus.
    
3. **Prometheus** (un sistema externo) periódicamente "raspa" (scrapea) este _endpoint_ y almacena las métricas históricas.
    
4. **Grafana** (otra herramienta) se usa para visualizar estos datos en tiempo real (gráficos, alertas).
    

Este es el estándar de la industria para el monitoreo de Microservicios.

---

## ✅ Buenas Prácticas y Errores Comunes

|Categoría|Buena Práctica|Error Común|
|---|---|---|
|**JSON Logs**|Usar **logs estructurados (JSON)**. Permiten que las herramientas de agregación (ELK, Splunk) filtren y analicen datos fácilmente.|Escribir logs en texto plano con `console.log()`.|
|**Contexto**|Añadir **contexto** relevante a los logs (ID de usuario, ID de sesión, URL).|Solo registrar mensajes simples sin los metadatos necesarios para diagnosticar.|
|**Seguridad**|**Nunca** logear información sensible (contraseñas, JWT sin firmar, números de tarjeta de crédito).|Logear el cuerpo completo de una petición (`req.body`) sin filtrar datos sensibles.|
|**Integración**|Usar `Morgan` para HTTP y pasarlo a `Winston` para consolidar todos los tipos de logs en un solo sistema.|Usar múltiples herramientas de _logging_ sin centralización.|

---

## 🔑 Resumen de Puntos Clave

- El _logging_ profesional requiere más que `console.log()`: necesita **logs estructurados** (JSON) y **niveles de gravedad**.
    
- **Winston** es el motor de _logging_ estándar de Node.js, permitiendo configurar _transports_ (consola, archivos, servicios externos).
    
- **Morgan** es el _middleware_ de Express para el _logging_ de peticiones HTTP, que se integra con Winston.
    
- Las **Métricas** (latencia, uso de recursos) son esenciales para el **Monitoreo** y se exponen típicamente a través de sistemas como **Prometheus** y **Grafana**.
    
- **La seguridad es clave:** nunca registres secretos o datos privados en tus logs.
    

La observabilidad es el último paso antes de sacar la aplicación de tu máquina. Ahora, nos centraremos en el proceso de llevar tu código a producción de forma automática y profesional.

Tu próxima clase se centrará en el flujo de automatización: **[[25 - CI-CD]]**.