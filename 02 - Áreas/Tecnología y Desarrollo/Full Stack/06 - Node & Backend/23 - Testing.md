¡Adelante! Con la **seguridad** implementada, es hora de asegurarnos de que el código que hemos escrito realmente haga lo que se supone que debe hacer y que no se rompa en el futuro. Esto se logra con el **Testing**.

El _testing_ es una habilidad profesional esencial para crear APIs **robustas, confiables y escalables**.

Aquí tienes la nota para la clase anterior: **[[22 - Seguridad en Node.js]]**.

---

# 23 - Testing

## 📚 Introducción: ¿Por qué Hacemos Testing?

En el desarrollo backend, los _tests_ son un conjunto de código automatizado que verifica que las funciones y componentes de tu aplicación se comporten como se espera.

El testing es vital porque:

1. **Garantía de Calidad:** Confirma que el código funciona correctamente hoy.
    
2. **Refactoring Seguro:** Te permite reestructurar el código sabiendo que si algo se rompe, el _test_ fallará inmediatamente.
    
3. **Detección de Regresiones:** Asegura que una nueva característica no introduzca errores en funcionalidades antiguas.
    

Node.js tiene herramientas maduras para el _testing_, siendo **Jest** el estándar de la industria.

---

## 🔬 Tipos de Testing en el Backend

Es importante diferenciar los niveles de prueba que aplicaremos a nuestra API:

|Tipo de Test|Propósito|Alcance|Herramientas Comunes|
|---|---|---|---|
|**Unitarios**|Probar la **unidad de código más pequeña** (una función, un método de clase) de forma aislada.|**Una función** (sin dependencias externas).|**Jest**, Mocha|
|**Integración**|Probar que **varios componentes** trabajen juntos correctamente (ej: un _router_ y un _middleware_).|Rutas de Express, ORM/BD (a veces con BD simulada).|**Jest**, Supertest|
|**End-to-End (E2E)**|Simular el **flujo completo** de un usuario desde el navegador o cliente, probando todo el sistema.|Cliente (navegador) ![](data:,) Servidor ![](data:,) BD.|Cypress, Playwright|

En el backend, nos centraremos principalmente en **Unitarios** y **de Integración**.

---

## 🛠 Jest: El Framework Estándar

**Jest** es un _framework_ de _testing_ desarrollado por Meta (Facebook) que funciona directamente con JavaScript y Node.js. Es rápido, fácil de configurar y viene con herramientas de aserción, _mocking_ y cobertura de código.

### Paso 1: Instalación

Bash

```
npm install --save-dev jest supertest
```

- **Jest:** El _framework_ principal.
    
- **Supertest:** Librería diseñada para probar APIs de Express (útil para tests de integración).
    

### Paso 2: Configuración del Script

Añade el script de _testing_ a tu `package.json` ( **[[07 - Gestión de dependencias]]**):

JSON

```
"scripts": {
    "test": "jest",
    "test:watch": "jest --watchAll"
},
```

### Paso 3: Código Base de un Test Unitario

Los archivos de _test_ deben llamarse con el sufijo **`.test.js`** o **`.spec.js`**.

**Archivo:** `calculadora.js`

JavaScript

```
// La unidad de código a probar
const sumar = (a, b) => a + b;
const restar = (a, b) => a - b;

module.exports = { sumar, restar };
```

**Archivo:** `calculadora.test.js`

JavaScript

```
const { sumar, restar } = require('./calculadora');

// 💡 describe: Agrupa tests relacionados
describe('Pruebas Unitarias de Calculadora', () => {
    
    // 💡 test (o it): Un caso de prueba individual
    test('debe sumar dos números correctamente', () => {
        // expect: La función a verificar
        // toBe: El matcher (aserción)
        expect(sumar(2, 3)).toBe(5);
        expect(sumar(0, 0)).toBe(0);
        expect(sumar(-1, 5)).toBe(4);
    });

    test('debe restar dos números correctamente', () => {
        expect(restar(10, 4)).toBe(6);
        // Usando un matcher más flexible
        expect(restar(10, 3)).not.toBe(8);
    });
});
```

**Ejecución:** `npm test`

---

## 🎭 Mocking y Spying

Para los tests **Unitarios** y de **Integración**, a menudo necesitamos **aislar** la unidad de prueba.

- **Mocking:** Reemplazar una dependencia externa (como una consulta a la BD o una llamada a un servicio externo) con una función falsa que devuelve un resultado predecible. Esto hace que el test sea rápido y determinista.
    
