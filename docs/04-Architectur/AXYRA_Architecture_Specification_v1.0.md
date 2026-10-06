# AXYRA — Architecture Specification

**Versión:** 1.0
**Estado:** Draft / Baseline
**Fecha:** 2026-10-06
**Proyecto:** AXYRA
**Documento:** Architecture Specification

---

# 1. Propósito

Este documento define la arquitectura técnica de AXYRA.

Su objetivo es establecer una estructura tecnológica y arquitectónica que permita transformar el modelo de dominio en una plataforma:

* Modular.
* Multi-tenant.
* Escalable.
* Segura.
* Mantenible.
* Observable.
* Reutilizable.
* Preparada para integraciones externas.
* Adecuada tanto para SaaS como para implementaciones individuales.

Este documento constituye el puente entre el diseño conceptual y la implementación.

---

# 2. Objetivos arquitectónicos

La arquitectura debe permitir:

1. Aislamiento completo entre Tenants.
2. Desarrollo modular.
3. Evolución independiente de módulos.
4. Seguridad desde el diseño.
5. Integración con servicios externos.
6. Escalabilidad progresiva.
7. Mantenimiento sencillo.
8. Automatización de procesos.
9. Observabilidad.
10. Reutilización del núcleo de AXYRA.
11. Despliegue continuo.
12. Capacidad de incorporar inteligencia artificial posteriormente.

---

# 3. Principios arquitectónicos

## 3.1 Domain First

El dominio de negocio debe ser independiente de los detalles tecnológicos.

```text
Domain
   ↓
Application
   ↓
Infrastructure
```

La lógica comercial no debe depender directamente de un proveedor externo.

---

## 3.2 Modularidad

Cada módulo debe poseer responsabilidades claramente definidas.

Ejemplo:

```text
Catalog
Inventory
Commerce
Customers
Services
Analytics
Identity
Configuration
Integrations
```

Los módulos deben poder evolucionar sin generar acoplamiento innecesario.

---

## 3.3 Multi-tenancy by Design

La arquitectura debe considerar el Tenant desde el inicio.

No se implementará multi-tenancy como una funcionalidad agregada posteriormente.

---

## 3.4 Security by Design

La seguridad debe aplicarse en:

* Autenticación.
* Autorización.
* API.
* Base de datos.
* Infraestructura.
* Gestión de secretos.
* Integraciones.
* Auditoría.

---

## 3.5 API First

Los módulos de negocio deben exponerse mediante contratos claros.

La interfaz web será un consumidor de la API y no la fuente de verdad del negocio.

---

## 3.6 Infrastructure Independence

El dominio no debe depender directamente de:

* Supabase.
* Vercel.
* WhatsApp.
* n8n.
* Proveedores de pago.
* Proveedores de almacenamiento.

Estos servicios deben estar detrás de interfaces o adaptadores cuando corresponda.

---

# 4. Estilo arquitectónico

AXYRA utilizará inicialmente una arquitectura:

```text
Modular Monolith
+
Layered Architecture
+
Domain-oriented Design
+
API-first
```

No se comenzará con microservicios.

---

# 5. Justificación del Modular Monolith

Durante la etapa inicial, separar AXYRA en múltiples microservicios produciría complejidad innecesaria:

* Mayor infraestructura.
* Mayor dificultad de desarrollo.
* Comunicación entre servicios.
* Observabilidad distribuida.
* Deployments independientes.
* Mayor superficie de errores.

Un Modular Monolith permite:

```text
Un solo sistema desplegable
        │
        ├── Identity
        ├── Catalog
        ├── Inventory
        ├── Customers
        ├── Commerce
        ├── Services
        ├── Analytics
        ├── Configuration
        └── Integrations
```

manteniendo límites internos claros.

Si en el futuro un módulo requiere separación, podrá extraerse.

---

# 6. Arquitectura de alto nivel

```text
                         INTERNET
                            │
             ┌──────────────┴──────────────┐
             │                             │
             ▼                             ▼
       Public Web                    Admin Web
             │                             │
             └──────────────┬──────────────┘
                            │
                            ▼
                    API / Application
                            │
          ┌─────────────────┼─────────────────┐
          │                 │                 │
          ▼                 ▼                 ▼
       Domain          Application       Integrations
          │                 │                 │
          └─────────────────┼─────────────────┘
                            │
                            ▼
                       Persistence
                            │
                            ▼
                       PostgreSQL
```

