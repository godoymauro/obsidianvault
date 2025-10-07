Ya tenemos la base teórica y el entorno configurado. Es momento de pasar a la parte más fascinante (y quizás la más importante) para entender el alto rendimiento de Node.js: su arquitectura interna.

Esta clase te revelará el secreto detrás de la velocidad de Node.js.

Aquí tienes la nota para la clase anterior: [[02 - Instalación y configuración]]

---

# 03 - Node.js Runtime

## 📚 Introducción: El Secreto del Rendimiento

En la [[01 - Introducción a Node.js]], hablamos de que Node.js es **"Single-Threaded"** y utiliza **"Non-Blocking I/O"**. Esta combinación es la clave de su eficiencia, permitiéndole manejar miles de conexiones simultáneas sin el alto consumo de memoria de los sistemas Multi-Threaded.

En esta clase, desglosaremos la pieza central que hace esto posible: el **Event Loop**.

---

## 🧠 El Modelo de Hilo Único (Single-Threaded)

Node.js opera su lógica de negocio principal en un **solo hilo de ejecución** (el hilo principal de JavaScript).

- **¿Qué significa?** Tu código JavaScript (las funciones, las variables, los bucles) se ejecuta secuencialmente en este único hilo.
    
- **La Ventaja:** No hay gastos generales de gestionar múltiples hilos, como el cambio de contexto o la necesidad de sincronización de datos. Esto lo hace muy rápido.
    
- **La Advertencia (¡Crucial!):** Si una tarea toma demasiado tiempo en este hilo (un bucle infinito o una operación matemática muy pesada y _síncrona_), **bloquea a todo el servidor**. Ninguna otra solicitud puede ser atendida mientras ese hilo está ocupado.
    

**Moral:** El hilo principal de Node.js debe estar **siempre libre** y solo debe manejar el código **sincrónico** muy rápido.

---

## ⏳ El Event Loop: El Cerebro Asíncrono

El **Event Loop (Bucle de Eventos)** no es parte de V8 ni del hilo único; es una parte fundamental del entorno Node.js (implementado por la librería **`libuv`**) que gestiona la asincronía y el _Non-Blocking I/O_.

El Event Loop es un ciclo infinito que realiza tres tareas constantemente:

1. **Observar** si el hilo principal está libre.
    
2. **Verificar** si alguna tarea asíncrona delegada ha terminado (ej: lectura de archivo, consulta a base de datos).
    
3. **Enviar** la función de _callback_ (el código que debe ejecutarse al terminar la tarea) de la tarea finalizada a la **Cola de _Callbacks_** (o **Cola de Tareas**).
    

### Componentes Clave

|Componente|Función|
|---|---|
|**Call Stack (Pila de Llamadas)**|El lugar donde se ejecuta el código síncrono. **Es el hilo principal JS.**|
|**Node.js APIs / `libuv`**|Módulos que manejan las operaciones I/O. **Delegamos las tareas lentas aquí.**|
|**Callback Queue (Cola de Tareas)**|Donde esperan las funciones de _callback_ una vez que su tarea asíncrona ha finalizado.|
|**Event Loop**|El árbitro que mueve los _callbacks_ de la **Cola** al **Call Stack** _solo si_ el _Stack_ está vacío.|

Exportar a Hojas de cálculo

### Diagrama del Flujo de Ejecución

Imagina el flujo cuando haces una consulta a una base de datos:

1. **Llamada a I/O:** Tu código llama a una función asíncrona (ej: `fs.readFile()` para leer un archivo). Esta función se ejecuta rápidamente y **pasa la tarea pesada** al _Node.js APIs_ (`libuv`).
    
2. **Hilo Libre:** El _Call Stack_ queda libre **inmediatamente**. El hilo principal puede seguir procesando otras peticiones. **(Non-Blocking I/O)**.
    
3. **Operación Externa:** `libuv` (que usa un **Pool de Hilos** por debajo, pero que es invisible para el desarrollador de JS) lee el archivo en segundo plano.
    
4. **Terminación:** Cuando la lectura termina, `libuv` coloca la función de _callback_ asociada (`(error, data) => {...}`) en la **Cola de Tareas**.
    