- **Spying:** Envolver una función existente para rastrear cuándo fue llamada, con qué argumentos y cuántas veces, sin alterar su implementación original.
    

**Ejemplo de Mocking (Simular una BD):**

JavaScript

```
// jest.mock('../db/usuario'); // Mocks el módulo completo de la BD
const UsuarioDB = require('../db/usuario');
const obtenerUsuario = require('../services/usuario');

describe('Prueba de Servicio de Usuario', () => {
    test('debe devolver un usuario si existe', async () => {
        // 💡 jest.fn(): Crea una función simulada
        UsuarioDB.findById.mockResolvedValue({ id: 1, nombre: 'Mock User' });

        const usuario = await obtenerUsuario(1);
        
        // 💡 Esperamos que la función simulada haya sido llamada
        expect(UsuarioDB.findById).toHaveBeenCalledWith(1); 
        expect(usuario.nombre).toBe('Mock User');
    });
});
```

---

## 🏗 Testing de Integración (APIs con Supertest)

Los tests de integración verifican que el _middleware_, el _router_ y el _handler_ de la ruta trabajen juntos. **Supertest** nos permite enviar peticiones HTTP simuladas a nuestra aplicación Express **sin** necesidad de que el servidor esté escuchando en un puerto.

### Ejemplo Práctico con Supertest

JavaScript

```
// Necesitas exportar tu aplicación Express en app.js (module.exports = app;)
const request = require('supertest');
const app = require('../app'); 

describe('Pruebas de Integración de API Tareas', () => {
    
    test('GET /api/v1/tareas debe devolver un array de tareas (200 OK)', async () => {
        // request(app): Indica que vamos a testear la instancia de Express
        const response = await request(app)
            .get('/api/v1/tareas')
            .expect(200) // 💡 Espera el status 200
            .expect('Content-Type', /json/); // Espera que sea JSON

        // Aserciones sobre el cuerpo de la respuesta
        expect(Array.isArray(response.body)).toBe(true);
        expect(response.body.length).toBeGreaterThanOrEqual(0);
    });
    
    test('POST /api/v1/tareas debe crear una nueva tarea (201 Created)', async () => {
        const nuevaTarea = { descripcion: 'Tarea de Testeando Express' };
        
        const response = await request(app)
            .post('/api/v1/tareas')
            .send(nuevaTarea) // Envía el cuerpo de la petición
            .expect(201); // Espera el status 201 Created

        expect(response.body.descripcion).toBe(nuevaTarea.descripcion);
        expect(response.body.id).toBeDefined();
    });
});
```

---

## ✅ Buenas Prácticas y Errores Comunes

|Categoría|Buena Práctica|Error Común|
|---|---|---|
|**Enfoque**|Aplicar tests unitarios a la lógica de negocio pura y tests de integración a la capa de API (rutas/DB).|Sobrecargar los tests unitarios con lógica que debería ser de integración (ej: probar si la BD funciona).|
|**Hooks**|Usar los _hooks_ de Jest (`beforeAll`, `beforeEach`, `afterAll`, `afterEach`) para inicializar recursos (conexiones a BD) y limpiarlos.|Dejar los datos de prueba en la BD después de la ejecución de los tests.|
|**Tests Asíncronos**|Usar **`async/await`** o devolver una **`Promise`** en los tests que tienen lógica asíncrona.|Olvidar `await` o no usar `done()` (en sintaxis antigua), haciendo que el test termine antes de que la lógica asíncrona se complete.|
|**Aislamiento**|Asegurar que un test falle solo si su funcionalidad específica falla, no por la configuración de otro test.|Usar el mismo estado de datos para varios tests sin limpiarlo entre ejecuciones.|

---

## 🔑 Resumen de Puntos Clave

- El **Testing** automatizado es esencial para la robustez y escalabilidad de una API.
    
- Los **Tests Unitarios** prueban funciones aisladas; los de **Integración** prueban el flujo de componentes (ej: la ruta de Express).
    
- **Jest** es el _framework_ estándar, con sintaxis clave como `describe`, `test`/`it`, y `expect`.
    
- El **Mocking** (simulación) es crucial para aislar el código de las dependencias lentas (BD, red).
    
- **Supertest** es el _middleware_ de Express para enviar peticiones HTTP simuladas en tests de integración.
    

Has cubierto la base de la estabilidad. El siguiente paso en el ciclo de vida de una API es cómo la monitoreamos y registramos lo que sucede en tiempo real.

Tu próxima clase se centrará en la observabilidad: **[[24 - Logs y monitoreo]]**.