---

# 7. Capas arquitectónicas

## 7.1 Presentation

Responsable de:

* Interfaces web.
* Formularios.
* Navegación.
* Visualización.
* Validaciones de presentación.
* Gestión de sesión en cliente.

No debe contener reglas centrales del negocio.

---

## 7.2 API

Responsable de:

* Recepción de solicitudes.
* Autenticación.
* Autorización.
* Validación.
* Serialización.
* Manejo de errores.
* Exposición de contratos.

---

## 7.3 Application

Coordina casos de uso.

Ejemplos:

```text
CreateProduct
UpdateProduct
CreateOrder
ConfirmOrder
CreateQuote
AcceptQuote
RequestService
AssignService
AdjustInventory
```

Esta capa coordina operaciones sin almacenar directamente detalles de infraestructura.

---

## 7.4 Domain

Contiene:

* Entidades.
* Value Objects.
* Reglas de negocio.
* Estados.
* Servicios de dominio.
* Eventos de dominio.
* Invariantes.

Esta es la capa central de AXYRA.

---

## 7.5 Infrastructure

Contiene implementaciones concretas para:

* Base de datos.
* Storage.
* Email.
* WhatsApp.
* APIs externas.
* Logging.
* Servicios de terceros.

---

# 8. Estructura conceptual

```text
Presentation
      │
      ▼
API
      │
      ▼
Application
      │
      ▼
Domain
      ▲
      │
Infrastructure
```

La dependencia conceptual debe apuntar hacia el dominio y no al contrario.

---

# 9. Módulos principales

## 9.1 Identity

Responsabilidades:

* Usuarios.
* Roles.
* Permisos.
* Autenticación.
* Sesiones.
* Autorización.

---

## 9.2 Tenant Management

Responsabilidades:

* Empresas.
* Estado del Tenant.
* Configuración base.
* Aislamiento.

---

## 9.3 Catalog

Responsabilidades:

* Productos.
* Categorías.
* Imágenes.
* Listas de precios.

---

## 9.4 Inventory

Responsabilidades:

* Existencias.
* Movimientos.
* Ajustes.
* Reservas.
* Políticas de inventario.

---

## 9.5 Customers

Responsabilidades:

* Clientes.
* Datos de contacto.
* Direcciones.
* Historial comercial.

---

## 9.6 Commerce

Responsabilidades:

* Carrito.
* Pedidos.
* Cotizaciones.
* Líneas comerciales.
* Estados comerciales.

---

## 9.7 Services

Responsabilidades:

* Tipos de servicio.
* Solicitudes.
* Asignaciones.
* Notas.
* Estados.

---

## 9.8 Analytics

Responsabilidades:

* Indicadores.
* Métricas.
* Reportes.
* Agregaciones.
* Dashboards.

Analytics no debe modificar directamente entidades comerciales.

---

## 9.9 Configuration

Responsabilidades:

* Configuración del Tenant.
* Horarios.
* Módulos.
* Preferencias.

---

## 9.10 Integrations

Responsabilidades:

* WhatsApp.
* Email.
* Pagos.
* Automatizaciones.
* Servicios externos.

---

# 10. Frontend Architecture

El frontend se dividirá conceptualmente en dos superficies principales:

```text
Public Application
        │
        ├── Home
        ├── Catalog
        ├── Product
        ├── Cart
        ├── Checkout / Order
        ├── Quotes
        └── Service Request


Administrative Application
        │
        ├── Dashboard
        ├── Products
        ├── Inventory
        ├── Customers
        ├── Orders
        ├── Quotes
        ├── Services
        ├── Analytics
        └── Configuration
```

Ambas superficies consumirán los mismos contratos backend.

---

# 11. Backend Architecture

El backend seguirá una estructura modular.

Conceptualmente:

```text
backend/
│
├── identity/
├── tenants/
├── catalog/
├── inventory/
├── customers/
├── commerce/
├── services/
├── analytics/
├── configuration/
├── integrations/
└── shared/
```

Cada módulo debe separar:

```text
domain/
application/
infrastructure/
presentation/
```

cuando la complejidad lo justifique.

---

# 12. API Architecture

La API utilizará contratos versionados.

Ejemplo:

