# ADR-001: Modular Monolith

## Status

PROPOSED

La decisión queda propuesta y pendiente de aprobación. El lenguaje de este documento
describe el resultado recomendado; la arquitectura no se considerará formalmente
adoptada hasta que el estado cambie a `ACCEPTED`.

## Date

2026-10-06

## Decision Type

Architecture

## Context

AXYRA es una plataforma SaaS multi-tenant orientada a empresas que necesitan gestionar productos, servicios, clientes, inventario, comercio y posteriormente capacidades de automatización e inteligencia artificial.

El sistema debe ser reutilizable para diferentes tipos de empresas y debe poder evolucionar progresivamente desde un MVP hasta una plataforma comercial de mayor escala.

En la etapa inicial, AXYRA será desarrollado por un equipo pequeño y tendrá un dominio considerable, pero todavía no existe suficiente evidencia para justificar una arquitectura distribuida basada en microservicios.

Los principales factores considerados son:

* Necesidad de desarrollar el producto rápidamente.
* Complejidad de dominio creciente.
* Necesidad de mantener límites claros entre módulos.
* Necesidad de aislamiento entre tenants.
* Necesidad de mantener una arquitectura mantenible.
* Posibilidad futura de escalar determinados componentes.
* Evitar infraestructura innecesariamente compleja.
* Mantener bajo el costo operativo inicial.
* Facilitar pruebas locales y despliegues.
* Mantener abierta la posibilidad de evolucionar hacia servicios independientes cuando exista una necesidad real.

Una arquitectura monolítica tradicional podría facilitar el inicio, pero podría generar un sistema altamente acoplado si no existen límites arquitectónicos explícitos.

Por otra parte, una arquitectura de microservicios introduciría desde el inicio complejidad adicional relacionada con:

* Comunicación entre servicios.
* Descubrimiento y configuración.
* Observabilidad distribuida.
* Gestión de errores de red.
* Consistencia de datos.
* Despliegues independientes.
* Seguridad entre servicios.
* Infraestructura adicional.
* Versionamiento de contratos.
* Mayor costo operativo.

Por lo tanto, se necesita una arquitectura que mantenga la simplicidad operacional de un monolito, pero conserve una separación estructural suficientemente fuerte para permitir evolución futura.

---

## Decision

AXYRA utilizará inicialmente una **arquitectura Modular Monolith**.

El sistema será desplegado inicialmente como una aplicación principal, pero internamente estará dividido en módulos de dominio claramente definidos.

La arquitectura combinará:

* Modular Monolith.
* Domain-Oriented Design.
* Layered Architecture.
* API-first.
* Separación entre dominio, aplicación e infraestructura.
* Contratos internos entre módulos.
* Persistencia centralizada inicialmente.
* Preparación para futura extracción de módulos cuando exista una justificación técnica.

La estructura conceptual será:

```text
                    AXYRA
                      │
        ┌─────────────┴─────────────┐
        │       Application         │
        │                           │
        │  ┌───────┐ ┌──────────┐  │
        │  │Catalog│ │Inventory │  │
        │  └───────┘ └──────────┘  │
        │                           │
        │  ┌──────────┐ ┌────────┐ │
        │  │Commerce  │ │Customers│ │
        │  └──────────┘ └────────┘ │
        │                           │
        │  ┌──────────┐ ┌────────┐ │
        │  │ Services │ │Identity│ │
        │  └──────────┘ └────────┘ │
        └───────────────────────────┘
                      │
               PostgreSQL
```

Los módulos compartirán inicialmente el mismo despliegue y la misma infraestructura principal, pero deberán mantener límites internos explícitos.

---

## Architectural Principle

La decisión fundamental es:

> **Un solo sistema desplegable no significa un sistema sin modularidad.**

AXYRA debe comportarse como un conjunto de módulos coherentes dentro de una misma aplicación.

La modularidad será principalmente **lógica y arquitectónica**, no física.

---

## Initial Modules

La aplicación podrá organizarse inicialmente alrededor de los siguientes módulos:

### Identity

Responsable de:

* Usuarios.
* Autenticación.
* Roles.
* Permisos.
* Sesiones.
* Identidad del usuario.

### Tenant Management

Responsable de:

* Tenants.
* Configuración de empresas.
* Estado del tenant.
* Resolución del contexto del tenant.

### Catalog

Responsable de:

* Productos.
* Categorías.
* Imágenes.
* Listas de precios.
* Información comercial del catálogo.

### Inventory

Responsable de:

* Inventario.
* Movimientos de inventario.
* Disponibilidad.
* Políticas de stock.

### Customers

Responsable de:

* Clientes.
* Direcciones.
* Información de contacto.

### Commerce