5. **El Event Loop:** El _Event Loop_ comprueba que el _Call Stack_ está vacío.
    
6. **Ejecución:** El _Event Loop_ toma el primer _callback_ de la Cola y lo empuja al _Call Stack_ para su ejecución.
    

---

## ❌ Error Común: Bloqueo del Hilo Principal

Si introduces una operación síncrona muy larga, Node.js se paraliza.

### Ejemplo de Código Bloqueante (¡Evitar!)

Crea un archivo `bloqueo.js`:

JavaScript

```
// bloqueo.js

// 1. Inicia el servidor HTTP (operación asíncrona delegada)
const http = require('http');
const server = http.createServer((req, res) => {
    if (req.url === '/bloquear') {
        // 2. ¡Atención! Bucle for que consume CPU de forma síncrona
        console.time('Bloqueo');
        let contador = 0;
        // Simulamos una tarea pesada que tarda varios segundos
        for (let i = 0; i < 5000000000; i++) { 
            contador++; // ¡Esto bloquea el único hilo JS!
        }
        console.timeEnd('Bloqueo');
        res.end(`Bloqueo terminado. Contador: ${contador}`);
    } else {
        res.end('Ruta normal y rapida.');
    }
});

server.listen(3000, () => {
    console.log('Servidor corriendo en http://localhost:3000');
});

// Prueba esto: 
// 1. Abre el navegador en http://localhost:3000 (funciona rápido).
// 2. Abre OTRA pestaña e inmediatamente accede a http://localhost:3000/bloquear.
// 3. Intenta recargar la primera pestaña (http://localhost:3000) de nuevo.
// ¡Verás que la ruta rápida también espera a que el bucle for termine!
```

### 💡 Lección Aprendida

El código bloqueante (que consume CPU de forma intensa y síncrona) debe ser evitado en el hilo principal. Si tienes lógica de CPU pesada, debes buscar soluciones como:

- **Delegar** a procesos externos (microservicios).
    
- Usar **Workers Threads** de Node.js (más avanzado).
    

---

## ✅ Buenas Prácticas

|Categoría|Buena Práctica|Impacto en el Event Loop|
|---|---|---|
|**I/O**|**Usar siempre las versiones asíncronas** de los módulos (ej: `fs.readFile` con _callbacks_ o `fs.promises.readFile` con Promesas).|Permite que la I/O sea delegada, liberando el hilo principal.|
|**Cálculo**|Si tienes cálculos intensivos, **usa Workers Threads** o divide la tarea en pedazos más pequeños (`process.nextTick` o `setImmediate`).|Mantiene el _Call Stack_ limpio, permitiendo que el _Event Loop_ siga funcionando sin interrupciones.|
|**Asincronía**|**Dominar `async/await`** y `Promises` para hacer el código asíncrono legible y manejable.|Simplifica la gestión de los _callbacks_ que el _Event Loop_ encola. (Ver [[08 - Asincronía en Node.js]]).|

Exportar a Hojas de cálculo

---

## 🔑 Resumen de Puntos Clave

- **Node.js es Single-Threaded:** El código JavaScript principal se ejecuta en un único hilo (**Call Stack**).
    
- **Non-Blocking I/O:** Las operaciones lentas (lectura/escritura, red, BD) se **delegan** a las APIs de Node.js (`libuv`) y a hilos secundarios que son invisibles para ti.
    
- **Event Loop:** La maquinaria que garantiza que los **callbacks** (las funciones que responden al evento) se ejecuten en el **Call Stack** _solo cuando_ este esté vacío.
    
- **La Regla de Oro:** **Nunca bloquees el hilo principal** con código síncrono que consume mucho CPU. El objetivo es que la lógica síncrona sea mínima y rápida.
    

Comprender el Event Loop es el primer paso para ser un desarrollador Node.js competente y eficiente.

Ahora que sabes cómo funciona internamente, podemos pasar a cómo organizar tu código en partes reutilizables. Tu siguiente clase: **[[04 - Módulos en Node.js]]**.