Hemos llegado al penúltimo paso teórico. Ya sabes escribir código limpio (ES6+), modular y bien estructurado. Pero, ¿cómo aseguras que no rompes algo que ya funcionaba? La respuesta es el **Testing**.

---

Aquí tienes los apuntes de la clase anterior: [[18 - ES6+ y nuevas características]]

# 19 - Testing en JavaScript

## ✍️ Introducción: El Control de Calidad del Código

El **Testing** (o pruebas) en programación es el proceso de verificar y validar que el _software_ funciona exactamente como se espera. Si una función debe sumar dos números, las pruebas aseguran que siempre lo haga y que no reste, multiplique o falle al recibir `null`.

Escribir pruebas automáticas tiene varios beneficios cruciales:

1. **Seguridad:** Asegura que los cambios futuros no rompan las funcionalidades existentes (**regresiones**).
    
2. **Documentación:** Las pruebas sirven como una documentación ejecutable de cómo debe funcionar cada parte del código.
    
3. **Confianza:** Te da la confianza para refactorizar (mejorar) el código sin miedo.
    

Aquí nos centraremos en las **pruebas unitarias** (Unit Testing), que son la forma más común y básica de testing en JavaScript.

---

## 💻 Explicación Detallada: Tipos de Pruebas y Conceptos Clave

### 1. Tipos de Pruebas Comunes

|Tipo de Prueba|Enfoque|Quién lo hace|Herramientas comunes|
|---|---|---|---|
|**Unitarias**|La parte más pequeña y aislada del código (una función, un método de clase).|Desarrollador|**Jest**, Vitest, Mocha|
|**Integración**|Asegura que dos o más unidades (ej: una función y una base de datos) trabajen juntas correctamente.|Desarrollador|Jest, Supertest|
|**End-to-End (E2E)**|Simula el flujo completo del usuario en el navegador (ej: iniciar sesión, añadir al carrito, pagar).|QA/Desarrollador|Cypress, Playwright|

Exportar a Hojas de cálculo

En esta clase nos enfocaremos en **Jest** (o su alternativa moderna **Vitest**), que es la librería de pruebas unitarias más popular y robusta en el ecosistema de JavaScript.

### 2. Conceptos Clave del Testing

|Concepto|Descripción|Analogía|
|---|---|---|
|**Suite de Pruebas**|Un grupo de pruebas relacionadas, definida por la función `describe()`.|Un capítulo en un libro de pruebas.|
|**Prueba/Test**|Una verificación individual, definida por la función `test()` o `it()`.|Una pregunta específica de examen.|
|**`Expectation` (Expectativa)**|La afirmación de lo que esperas que sea el resultado.|_Espero_ que el resultado sea 5.|
|**`Matcher`**|El método que Jest usa para comparar el resultado real con la expectativa.|`toBe()`, `toEqual()`, `toBeDefined()`.|

Exportar a Hojas de cálculo

### 3. La Estructura del Código de Prueba

Una prueba unitaria sigue generalmente este patrón: **AAA** (Arrange, Act, Assert).

1. **Arrange (Preparar):** Configurar los datos, variables o condiciones necesarios.
    
2. **Act (Actuar):** Ejecutar la función o el código que se está probando.
    
3. **Assert (Asegurar/Afirmar):** Usar `expect()` y `matcher` para verificar el resultado.
    

JavaScript

```
// Estructura de una suite de pruebas con Jest/Vitest

describe('Pruebas para la función de Impuestos', () => { // Suite
    
    // Test individual
    test('Debe calcular el IVA correctamente para un valor simple', () => { 
        // 1. Arrange (Preparar)
        const montoBase = 100;
        const TASA = 0.21;
        
        // 2. Act (Actuar/Ejecutar)
        const resultado = calcularIVA(montoBase, TASA);
        
        // 3. Assert (Asegurar/Expectativa)
        // Espero que el resultado sea 121
        expect(resultado).toBe(121); 
    });
    
    // Otro test...
    test('Debe devolver 0 si el monto base es negativo', () => { 
        // ...
        expect(calcularIVA(-50, 0.21)).toBe(0);
    });
});
```

### 4. Matchers Comunes de Jest

