¡Fantástico! Has puesto tu API en producción. Ahora, pasamos a la **vanguardia del desarrollo backend**. Si REST es la base, **GraphQL** es la optimización para el consumo moderno de datos.

Entramos al **Nivel 6: Avanzado y arquitecturas modernas**.

Aquí tienes la nota para la clase anterior: **[[26 - Despliegue]]**.

---

# 27 - GraphQL con Node.js

## 📚 Introducción: El Problema de REST y la Solución GraphQL

Hemos aprendido a construir APIs **RESTful** ( **[[15 - APIs RESTful]]**), donde el servidor define los _endpoints_. Esto tiene dos problemas principales:

1. **Over-fetching (Sobre-obtención de datos):** El _endpoint_ REST a menudo devuelve **más datos** de los que el cliente realmente necesita (ej: necesitas solo el nombre y el ID de un usuario, pero la API te da 20 campos). Esto desperdicia ancho de banda y ralentiza la aplicación.
    
2. **Under-fetching (Sub-obtención de datos) / Múltiples Peticiones:** El cliente necesita hacer **varias peticiones** para obtener todos los datos relacionados (ej: una petición para obtener el usuario, y luego otra petición separada por cada pedido de ese usuario).
    

**GraphQL** es un **lenguaje de consulta para APIs** que permite al cliente **solicitar exactamente los datos que necesita**, y nada más, en una sola petición.

---

## 🧐 ¿Qué es GraphQL?

|Característica|GraphQL|REST|
|---|---|---|
|**Petición**|Siempre se realiza una sola petición HTTP **POST** a un único _endpoint_ (ej: `/graphql`).|Múltiples _endpoints_ basados en recursos (ej: `/usuarios`, `/pedidos/1`).|
|**Control**|El **Cliente** define los datos (campos) que desea en el _payload_ de la petición.|El **Servidor** define la estructura de los datos que devuelve.|
|**Estructura**|Fuertemente tipado mediante un **Schema** que define todos los tipos de datos disponibles.|Sin un esquema estricto por defecto.|
|**Operaciones**|**Queries** (obtener datos), **Mutations** (modificar datos), **Subscriptions** (tiempo real).|GET, POST, PUT/PATCH, DELETE.|

### Ejemplo de Consulta (Query)

**Petición GraphQL (Cliente):**

GraphQL

```
query ObtenerUsuario {
  usuario(id: "1") {
    nombre
    email
    pedidos {
      id
      total
    }
  }
}
```

**Respuesta JSON (Servidor):**

JSON

```
{
  "data": {
    "usuario": {
      "nombre": "Alice",
      "email": "alice@mail.com",
      "pedidos": [
        { "id": "101", "total": 50.00 },
        { "id": "102", "total": 12.50 }
      ]
    }
  }
}
```

**Observa:** El cliente obtuvo el usuario Y sus pedidos **en una sola petición**, y solo obtuvo los campos `nombre`, `email`, `id` y `total`, ignorando cualquier otro campo de la base de datos.

---

## 🚀 Implementación en Node.js con Apollo Server

El _framework_ más popular para construir servicios GraphQL en Node.js es **Apollo Server**.

### 1. Definición del Schema (Type Definitions)

El **Schema** es el contrato entre el cliente y el servidor. Define los tipos de datos disponibles y las operaciones (Queries y Mutations).

JavaScript

```
// Necesitarás: npm install @apollo/server graphql
const { ApolloServer } = require('@apollo/server');
const { gql } = require('graphql-tag');

// Usamos la etiqueta gql para parsear el string del Schema
const typeDefs = gql`
  # 1. Definición de Tipos de Datos
  type Pedido {
    id: ID!
    total: Float!
  }

  type Usuario {
    id: ID!
    nombre: String!
    pedidos: [Pedido!] # Relación: un usuario tiene una lista de pedidos
  }

  # 2. Definición de Queries (Puntos de entrada de lectura)
  type Query {
    # Una query llamada 'usuario' que requiere un argumento 'id'
    usuario(id: ID!): Usuario
    usuarios: [Usuario]
  }

  # 3. Definición de Mutations (Puntos de entrada de escritura)
  type Mutation {
    crearUsuario(nombre: String!, email: String!): Usuario
  }
`;
```

### 2. Los Resolvers (La Lógica de Negocio)

Los **Resolvers** son funciones que le dicen a GraphQL **cómo obtener los datos** para un campo específico del Schema. Es aquí donde llamas a tu base de datos (ORM/ODM).

JavaScript