```text
/api/v1/products
/api/v1/categories
/api/v1/customers
/api/v1/orders
/api/v1/quotes
/api/v1/services
/api/v1/inventory
```

La versión inicial será:

```text
v1
```

Cambios incompatibles deberán introducir una nueva versión.

---

# 13. Request lifecycle

Una solicitud típica seguirá:

```text
Client
  │
  ▼
HTTP Request
  │
  ▼
Authentication
  │
  ▼
Tenant Resolution
  │
  ▼
Authorization
  │
  ▼
Validation
  │
  ▼
Application Use Case
  │
  ▼
Domain Logic
  │
  ▼
Persistence
  │
  ▼
Response
```

---

# 14. Tenant Resolution

Cada solicitud autenticada debe permitir determinar el Tenant actual.

Conceptualmente:

```text
Request
   │
   ▼
Authenticated User
   │
   ▼
User → Tenant
   │
   ▼
Tenant Context
```

El Tenant Context será utilizado para limitar las operaciones.

---

# 15. Multi-tenancy Strategy

La estrategia inicial será:

```text
Shared Database
+
Shared Schema
+
tenant_id
+
Database-level isolation
```

Cada entidad operativa tendrá una relación con el Tenant.

La seguridad definitiva será reforzada mediante políticas de base de datos.

---

# 16. Database

La arquitectura utilizará PostgreSQL como motor de persistencia relacional.

Razones:

* Modelo relacional adecuado al dominio.
* Integridad referencial.
* Transacciones.
* Constraints.
* Índices.
* JSON/JSONB cuando sea necesario.
* Soporte robusto para multi-tenancy.
* Row Level Security.
* Ecosistema maduro.

El proveedor administrado de PostgreSQL se decidirá mediante un ADR específico.

---

# 17. Supabase

Supabase podrá utilizarse como plataforma de infraestructura para:

* PostgreSQL.
* Authentication.
* Storage.
* APIs auxiliares.
* Row Level Security.

Sin embargo, AXYRA no debe acoplar su dominio a Supabase.

La arquitectura debe permitir reemplazar el proveedor en el futuro.

---

# 18. Authentication

La autenticación será responsabilidad de una capa especializada.

Conceptualmente:

```text
User
  │
  ▼
Authentication Provider
  │
  ▼
Authenticated Identity
  │
  ▼
AXYRA Authorization
```

La autenticación determina quién es el usuario.

La autorización determina qué puede hacer.

---

# 19. Authorization

AXYRA utilizará autorización basada en:

```text
User
  ↓
Role
  ↓
Permission
```

Ejemplo:

```text
inventory.adjust
```

permite realizar ajustes de inventario.

La autorización debe evaluarse en backend.

Nunca se confiará únicamente en controles del frontend.

---

# 20. Row Level Security

La base de datos deberá aplicar aislamiento mediante Row Level Security cuando el proveedor y arquitectura definitiva lo permitan.

Conceptualmente:

```text
User
 ↓
Tenant Context
 ↓
Database Policy
 ↓
Allowed Rows
```

Una consulta nunca debe poder devolver datos de otro Tenant.

---

# 21. Transactions

Las operaciones que modifican múltiples entidades relacionadas deberán utilizar transacciones.

Ejemplo:

```text
Confirm Order
     │
     ├── Validate order
     ├── Validate inventory
     ├── Reserve / deduct inventory
     ├── Update order status
     └── Create audit record
```

Estas operaciones deben mantener consistencia transaccional.

---

# 22. Inventory Consistency

Las operaciones de inventario son especialmente sensibles.

Una modificación deberá:

1. Validar el producto.
2. Validar el Tenant.
3. Validar cantidades.
4. Crear el movimiento.
5. Actualizar el estado de inventario.
6. Registrar auditoría cuando corresponda.

No debe existir una modificación de stock sin trazabilidad.

---

# 23. Commerce Consistency

Una orden debe conservar:

* Productos.
* Cantidades.
* Precios.
* Descuentos.
* Impuestos.
* Totales.

Los valores históricos no deben depender de consultas posteriores al catálogo.

---

# 24. External Integrations

Las integraciones externas utilizarán adaptadores.

Conceptualmente:

```text
Application
     │
     ▼
Integration Interface
     │
     ├── WhatsApp Adapter
     ├── Email Adapter
     ├── Payment Adapter
     └── Automation Adapter
```

