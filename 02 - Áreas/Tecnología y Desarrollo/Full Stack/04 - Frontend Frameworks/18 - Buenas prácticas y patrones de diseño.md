¡Continuemos con las bases de la excelencia! Hemos cubierto las herramientas y librerías. Ahora, abordaremos cómo estructurar y organizar la lógica para garantizar que nuestras aplicaciones sean fáciles de escalar y mantener. El tema es **Buenas Prácticas y Patrones de Diseño**.

---

Aquí tienes los apuntes de la clase anterior: [[17 - Component libraries y ecosistema]]

## 18 - Buenas Prácticas y Patrones de Diseño

### Introducción al tema: Contexto y Utilidad

Cuando una aplicación crece, el código sin estructura clara se convierte rápidamente en una pesadilla de mantenimiento, un fenómeno conocido como **"código espagueti"**. Los **Patrones de Diseño** son soluciones probadas a problemas comunes de arquitectura de software, y las **Buenas Prácticas** son guías para escribir código limpio, legible y eficiente.

Aplicar estos conceptos nos proporciona:

1. **Mantenibilidad:** Facilita la identificación y corrección de errores.
    
2. **Escalabilidad:** Permite que nuevos desarrolladores se unan al proyecto y que nuevas características se añadan sin romper el código existente.
    
3. **Coherencia:** Garantiza que todos en el equipo sigan la misma estructura y convención.
    

### Prerrequisitos

- Dominio de la composición de componentes (ver [[04 - Componentes básicos]]).
    
- Comprensión del Estado y las Props (ver [[05 - Estado en componentes]] y [[06 - Props y comunicación entre componentes]]).
    

---

### Explicación Completa y Extendida

#### 1. Patrones de Organización de Componentes: Atomic Design

**Atomic Design** es una metodología de diseño de interfaz de usuario que descompone la UI en cinco niveles distintos, organizados de lo simple a lo complejo. Su objetivo es crear un sistema de diseño modular y reutilizable.

|Nivel|Descripción|Ejemplos en Código|Principio Clave|
|---|---|---|---|
|**1. Átomos (Atoms)**|Elementos HTML indivisibles y básicos.|`<Boton>`, `<Input>`, `<Titulo>`.|Mínima funcionalidad.|
|**2. Moléculas (Molecules)**|Grupos de Átomos que funcionan como una unidad.|`CampoFormulario` (etiqueta + input + error), `BarraBúsqueda`.|Combinación funcional.|
|**3. Organismos (Organisms)**|Grupos de Moléculas y/o Átomos que forman una sección compleja de la interfaz.|`Header`, `FormularioLogin`, `TarjetaProducto`.|Componente de sección completa.|
|**4. Plantillas (Templates)**|Estructuras de la página (layouts) sin contenido real. Usan Organismos dispuestos en un esqueleto.|`LayoutDashboard`, `PaginaProducto`.|Define la disposición (wireframe).|
|**5. Páginas (Pages)**|Instancias específicas de las Plantillas con **contenido real** y datos finales.|`DashboardUsuarioA`, `PaginaDetalleProductoID=123`.|Máxima fidelidad (los usuarios ven esto).|

Exportar a Hojas de cálculo

**Ventaja Clave:** Al empezar desde los Átomos, se asegura la máxima **reutilización**. Un cambio en un Átomo (ej: el color primario del `<Boton>`) se propaga a través de todos los niveles.

#### 2. Separación de Preocupaciones: Componentes Contenedor vs. Presentacionales

Este es quizás el patrón arquitectónico más importante en Frontend: la separación de la **Lógica (Contenedor)** de la **Apariencia (Presentacional)**.

|Componente|Rol|Lógica Clave|
|---|---|---|
|**Presentacional (Dummy/Puro)**|**Cómo se ve.** Solo se encarga de recibir _props_ y renderizar la UI. No tiene estado propio (excepto quizás de UI, ej: _toggle_).|No contiene lógica de negocio, _fetching_ de datos ni interacción con _stores_ globales.|
|**Contenedor (Smart/Inteligente)**|**Cómo funciona.** Contiene la lógica de negocio, gestiona el estado (local y global), llama a APIs y pasa los datos y _callbacks_ (funciones) a los componentes Presentacionales.|Contiene `useEffect` / `onMounted`, llamadas a _stores_ y lógica de negocio compleja.|

Exportar a Hojas de cálculo

**Ejemplo Práctico:**

- **`ListaProductosContainer` (Contenedor):** Llama a `useFetch('/api/productos')`, gestiona el estado de `loading` y `error`.
    
- **`ListaProductos` (Presentacional):** Recibe el array de productos como _prop_ y los mapea para renderizar un `<TarjetaProducto>` por cada uno.
    

#### 3. Buenas Prácticas de Código Limpio

Independientemente del _framework_, estas reglas mejorarán la calidad del código:

- **Convención de Nombres (Naming Conventions):**
    
    - **Componentes:** Siempre **PascalCase** (`TarjetaProducto`).
        
    - **Hooks/Composables:** Siempre prefijo **`use`** (`useFetchData`).
        
    - **Props de Evento:** Prefijo **`on`** o **`al`** (`onClick`, `onUpdateValue`).
        