```
// Simulación de una BD/Servicio (usando async/await es la norma)
const usuarioService = require('../services/usuarioService'); 

const resolvers = {
  // Resuelve las queries del Schema
  Query: {
    // Resolver para la query 'usuario(id: ID!)'
    usuario: async (parent, args, context, info) => {
      // args.id contiene el valor que el cliente envió
      return usuarioService.findById(args.id); 
    },
    usuarios: async () => {
        return usuarioService.findAll();
    }
  },

  // Resuelve las mutations del Schema
  Mutation: {
    crearUsuario: async (parent, args) => {
      return usuarioService.create(args.nombre, args.email);
    },
  },

  // Resolver para manejar las RELACIONES (el campo 'pedidos' de Usuario)
  Usuario: {
    pedidos: async (parent, args, context, info) => {
      // 💡 parent: Contiene el objeto 'Usuario' que se está resolviendo
      // Lógica: Buscar todos los pedidos donde el id_usuario es igual al id del padre (usuario)
      return usuarioService.findPedidosByUserId(parent.id); 
    }
  }
};
```

**Resolución de Campos:** GraphQL es eficiente porque resuelve cada campo individualmente. Cuando el cliente pide `pedidos` en la consulta, el resolver `Usuario.pedidos` se ejecuta y llama a la BD solo para ese usuario específico.

### 3. Configuración del Servidor

JavaScript

```
const { startStandaloneServer } = require('@apollo/server/standalone');

async function startApolloServer() {
  const server = new ApolloServer({
    typeDefs,
    resolvers,
  });

  // startStandaloneServer configura el servidor HTTP (Express/Koa)
  const { url } = await startStandaloneServer(server, {
    listen: { port: 4000 },
  });

  console.log(`🚀 GraphQL Server corriendo en: ${url}`);
}

startApolloServer();
```

---

## 💡 Conceptos Clave Adicionales

- **Mutations:** Se usan para operaciones de escritura (CREATE, UPDATE, DELETE). Son el equivalente a los métodos POST, PUT, DELETE de REST.
    
- **Subscriptions:** Permiten al cliente suscribirse a eventos para recibir datos en tiempo real a través de WebSockets (ver [[28 - WebSockets y tiempo real]]).
    
- **Data Loaders:** Herramienta esencial para optimizar GraphQL. Resuelve el problema de las **N+1 queries** al agrupar peticiones a la base de datos en una sola operación por lote.
    

---

## ✅ Buenas Prácticas y Consejos

|Categoría|Buena Práctica|Error Común|
|---|---|---|
|**REST vs GraphQL**|Usar GraphQL para la API principal de datos y REST para _endpoints_ simples (ej: Health check, subir archivos).|Reemplazar REST por GraphQL en todos los escenarios, incluyendo la subida de archivos grandes (donde REST es mejor).|
|**Tipado**|Usar los tipos escalares de GraphQL (`String`, `Int`, `Boolean`, `ID`, `Float`) y definir todos los campos.|Dejar campos sin definir, lo que rompe el contrato entre cliente y servidor.|
|**N+1**|Implementar **Data Loaders** desde el inicio para evitar sobrecargar la base de datos con consultas repetitivas.|Dejar que el _resolver_ `pedidos` ejecute una consulta a la BD por cada usuario en la lista (N+1 problema).|
|**Errores**|GraphQL maneja errores a nivel de campos, no HTTP. Usar _custom errors_ que se propaguen para un manejo estandarizado.|Usar códigos de estado HTTP 4xx/5xx ya que GraphQL casi siempre responde con 200 OK.|

---

## 🔑 Resumen de Puntos Clave

- **GraphQL** permite al cliente especificar **exactamente qué datos necesita**, resolviendo el _over-fetching_ y el _under-fetching_ de REST.
    
- El núcleo son el **Schema (`typeDefs`)**, que define los tipos y las operaciones, y los **Resolvers**, que contienen la lógica de negocio (llamadas a la BD).
    
- La **resolución de campos** de GraphQL es eficiente y resuelve relaciones de forma declarativa.
    
- **Apollo Server** es el _framework_ estándar de Node.js para implementar GraphQL.
    
- **Queries** son para lectura, **Mutations** para escritura y **Subscriptions** para tiempo real.
    

Hemos cubierto la obtención de datos de forma eficiente. ¿Qué pasa si necesitas enviar datos al cliente en el instante en que ocurren?

Tu próxima clase te introducirá a la comunicación bidireccional: **[[28 - WebSockets y tiempo real]]**.

---