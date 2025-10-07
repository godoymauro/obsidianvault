Hemos optimizado el rendimiento. Ahora, vamos a la parte más importante para garantizar la calidad y la mantenibilidad a largo plazo de nuestro código: el **Testing**. No importa qué tan rápido sea un componente si no es confiable.

---

Aquí tienes los apuntes de la clase anterior: [[14 - Performance y lazy loading]]

## 15 - Testing

### Introducción al tema: Contexto y Utilidad

El **Testing** es el proceso de verificar que tu código se comporta de la manera esperada bajo diferentes condiciones. En el desarrollo Frontend moderno, el testing es crucial para asegurar que los componentes de la interfaz, la lógica de negocio y la interacción con el usuario funcionan correctamente después de cada cambio o actualización.

La utilidad del testing es crítica:

1. **Prevención de Regresiones:** Garantiza que las nuevas funciones o las refactorizaciones no rompan el código existente.
    
2. **Confianza en la Refactorización:** Permite reestructurar código con la certeza de que las pruebas detectarán si algo falla.
    
3. **Documentación Implícita:** Los tests bien escritos documentan el comportamiento esperado de un componente.
    

El testing se clasifica generalmente en la **Pirámide de Testing**:

- **Tests Unitarios (Unit Tests):** Prueban la unidad más pequeña de lógica (una función, un Hook, un Composable) de forma aislada. Son rápidos y numerosos.
    
- **Tests de Integración (Integration Tests):** Prueban cómo interactúan dos o más unidades (ej: un componente que usa un Hook de estado). Son más lentos que los unitarios.
    
- **Tests E2E (End-to-End Tests):** Simulan el flujo completo de un usuario en un entorno de navegador real (ej: "Usuario se logea, añade un producto al carrito y realiza el checkout"). Son los más lentos y costosos.
    

### Prerrequisitos

- Conocimiento de cómo la lógica se separa en Hooks/Composables (ver [[09 - Hooks y composables]]).
    
- Entendimiento del manejo de Eventos y Estado (ver [[07 - Manejo de eventos]] y [[05 - Estado en componentes]]).
    

---

### Explicación Completa y Extendida

#### 1. Herramientas Estándar del Ecosistema

|Framework|Runner Principal|Librería de DOM Virtual|Herramienta E2E Sugerida|
|---|---|---|---|
|**React**|**Jest** / **Vitest**|**React Testing Library (RTL)**|Cypress / Playwright|
|**Vue**|**Vitest**|**Vue Test Utils** + RTL|Cypress / Playwright|
|**Svelte**|**Vitest**|**Svelte Testing Library**|Cypress / Playwright|

Exportar a Hojas de cálculo

**Nota sobre RTL:** La **Testing Library** (en todas sus variantes) promueve la filosofía de probar los componentes de la misma manera en que el usuario los usaría, sin enfocarse en detalles internos de implementación. Esto hace que los tests sean más robustos a los cambios de código (_refactorings_).

#### 2. Tests Unitarios y de Integración (React/RTL)

En React, el objetivo principal es simular la renderización del componente en un DOM virtual y luego interactuar con él.

**Pasos Clave de un Test de Componente con RTL:**

1. **Renderizar:** Usar `render()` para montar el componente en el DOM virtual.
    
2. **Consulta:** Usar métodos de consulta (ej: `getByRole`, `getByText`) para encontrar elementos de la UI.
    
3. **Interactuar:** Usar `fireEvent` o `userEvent` para simular clics, inputs de teclado, etc.
    
4. **Aserción:** Usar el método `expect()` para verificar que la UI o la lógica de negocio hayan cambiado.
    

**Ejemplo React Testing Library (RTL):**

Vamos a probar un componente `Contador` que incrementa un número al hacer click.

JavaScript

```
// __tests__/Contador.test.jsx
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import Contador from '../src/components/Contador';

// userEvent simula una interacción de usuario más realista que fireEvent
const user = userEvent.setup(); 

test('El contador debe incrementar el valor al hacer click', async () => {
  // 1. Renderizar el componente
  render(<Contador initialValue={5} />);
  
  // 2. Consulta: Encontrar el botón por su rol (la forma preferida de RTL)
  const boton = screen.getByRole('button', { name: /incrementar/i });
  
  // 3. Aserción inicial
  expect(screen.getByText('Valor: 5')).toBeInTheDocument();
  
  // 4. Interactuar: Simular el click
  await user.click(boton);
  
  // 5. Aserción final: Verificar el nuevo estado del DOM
  expect(screen.getByText('Valor: 6')).toBeInTheDocument();
  
  // 6. Verificar que el valor anterior YA NO esté presente
  expect(screen.queryByText('Valor: 5')).not.toBeInTheDocument();
});
```

#### 3. Mocks (Simulaciones)

Cuando pruebas un componente, a menudo necesitas aislarlo de sus dependencias (ej: llamadas a API, estado global, librerías de terceros). Para esto se usan **Mocks** (simulaciones).

- **Mockear API calls:** En un test de componente, no quieres hacer una llamada real a la red. Usas `jest.mock('axios')` o la librería `msw` (Mock Service Worker) para interceptar las peticiones y devolver datos ficticios.
    
- **Mockear Hooks/Composables:** Si tu componente usa un hook complejo (`useFetch`), lo _mockeas_ para que devuelva datos predecibles.
    

JavaScript

```
// Ejemplo de Mock de API con Jest (usando fetch global)
global.fetch = jest.fn(() =>
  Promise.resolve({
    json: () => Promise.resolve({ nombre: 'Test User' }),
  })
);
```

#### 4. Tests Unitarios en Vue (Vitest + Vue Test Utils)