Responsable de:

* Carritos.
* Órdenes.
* Cotizaciones.
* Conversión de cotizaciones a órdenes.

### Services

Responsable de:

* Tipos de servicio.
* Solicitudes de servicio.
* Asignaciones.
* Notas de servicio.

### Configuration

Responsable de:

* Configuración del tenant.
* Horarios.
* Configuración de módulos.
* Parámetros operativos.

### Analytics

Responsable de:

* Métricas.
* Indicadores.
* Consultas analíticas.
* Dashboards.

### Integrations

Responsable de:

* WhatsApp.
* Servicios externos.
* APIs externas.
* Adaptadores de terceros.

---

## Module Boundaries

Los módulos deberán respetar límites definidos.

Un módulo no deberá modificar directamente el estado interno de otro módulo sin pasar por una interfaz o mecanismo explícitamente definido.

Ejemplo:

```text
Commerce
   │
   │ solicita disponibilidad
   ▼
Inventory
```

No:

```text
Commerce
   │
   └── modifica directamente inventory.stock
```

La comunicación deberá realizarse mediante:

* Application Services.
* Interfaces.
* Contracts.
* Domain Events cuando sea apropiado.

---

## Layered Structure

Cada módulo deberá poder organizarse siguiendo una separación similar a:

```text
Module
├── domain/
├── application/
├── infrastructure/
└── presentation/
```

### Domain

Contendrá:

* Entidades.
* Value Objects.
* Reglas de negocio.
* Domain Services.
* Domain Events.

### Application

Contendrá:

* Use Cases.
* Application Services.
* Orquestación.
* Transacciones.
* Interfaces requeridas por el dominio.

### Infrastructure

Contendrá:

* Repositorios concretos.
* ORM.
* PostgreSQL.
* Servicios externos.
* Implementaciones técnicas.

### Presentation

Contendrá:

* Controllers.
* API endpoints.
* DTOs.
* Validación de entrada.

---

## Rules

Se establecen las siguientes reglas arquitectónicas iniciales:

### Rule 1 — No microservices initially

AXYRA no se dividirá inicialmente en microservicios independientes.

### Rule 2 — Modules must have boundaries

Los módulos deberán tener responsabilidades claramente definidas.

### Rule 3 — No arbitrary cross-module access

Un módulo no deberá acceder directamente a las estructuras internas de otro módulo.

### Rule 4 — Domain logic stays in the domain

Las reglas de negocio no deberán depender directamente de frameworks o proveedores externos.

### Rule 5 — Infrastructure stays replaceable

La infraestructura deberá poder cambiarse sin modificar innecesariamente el dominio.

### Rule 6 — Shared code must remain limited

El código compartido deberá mantenerse pequeño y estable.

No se deberá crear un "common" gigante que termine convirtiéndose en una dependencia global de todos los módulos.

### Rule 7 — Database access must respect module boundaries

Aunque inicialmente exista una base de datos compartida, el acceso lógico a los datos deberá respetar la responsabilidad de cada módulo.

---

## Alternatives Considered

### Alternative 1 — Traditional Monolith

Una aplicación monolítica sin separación estricta de módulos.

**Ventajas:**

* Fácil de iniciar.
* Baja complejidad inicial.
* Despliegue sencillo.

**Desventajas:**

* Mayor riesgo de acoplamiento.
* Límites de dominio débiles.
* Mayor dificultad para evolucionar el sistema.
* Mayor riesgo de convertir el código en un "big ball of mud".

**Decision:** Rejected.

---

### Alternative 2 — Microservices

Separar desde el inicio cada dominio importante como un servicio independiente.

**Ventajas:**

* Escalabilidad independiente.
* Despliegues independientes.
* Aislamiento físico.
* Posibilidad de utilizar tecnologías diferentes.

**Desventajas:**

* Mayor complejidad.
* Mayor costo operacional.
* Comunicación de red.
* Problemas de consistencia distribuida.
* Observabilidad más compleja.
* Mayor esfuerzo de DevOps.
* Mayor dificultad para desarrollo y debugging.

**Decision:** Rejected for initial architecture.

---

### Alternative 3 — Serverless per Module

Implementar cada funcionalidad como funciones independientes.

**Ventajas:**

* Escalabilidad automática.
* Pago por uso.
* Bajo mantenimiento de servidores.

**Desventajas:**

* Mayor dependencia del proveedor.
* Complejidad creciente en dominios complejos.
* Dificultad para manejar transacciones complejas.
* Mayor riesgo de fragmentación arquitectónica.
* Debugging y testing más complejos.

**Decision:** Rejected as primary architecture.

---

## Consequences

### Positive Consequences

#### Reduced Complexity

