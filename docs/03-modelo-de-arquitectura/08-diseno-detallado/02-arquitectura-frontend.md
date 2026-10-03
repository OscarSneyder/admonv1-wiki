# Arquitectura del Frontend

Este frontend está construido con **React + Vite** y sigue una **arquitectura orientada a features basada en los principios de Clean Architecture**.

El objetivo arquitectónico principal es separar las reglas de negocio, la lógica de aplicación, las integraciones externas y los aspectos de interfaz de usuario, de modo que los componentes de React no queden acoplados directamente a las APIs del backend ni a detalles de infraestructura.

---

## 🛠️ Tecnologías

El frontend utiliza actualmente:

* **React**
* **Vite**
* **JavaScript**
* **React Router**
* **Fetch API**
* **CSS**
* **ESLint**

React se encarga de construir la interfaz de usuario, mientras que Vite proporciona el entorno de desarrollo y el proceso de construcción para producción.

React Router gestiona la navegación del lado del cliente.

La comunicación con el backend se implementa mediante adaptadores de infraestructura que usan la Fetch API nativa.

---

## 🏗️ Estilo arquitectónico

El proyecto sigue un enfoque de **Clean Architecture orientada a features (Feature-First)**.

En lugar de organizar toda la aplicación exclusivamente por tipo técnico:

```text
components/
pages/
services/
api/
```

la aplicación se organiza principalmente alrededor de **features de negocio**:

```text
features/
├── authentication/
├── convocations/
├── applications/
└── documents/
```

Cada feature es dueña de los componentes y la lógica necesarios para implementar su capacidad de negocio.

Internamente, las features se dividen en capas arquitectónicas:

```text
feature/
├── domain/
├── application/
├── infrastructure/
└── presentation/
```

Esto proporciona dos niveles de organización:

```text
Feature de negocio
      │
      ├── Dominio
      ├── Aplicación
      ├── Infraestructura
      └── Presentación
```

La feature define **a qué capacidad de negocio pertenece el código**, mientras que las capas internas definen **qué responsabilidad tiene el código**.

---

## 🧱 Capas de Clean Architecture

El flujo de dependencias sigue los principios de Clean Architecture:

```text
┌──────────────────────────────┐
│         Presentación         │
│  Páginas / Componentes React │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│          Aplicación          │
│         Casos de uso         │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│            Dominio           │
│     Conceptos de negocio     │
└──────────────────────────────┘

        Infraestructura
              │
              │ implementa / integra
              ▼
     APIs del backend / HTTP
```

La regla importante es que la lógica de negocio y de aplicación no debe depender directamente de componentes de React ni de detalles de implementación HTTP.

---

## 📂 Estructura de una feature

Una feature puede seguir una estructura similar a:

```text
src/features/
└── convocations/
    ├── domain/
    │   ├── entities/
    │   └── repositories/
    │
    ├── application/
    │   └── use-cases/
    │
    ├── infrastructure/
    │   └── api/
    │
    └── presentation/
        ├── components/
        └── pages/
```

El contenido exacto de cada feature puede variar según su complejidad.

No es obligatorio que todas las features contengan todas las carpetas posibles.

---

## 🧠 Capa de dominio

La capa `domain` contiene los conceptos que representan el negocio.

```text
domain/
├── entities/
└── repositories/
```

Sus responsabilidades típicas incluyen:

* Entidades de negocio.
* Modelos de dominio.
* Contratos de repositorio.
* Conceptos de negocio independientes de React.
* Reglas que no dependen de la infraestructura.

La capa de dominio debe evitar dependencias de:

```text
React
React Router
Fetch API
HTTP
APIs del navegador
Detalles de implementación del backend
```

Esto mantiene los conceptos de negocio aislados de las tecnologías externas.

---

## ⚙️ Capa de aplicación

La capa `application` contiene los **casos de uso** de la aplicación.

```text
application/
└── use-cases/
```

Algunos ejemplos son operaciones como:

```text
GetConvocations
GetConvocationById
CreateApplication
GetApplications
GetApplicationById
GetDocumentTypes
UploadDocument
Login
```

Un caso de uso representa una acción que la aplicación puede realizar.

La capa de presentación debe invocar estos casos de uso en lugar de implementar los flujos de negocio directamente dentro de los componentes de React.

Conceptualmente:

```text
Página React
    │
    ▼
Caso de uso
    │
    ▼
Repositorio / Gateway
```

Así, las páginas se enfocan principalmente en el estado de la interfaz, los eventos y el renderizado.

---

## 🔌 Capa de infraestructura

La capa `infrastructure` contiene las implementaciones que se comunican con sistemas externos.

```text
infrastructure/
└── api/
```

Esto incluye aspectos como:

* Peticiones HTTP al backend.
* Fetch API.
* Encabezados de autenticación.
* Endpoints de la API.
* Transformaciones de peticiones y respuestas.
* Integraciones con almacenamiento externo.

Por ejemplo:

```text
GET  /api/calls
GET  /api/calls/:id
POST /api/applications
POST /api/documents/presigned-url
```

Por lo tanto, la infraestructura es responsable de **cómo se obtiene la información**, mientras que la capa de aplicación define **qué operación debe realizarse**.

---

## 🖥️ Capa de presentación