En Vue, se usa **Vue Test Utils (VTU)** para montar el componente y acceder a sus propiedades, aunque los métodos de consulta de RTL son ahora el estándar para la interacción. **Vitest** es el _runner_ moderno que reemplaza a Jest en el ecosistema Vite.

JavaScript

```
// Ejemplo Vue Test Utils + Vitest
import { mount } from '@vue/test-utils';
import { screen, fireEvent } from '@testing-library/vue';
import Contador from '../src/components/Contador.vue';

test('El contador debe incrementar el valor al hacer click en Vue', async () => {
  const wrapper = mount(Contador); // Montar el componente
  
  // Aserción inicial (VTU o RTL)
  expect(wrapper.text()).toContain('Valor: 0');

  const boton = screen.getByRole('button'); // RTL query
  await fireEvent.click(boton); // Simular evento

  // Aserción final
  expect(wrapper.text()).toContain('Valor: 1');
});
```

#### 5. Tests End-to-End (E2E)

Los tests E2E son la capa superior de la pirámide. Confirman que todo (Frontend, Backend, red, base de datos) funciona conjuntamente. **Cypress** y **Playwright** son las herramientas dominantes:

- **Cypress:** Un marco de pruebas moderno que ejecuta las pruebas directamente en el navegador. Excelente para desarrolladores Frontend.
    
- **Playwright (Microsoft):** Soporta múltiples navegadores (Chromium, Firefox, WebKit) y lenguajes de programación. Se está convirtiendo en el estándar para E2E robusto.
    

**Ejemplo de Flujo E2E (Pseudocódigo de Cypress):**

JavaScript

```
// Cypress (E2E)
it('Debería permitir al usuario autenticarse y ver el dashboard', () => {
  cy.visit('/login'); // 1. Navegar a la página de login
  
  // 2. Interactuar con los inputs
  cy.get('#email').type('test@example.com');
  cy.get('#password').type('password123');
  
  // 3. Enviar el formulario
  cy.get('button[type="submit"]').click();
  
  // 4. Aserción final: verificar la URL y el contenido
  cy.url().should('include', '/dashboard');
  cy.contains('Bienvenido al Dashboard').should('be.visible');
});
```

---

### Ejercicios Prácticos Paso a Paso

**Objetivo:** Escribir un test unitario para un Hook/Composable que maneje un _toggle_ booleano (`useToggle` visto en [[09 - Hooks y composables]]).

1. **Herramienta:** Utiliza la función `renderHook` de React Testing Library (o su equivalente en VTU) para probar _hooks_ aislados.
    
2. **Prueba de Estado Inicial:** Verifica que el valor inicial sea `false`.
    
3. **Prueba de Acción:** Llama a la función _toggle_ y verifica que el valor cambie a `true`.
    

JavaScript

```
// __tests__/useToggle.test.js (React)
import { renderHook, act } from '@testing-library/react';
import { useToggle } from '../src/hooks/useToggle';

test('useToggle debe cambiar el valor al llamar a toggle', () => {
  // 1. Renderizar el hook
  const { result } = renderHook(() => useToggle(false));

  // 2. Aserción inicial
  expect(result.current[0]).toBe(false);

  // 3. Act: Envolver la llamada a la función para simular la actualización del estado
  act(() => {
    result.current[1](); // Llamar a la función toggle
  });

  // 4. Aserción final
  expect(result.current[0]).toBe(true);
});
```

### Errores Comunes y Troubleshooting

- **Probar Detalles de Implementación (Anti-Patrón RTL):** Usar `wrapper.find('div.clase-interna')` o acceder directamente al estado del componente. Esto hace que las pruebas se rompan si cambias solo la estructura interna sin cambiar el comportamiento para el usuario.
    
    - **Solución:** Usa **Consultas Guiadas por el Usuario** (ej: `getByRole`, `getByLabelText`, `getByText`).
        
- **Olvidar `await` en Interacciones:** Muchas interacciones (clics, cambios de estado) son asíncronas en el DOM virtual. Olvidar `await user.click(...)` o `await fireEvent(...)` puede llevar a errores _flaky_ (intermitentes).
    
- **No Usar Mocks:** Hacer llamadas reales a la API o usar la hora actual en los tests, lo que hace que fallen en diferentes entornos o cuando el _backend_ no está disponible.
    
    - **Solución:** Mocks para I/O (Input/Output).
        

### Resumen de Puntos Clave

- El **Testing** es vital para prevenir regresiones y garantizar la confiabilidad del código.
    
- La **Pirámide de Testing** prioriza los tests **Unitarios** y de **Integración** sobre los E2E.
    
- **React Testing Library (RTL)** es el estándar para probar componentes de forma que simule la experiencia del usuario (por comportamiento, no por implementación).
    
- Se usan **Mocks** (simulaciones) para aislar las dependencias (APIs, _timers_) durante los tests.
    
- **Jest/Vitest** son los _runners_ más comunes, y **Cypress/Playwright** son la elección para pruebas **E2E**.
    
- **Anti-Patrón:** Evita probar los detalles internos del componente; enfócate en lo que el usuario ve y puede interactuar.
    

### Enlaces Internos

👉 El código que necesita ser cubierto por tests a menudo incluye lógica de optimización (para garantizar que las _props_ de `memo` funcionen) y lógica de estado: [[14 - Performance y lazy loading]].

👉 La fiabilidad de la interfaz es la base antes de añadir efectos visuales: [[16 - Animaciones y transiciones]].

---

**Siguiente paso:** Nuestro código es confiable. Ahora, vamos a la parte más visible para el usuario: la **experiencia visual**. Pasemos a [[16 - Animaciones y transiciones]].