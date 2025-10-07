¡Claro que sí! Acabamos de ver la complejidad de los **Microservicios** y la gestión de la comunicación. Ahora, vamos a simplificar la infraestructura al máximo con el paradigma **Serverless**, delegando por completo la gestión del servidor.

Esta clase te enseñará a construir funciones bajo demanda, la forma más económica y escalable de correr código para muchas tareas backend.

Aquí tienes la nota para la clase anterior: **[[29 - Microservicios]]**.

---

# 30 - Serverless

## 📚 Introducción: El Mito de "Sin Servidor"

El término **Serverless** (_Sin Servidor_) es engañoso. Los servidores existen; lo que desaparece es la **gestión de la infraestructura** por parte del desarrollador. Con Serverless, ya no te preocupas por:

1. **Aprovisionamiento:** Configurar servidores (EC2, VPS).
    
2. **Escalabilidad:** Implementar balanceadores de carga o _auto-scaling_.
    
3. **Mantenimiento:** Parchear el sistema operativo o el _runtime_ de Node.js.
    

Simplemente escribes tu código (una función) y lo subes a la nube. El proveedor de la nube (AWS Lambda, Google Cloud Functions, Azure Functions) lo ejecuta en respuesta a un evento y escala automáticamente de cero a miles de instancias.

### 🧠 FaaS (Function as a Service)

La tecnología central de Serverless es **FaaS**, donde la unidad de código es una **función** que se ejecuta en respuesta a un evento (una petición HTTP, un archivo subido a S3, un mensaje en una cola).

---

## 🚀 AWS Lambda: El Estándar de la Industria

**AWS Lambda** es el servicio FaaS más popular y maduro. Node.js es una de las opciones de _runtime_ más eficientes para Lambda.

### El Handler de Lambda

Una función Lambda en Node.js siempre tiene la misma firma: un _handler_ que recibe tres argumentos y es asíncrono.

JavaScript

```
// index.js (El "handler" de la función Lambda)

/**
 * @param {Object} event - Los datos del evento que disparó la función (ej: la petición HTTP).
 * @param {Object} context - Metadatos de la ejecución (ej: tiempo restante).
 * @returns {Object} - La respuesta de la función (ej: respuesta HTTP JSON).
 */
exports.handler = async (event, context) => {
    // 1. Logging de inicio
    console.log('Evento recibido:', JSON.stringify(event, null, 2));

    // 2. Lógica de negocio (ej: si el evento es una petición HTTP de API Gateway)
    if (event.httpMethod === 'GET' && event.path === '/productos') {
        const productoId = event.queryStringParameters?.id;
        
        // Simulación de lógica de BD asíncrona (como una conexión a DynamoDB o RDS)
        const productos = await obtenerDatosDeProducto(productoId); 

        // 3. Devolver la respuesta en el formato esperado por AWS API Gateway
        return {
            statusCode: 200,
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify(productos)
        };
    }
    
    return {
        statusCode: 404,
        body: JSON.stringify({ message: "Ruta Lambda no encontrada" })
    };
};

async function obtenerDatosDeProducto(id) {
    // Simula una operación I/O asíncrona que Node.js maneja bien.
    return new Promise(resolve => setTimeout(() => {
        resolve(id ? { id, nombre: 'Serverless Item' } : [{id: 1, nombre: 'All'}]);
    }, 50));
}
```

---

## 💡 Ventajas y Desafíos de Serverless

|Categoría|Ventajas|Desafíos (El lado oscuro)|
|---|---|---|
|**Costo**|**Pago por uso.** Solo pagas por el tiempo de ejecución (milisegundos) y la memoria utilizada.|El costo puede volverse impredecible en tráfico masivo si la lógica no está optimizada.|
|**Escalabilidad**|**Escala automática e infinita** (de cero a miles).|**Cold Starts** (Arranque en frío). La primera petición a una función inactiva tiene una latencia extra.|
|**Node.js**|Ideal para el modelo **I/O Asíncrono** de Node.js, donde la función pasa mucho tiempo esperando I/O sin consumir CPU.|El límite de recursos (RAM y CPU) es estricto. No es bueno para tareas intensivas de CPU.|
|**Gestión**|No hay servidores que gestionar.|**Vendor Lock-in:** El código está fuertemente acoplado a la infraestructura del proveedor (ej: AWS, Google).|