Esto evita que el dominio dependa directamente de un proveedor.

---

# 25. WhatsApp Architecture

MVP:

```text
Order
  │
  ▼
WhatsApp URL Generator
  │
  ▼
WhatsApp
```

Evolución:

```text
Order
  │
  ▼
WhatsApp Interface
  │
  ▼
WhatsApp Business API Adapter
```

El cambio de integración no debe requerir rediseñar Commerce.

---

# 26. Storage Architecture

Las imágenes de productos y archivos deberán almacenarse fuera de la base de datos principal.

Conceptualmente:

```text
Product
   │
   ▼
Storage Reference
   │
   ▼
Object Storage
```

La base de datos almacenará referencias y metadatos.

---

# 27. Caching

El caching no será una dependencia obligatoria del dominio.

Inicialmente se priorizará:

* Correctitud.
* Simplicidad.
* Consistencia.

El caching podrá incorporarse posteriormente en:

* Catálogo público.
* Configuración.
* Consultas frecuentes.
* Analytics.

---

# 28. Background Jobs

Las tareas no críticas para la respuesta inmediata podrán ejecutarse de forma asíncrona.

Ejemplos:

```text
Generate report
Send notification
Process webhook
Send email
Synchronize integration
Generate analytics aggregate
```

La infraestructura concreta para jobs será definida posteriormente.

---

# 29. Observability

AXYRA deberá contemplar:

## Logging

Registro de errores y eventos técnicos.

## Metrics

Métricas de:

* Requests.
* Latencia.
* Errores.
* Uso de recursos.
* Operaciones comerciales.

## Tracing

Se podrá incorporar tracing distribuido cuando la arquitectura evolucione.

---

# 30. Auditability

Las operaciones administrativas y comerciales relevantes deberán poder auditarse.

Ejemplo:

```text
User
  │
  ▼
Action
  │
  ├── Entity
  ├── Previous Value
  ├── New Value
  └── Timestamp
```

---

# 31. Error Handling

La API utilizará respuestas de error consistentes.

Conceptualmente:

```json
{
  "error": {
    "code": "INVENTORY_INSUFFICIENT",
    "message": "Insufficient inventory.",
    "details": {}
  }
}
```

Los mensajes expuestos al usuario no deben revelar información sensible.

---

# 32. Validation

La validación se realizará en múltiples niveles:

```text
Frontend
   ↓
API
   ↓
Application
   ↓
Domain
   ↓
Database
```

La validación del frontend mejora UX.

La validación backend garantiza seguridad y consistencia.

---

# 33. Security Boundaries

Las principales fronteras de seguridad serán:

```text
Internet
   │
   ▼
Frontend
   │
   ▼
API
   │
   ├── Authentication
   ├── Authorization
   ├── Tenant Isolation
   ├── Validation
   └── Rate Limiting
   │
   ▼
Application
   │
   ▼
Database
```

---

# 34. Rate Limiting

Las operaciones públicas y sensibles deberán considerar rate limiting.

Especialmente:

* Login.
* Registro.
* Solicitudes de servicio.
* Creación de pedidos.
* Endpoints públicos.
* Integraciones.

La estrategia concreta será definida posteriormente.

---

# 35. Secrets Management

Los secretos nunca deben almacenarse:

* En Git.
* En código fuente.
* En frontend.
* En documentación pública.

Ejemplos:

```text
DATABASE_URL
API_KEYS
JWT_SECRET
WHATSAPP_TOKEN
PAYMENT_SECRET
STORAGE_SECRET
```

deben gestionarse mediante variables de entorno o un gestor de secretos.

---

# 36. Deployment Architecture

La arquitectura inicial podrá desplegarse mediante:

```text
                    GitHub
                       │
                       ▼
                CI / Deployment
                       │
             ┌─────────┴─────────┐
             ▼                   ▼
        Web Application      API / Backend
             │                   │
             └─────────┬─────────┘
                       ▼
                  PostgreSQL
                       │
             ┌─────────┴─────────┐
             ▼                   ▼
          Storage            Integrations
```

Los proveedores definitivos serán establecidos mediante ADRs.

---

# 37. Environments

AXYRA deberá contemplar como mínimo:

```text
Development
Staging
Production
```

## Development

Uso local del equipo.

## Staging

Validación previa a producción.

## Production

