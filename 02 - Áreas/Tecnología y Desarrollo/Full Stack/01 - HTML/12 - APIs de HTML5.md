¡Absolutamente! Hemos garantizado la calidad del código, por lo que es el momento de dotar a nuestra página de capacidades interactivas avanzadas. Damos inicio a la **Fase IV: APIs y Especialización**.

---

Aquí tienes los apuntes de la clase anterior: [[11 - Buenas prácticas de organización y validación de código]]

## Clase 12 - APIs de HTML5 (Geolocalización, Web Storage, Web Workers, etc.) 🚀

### Introducción

Esta clase te introduce al nivel avanzado de **HTML5 como plataforma de aplicación**, no solo como lenguaje de marcado. Las **APIs (Application Programming Interfaces)** de HTML5 son funcionalidades que el navegador expone al código **JavaScript** para interactuar con el dispositivo del usuario, almacenar datos localmente o ejecutar procesos en segundo plano. Dominar estas APIs es lo que te permite crear **aplicaciones web progresivas (PWAs)** y experiencias ricas y modernas, expandiendo lo que es posible hacer con la web.

> **Pre-requisito:** El uso de estas APIs es puramente a través de **JavaScript**. Asumiremos que tienes una base en JS para interactuar con los objetos del navegador.

---

### 1. Web Storage: Almacenamiento Persistente en el Cliente

Antes de HTML5, la única forma de guardar información del lado del cliente eran las _cookies_, limitadas en tamaño y enviadas innecesariamente con cada solicitud al servidor. **Web Storage** ofrece un almacenamiento más grande (hasta 5-10MB por dominio), más rápido y más seguro.

#### a) `localStorage` (Almacenamiento Local)

- **Propósito:** Almacena datos **persistentemente** sin fecha de caducidad. Los datos permanecen incluso después de cerrar el navegador, la pestaña o reiniciar la computadora.
    
- **Uso Típico:** Preferencias de tema (modo oscuro), tokens de sesión que deben durar días, configuraciones de usuario.
    
- **Métodos (JavaScript):**
    
    ```
    // 1. Guardar un dato (siempre como string)
    localStorage.setItem('tema', 'oscuro');
    
    // 2. Obtener un dato
    let temaActual = localStorage.getItem('tema'); // 'oscuro'
    
    // 3. Eliminar un dato
    // localStorage.removeItem('tema'); 
    
    // 4. Limpiar todo el storage del dominio
    // localStorage.clear(); 
    ```
    

#### b) `sessionStorage` (Almacenamiento de Sesión)

- **Propósito:** Almacena datos **solo por la duración de la sesión** (mientras la pestaña o ventana esté abierta). Los datos se eliminan automáticamente cuando se cierra la pestaña.
    
- **Uso Típico:** Estado de formularios a medio llenar, filtros de búsqueda temporales, datos de una sola sesión de compra.
    
- **Acceso (JavaScript):** La sintaxis es idéntica a `localStorage`, solo se llama al objeto `sessionStorage`.
    

> **Advertencia de Seguridad:** Aunque los datos de `Web Storage` no son accesibles a otros dominios, pueden ser leídos por scripts del mismo dominio (incluyendo _scripts_ maliciosos inyectados por XSS). **Nunca** almacenes contraseñas, tokens de tarjetas de crédito o información altamente sensible.

---

### 2. Geolocalización API

Esta API permite acceder a la ubicación geográfica del usuario. Es una herramienta poderosa, pero está fuertemente ligada a la seguridad y la privacidad.

- **Propósito:** Obtener las coordenadas de latitud y longitud, o monitorear la posición del usuario.
    
- **Seguridad:** **Requiere el permiso explícito del usuario** (el navegador mostrará una alerta) y solo funciona en contextos seguros (`https://`).
    

#### Uso Básico (JavaScript)

```
if (navigator.geolocation) {
    // getCurrentPosition pide la ubicación una sola vez
    navigator.geolocation.getCurrentPosition(mostrarPosicion, manejarError);
    // Para monitorear continuamente: navigator.geolocation.watchPosition(...)
} else {
    // Proporcionar un fallback (plan B)
    console.error("Geolocalización no soportada o bloqueada.");
}

function mostrarPosicion(position) {
    const lat = position.coords.latitude;
    const lon = position.coords.longitude;
    // Ahora puedes usar lat y lon para mostrar un mapa o sugerir negocios cercanos
    console.log(`Ubicación: Latitud ${lat}, Longitud ${lon}`);
}
```

---

### 3. Web Workers: Multihilo en el Navegador

JavaScript es **monohilo** (ejecuta tareas secuencialmente). Si una tarea es muy pesada (ej. un cálculo complejo o procesamiento de datos), el navegador se congela.

- **Propósito de Web Workers:** Permite ejecutar _scripts_ pesados en un **hilo separado** (_background thread_), evitando que la interfaz del usuario se congele.
    
- **Limitación Clave:** Los _workers_ **no tienen acceso al DOM** (el HTML del documento), solo manejan datos. La comunicación entre el hilo principal y el _worker_ se realiza mediante mensajes.
    