La capa `presentation` contiene todo lo relacionado directamente con React y la interfaz de usuario.

```text
presentation/
├── pages/
└── components/
```

### Páginas

Las páginas representan pantallas completas de la aplicación asociadas a rutas.

Sus responsabilidades incluyen:

* Coordinar el estado de la interfaz.
* Leer los parámetros de la ruta.
* Invocar los casos de uso de la aplicación.
* Manejar los estados de carga y error.
* Renderizar componentes.

### Componentes

Los componentes contienen elementos de interfaz reutilizables usados por páginas u otros componentes.

Algunos ejemplos pueden ser:

```text
Loading
ErrorMessage
StatusBadge
Navbar
```

La lógica de negocio y la comunicación directa con el backend deben mantenerse fuera de los componentes de presentación reutilizables siempre que sea posible.

---

## 🔄 Flujo de una petición

Una petición típica sigue este flujo:

```text
Usuario
 │
 ▼
Página React
 │
 ▼
Caso de uso de aplicación
 │
 ▼
Repositorio / Gateway
 │
 ▼
Adaptador de API de infraestructura
 │
 ▼
API del backend
```

La respuesta regresa a través de los mismos límites de abstracción:

```text
API del backend
 │
 ▼
Adaptador de infraestructura
 │
 ▼
Aplicación
 │
 ▼
Presentación
 │
 ▼
Interfaz de usuario
```

Esto evita que las páginas queden fuertemente acopladas a los detalles de implementación del backend.

---

## 📦 Independencia de las features

Cada feature debe contener la lógica asociada a su propia capacidad de negocio.

Por ejemplo:

```text
features/
├── authentication/
├── convocations/
├── applications/
└── documents/
```

El objetivo es evitar una estructura global en la que funcionalidades de negocio no relacionadas terminen mezcladas dentro de carpetas genéricas como:

```text
pages/
services/
api/
components/
```

Que cada feature sea dueña de su funcionalidad hace que el código sea más fácil de recorrer y permite que las funcionalidades evolucionen con menos dependencias entre módulos no relacionados.

---

## 🔐 Autenticación

La autenticación debe tratarse como una capacidad propia de la aplicación, en lugar de incrustar su lógica directamente en features no relacionadas.

Conceptualmente:

```text
Autenticación
      │
      ▼
Gestión del token
      │
      ▼
Peticiones a la API de infraestructura
      │
      ▼
Authorization: Bearer <token>
```

Otras features pueden consumir la información de autenticación, pero no deben ser responsables de implementar el proceso de inicio de sesión.

---

## 🌐 Comunicación con el backend

Las features del frontend se comunican con el backend a través de adaptadores de infraestructura.

La capa de presentación debe evitar patrones como:

```text
Componente React
      │
      ▼
fetch(...)
```

El flujo preferido es:

```text
Componente React
      │
      ▼
Caso de uso
      │
      ▼
Gateway / Repositorio
      │
      ▼
Fetch API
```

Esto establece un límite claro entre el código de interfaz y la comunicación externa.

---

## 🧩 Principios arquitectónicos

La arquitectura sigue varios principios:

### Separación de responsabilidades

Cada capa tiene una responsabilidad claramente definida.

### Dirección de las dependencias

La lógica de negocio no debe depender directamente de detalles de presentación ni de infraestructura.

### Propiedad por feature

La funcionalidad de negocio se agrupa por feature y no exclusivamente por tipo técnico.

### Bajo acoplamiento

Los componentes de React deben tener un conocimiento mínimo de los detalles de implementación del backend.

### Alta cohesión

El código relacionado con la misma capacidad de negocio permanece junto.

### Infraestructura reemplazable

Las implementaciones HTTP pueden evolucionar sin requerir cambios importantes en la presentación ni en la lógica de negocio.

---

## 🧭 Vista general de la arquitectura

```text
src/
│
├── features/
│   │
│   ├── authentication/
│   │   ├── domain/
│   │   ├── application/
│   │   ├── infrastructure/
│   │   └── presentation/
│   │
│   ├── convocations/
│   │   ├── domain/
│   │   ├── application/
│   │   ├── infrastructure/
│   │   └── presentation/
│   │
│   ├── applications/
│   │   ├── domain/
│   │   ├── application/
│   │   ├── infrastructure/
│   │   └── presentation/
│   │
│   └── documents/
│       ├── domain/
│       ├── application/
│       ├── infrastructure/
│       └── presentation/
│
├── shared/
│   └── ...
│
├── App.jsx
└── main.jsx
```

---

## 📌 Resumen de la arquitectura

```text
Arquitectura       : Principios de Clean Architecture
Organización       : Por features (Feature First)
Interfaz           : React
Construcción       : Vite
Enrutamiento       : React Router
Cliente HTTP       : Fetch API

Capas de cada feature:
  Dominio          → Conceptos y contratos de negocio
  Aplicación       → Casos de uso
  Infraestructura  → Implementaciones de API y servicios externos
  Presentación     → Páginas y componentes de React
```

La arquitectura está diseñada para que el frontend crezca agregando o evolucionando features de negocio, sin concentrar la lógica de aplicación dentro de los componentes de React ni crear dependencias directas entre la interfaz y los servicios externos.