Sistema utilizado por clientes reales.

Cada entorno debe poseer configuración y credenciales independientes.

---

# 38. CI/CD

El repositorio deberá evolucionar hacia un pipeline que ejecute:

```text
Commit
  ↓
Lint
  ↓
Type Check
  ↓
Unit Tests
  ↓
Integration Tests
  ↓
Build
  ↓
Deploy
```

Los pasos exactos dependerán del stack definitivo.

---

# 39. Testing Architecture

Se utilizarán diferentes niveles:

## Unit Tests

Para:

* Reglas de dominio.
* Cálculos.
* Validaciones.
* Casos de uso aislados.

## Integration Tests

Para:

* Base de datos.
* API.
* Autenticación.
* Integraciones internas.

## End-to-End

Para flujos completos:

```text
Catalog
 → Cart
 → Order
 → Confirmation
```

y:

```text
Service Request
 → Assignment
 → Completion
```

---

# 40. Performance Principles

La arquitectura priorizará:

* Consultas eficientes.
* Índices adecuados.
* Paginación.
* Lazy loading.
* Optimización de imágenes.
* Caching selectivo.
* Procesamiento asíncrono.

No se realizará optimización prematura sin evidencia.

---

# 41. Scalability Strategy

La escalabilidad será progresiva.

## Etapa 1

```text
Modular Monolith
+
Managed PostgreSQL
+
Object Storage
```

## Etapa 2

Optimización de:

* Database.
* Caching.
* Background jobs.
* CDN.

## Etapa 3

Separación de módulos que realmente requieran independencia.

Ejemplo:

```text
Commerce
Analytics
Integrations
```

podrían convertirse en servicios independientes únicamente cuando exista una razón técnica o comercial.

---

# 42. AI Readiness

AXYRA debe poder incorporar capacidades de inteligencia artificial sin convertir el dominio principal en un sistema dependiente de IA.

Posibles capacidades futuras:

* Agentes de atención.
* Asistente comercial.
* Análisis de ventas.
* Generación de cotizaciones.
* Automatización de WhatsApp.
* Predicción de inventario.
* Recomendaciones.

La IA será considerada una capacidad adicional:

```text
AXYRA Core
     │
     └── AI Layer
             │
             ├── Agents
             ├── Tools
             ├── Knowledge
             └── Automation
```

El núcleo transaccional no debe depender de una respuesta generativa para mantener consistencia.

---

# 43. Analytics Architecture

Analytics deberá consumir información operacional sin convertirse en la fuente primaria de verdad.

Conceptualmente:

```text
Operational Data
      │
      ▼
Analytics Processing
      │
      ▼
Metrics
      │
      ▼
Dashboard
```

Los indicadores iniciales pueden incluir:

* Ventas.
* Pedidos.
* Productos más vendidos.
* Inventario.
* Clientes.
* Cotizaciones.
* Servicios.
* Ingresos.

---

# 44. Frontend / Backend Boundary

El frontend no tendrá autoridad para:

* Modificar inventario directamente.
* Cambiar precios sin autorización.
* Cambiar estados arbitrariamente.
* Crear auditorías manualmente.
* Saltarse permisos.
* Acceder a datos de otro Tenant.

Todas estas operaciones pasarán por backend y/o mecanismos de seguridad de persistencia.

---

# 45. API Boundary

La API será el contrato entre clientes y aplicación.

No se expondrán directamente:

* Credenciales de base de datos.
* Secretos.
* Tokens privados.
* Operaciones administrativas no autorizadas.

---

# 46. Data Ownership

Cada módulo será responsable de sus datos y reglas.

Ejemplo:

```text
Catalog
   owns → Product

Inventory
   owns → Inventory

Commerce
   owns → Order

Services
   owns → ServiceRequest
```

Otros módulos podrán consultar información mediante contratos definidos.

---

# 47. Dependency Rules

Las dependencias deben mantenerse controladas.

Ejemplo permitido:

```text
Commerce
   ↓
Catalog
```

para consultar productos.

Pero debe evitarse:

```text
Catalog
   ↓
Commerce
   ↓
Catalog
   ↓
Commerce
```

cuando genere ciclos de dependencia.

---

# 48. Shared Kernel

Existirá un conjunto pequeño de componentes compartidos:

