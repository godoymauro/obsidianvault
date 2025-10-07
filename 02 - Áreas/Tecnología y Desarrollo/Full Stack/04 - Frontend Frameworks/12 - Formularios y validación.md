¡Por supuesto! Hemos cubierto el estado global. Ahora nos enfocaremos en la interacción más crítica y frecuente con el usuario: los **Formularios y la Validación**. Un formulario bien manejado es la columna vertebral de cualquier aplicación de negocio.

---

Aquí tienes los apuntes de la clase anterior: [[11 - State management global]]

## 12 - Formularios y Validación

### Introducción al tema: Contexto y Utilidad

Los formularios son el puente principal por donde los datos del usuario entran a nuestra aplicación. Manejarlos eficientemente implica tres desafíos:

1. **Captura de Datos (Binding):** Vincular la entrada del usuario en el campo (`<input>`) al **Estado** del componente.
    
2. **Validación:** Asegurar que los datos cumplen con las reglas de negocio (ej: el email tiene formato correcto, la contraseña es de 8 caracteres, el campo no está vacío).
    
3. **Manejo de Envío (Submission):** Recolectar todos los datos validados y enviarlos al servidor.
    

La utilidad de las técnicas y librerías que veremos es **reducir la complejidad** de estos tres puntos, abstraer la lógica de validación y mejorar la **experiencia del usuario (UX)** al ofrecer _feedback_ inmediato sobre errores.

### Prerrequisitos

- Dominio del manejo de eventos (`onChange`, `onInput`, `v-model`, `bind:value`) (ver [[07 - Manejo de eventos]]).
    
- Entendimiento del Estado en componentes (ver [[05 - Estado en componentes]]).
    
- Conocimiento de Hooks y Composables para la reutilización de lógica (ver [[09 - Hooks y composables]]).
    

---

### Explicación Completa y Extendida

#### 1. Formularios Controlados vs. No Controlados

La primera decisión es cómo gestionamos el valor del campo:

|Tipo|Definición|Uso Principal|
|---|---|---|
|**Formulario Controlado**|El valor del campo está **siempre** determinado por el estado de JavaScript. Cada pulsación de tecla actualiza el estado, y ese estado se inyecta de vuelta al campo.|**React**, librerías de Vue/Svelte. Es el método más común y robusto para la validación.|
|**Formulario No Controlado**|El valor es gestionado internamente por el DOM. Se utiliza una referencia (Ref/useRef) para obtener el valor solo al momento de enviar.|Casos simples donde no se necesita validación instantánea o React necesita una referencia directa al DOM.|

Exportar a Hojas de cálculo

##### Implementación del Formulario Controlado (El Estándar)

- **React:** Se requiere vincular explícitamente `value` y `onChange`.
    
- **Vue/Svelte:** Se usa la directiva de enlace bidireccional (`v-model`/`bind:value`) para simplificar esta vinculación.
    

#### 2. React: Librerías para Abstraer la Complejidad

Manejar la validación y el estado de múltiples campos en React puede generar mucho _boilerplate_ (código repetitivo con `useState` y _handlers_). Por eso, se usan librerías que abstraen el estado del formulario.

|Librería|Mecanismo|Filosofía|
|---|---|---|
|**React Hook Form (Recomendado)**|Utiliza formularios **No Controlados** por defecto (usa `ref`) para **minimizar re-renders**, pero ofrece API para _input_ controlado.|Rendimiento primero. Mantiene el estado en la librería y solo se suscribe a los cambios del formulario cuando es necesario.|
|**Formik**|Utiliza formularios **Controlados** por defecto. Mantiene el estado del formulario en un componente contenedor.|Abstracción completa del formulario, estado interno más transparente.|

Exportar a Hojas de cálculo

**Ejemplo React Hook Form (RHF):**

RHF nos da funciones claves como `register` y `handleSubmit`.

JavaScript

```
import { useForm } from 'react-hook-form';

function FormularioRegistro() {
  // 1. Inicializar el hook
  const { register, handleSubmit, formState: { errors } } = useForm();

  // 2. Función que se llama al enviar si la validación es EXITOSA
  const onSubmit = (data) => {
    console.log("Datos validados:", data);
  };

  return (
    // 3. Pasar handleSubmit a onSubmit del formulario
    <form onSubmit={handleSubmit(onSubmit)}>
      <input 
        placeholder="Nombre" 
        // 4. Registrar el input, incluyendo reglas de validación
        {...register("nombre", { required: "El nombre es obligatorio", minLength: 3 })}
      />
      {/* 5. Mostrar errores */}
      {errors.nombre && <p className="error">{errors.nombre.message}</p>}

      <button type="submit">Registrar</button>
    </form>
  );
}
```

#### 3. Vue: VeeValidate (Basado en Esquemas)

Vue tradicionalmente gestiona bien los formularios simples con `v-model`. Para validaciones complejas, **VeeValidate** es la librería estándar. VeeValidate es compatible con librerías de validación de esquemas como **Yup**.

- **Componentes Clave:** `Form`, `Field`, `ErrorMessage`.
    
- **Concepto:** Se define un **esquema de validación** (usando Yup) que describe las reglas para cada campo. VeeValidate se encarga de aplicar estas reglas.
    

**Ejemplo VeeValidate:**

HTML

