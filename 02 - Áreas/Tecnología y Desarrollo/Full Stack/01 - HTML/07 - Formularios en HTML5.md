¡Excelente ritmo! La interacción con el usuario es lo que convierte a un sitio web estático en una aplicación dinámica. Es momento de dominar la captura de datos.

---

Aquí tienes los apuntes de la clase anterior: [[06 - Listas y tablas]]

## Clase 07 - Formularios en HTML5 (inputs, atributos, validaciones) 📝

### Introducción

Los **formularios** son la puerta de entrada para la interacción del usuario con nuestro servidor. Desde un simple _login_ hasta una compleja compra _online_, el elemento `<form>` es esencial. HTML5 revolucionó los formularios, introduciendo **nuevos tipos de `input`** y **validación nativa** sin necesidad de escribir una línea de JavaScript. Entender la semántica, los atributos y la configuración de envío es vital para procesar datos de forma segura y eficiente.

---

### 1. El Contenedor Principal: `<form>`

Todo formulario debe estar envuelto en la etiqueta `<form>`. Esta etiqueta tiene dos atributos esenciales que definen qué sucede cuando el usuario hace clic en el botón de envío.

|Atributo|Función|Valores Típicos|
|---|---|---|
|**`action`**|Especifica la URL o el archivo al que se enviarán los datos del formulario.|`procesar.php`, `/api/login`, `index.html`|
|**`method`**|Define el método HTTP utilizado para enviar los datos.|**`GET`** (datos en la URL) o **`POST`** (datos en el cuerpo de la solicitud).|

#### Diferencia entre `GET` y `POST`

- **`GET`**: Los datos se envían a través de la URL (aparecen en la barra de direcciones).
    
    - **Uso:** Búsquedas, filtros o cuando el envío es **idempotente** (no cambia datos en el servidor).
        
    - **Desventaja:** Limitado en cantidad de datos y no es seguro (visibilidad de datos).
        
- **`POST`**: Los datos se envían ocultos en el cuerpo de la solicitud HTTP.
    
    - **Uso:** Creación de usuarios, _logins_, envío de datos sensibles o archivos.
        
    - **Ventaja:** Ilimitado en cantidad de datos y más seguro (datos no visibles en URL).
        

```
<form action="/login" method="POST">
    </form>
```

---

### 2. Elementos de Entrada (Inputs)

La etiqueta `<input>` es la más versátil en los formularios. Su comportamiento cambia radicalmente según el atributo **`type`**.

| `type` (HTML4) | `type` (HTML5 Nuevo) | Función y Validaciones Nativas                                                 |
| -------------- | -------------------- | ------------------------------------------------------------------------------ |
| **`text`**     |                      | Texto de una línea.                                                            |
| **`password`** |                      | Oculta el texto.                                                               |
| **`checkbox`** |                      | Selección de múltiples opciones.                                               |
| **`radio`**    |                      | Selección de una única opción dentro de un grupo (usa el atributo **`name`**). |
| **`submit`**   |                      | Botón para enviar el formulario.                                               |
| **`email`**    |                      | Campo que requiere formato de correo.                                          |
| **`url`**      |                      | Campo que requiere formato de URL.                                             |
| **`number`**   |                      | Campo que solo acepta números y permite controles de incremento.               |
| **`date`**     |                      | Selector de calendario nativo.                                                 |
| **`range`**    |                      | Slider con valor numérico (usa `min` y `max`).                                 |


#### El Atributo `name` (¡Crucial!)

El atributo **`name`** es el más importante para la transferencia de datos. El valor de este atributo es la **clave** que el servidor usará para identificar el dato enviado.

```
<input type="text" name="nombre_usuario"> 

<input type="radio" name="plan" value="basico"> Básico
<input type="radio" name="plan" value="premium"> Premium
```

---

### 3. Atributos de Validación y Usabilidad

HTML5 te permite aplicar reglas de validación en el lado del cliente (navegador) antes de que se envíen al servidor.

|Atributo|Función|Uso con `type`|
|---|---|---|
|**`required`**|El campo no puede estar vacío. Muestra un mensaje de error nativo.|Casi todos los `input`|
|**`placeholder`**|Texto de ejemplo o pista dentro del campo (desaparece al escribir).|`text`, `email`, `password`, etc.|
|**`maxlength`**|Número máximo de caracteres permitidos.|`text`, `password`|
|**`min`/`max`**|Valor mínimo y máximo para un campo numérico.|`number`, `range`, `date`|
|**`step`**|Intervalo de incremento/decremento.|`number`, `range`|
|**`pattern`**|Expresión regular que el valor debe cumplir (validación avanzada).|`text`, `tel` (para teléfonos)|