```text
shared/
├── errors/
├── result/
├── identifiers/
├── dates/
├── money/
├── validation/
└── utilities/
```

El Shared Kernel debe mantenerse pequeño.

No debe convertirse en un lugar donde se almacene lógica de negocio de todos los módulos.

---

# 49. Money Handling

Los valores monetarios no deben representarse mediante operaciones binarias imprecisas.

La estrategia exacta se definirá en Database Design y deberá garantizar:

* Precisión.
* Redondeo consistente.
* Moneda explícita.
* Reproducibilidad de cálculos.

---

# 50. Time Handling

Las fechas y horas deben manejarse de forma consistente.

Se deberán considerar:

* UTC.
* Zona horaria del Tenant.
* Horarios comerciales.
* Fechas de cotizaciones.
* Agendamiento de servicios.

La estrategia definitiva se definirá en Database Design.

---

# 51. Architecture Decisions

Las decisiones arquitectónicas importantes no deben quedar únicamente en este documento.

Se crearán ADRs para decisiones como:

```text
ADR-001 Modular Monolith
ADR-002 Database Provider
ADR-003 Frontend Framework
ADR-004 Backend Framework
ADR-005 Authentication
ADR-006 Multi-tenancy
ADR-007 Row Level Security
ADR-008 Storage
ADR-009 API Strategy
ADR-010 Deployment
ADR-011 Testing Strategy
ADR-012 Observability
```

La numeración definitiva será establecida cuando se cree el índice de ADRs.

---

# 52. Technology Selection

La arquitectura conceptual permite evaluar tecnologías concretas.

Candidatos iniciales:

### Frontend

* React.
* Next.js.

### Backend

* Node.js.
* TypeScript.
* Framework backend compatible con arquitectura modular.

### Database

* PostgreSQL.

### Managed Platform

* Supabase.

### Deployment

* Vercel y/o proveedor backend especializado.

### Automation

* n8n.

### Repository

* GitHub.

Estas tecnologías son candidatas y no quedan definitivamente aprobadas por este documento.

Las elecciones definitivas deben registrarse mediante ADRs.

---

# 53. Repository Architecture

La estructura del proyecto deberá reflejar los límites arquitectónicos.

Propuesta inicial:

```text
AXYRA/
│
├── apps/
│   ├── web/
│   └── api/
│
├── packages/
│   ├── domain/
│   ├── application/
│   ├── shared/
│   └── contracts/
│
├── infrastructure/
│
├── tests/
│
├── docs/
│
└── README.md
```

Esta estructura podrá ajustarse después de seleccionar el stack definitivo.

---

# 54. Architecture Evolution

AXYRA no debe diseñarse suponiendo que la arquitectura inicial será permanente.

La evolución esperada es:

```text
MVP
  ↓
Modular Monolith
  ↓
Optimization
  ↓
Selective Extraction
  ↓
Potential Distributed Architecture
```

La extracción de servicios solo se realizará cuando exista una justificación técnica, operativa o comercial.

---

# 55. Architectural Constraints

Las siguientes restricciones aplican al proyecto:

1. No comenzar con microservicios.
2. No acoplar el dominio a proveedores.
3. No confiar en el frontend para seguridad.
4. No permitir acceso cross-tenant.
5. No almacenar secretos en el repositorio.
6. No modificar inventario sin trazabilidad.
7. No perder información histórica de órdenes.
8. No introducir dependencias externas sin evaluar su impacto.
9. No agregar complejidad sin una necesidad concreta.
10. Toda decisión arquitectónica importante debe documentarse.

---

# 56. Architecture Quality Attributes

La arquitectura será evaluada principalmente por:

| Atributo         | Prioridad |
| ---------------- | --------- |
| Security         | Critical  |
| Tenant Isolation | Critical  |
| Maintainability  | High      |
| Reliability      | High      |
| Scalability      | High      |
| Testability      | High      |
| Performance      | Medium    |
| Portability      | Medium    |
| Observability    | High      |
| Extensibility    | High      |

---

# 57. Definition of Done — Architecture

La arquitectura podrá considerarse estable cuando:

* [ ] Los límites de módulos estén definidos.
* [ ] La estrategia multi-tenant esté definida.
* [ ] La estrategia de autenticación esté definida.
* [ ] La estrategia de autorización esté definida.
* [ ] La estrategia de persistencia esté definida.
* [ ] La estrategia de API esté definida.
* [ ] La estrategia de deployment esté definida.
* [ ] Los principales ADRs estén aprobados.
* [ ] Las decisiones tecnológicas principales estén justificadas.
* [ ] Los riesgos arquitectónicos estén identificados.