- **Principio DRY (Don't Repeat Yourself):** Reutiliza la lógica duplicada en **Custom Hooks** o **Composables** (ver [[09 - Hooks y composables]]).
    
- **Inmutabilidad:** Nunca mutar el estado o las _props_ directamente. Siempre crear una copia y actualizarla (especialmente crucial para arrays y objetos).
    
    - _Mal:_ `state.productos.push(nuevo)`
        
    - _Bien:_ `setState(prev => ({ ...prev, productos: [...prev.productos, nuevo] }))`
        
- **Early Returns / Guard Clauses:** Manejar los casos de error o _loading_ al principio de la función/componente para evitar anidamiento profundo.
    
    JavaScript
    
    ```
    function Componente({ data, loading }) {
      if (loading) return <Spinner />; // Early Return
      if (!data) return <p>No hay datos.</p>; // Guard Clause
      // Lógica principal aquí
    }
    ```
    

#### 4. Clean Architecture en Frontend (Concepto Avanzado)

Para proyectos masivos, se puede aplicar la **Arquitectura Limpia (Clean Architecture)** al frontend. El objetivo es aislar el **Núcleo de la Aplicación** (la lógica de negocio) de los detalles de la tecnología (el _framework_, la librería de _fetch_, el _router_).

- **Capas:**
    
    1. **Entidades (Core):** Modelos de datos puros y lógica de negocio.
        
    2. **Casos de Uso (Business Logic):** La orquestación de la lógica (ej: `autenticarUsuario()`, `crearPedido()`).
        
    3. **Adaptadores/Interfaces:** Traducción entre el _framework_ y el núcleo.
        
    4. **Frameworks/Drivers:** El _framework_ (React/Vue), la UI y las llamadas a la API (la capa externa).
        

**Ventaja:** Si decides cambiar de React a Vue en 5 años, la capa de **Casos de Uso** y **Entidades** permanece intacta, desacoplando el _frontend_ de la decisión tecnológica.

---

### Ejercicios Prácticos Paso a Paso

**Objetivo:** Refactorizar un componente monolítico en el patrón Contenedor/Presentacional.

1. **Monolito (`TarjetaProducto.jsx`):** Contiene el `useFetch`, la lógica de `loading` y el marcado (HTML) de la tarjeta.
    
2. **Refactorización - Contenedor:** Crea `TarjetaProductoContainer.jsx`.
    
    - Mueve el `useFetch` y la lógica de `loading/error` aquí.
        
    - Pasa los `datos` limpios y las funciones de acción (ej: `addToCart`) a un componente hijo.
        
3. **Refactorización - Presentacional:** Crea `TarjetaProductoView.jsx` (o simplemente `TarjetaProducto`).
    
    - Solo recibe las _props_ de `data` y `callbacks`.
        
    - Renderiza el marcado basado en esas _props_.
        
    - Ya no tiene estado ni `useEffect`.
        

JavaScript

```
// TarjetaProductoContainer.jsx (El Contenedor)
function TarjetaProductoContainer({ productId }) {
  const { data: producto, loading } = useFetch(`/api/productos/${productId}`);
  const handleAddToCart = () => { /* Lógica de store global */ };

  if (loading) return <Spinner />;

  // Pasa solo los datos y callbacks necesarios a la vista
  return <TarjetaProductoView producto={producto} onAddToCart={handleAddToCart} />;
}

// TarjetaProductoView.jsx (El Presentacional/Puro)
function TarjetaProductoView({ producto, onAddToCart }) {
  return (
    <div className="card">
      <h2>{producto.nombre}</h2>
      <p>Precio: ${producto.precio}</p>
      <Boton onClick={() => onAddToCart(producto.id)}>Añadir</Boton>
    </div>
  );
}
```

### Errores Comunes y Troubleshooting

- **Romper el SRP (Principio de Responsabilidad Única):** Intentar que un componente haga demasiadas cosas (ej: gestionar el estado global, el _fetch_, la lógica de negocio y la apariencia).
    
    - **Solución:** Extrae la lógica compleja a **Hooks/Composables** y separa la lógica de la apariencia con el patrón **Contenedor/Presentacional**.
        
- **Abstracción Excesiva:** Crear demasiados niveles de anidamiento de componentes (demasiados Átomos y Moléculas) para lógica muy simple. El código se vuelve difícil de navegar.
    
    - **Tip:** Abstraer solo cuando se necesite reutilización o cuando el componente supere las 50–70 líneas de código.
        
- **Importaciones Relativas Largas:** Usar `import Componente from '../../../common/utils/Componente';`.
    
    - **Solución:** Configurar **Alias de Ruta** en el _bundler_ (ej: `@/components/Componente` en Vite/Webpack) para importaciones absolutas claras.
        

### Resumen de Puntos Clave

- Los **Patrones de Diseño** y las **Buenas Prácticas** son esenciales para la mantenibilidad y escalabilidad de las SPAs.
    
- **Atomic Design** (Átomos, Moléculas, Organismos) es la metodología principal para organizar la UI de forma modular y reutilizable.
    
- El patrón **Contenedor/Presentacional** es clave: los **Contenedores** manejan **Lógica** y **Estado**, y los **Presentacionales** manejan solo la **Apariencia** a través de _props_.
    
- Las buenas prácticas incluyen: el uso de **Convenciones de Nombres**, la aplicación del principio **DRY** (a través de Hooks/Composables), la **Inmutabilidad** y el uso de **Early Returns**.
    
- La **Arquitectura Limpia** es un enfoque avanzado para desacoplar la lógica de negocio del _framework_ subyacente.
    

### Enlaces Internos

👉 La reutilización de lógica es el objetivo principal de este módulo: [[09 - Hooks y composables]].

👉 La separación de preocupaciones es clave para escribir tests efectivos: [[15 - Testing]].

---

**Siguiente paso:** Hemos cubierto todos los conceptos fundamentales y avanzados. ¡Estamos listos para construir! El último nivel se centra en la aplicación práctica de todo lo aprendido. Pasemos a [[19 - Proyecto guiado Dashboard SPA]].