#### Semántica y Accesibilidad: `<label>`

El elemento `<label>` mejora la usabilidad y la accesibilidad al asociar un texto descriptivo con un campo de formulario.

1. Usa el atributo **`for`** en el `<label>`.
    
2. El valor de `for` debe ser **idéntico** al atributo **`id`** del `<input>` asociado.
    

```
<label for="campo-correo">Correo Electrónico:</label>
<input type="email" id="campo-correo" name="email" required> 
```

---

### 4. Otros Elementos de Formulario

Más allá del `<input>`, hay otros elementos clave para formularios complejos:

|Etiqueta|Función|
|---|---|
|**`<textarea>`**|Campo de texto multilínea (para comentarios largos).|
|**`<select>` / `<option>`**|Menú desplegable para elegir una opción de una lista.|
|**`<datalist>`**|Lista de sugerencias para un campo de texto (no fuerza al usuario a elegir solo de la lista).|
|**`<fieldset>` / `<legend>`**|Agrupa lógicamente un conjunto de campos relacionados (ej. "Datos Personales"). `<legend>` es el título del grupo.|


```
<fieldset>
    <legend>Detalles del Pedido</legend>
    <label for="notas">Notas Adicionales:</label>
    <textarea id="notas" name="notas_pedido" rows="4" cols="50"></textarea>
</fieldset>
```

---

### Práctica recomendada

**Objetivo:** Crear un formulario de contacto semántico y con validaciones nativas.

1. Crea un nuevo archivo llamado `formulario.html`.
    
2. Implementa un formulario usando **`POST`** y con validación y usabilidad:
    

```
<body>
    <h1>Formulario de Contacto</h1>
    
    <form action="/enviar-datos" method="POST">
        
        <p>
            <label for="nombre">Nombre Completo:</label>
            <input type="text" id="nombre" name="nombre" placeholder="Juan Pérez" required>
        </p>

        <p>
            <label for="correo">Correo Electrónico:</label>
            <input type="email" id="correo" name="email" required>
        </p>

        <p>
            <label for="puntuacion">Satisfacción (1 al 10):</label>
            <input type="range" id="puntuacion" name="rating" min="1" max="10">
        </p>

        <p>
            <label for="mensaje">Tu Mensaje:</label>
            <textarea id="mensaje" name="mensaje" rows="5" placeholder="Escribe tu consulta aquí..."></textarea>
        </p>

        <button type="submit">Enviar Formulario</button> 
        </form>
</body>
```

3. Visualiza el archivo. Intenta enviar el formulario **sin llenar el campo de Correo Electrónico**. Observa cómo el navegador muestra un mensaje de error nativo de validación de HTML5, sin necesidad de JavaScript.
    

---

### Cierre

Has aprendido que los formularios son más que simples cajas de texto. Dominas el uso de `GET` vs. `POST`, la asociación crucial entre `<label>` e `<input>` (vía `for` e `id`), y la potencia de los nuevos `type` y atributos de validación (`required`, `pattern`). La próxima clase llevará la semántica a un nivel superior, estructurando todo el documento.

---

## Resumen de puntos claves

| Elemento/Atributo | Función                                    | Regla de Oro                                              |
| ----------------- | ------------------------------------------ | --------------------------------------------------------- |
| **`<form>`**      | Contenedor de formulario.                  | Usar `method="POST"` para datos sensibles.                |
| **`action`**      | URL a donde se envían los datos.           | Es el destino del formulario.                             |
| **`<input>`**     | Elemento de entrada de datos.              | Su comportamiento depende del atributo **`type`**.        |
| **`name`**        | Identificador del dato para el servidor.   | **Obligatorio** para que el dato se envíe.                |
| **`<label>`**     | Etiqueta descriptiva del campo.            | **Siempre** asociarlo con el `input` usando `for` e `id`. |
| **`required`**    | Atributo booleano de validación.           | Habilita la validación nativa del navegador.              |
| **Nuevos `type`** | `email`, `url`, `number`, `date`, `range`. | Permiten la validación automática sin JavaScript.         |

**👉 Hemos capturado los datos. Ahora, ¿cómo organizamos el sitio completo? Vamos a la siguiente clase:** [[08 - Elementos semánticos]]