---

# 58. Próximo paso

Después de este documento se continuará con:

```text
AXYRA Architecture Specification
          ↓
Architecture Decision Records
          ↓
Database Design
```

Los ADRs convertirán las decisiones arquitectónicas generales en decisiones técnicas concretas y justificadas.

Posteriormente se diseñará el esquema físico de PostgreSQL.

---

# 59. Mapa de dependencias por módulo

El sistema debe mantenerse acoplado por contratos, no por implementación interna. La siguiente estructura funcional define cómo los módulos interactúan.

## 59.1 Identity

Depende de:
- shared/auth
- shared/validation
- shared/errors
- shared/identifiers

Expone:
- login
- logout
- user profile
- role management
- permission validation

## 59.2 Tenant Management

Depende de:
- Identity
- Configuration
- shared/tenant-context

Expone:
- tenant creation
- tenant settings
- tenant status
- business metadata

## 59.3 Catalog

Depende de:
- Tenant Management
- Configuration
- shared/validation

Expone:
- product CRUD
- category CRUD
- price list management
- public catalog queries

## 59.4 Inventory

Depende de:
- Catalog
- Tenant Management
- shared/transactions
- shared/audit

Expone:
- stock queries
- inventory movements
- adjustments
- reservation operations

## 59.5 Commerce

Depende de:
- Catalog
- Inventory
- Customers
- Configuration
- shared/validation
- shared/money

Expone:
- cart operations
- order creation
- quote creation
- conversion quote → order

## 59.6 Services

Depende de:
- Customers
- Identity
- Configuration
- shared/notifications

Expone:
- service requests
- assignment operations
- notes and comments
- service status updates

## 59.7 Analytics

Depende de:
- Commerce
- Inventory
- Services
- Configuration

Expone:
- dashboards
- reports
- aggregated KPIs

No debe ser la fuente primaria de verdad del negocio.

---

# 60. Contratos de integración entre módulos

La comunicación entre módulos debe ocurrir por interfaces de servicio o por APIs internas, no por consultas directas de la base de datos desde una capa de presentación.

Ejemplos:

```text
Commerce -> Catalog
Commerce -> Inventory
Services -> Identity
Analytics -> Commerce
Analytics -> Inventory
Tenant Management -> Configuration
Configuration -> Catalog
``` 

## 60.1 Regla de acoplamiento

Un módulo solo debe conocer lo necesario de otro módulo para cumplir su responsabilidad.

Se debe evitar:
- llamadas de dominio a infraestructura desde módulos de negocio
- lógica de UI en capas de aplicación
- acceso directo a tablas de otros módulos
- acoplamiento circular entre módulos

---

# 61. Seguridad y autorización por capa

La seguridad deberá validarse en varios niveles y no depender de la capa de cliente.

## 61.1 Frontend

- Validación UX.
- Mejora de experiencia.
- Evita errores de uso.
- No sustituye validación real.

## 61.2 API

- Validación de payload.
- Autenticación.
- Autorización por permiso y scope.
- Contexto de Tenant.
- Sanitización y serialización.

## 61.3 Application

- Casos de uso protegidos.
- Validación compleja del negocio.
- Llamadas a dominio con contexto autorizado.

## 61.4 Domain

- Regla de negocio.
- Invariantes.
- Validación de dominio.
- Estados y transiciones.

## 61.5 Database

- Foreign keys.
- Unique constraints.
- Row Level Security.
- Contexto de Tenant.
- Filtros persistentes.

---

# 62. Criterios para la capa de integración externa

Las integraciones deben quedar encapsuladas detrás de interfaces. Cada integrador debe manejar:

- autenticación del proveedor
- retries y timeouts
- manejo de errores
- mapeo a modelos internos
- trazabilidad de llamadas
- fallbacks seguros

## 62.1 WhatsApp

Debe encapsularse en un servicio de integración:

```text
Order -> WhatsApp Service -> WhatsApp URL / Provider
```

No debe existir lógica comercial mezclada con la redirección o payload final del mensaje.

## 62.2 Email