|Matcher|Descripción|Ejemplo|
|---|---|---|
|**`.toBe(valor)`**|Compara la igualdad estricta (`===`) de valores primitivos.|`expect(num).toBe(5);`|
|**`.toEqual(objeto)`**|Compara recursivamente la igualdad de Arrays u Objetos (contenido).|`expect(array).toEqual([1, 2]);`|
|**`.toBeGreaterThan(num)`**|Verifica si un valor es mayor que otro.|`expect(saldo).toBeGreaterThan(100);`|
|**`.not.toBe(valor)`**|Niega la expectativa.|`expect(a).not.toBe(b);`|
|**`.resolves` / `.rejects`**|Se usa para probar Promesas (código asíncrono).|`expect(fetchData()).resolves.toBe('OK');`|

Exportar a Hojas de cálculo

---

## 🛠️ Ejemplos Prácticos: Estructurando una Prueba Unitaria

Para ejecutar este ejemplo, necesitarías instalar Node.js (si no lo tienes, ver [[02 - Configuración del entorno]]) y la librería Jest/Vitest con NPM. Sin embargo, para fines de apunte, nos centraremos en la estructura.

**Archivo 1: `calculadora.js` (El código a probar)**

JavaScript

```
// Código a exportar
function sumar(a, b) {
    if (typeof a !== 'number' || typeof b !== 'number') {
        throw new Error('Solo se permiten números.');
    }
    return a + b;
}

function restar(a, b) {
    return a - b;
}

// Exportamos las funciones (ver Clase 18)
module.exports = { sumar, restar }; 
// En un entorno moderno con ES6 Modules, usaríamos: export { sumar, restar };
```

**Archivo 2: `calculadora.test.js` (El código de prueba)**

JavaScript

```
// Importamos las funciones a probar
const { sumar, restar } = require('./calculadora'); 

// ----------------------------------------------------
// SUITE DE PRUEBAS
// ----------------------------------------------------
describe('Pruebas Unitarias para la Calculadora', () => {

    // Prueba 1: Éxito en la suma de enteros
    test('Debe sumar dos números enteros correctamente', () => {
        // 1. Arrange (Datos de entrada esperados)
        const num1 = 5;
        const num2 = 10;
        
        // 2. Act (Ejecución)
        const resultado = sumar(num1, num2);
        
        // 3. Assert (Verificación)
        expect(resultado).toBe(15); // Uso de Matcher:toBe (igualdad estricta)
    });
    
    // Prueba 2: Prueba de resta (Matcher distinto)
    test('La resta debe ser menor al primer número', () => {
        const resultado = restar(20, 5);
        expect(resultado).toBeLessThan(20); // Matcher: toBeLessThan
    });
    
    // Prueba 3: Manejo de errores (Excepciones)
    test('Debe lanzar un error si se pasa un string a la función sumar', () => {
        // Para probar que una función lanza un error, envolvemos la llamada
        // dentro de una función anónima.
        expect(() => sumar("a", 5)).toThrow('Solo se permiten números.');
        expect(() => sumar("a", 5)).toThrow(Error); // Podemos chequear el tipo de error
    });

});
```

---

## ✅ Resumen de Puntos Clave

- El **Testing** es crucial para garantizar la calidad, prevenir **regresiones** y documentar el código.
    
- Las **Pruebas Unitarias** (`describe`, `test`/`it`) se enfocan en la funcionalidad más pequeña (una función o método).
    
- La estructura de una prueba sigue el patrón **AAA**: **A**rrange (Preparar), **A**ct (Actuar), **A**ssert (Asegurar).
    
- La **`Expectation`** (`expect()`) y los **`Matchers`** (`.toBe()`, `.toEqual()`, `.toThrow()`) son el corazón de la verificación.
    
- **Jest** o **Vitest** son los _frameworks_ de prueba estándar en JavaScript.
    
- Probar el manejo de errores se hace con **`expect().toThrow()`**.
    

---

¡Felicidades! Has completado el Nivel 4 y con ello, todo el currículum teórico. Estás listo para el proyecto final.

El próximo paso es aplicar todo lo que has aprendido (DOM, Eventos, Funciones, Clases y buenas prácticas) para construir una aplicación completa.

👉 Continuar con [[20 - Proyecto final Aplicación completa]]