```
<template>
  <Form @submit="onSubmit" :validation-schema="schema">
    <Field name="email" type="email" placeholder="Email" />
    <ErrorMessage name="email" class="error" />
    <button type="submit">Enviar</button>
  </Form>
</template>

<script setup>
  import { Form, Field, ErrorMessage } from 'vee-validate';
  import * as yup from 'yup'; // Librería Yup para definir el esquema

  // 1. Definir el esquema de validación con Yup
  const schema = yup.object({
    email: yup.string().required('El email es obligatorio').email('Email inválido'),
  });

  // 2. Función que se ejecuta al enviar
  const onSubmit = (values) => {
    console.log('Datos validados:', values);
  };
</script>
```

#### 4. Svelte: Librerías Livianas (Felte, Svelte Forms)

Svelte, debido a su filosofía de ser un compilador ligero, tiene soluciones que a menudo son más sencillas. **Felte** es una opción popular, ya que aprovecha la reactividad de Svelte y los _bindings_ bidireccionales (`bind:value`).

**Concepto Felte:** Ofrece un _hook_ o _composable_ (`createForm`) que te devuelve las funciones necesarias para la validación y el _binding_.

**Ejemplo Felte (Svelte):**

HTML

```
<script>
  import { createForm } from 'felte';
  import { reporter } from '@felte/reporter-svelte'; // Plugin para mostrar errores

  // 1. Inicializar el form con una función de validación
  const { form, data, errors, isValid } = createForm({
    onSubmit: async (values) => {
      console.log('Datos enviados:', values);
    },
    validate: (values) => {
      // 2. Función de validación (similar a Redux-Form o lógica custom)
      const errors = {};
      if (!values.nombre) errors.nombre = 'El nombre es obligatorio';
      return errors;
    }
  }, {
    // 3. Usa el reporter para inyectar las clases de error
    extend: reporter, 
  });
</script>

<form use:form> <input name="nombre" type="text" placeholder="Nombre" />
  {#if $errors.nombre}
    <p class="error">{$errors.nombre}</p>
  {/if}
  
  <button type="submit" disabled={!$isValid}>Guardar</button>
</form>
```

---

### Tablas Comparativas de Soluciones de Formulario

|Framework|Librería Recomendada|Enfoque de Binding|Mecanismo de Validación|
|---|---|---|---|
|**React**|**React Hook Form**|No Controlado (Refs) por defecto|Esquemas (Yup, Zod) o reglas inline.|
|**Vue**|**VeeValidate**|Controlado (`v-model` con componentes)|Esquemas (Yup) o reglas basadas en cadenas.|
|**Svelte**|**Felte**|Bidireccional (`bind:value` con _actions_)|Función `validate` custom o integraciones con Yup.|

Exportar a Hojas de cálculo

### Ejercicios Prácticos Paso a Paso

**Objetivo:** Crear un componente de _input_ reutilizable que muestre un mensaje de error si el campo está vacío al enviar.

1. **Componente Padre:** Usa la librería (`useForm`, `Form` o `createForm`).
    
2. **Validación Requerida:** Agrega la regla de que el campo `username` es obligatorio.
    
3. **Manejo de Error:** Muestra el mensaje de error inmediatamente después de que el usuario intente enviar el formulario.
    

_(El ejemplo de React Hook Form ilustra perfectamente este ejercicio, mostrando la forma más concisa de lograrlo usando la función `register` y accediendo al objeto `errors`.)_

### Errores Comunes y Troubleshooting

- **React Hook Form: No Usar `register`:** Olvidar aplicar el _spread operator_ `...register("nombre")` al _input_. RHF no puede rastrear un campo que no ha sido registrado.
    
- **Validación en Tiempo Real vs. Envío:** La validación puede ser costosa. Por defecto, RHF valida en el evento `onSubmit` y luego en `onChange` para los campos que ya tienen errores.
    
    - **Tip:** Si el rendimiento es vital, configura RHF con la opción `mode: 'onBlur'` para validar solo cuando el campo pierde el foco.
        
- **Inmutabilidad en el Envío:** Asegúrate de que, en el _handler_ de envío (`onSubmit`), la data que envías al servidor está limpia y en el formato correcto (ej: quita campos de confirmación de contraseña).
    
- **Vue/Svelte: Olvidar la Suscripción:** En librerías basadas en _hooks_ o _composables_, debes recordar acceder a las variables de _feedback_ (como `isValid` o `errors`) con la sintaxis de reactividad correcta (`$errors` en Svelte, `errors.value` en Vue) para que la UI se actualice.
    

### Resumen de Puntos Clave

- Los formularios controlados (valor gestionado por JS) son el estándar para la validación avanzada.
    
- **React Hook Form (RHF)** es la opción más popular en React por su rendimiento, utilizando el patrón de **Formularios No Controlados** (Refs) por defecto.
    
- **VeeValidate** es el estándar en Vue, usando un enfoque basado en **Esquemas de Yup** para definir las reglas de validación.
    
- **Felte** es una opción liviana en Svelte que aprovecha la reactividad integrada del compilador.
    
- La **Validación** debe ofrecer _feedback_ inmediato al usuario para una buena **UX**, y las librerías simplifican la visualización de los mensajes de error.
    
- La clave es externalizar la lógica de estado y validación del formulario del componente en sí, usando _hooks_ o _composables_ específicos.
    

### Enlaces Internos

👉 El paso natural después de validar los datos del formulario es enviarlos a una API, lo que veremos en detalle en [[13 - Fetch y consumo de APIs]].

👉 La validación compleja podría beneficiarse de abstracciones de estado que hemos visto en [[11 - State management global]].

---

**Siguiente paso:** Los formularios nos llevan directamente al consumo de datos externos. Es hora de dominar la interacción con los servicios de _backend_. Pasemos a [[13 - Fetch y consumo de APIs]].