Se utilizará para notificaciones o confirmations del negocio, pero no como fuente de verdad del negocio.

## 62.3 Storage

Debe manejar imágenes, archivos del servicio y referencias a objetos.

---

# 63. Operación y resiliencia

El sistema debe responder ante fallos sin destruir consistencia.

## 63.1 Reintentos

Se aplicarán en integraciones externas con políticas adecuadas.

## 63.2 Timeouts

Todos los clientes externos deberán tener limites de timeout explícitos.

## 63.3 Circuit Breaker

Cuando una integración falle repetidamente, la aplicación no debe quedar en un estado degradado sin control.

## 63.4 Idempotencia

Operaciones sensibles como creación de pedido o ajuste de inventario deben ser idempotentes en la medida de lo posible.

## 63.5 Consistencia

Las operaciones que modifican inventario y pedidos deben ejecutarse en transacciones explícitas.

---

# 64. Observabilidad y diagnóstico

AXYRA debe registrar información suficiente para detectar fallas y mejorar decisiones operativas.

## 64.1 Logs

Relevantes en:
- autenticación
- autorización
- creación/actualización de entidades críticas
- inventario
- pedidos
- servicios
- integraciones externas
- errores de negocio y validación

## 64.2 Metrics

Debe medirse al menos:
- latencia por endpoint
- volumen de requests
- tasa de error
- pedidos por día
- inventario ajustado
- servicios creados y finalizados
- crecimiento de tenants

## 64.3 Alerts

Con prioridad en:
- inventario crítico
- fallas de integraciones
- errores de autenticación repetidos
- errores de base de datos o backend
- picos de uso irregulares

---

# 65. Calidad de implementación y arquitectura

La arquitectura será aceptada si cumple estas condiciones operativas:

- Los módulos tienen responsabilidad clara.
- Los componentes no comparten lógica más allá del shared kernel.
- El tenant se aplica en todas las operaciones críticas.
- La aplicación no depende del frontend para políticas de seguridad.
- La base de datos conserva la verdad del negocio.
- Los eventos y trazabilidad son observables.
- Los cambios de infraestructura no obligan a cambiar el dominio.

---

# 66. Roadmap técnico sugerido

## Fase 1 — MVP foundation

- Tenant creation
- Auth and roles
- Product catalog
- Price list management
- Inventory and movements
- Customer creation
- Cart and checkout flow
- Order creation
- WhatsApp redirection
- Service request basic flow
- Dashboard basics

## Fase 2 — Operations and scale

- Quote workflows
- Improved dashboard
- inventory alerts
- audit logs
- module configuration
- improved validation and errors
- background jobs

## Fase 3 — Growth and integration

- more automation
- business API integrations
- storage optimization
- analytics advanced
- AI features
- external ERP/CRM connections

---

# 67. Riesgos arquitectónicos principales

Los riesgos que más pueden afectar la evolución de AXYRA son:

1. Fuga de datos entre tenants.
2. Inventario inconsistente por validación deficiente.
3. Dependencia directa de WhatsApp o un proveedor externo.
4. Acoplamiento entre UI y lógica de negocio.
5. Complejidad creciente por falta de límites de módulo.
6. Ausencia de auditoría para cambios importantes.
7. Duplicación de reglas comerciales entre capas.
8. Optimización prematura sin necesidad comprobada.

La mitigación de estos riesgos debe documentarse en ADRs y revisión de diseño.

---

# 68. Cierre arquitectónico

Este documento establece una base sólida para la arquitectura de AXYRA: modular, multi-tenant, segura y preparada para crecer de forma progresiva. La intención central es evitar que el sistema se vuelva una colección de pantallas y tablas desconectadas, y en su lugar mantener una estructura de dominio, aplicación e infraestructura coherente.

La arquitectura propuesta pone al negocio en el centro, con una capa técnica que soporte crecimiento sin comprometer seguridad ni trazabilidad.

El siguiente artefacto técnico recomendado es:

```text
docs/05-ADR/AXYRA_ADR_Index_v1.0.md
```

Allí se registrarán las decisiones clave de la pila, la arquitectura y la infraestructura con justificación técnica.

---

# 69. Estado del documento

**Versión:** 1.0
**Estado:** Baseline arquitectónica
**Siguiente revisión recomendada:** cuando se definan proveedores, stack, ADRs y base de datos definitiva