#### Uso (Requiere dos archivos JS: Main y Worker)

1. **Script Principal (Main Thread - `app.js`):** Crea y se comunica con el _worker_.
    
    ```
    const worker = new Worker('calculo.js');
    
    // 1. Escucha mensajes del worker (el resultado)
    worker.onmessage = function(event) {
        document.getElementById('resultado').textContent = 'Resultado: ' + event.data;
    };
    
    // 2. Envía un mensaje al worker
    worker.postMessage(1000000); 
    ```
    
1. **Script del Worker (`calculo.js`):** El código que ejecuta la tarea pesada.
    
    ```
    // Código dentro de calculo.js (No tiene acceso a 'document')
    onmessage = function(event) {
        const numero = event.data;
        let suma = 0;
        for(let i = 0; i < numero; i++) {
            suma += i; // Simulación de cálculo pesado
        }
    
        // 3. Devuelve el resultado al hilo principal
        postMessage(suma); 
    };
    ```
    

---

### 4. Otras APIs Relevantes

|API|Función|Uso Típico|
|---|---|---|
|**`Drag and Drop`**|Permite arrastrar elementos de un lugar a otro.|Subida de archivos, reordenamiento de listas (Kanban).|
|**`Notifications API`**|Muestra notificaciones fuera de la ventana del navegador.|Alertas de mensajes, recordatorios.|
|**`File API`**|Permite a JavaScript acceder y leer la estructura de los archivos seleccionados por el usuario.|Previsualización local de imágenes, análisis de archivos de texto.|
|**`History API`**|Manipulación del historial del navegador sin recargar la página.|Creación de **SPA (Single Page Applications)**.|

---

### Práctica recomendada

**Objetivo:** Implementar la API de `localStorage` para guardar una preferencia de tema (simulando una PWA).

1. Crea un archivo llamado `api_storage.html`.
    
2. Implementa la estructura base y el script para el manejo del tema:
    

```
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Web Storage y Persistencia</title>
</head>
<body>
    <h1>Personalización del Sitio</h1>
    <p>El tema actual es: <strong id="tema-actual">Claro</strong></p>

    <button onclick="alternarTema()">Alternar Tema</button>

    <script>
        // Función que aplica y guarda la preferencia
        function alternarTema() {
            let tema = localStorage.getItem('prefTema') || 'claro'; // Leer la preferencia
            
            if (tema === 'claro') {
                tema = 'oscuro';
                document.body.style.backgroundColor = '#282c34';
                document.body.style.color = '#abb2bf';
            } else {
                tema = 'claro';
                document.body.style.backgroundColor = '#ffffff';
                document.body.style.color = '#333333';
            }

            // 1. Guardar la nueva preferencia PERSISTENTEMENTE
            localStorage.setItem('prefTema', tema); 
            document.getElementById('tema-actual').textContent = tema.toUpperCase();
        }

        // Cargar el tema al cargar la página (para persistencia)
        window.onload = function() {
            if (localStorage.getItem('prefTema') === 'oscuro') {
                // Aplicamos los estilos al cargar si la preferencia es oscuro
                document.body.style.backgroundColor = '#282c34';
                document.body.style.color = '#abb2bf';
                document.getElementById('tema-actual').textContent = 'OSCURO';
            }
        };

    </script>
</body>
</html>
```

3. Visualiza el archivo, haz clic en el botón, y luego **cierra y vuelve a abrir** el archivo. El tema que seleccionaste debería persistir. Usa las Herramientas de Desarrollador (pestaña **Application** o **Almacenamiento**) para ver la clave `prefTema` y su valor en `localStorage`.
    

---

### Cierre

Has desbloqueado el potencial de HTML5 como plataforma de aplicaciones. El uso de **APIs nativas** como `Web Storage` (para persistencia de datos) y `Web Workers` (para rendimiento multihilo) es lo que separa un sitio web estático de una aplicación web completa y profesional. Siempre recuerda la prioridad de la seguridad (HTTPS para Geolocalización) y la limitación de Web Workers.

---

## Resumen de puntos claves

|API|Función Principal|Seguridad/Nota|
|---|---|---|
|**`localStorage`**|Almacenamiento persistente en el cliente (sin caducidad).|No usar para datos altamente sensibles.|
|**`sessionStorage`**|Almacenamiento temporal (dura solo mientras la pestaña esté abierta).|Ideal para estados de sesión no críticos.|
|**Geolocalización**|Obtener la ubicación geográfica del usuario.|**Requiere HTTPS y permiso explícito** del usuario.|
|**Web Workers**|Ejecutar tareas pesadas en un hilo de fondo.|**No pueden acceder al DOM**; se comunican por mensajes.|
|**APIs Modernas**|Capacidades extendidas del navegador.|Esenciales para el desarrollo de **PWAs**.|

**👉 Hemos dotado a nuestra web de lógica avanzada. Ahora, vamos a darle la capacidad de dibujar y animar gráficos vectoriales. Vamos a la siguiente clase:** [[13 - Canvas y SVG]]