El sistema puede comenzar con una infraestructura relativamente sencilla.

#### Faster Development

El equipo puede desarrollar funcionalidades sin administrar múltiples servicios independientes.

#### Easier Debugging

Los errores pueden rastrearse dentro de un único sistema.

#### Easier Local Development

El entorno local será más sencillo de ejecutar y reproducir.

#### Transactional Consistency

Las operaciones que involucren varios módulos podrán utilizar transacciones de base de datos cuando sea apropiado.

Por ejemplo:

```text
Create Order
    │
    ├── Validate Customer
    ├── Validate Inventory
    ├── Create Order
    └── Register Inventory Movement
```

#### Future Evolution

Los módulos estarán preparados para convertirse posteriormente en servicios independientes si existe una necesidad real.

---

## Negative Consequences

### Shared Deployment

Inicialmente, todos los módulos compartirán el mismo ciclo de despliegue.

### Shared Runtime

Un problema grave en una parte del sistema podría afectar al proceso completo.

### Potential Coupling

Si las reglas arquitectónicas no se respetan, los módulos pueden terminar altamente acoplados.

### Scaling Granularity

Inicialmente no será posible escalar físicamente cada módulo de forma independiente.

---

## Risk Mitigation

Para reducir los riesgos de esta decisión se utilizarán:

* Límites explícitos entre módulos.
* Code review.
* Tests de integración.
* Tests unitarios.
* Contratos internos.
* Dependencias controladas.
* Documentación arquitectónica.
* ADRs.
* Observabilidad.
* Reglas de acceso a datos.
* Domain Events cuando sea apropiado.

Además, cualquier dependencia entre módulos deberá tener una razón arquitectónica identificable.

---

## Future Extraction Strategy

Si en el futuro un módulo necesita convertirse en un servicio independiente, la extracción deberá seguir un proceso controlado.

Ejemplo:

```text
Modular Monolith

Commerce ─────┐
Catalog ──────┤
Inventory ────┤── PostgreSQL
Customers ────┘
```

Podría evolucionar hacia:

```text
                  API Gateway
                      │
        ┌─────────────┼─────────────┐
        │             │             │
     Commerce      Inventory      Catalog
        │             │             │
        ▼             ▼             ▼
       DB            DB            DB
```

Sin embargo, esta transformación solamente deberá realizarse cuando exista una necesidad demostrable.

No se considerará una mejora simplemente por utilizar microservicios.

---

## Conditions for Reconsideration

Esta ADR deberá revisarse si ocurre alguno de los siguientes escenarios:

1. Un módulo requiere escalabilidad significativamente diferente al resto.
2. Un módulo necesita despliegues independientes con alta frecuencia.
3. Un módulo requiere aislamiento de fallos.
4. Un módulo necesita una tecnología incompatible con el runtime principal.
5. El equipo crece y existen equipos independientes responsables de diferentes dominios.
6. La carga de trabajo genera un cuello de botella específico.
7. Los límites de un módulo se vuelven suficientemente estables para justificar su extracción.
8. Los costos operacionales del monolito superan los beneficios de mantenerlo.
9. Existen requisitos regulatorios o de seguridad que exijan aislamiento físico.
10. La arquitectura actual impide una evolución razonable del producto.

---

## Relationship with Multi-Tenancy

El uso de un Modular Monolith no elimina la necesidad de aislamiento entre tenants.

La arquitectura deberá garantizar que:

```text
Tenant A
   │
   ├── Products
   ├── Orders
   ├── Customers
   └── Inventory

Tenant B
   │
   ├── Products
   ├── Orders
   ├── Customers
   └── Inventory
```

No puedan acceder accidentalmente a información perteneciente al otro tenant.

Los mecanismos específicos de multi-tenancy serán definidos en:

* ADR-003 — Multi-Tenancy.
* ADR-004 — Row Level Security.

---

## Relationship with Database Architecture

La decisión de utilizar un Modular Monolith no determina por sí sola el motor de base de datos.

La selección del motor y la estrategia de persistencia serán definidas en:

**ADR-002 — Database.**

---

## Relationship with API Architecture

Los módulos deberán poder exponerse mediante interfaces API claramente definidas.

La estrategia específica de API será definida posteriormente en:

**ADR-009 — API Strategy.**

---

## Architectural Constraints

Esta decisión establece las siguientes restricciones:

* No implementar microservicios sin una ADR que justifique la extracción.
* No introducir comunicación de red interna donde una llamada modular sea suficiente.
* No crear bases de datos independientes por módulo durante la fase inicial sin justificación.
* No permitir acceso indiscriminado entre módulos.
* No convertir el módulo `shared` en un repositorio de lógica de negocio global.
* No introducir infraestructura distribuida solamente por razones de moda tecnológica.