### El Problema del Cold Start

Cuando una función ha estado inactiva, la nube tiene que "calentar" el entorno: descargar el código, iniciar el _runtime_ de Node.js, y ejecutar el _handler_. Esto puede tardar unos pocos cientos de milisegundos, lo que se traduce en latencia para el usuario.

**Solución:** Escribir código con Node.js que se inicialice rápidamente y, en producción, usar **Provisioned Concurrency** (pre-calentar instancias) o el patrón **Keep-Alive** (peticiones ficticias periódicas).

---

## 📦 Herramientas de Despliegue (Frameworks)

Desplegar una sola función puede requerir configurar múltiples recursos de la nube (API Gateway, IAM, la propia función Lambda). Para gestionar esto de forma eficiente, usamos _frameworks_ de infraestructura como código (IaC).

|Framework|Descripción|Uso|
|---|---|---|
|**Serverless Framework**|Abstracción de múltiples proveedores (AWS, Azure, GCP). Facilita la configuración y el despliegue mediante un archivo YAML.|El más popular para empezar.|
|**AWS SAM (Serverless Application Model)**|Herramienta nativa de AWS. Extensión de CloudFormation, ideal si ya estás en el ecosistema AWS.|Integración profunda con AWS.|

### Ejemplo Serverless Framework (YAML)

YAML

```
# serverless.yml (Simplificado)
service: mi-api-serverless
frameworkVersion: '3'

provider:
  name: aws
  runtime: nodejs20.x # Especificamos el runtime de Node.js
  stage: dev
  region: us-east-1

functions:
  # 💡 Define una función Lambda llamada 'hello'
  hello:
    handler: index.handler # Archivo.funcion
    events:
      # 💡 Define el evento que la dispara: una petición HTTP GET
      - http:
          path: /hello
          method: get
```

Con este archivo, un simple comando como `sls deploy` despliega la función Lambda, configura el _endpoint_ de API Gateway y crea los permisos IAM necesarios.

---

## ✅ Buenas Prácticas y Errores Comunes

|Categoría|Buena Práctica|Error Común|
|---|---|---|
|**Scope**|Escribir funciones que hagan **una sola cosa** (Single Responsibility Principle).|Crear un monolito dentro de una función Lambda.|
|**Conexiones**|**Reutilizar la conexión a la BD** fuera del _handler_. La inicialización de la conexión debe hacerse a nivel global para reutilizarla en el _Cold Start_.|Abrir y cerrar la conexión a la BD dentro del _handler_ de la función (cada ejecución es una nueva conexión).|
|**Dependencias**|Mantener el número de dependencias (`node_modules`) al mínimo para acelerar el tiempo de carga del código (_Cold Start_).|Incluir dependencias de desarrollo o grandes librerías innecesarias.|
|**Memoria**|Asignar la memoria (RAM) óptima para tu función, ya que esto afecta directamente al CPU disponible y al costo.|Usar la RAM por defecto (128MB) para tareas que requieren más potencia.|

---

## 🔑 Resumen de Puntos Clave

- **Serverless (FaaS)** delega la gestión del servidor, permitiendo al desarrollador centrarse en la **lógica de negocio**.
    
- **AWS Lambda** es el servicio FaaS líder. Las funciones se ejecutan en respuesta a eventos (ej: HTTP, BD, archivos).
    
- La ventaja principal es el **pago por uso** y la **escalabilidad automática** (de cero a miles).
    
- Los **Cold Starts** son el principal desafío de rendimiento. Se mitigan optimizando el código de inicialización y reutilizando conexiones (ej: BD).
    
- El **Serverless Framework** o **AWS SAM** son necesarios para gestionar y desplegar la infraestructura como código.
    

Hemos cubierto casi todas las arquitecturas modernas. El último gran tema para un desarrollador backend de Node.js es cómo asegurar que la aplicación mantenga su rendimiento bajo una carga intensa.

Tu próxima clase se centrará en la optimización y escalabilidad: **[[31 - Escalabilidad y performance]]**.