---

## Implementation Acceptance Criteria

La decisión podrá considerarse aplicada cuando la solución inicial cumpla, como
mínimo, los siguientes criterios:

* Los módulos y sus responsabilidades estén identificados en la estructura del código.
* Cada módulo exponga sus operaciones a otros módulos mediante contratos definidos.
* Las dependencias entre módulos no formen ciclos.
* Un módulo no escriba directamente en las tablas o estructuras privadas de otro módulo.
* Las reglas de negocio residan en el dominio o en los casos de uso correspondientes,
   no en controladores ni adaptadores externos.
* Las operaciones que requieran consistencia entre módulos definan explícitamente
   sus límites transaccionales y su manejo de fallos.
* Las pruebas cubran las reglas críticas y las interacciones entre módulos.

Estos criterios no fijan framework, ORM, proveedor cloud ni estructura física de
despliegue; esas decisiones se documentarán en sus ADR correspondientes.

---

## Risks

| Riesgo | Impacto | Mitigación |
| ------ | ------- | ---------- |
| Los límites modulares se respetan solo en documentación. | El sistema deriva en un monolito acoplado y difícil de cambiar. | Aplicar revisión de dependencias, convenciones de código y pruebas de arquitectura cuando la estructura del proyecto esté definida. |
| Los módulos comparten persistencia sin propiedad clara. | Cambios de esquema y escrituras cruzadas pueden romper otros dominios. | Asignar propiedad de datos por módulo y restringir escrituras a contratos del módulo propietario. |
| Las transacciones abarcan demasiados módulos. | Se incrementa el acoplamiento y se dificulta una extracción futura. | Mantener los casos de uso transaccionales acotados y documentar consistencia eventual cuando sea aceptable. |
| El runtime compartido amplifica fallos o consumo excesivo. | Una carga o defecto puede afectar toda la aplicación. | Medir recursos, aislar trabajos costosos y reconsiderar despliegue independiente solo con evidencia operativa. |
| La extracción futura se asume automática. | La separación lógica puede no bastar para una distribución segura. | Tratar cada extracción como una migración explícita de datos, contratos, operación y seguridad, con su propia ADR. |

---

## Revisit Conditions

Revisar esta ADR si se cumple alguna de estas condiciones:

1. Un módulo necesita escalar o desplegarse de manera independiente de forma sostenida.
2. Se requieren límites de aislamiento de fallos que el runtime compartido no pueda satisfacer.
3. Existen equipos independientes que necesitan ownership y ciclos de entrega autónomos.
4. Los límites del dominio o los contratos internos cambian con frecuencia y bloquean entregas.
5. Requisitos regulatorios, de seguridad o disponibilidad exigen aislamiento físico.
6. La evidencia de costos y operación demuestra que mantener todos los módulos en un
    único despliegue ya no es adecuado.

Una revisión no implica automáticamente adoptar microservicios. Se deberán evaluar
primero alternativas como aislar procesos de trabajo o extraer únicamente el módulo
que presente la necesidad demostrable.

---

## Decision Summary

| Aspecto                    | Decisión                                  |
| -------------------------- | ----------------------------------------- |
| Arquitectura inicial       | Modular Monolith                          |
| Despliegues                | Principalmente uno                        |
| Módulos                    | Separados lógicamente                     |
| Microservicios             | No inicialmente                           |
| Base de datos              | Compartida inicialmente                   |
| Límites de dominio         | Obligatorios                              |
| Comunicación entre módulos | Contratos / Application Services / Events |
| Escalabilidad              | Vertical y posteriormente selectiva       |
| Extracción de servicios    | Solamente cuando exista justificación     |
| Complejidad operacional    | Mantenerla baja inicialmente              |

---

## Related Documents

* `AXYRA_Architecture_Specification_v1.0.md`
* `AXYRA_Domain_Model_v1.0.md`
* `AXYRA_Conceptual_ERD_v1.0.md`
* `ADR-002-database.md`
* `ADR-003-multi-tenancy.md`
* `ADR-004-row-level-security.md`

---

## Final Decision

La decisión propuesta es que AXYRA adopte **Modular Monolith** como arquitectura inicial.

Esta decisión busca maximizar la velocidad de desarrollo, mantenibilidad y simplicidad operacional, manteniendo simultáneamente límites arquitectónicos suficientes para permitir la evolución futura del sistema.

La adopción de microservicios queda explícitamente fuera del alcance inicial y solamente podrá reconsiderarse cuando exista una necesidad técnica, operacional o de negocio que justifique la complejidad adicional. Esta propuesta requiere aprobación antes de pasar a `ACCEPTED`.
