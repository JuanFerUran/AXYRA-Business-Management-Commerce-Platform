# AXYRA

## Requirements Specification v1.0

**Producto:** AXYRA
**Documento:** Requirements Specification
**Versión:** 1.0
**Estado:** Baseline inicial
**Dependencia:** AXYRA Product Definition v1.0

---

# 1. Introducción

Este documento define los requisitos funcionales, no funcionales y reglas de negocio para la primera versión de AXYRA.

Los requisitos establecidos aquí servirán como referencia para el diseño de arquitectura, modelo de dominio, base de datos, API, frontend, pruebas y despliegue.

AXYRA será diseñada como una plataforma multi-tenant y modular capaz de adaptarse a diferentes tipos de empresas.

---

# 2. Convenciones

## 2.1 Identificadores

Los requisitos utilizarán identificadores únicos:

```text
FR-XXX-###
NFR-###
BR-###
UC-###
AC-###
```

Donde:

* `FR` = Functional Requirement.
* `NFR` = Non-Functional Requirement.
* `BR` = Business Rule.
* `UC` = Use Case.
* `AC` = Acceptance Criteria.

---

# 3. Prioridades

Cada requisito tendrá una prioridad:

### MUST

Obligatorio para el MVP.

### SHOULD

Importante, pero puede implementarse después del núcleo inicial.

### COULD

Deseable para futuras iteraciones.

### WON'T

Fuera del alcance actual.

---

# 4. Actores del sistema

## 4.1 Visitante

Persona que accede públicamente a la plataforma sin autenticarse.

Puede:

* Consultar información pública.
* Navegar por el catálogo.
* Consultar productos.
* Realizar búsquedas.
* Agregar productos al carrito.

---

## 4.2 Cliente

Persona que realiza una solicitud comercial.

Puede:

* Crear pedidos.
* Solicitar cotizaciones.
* Solicitar servicios.
* Proporcionar información de contacto.
* Continuar el proceso comercial mediante WhatsApp.

No será obligatorio crear una cuenta para realizar un pedido durante el MVP.

---

## 4.3 Vendedor

Usuario perteneciente a un tenant con permisos relacionados con la operación comercial.

Puede:

* Consultar clientes.
* Gestionar pedidos.
* Crear pedidos.
* Consultar productos.
* Consultar inventario según permisos.

---

## 4.4 Usuario de inventario

Usuario encargado de la gestión de productos y existencias.

Puede:

* Crear productos.
* Modificar productos.
* Gestionar categorías.
* Registrar movimientos.
* Consultar existencias.

---

## 4.5 Técnico

Usuario encargado de atender servicios.

Puede:

* Consultar servicios asignados.
* Actualizar estados.
* Registrar observaciones.
* Marcar servicios como finalizados.

---

## 4.6 Administrador

Usuario encargado de administrar un tenant.

Puede gestionar:

* Productos.
* Inventario.
* Pedidos.
* Clientes.
* Servicios.
* Usuarios.
* Configuración.

---

## 4.7 Owner

Propietario del tenant.

Tendrá control completo sobre la configuración y operación de su empresa.

---

## 4.8 Super Admin

Usuario perteneciente a AXYRA y no a un tenant comercial.

Puede administrar aspectos globales de la plataforma.

Sus capacidades deberán mantenerse separadas de las capacidades de los administradores de cada empresa.

---

# 5. Requisitos de multi-tenancy

## FR-TEN-001 — Creación de tenant

**Prioridad:** MUST

El sistema deberá permitir la creación de una empresa dentro de AXYRA.

La empresa será representada como un tenant independiente.

---

## FR-TEN-002 — Aislamiento de datos

**Prioridad:** MUST

El sistema deberá garantizar que los datos pertenecientes a un tenant no sean accesibles por usuarios pertenecientes a otro tenant.

---

## FR-TEN-003 — Asociación de recursos

**Prioridad:** MUST

Los recursos empresariales deberán estar asociados a un tenant.

Como mínimo:

* Productos.
* Categorías.
* Inventario.
* Clientes.
* Pedidos.
* Servicios.
* Usuarios.
* Configuraciones.

---

## FR-TEN-004 — Configuración independiente

**Prioridad:** MUST

Cada tenant deberá poder mantener configuraciones independientes.

---

# 6. Gestión empresarial

## FR-BIZ-001 — Información empresarial

**Prioridad:** MUST

El sistema deberá permitir almacenar:

* Nombre comercial.
* Razón social, si aplica.
* Identificación tributaria.
* Logo.
* Descripción.
* Teléfono.
* Correo.
* Dirección.
* Ciudad.
* País.

---

## FR-BIZ-002 — Configuración comercial

**Prioridad:** MUST

Cada empresa deberá poder definir las características de su operación comercial.

---

## FR-BIZ-003 — Modalidad de venta

**Prioridad:** MUST

Cada empresa deberá poder configurar qué modalidades de venta utiliza:

* Detal.
* Mayorista.
* Ambas.

---

## FR-BIZ-004 — Configuración de inventario

**Prioridad:** MUST

Cada empresa deberá poder configurar el comportamiento de su inventario.

---

## FR-BIZ-005 — Comportamiento ante stock insuficiente

**Prioridad:** MUST

Cada empresa deberá poder definir qué ocurre cuando un cliente intenta solicitar un producto sin disponibilidad suficiente.

Las opciones iniciales serán:

* Bloquear pedido.
* Permitir solicitud/cotización.
* Permitir pedido bajo condiciones definidas por el negocio.

---

# 7. Catálogo

## FR-CAT-001 — Crear categoría

**Prioridad:** MUST

Los usuarios autorizados deberán poder crear categorías.

---

## FR-CAT-002 — Modificar categoría

**Prioridad:** MUST

Los usuarios autorizados deberán poder modificar categorías.

---

## FR-CAT-003 — Desactivar categoría

**Prioridad:** MUST

El sistema deberá permitir desactivar categorías sin eliminar necesariamente el historial asociado.

---

## FR-CAT-004 — Crear producto

**Prioridad:** MUST

Los usuarios autorizados deberán poder crear productos.

Un producto deberá poder contener como mínimo:

* Nombre.
* SKU/referencia.
* Descripción.
* Categoría.
* Imágenes.
* Estado.
* Información comercial.

---

## FR-CAT-005 — Identificador de producto

**Prioridad:** MUST

Cada producto deberá contar con un identificador único dentro de su tenant.

---

## FR-CAT-006 — Productos activos

**Prioridad:** MUST

El sistema deberá permitir activar o desactivar productos.

---

## FR-CAT-007 — Precio del producto

**Prioridad:** MUST

Un producto podrá tener diferentes precios según las listas configuradas por el tenant.

---

## FR-CAT-008 — Listas de precios

**Prioridad:** MUST

El sistema deberá permitir crear diferentes listas de precios.

Ejemplos:

* Precio detal.
* Precio mayorista.
* Precio distribuidor.
* Precio especial.

El número y nombre de las listas deberán poder configurarse posteriormente.

---

## FR-CAT-009 — Precio configurable por empresa

**Prioridad:** MUST

Las listas de precios deberán pertenecer al tenant correspondiente.

---

# 8. Inventario

## FR-INV-001 — Existencia

**Prioridad:** MUST

El sistema deberá almacenar la cantidad disponible de cada producto.

---

## FR-INV-002 — Movimiento de inventario

**Prioridad:** MUST

Cada modificación relevante de inventario deberá generar un movimiento registrado.

---

## FR-INV-003 — Tipos de movimiento

**Prioridad:** MUST

El sistema deberá soportar inicialmente:

* Entrada.
* Salida.
* Ajuste positivo.
* Ajuste negativo.

---

## FR-INV-004 — Historial

**Prioridad:** MUST

El sistema deberá conservar el historial de movimientos de inventario.

---

## FR-INV-005 — Usuario responsable

**Prioridad:** MUST

Cada movimiento deberá registrar el usuario responsable cuando corresponda.

---

## FR-INV-006 — Stock mínimo

**Prioridad:** MUST

Cada producto podrá tener un nivel de stock mínimo configurable.

---

## FR-INV-007 — Alerta de inventario

**Prioridad:** SHOULD

El sistema deberá identificar productos cuyo inventario se encuentre por debajo del mínimo establecido.

---

## FR-INV-008 — Trazabilidad

**Prioridad:** MUST

El sistema deberá poder determinar cómo se obtuvo el stock actual a partir de sus movimientos registrados.

---

# 9. Clientes

## FR-CUS-001 — Registro de cliente

**Prioridad:** MUST

El sistema deberá permitir registrar información básica de clientes.

---

## FR-CUS-002 — Cliente sin cuenta

**Prioridad:** MUST

Un cliente podrá realizar un pedido sin crear una cuenta.

---

## FR-CUS-003 — Datos mínimos de pedido

**Prioridad:** MUST

Para finalizar un pedido mediante WhatsApp deberán solicitarse como mínimo:

* Nombre.
* Teléfono.
* Productos.
* Cantidades.

Podrán solicitarse datos adicionales según configuración del negocio.

---

## FR-CUS-004 — Historial

**Prioridad:** SHOULD

El sistema deberá permitir consultar el historial de pedidos asociados a un cliente identificado.

---

# 10. Carrito

## FR-CART-001 — Agregar producto

**Prioridad:** MUST

El visitante podrá agregar productos disponibles al carrito.

---

## FR-CART-002 — Modificar cantidad

**Prioridad:** MUST

El cliente podrá modificar las cantidades de los productos incluidos en el carrito.

---

## FR-CART-003 — Eliminar producto

**Prioridad:** MUST

El cliente podrá eliminar productos del carrito.

---

## FR-CART-004 — Validación de disponibilidad

**Prioridad:** MUST

El sistema deberá validar la disponibilidad del producto antes de crear el pedido.

---

## FR-CART-005 — Cálculo

**Prioridad:** MUST

El sistema deberá calcular el subtotal y total estimado del pedido.

---

# 11. Pedidos

## FR-ORD-001 — Crear pedido

**Prioridad:** MUST

El sistema deberá permitir crear un pedido a partir del carrito.

---

## FR-ORD-002 — Identificador

**Prioridad:** MUST

Cada pedido deberá contar con un identificador único.

---

## FR-ORD-003 — Estado

**Prioridad:** MUST

Los pedidos deberán manejar estados.

Estados iniciales:

```text
PENDING
CONFIRMED
PROCESSING
COMPLETED
CANCELLED
```

---

## FR-ORD-004 — Productos

**Prioridad:** MUST

El pedido deberá conservar los productos, cantidades y precios utilizados en el momento de su creación.

---

## FR-ORD-005 — Cliente

**Prioridad:** MUST

El pedido deberá almacenar la información del cliente necesaria para su gestión.

---

## FR-ORD-006 — Tenant

**Prioridad:** MUST

Cada pedido deberá pertenecer a un único tenant.

---

# 12. WhatsApp

## FR-WHA-001 — Generación de mensaje

**Prioridad:** MUST

El sistema deberá generar un mensaje estructurado con la información del pedido.

---

## FR-WHA-002 — Redirección

**Prioridad:** MUST

El sistema deberá permitir redirigir al cliente hacia WhatsApp mediante un enlace generado dinámicamente.

---

## FR-WHA-003 — Número empresarial

**Prioridad:** MUST

Cada tenant deberá poder configurar el número de WhatsApp utilizado para recibir pedidos.

---

## FR-WHA-004 — Abstracción

**Prioridad:** SHOULD

La arquitectura deberá abstraer la integración de WhatsApp para permitir posteriormente reemplazar el enlace directo por una integración mediante API.

---

# 13. Cotizaciones

## FR-QUO-001 — Solicitud de cotización

**Prioridad:** MUST

El cliente podrá solicitar una cotización cuando el negocio tenga habilitada esta funcionalidad.

---

## FR-QUO-002 — Productos cotizados

**Prioridad:** MUST

Una cotización podrá contener múltiples productos y cantidades.

---

## FR-QUO-003 — Estado

**Prioridad:** SHOULD

Las cotizaciones podrán manejar estados:

```text
REQUESTED
IN_REVIEW
SENT
ACCEPTED
REJECTED
EXPIRED
```

---

# 14. Servicios

## FR-SRV-001 — Solicitud

**Prioridad:** MUST

El cliente podrá solicitar servicios ofrecidos por el negocio.

---

## FR-SRV-002 — Información

**Prioridad:** MUST

Una solicitud podrá contener:

* Cliente.
* Tipo de servicio.
* Descripción.
* Dirección.
* Fecha solicitada.
* Información de contacto.
* Archivos o imágenes cuando corresponda.

---

## FR-SRV-003 — Estado

**Prioridad:** MUST

Los servicios podrán manejar:

```text
REQUESTED
SCHEDULED
IN_PROGRESS
COMPLETED
CANCELLED
```

---

## FR-SRV-004 — Asignación

**Prioridad:** MUST

Los usuarios autorizados podrán asignar un servicio a un técnico.

---

## FR-SRV-005 — Observaciones

**Prioridad:** MUST

El técnico podrá registrar observaciones relacionadas con el servicio.

---

# 15. Usuarios

## FR-USR-001 — Autenticación

**Prioridad:** MUST

Los usuarios administrativos deberán autenticarse antes de acceder a funciones protegidas.

---

## FR-USR-002 — Usuarios por tenant

**Prioridad:** MUST

Los usuarios empresariales deberán pertenecer a un tenant.

---

## FR-USR-003 — Roles

**Prioridad:** MUST

Los usuarios deberán tener uno o más roles según el diseño final de autorización.

---

## FR-USR-004 — Permisos

**Prioridad:** MUST

El acceso a funciones administrativas deberá controlarse mediante permisos.

---

## FR-USR-005 — Desactivación

**Prioridad:** MUST

Un usuario podrá ser desactivado sin eliminar necesariamente su historial.

---

# 16. Dashboard

## FR-DAS-001 — Dashboard empresarial

**Prioridad:** MUST

El sistema deberá proporcionar un dashboard para usuarios autorizados.

---

## FR-DAS-002 — Indicadores

**Prioridad:** MUST

El dashboard deberá mostrar indicadores relevantes para la operación.

Inicialmente:

* Pedidos.
* Ventas registradas.
* Ingresos.
* Productos.
* Inventario.
* Servicios.

---

## FR-DAS-003 — Productos destacados

**Prioridad:** SHOULD

El dashboard podrá mostrar productos con mayor cantidad de ventas o solicitudes.

---

## FR-DAS-004 — Inventario bajo

**Prioridad:** SHOULD

El dashboard podrá mostrar productos con inventario bajo.

---

# 17. Estadísticas

## FR-ANA-001 — Ventas

**Prioridad:** MUST

El sistema deberá permitir consultar información de ventas registradas.

---

## FR-ANA-002 — Periodos

**Prioridad:** SHOULD

Las estadísticas podrán filtrarse por:

* Día.
* Semana.
* Mes.
* Periodo personalizado.

---

## FR-ANA-003 — Productos

**Prioridad:** SHOULD

El sistema podrá mostrar productos con mayor actividad comercial.

---

# 18. Configuración

## FR-CFG-001 — Configuración empresarial

**Prioridad:** MUST

El owner o administrador autorizado podrá configurar parámetros del negocio.

---

## FR-CFG-002 — Módulos

**Prioridad:** SHOULD

El sistema deberá estar preparado para activar o desactivar módulos según las necesidades del tenant.

---

## FR-CFG-003 — Modalidad comercial

**Prioridad:** MUST

El tenant podrá configurar si trabaja con:

* Detal.
* Mayorista.
* Detal y mayorista.

---

# 19. Seguridad

## FR-SEC-001 — Autorización

**Prioridad:** MUST

El sistema deberá validar los permisos del usuario antes de ejecutar operaciones protegidas.

---

## FR-SEC-002 — Tenant context

**Prioridad:** MUST

Cada operación empresarial deberá ejecutarse dentro del contexto del tenant correspondiente.

---

## FR-SEC-003 — Protección de recursos

**Prioridad:** MUST

Un usuario no deberá poder acceder directamente a recursos pertenecientes a otro tenant modificando identificadores o parámetros de una solicitud.

---

# 20. Auditoría

## FR-AUD-001 — Eventos administrativos

**Prioridad:** SHOULD

El sistema deberá registrar eventos administrativos relevantes.

---

## FR-AUD-002 — Usuario responsable

**Prioridad:** SHOULD

Los eventos auditables deberán identificar al usuario que realizó la acción.

---

# 21. Requisitos no funcionales

## NFR-001 — Seguridad

AXYRA deberá aplicar buenas prácticas de seguridad en autenticación, autorización, manejo de sesiones y acceso a datos.

---

## NFR-002 — Multi-tenancy

El aislamiento de tenants deberá estar implementado en la capa de datos y/o backend, no depender únicamente del frontend.

---

## NFR-003 — Escalabilidad

La arquitectura deberá permitir agregar nuevos tenants sin requerir cambios estructurales en la aplicación.

---

## NFR-004 — Mantenibilidad

El código deberá organizarse en módulos con responsabilidades claramente definidas.

---

## NFR-005 — Testabilidad

Las funcionalidades críticas deberán poder probarse automáticamente.

---

## NFR-006 — Disponibilidad

La plataforma deberá estar diseñada para ejecutarse como servicio web disponible de forma continua en producción.

---

## NFR-007 — Rendimiento

Las operaciones comunes del sistema deberán responder en tiempos adecuados para una aplicación web moderna.

---

## NFR-008 — Responsive

La interfaz pública y administrativa deberá funcionar correctamente en dispositivos móviles, tablets y computadores.

---

## NFR-009 — Accesibilidad

La interfaz deberá seguir buenas prácticas básicas de accesibilidad web.

---

## NFR-010 — Observabilidad

La plataforma deberá permitir registrar errores y eventos importantes para facilitar diagnóstico y mantenimiento.

---

# 22. Reglas de negocio

## BR-001 — Aislamiento empresarial

Un recurso empresarial solamente podrá pertenecer a un tenant.

---

## BR-002 — SKU

El SKU deberá ser único dentro del tenant.

---

## BR-003 — Precios

Una empresa podrá utilizar una o varias listas de precios.

---

## BR-004 — Modalidad comercial

Cada tenant definirá si trabaja con ventas al detal, mayoristas o ambas.

---

## BR-005 — Stock

Cada tenant definirá el comportamiento ante productos sin disponibilidad suficiente.

---

## BR-006 — Cliente

La creación de una cuenta no será obligatoria para realizar pedidos durante el MVP.

---

## BR-007 — WhatsApp

El pedido será enviado al canal de WhatsApp configurado por el tenant.

---

## BR-008 — Pagos

Los pagos electrónicos no forman parte del MVP.

---

## BR-009 — Inventario

Las modificaciones de inventario deberán mantener trazabilidad.

---

## BR-010 — Historial

Los registros utilizados para trazabilidad comercial no deberán eliminarse físicamente cuando su eliminación comprometa el historial del negocio.

---

# 23. Casos de uso principales

## UC-001 — Consultar catálogo

Visitante consulta productos y categorías.

---

## UC-002 — Crear pedido

Cliente selecciona productos y genera un pedido.

---

## UC-003 — Finalizar pedido mediante WhatsApp

AXYRA genera el pedido y redirige al cliente a WhatsApp.

---

## UC-004 — Solicitar servicio

Cliente registra una solicitud de servicio.

---

## UC-005 — Gestionar inventario

Usuario autorizado registra movimientos y consulta existencias.

---

## UC-006 — Gestionar productos

Usuario autorizado crea y modifica productos.

---

## UC-007 — Gestionar pedidos

Usuario autorizado consulta y actualiza pedidos.

---

## UC-008 — Gestionar servicios

Usuario autorizado administra solicitudes y asigna técnicos.

---

## UC-009 — Consultar dashboard

Usuario autorizado consulta indicadores del negocio.

---

## UC-010 — Administrar usuarios

Owner o administrador autorizado gestiona usuarios y permisos.

---

# 24. Criterios generales de aceptación

El MVP deberá cumplir como mínimo:

### AC-001

Un negocio puede registrarse y configurar su información básica.

### AC-002

Un administrador puede crear categorías y productos.

### AC-003

Un administrador puede gestionar existencias.

### AC-004

Un visitante puede consultar el catálogo.

### AC-005

Un visitante puede agregar productos al carrito.

### AC-006

Un cliente puede crear un pedido sin registrarse.

### AC-007

El sistema puede generar un pedido correctamente.

### AC-008

El cliente puede ser redirigido a WhatsApp con la información del pedido.

### AC-009

Un administrador puede consultar y gestionar el pedido.

### AC-010

Un cliente puede solicitar un servicio.

### AC-011

Un administrador puede gestionar la solicitud del servicio.

### AC-012

Un técnico puede consultar servicios asignados.

### AC-013

Los usuarios solamente pueden acceder a las funciones permitidas por sus permisos.

### AC-014

Los datos de diferentes tenants permanecen aislados.

### AC-015

El owner puede configurar las modalidades comerciales del negocio.

---

# 25. Funcionalidades fuera del MVP

Las siguientes funcionalidades no serán necesarias para la primera versión:

* Procesamiento de pagos.
* Facturación electrónica.
* Integración bancaria.
* Aplicación móvil nativa.
* CRM avanzado.
* Contabilidad.
* Nómina.
* Gestión avanzada de proveedores.
* Multi-sucursal avanzada.
* IA.
* Automatización avanzada.
* WhatsApp Business API completa.

Estas funcionalidades podrán incorporarse posteriormente.

---

# 26. Trazabilidad

Los requisitos deberán mantenerse relacionados con:

```text
Product Definition
        ↓
Requirements
        ↓
Domain Model
        ↓
Architecture
        ↓
Implementation
        ↓
Tests
```

Cada funcionalidad importante deberá poder rastrearse desde su definición hasta su implementación y pruebas.

---

# 27. Estado del documento

Este documento representa la línea base inicial de requisitos de AXYRA.

Los cambios posteriores deberán registrarse mediante control de versiones y documentación de cambios.

**Estado:** Baseline v1.0

---

# 28. Modelo de dominio complementario

El siguiente modelo de dominio complementa la especificación funcional y permite convertir los requisitos en entidades, relaciones y reglas operativas para diseño y desarrollo.

## 28.1 Entidades principales

### Tenant
- Identificador único.
- Nombre comercial y configuración del negocio.
- Estado activo/inactivo.
- Configuración de modalidad comercial, WhatsApp, módulos y reglas operativas.

### User
- Identificador único.
- Datos básicos de autenticación y perfil.
- Relación con un tenant.
- Roles y permisos asociados.
- Estado activo/desactivado.

### Role
- Nombre del rol (Owner, Admin, Seller, Inventory, Technician, etc.).
- Conjunto de permisos asociados.
- Relación con múltiples usuarios.

### Permission
- Identificador del permiso.
- Nombre y descripción.
- Dominio funcional al que aplica (productos, inventario, pedidos, usuarios, servicios, etc.).

### BusinessProfile
- Información legal y operativa del negocio.
- Razón social, RUT/NIT, dirección, ciudad, país, teléfono, correo, logo y descripción.

### Category
- Nombre, descripción, estado, orden de visualización.
- Relación con productos y posibilidad de activación/desactivación.

### Product
- Nombre, SKU, descripción, categoría, imágenes y estado.
- Metadatos de comercialización y disponibilidad.
- Asociado a una o más listas de precios.

### PriceList
- Nombre de la lista (detal, mayorista, distribuidor, especial).
- Relación con el tenant y los productos que usan esa lista.

### ProductPrice
- Producto, lista de precio, moneda, valor y vigencia opcional.
- Permite soportar múltiples condiciones comerciales.

### InventoryItem
- Producto asociado.
- Stock actual, stock mínimo, stock máximo opcional y estado de disponibilidad.

### InventoryMovement
- Tipo: entrada, salida, ajuste positivo, ajuste negativo.
- Fecha, cantidad, motivo, referencia, usuario responsable.
- Relación con el producto y el tenant.

### Customer
- Datos básicos para gestión comercial.
- Posible asociación con pedido y historial.
- Puede existir sin cuenta de usuario.

### Cart
- Identificador del carrito.
- Relación temporal con un visitante o cliente.
- Estado activo o convertido en pedido.

### CartItem
- Producto, cantidad, precio aplicado, subtotal.
- Estado de validación y disponibilidad.

### Order
- Identificador único.
- Cliente, tenant, subtotal, impuestos si aplica, total, estado y fecha.
- Relación con productos y observaciones.

### OrderItem
- Producto, cantidad, precio unitario y subtotal al momento de la compra.
- Conserva la información comercial necesaria para trazabilidad.

### QuoteRequest
- Solicitud de cotización.
- Productos, cantidades, cliente, estados y comentarios.

### ServiceRequest
- Tipo de servicio, descripción, dirección, fecha solicitada, archivos, cliente, estado y técnico asignado.

### ServiceAssignment
- Relación entre servicio y técnico.
- Estado de asignación, comentarios y fecha.

### DashboardMetric
- Indicador agregado por tenant y periodo.
- Puede ser pedido, venta, ingreso, servicio o inventario bajo.

## 28.2 Relaciones clave

- Un tenant tiene muchos usuarios, productos, clientes, pedidos, servicios, movimientos y configuraciones.
- Un usuario pertenece a un tenant y posee varios roles.
- Un rol está compuesto por varios permisos.
- Un producto pertenece a una categoría y a un tenant.
- Un producto puede tener múltiples precios asociados a diferentes listas.
- Un producto tiene un inventario por tenant y múltiples movimientos históricos.
- Un cliente puede crear varios pedidos o cotizaciones.
- Un pedido pertenece a un tenant y contiene varios items.
- Un servicio pertenece a un tenant y puede asignarse a un técnico.
- Un movimiento de inventario debe conservar trazabilidad del cambio y del responsable.

## 28.3 Reglas de integridad

- Un recurso empresarial no puede pertenecer a más de un tenant.
- El SKU debe ser único dentro del tenant.
- El stock debe calcularse a partir del historial de movimientos.
- El carrito no debe generar un pedido si no cumple validación de disponibilidad.
- Los usuarios no pueden ejecutar acciones fuera del tenant al que pertenecen.
- El historial de ventas, inventario y servicios debe permanecer auditable.

---

# 29. Historias de usuario complementarias

## HU-01 — Registro de negocio
**Como** propietario de un negocio, **quiero** registrar mi empresa en AXYRA, **para** operar mi negocio dentro de un tenant aislado.

**Criterios de aceptación:**
- El negocio puede completar nombre, dirección, contacto y configuración básica.
- El tenant queda creado con aislamiento de datos.
- El owner recibe permisos administrativos iniciales.

## HU-02 — Gestión del catálogo
**Como** administrador, **quiero** crear y modificar categorías y productos, **para** mantener el catálogo actualizado.

**Criterios de aceptación:**
- El administrador puede crear una categoría con nombre y estado.
- El administrador puede crear un producto con SKU, nombre, descripción y categoría.
- El producto puede activarse o desactivarse sin eliminarse.

## HU-03 — Gestión de inventario
**Como** usuario de inventario, **quiero** registrar movimientos de stock, **para** mantener el inventario consistente y trazable.

**Criterios de aceptación:**
- El sistema registra entrada, salida o ajuste.
- El stock disponible refleja el historial de movimientos.
- El movimiento identifica al responsable y la fecha.

## HU-04 — Pedido sin cuenta
**Como** cliente, **quiero** comprar sin crear una cuenta, **para** completar la compra en menos pasos.

**Criterios de aceptación:**
- El cliente puede agregar productos al carrito y completar el pedido sin iniciar sesión.
- Se solicita la información mínima requerida para la gestión del pedido.
- El pedido queda asociado al tenant y se genera el mensaje de WhatsApp.

## HU-05 — Cotización comercial
**Como** cliente, **quiero** solicitar una cotización con varios productos, **para** obtener una propuesta antes de confirmar la compra.

**Criterios de aceptación:**
- Se puede agregar más de un producto a la cotización.
- La cotización conserva cantidades y precios aplicados.
- El estado puede cambiar según revisión y aprobación.

## HU-06 — Gestión de servicios
**Como** técnico, **quiero** consultar servicios asignados y registrar observaciones, **para** atender correctamente cada caso.

**Criterios de aceptación:**
- El técnico visualiza servicios asignados.
- Puede actualizar el estado del servicio.
- Puede registrar observaciones relevantes para la operación.

## HU-07 — Dashboard operativo
**Como** administrador, **quiero** consultar indicadores claves del negocio, **para** tomar decisiones con información actualizada.

**Criterios de aceptación:**
- El dashboard presenta ventas, pedidos, servicios y stock crítico.
- Los datos se filtran por tenant y periodo.
- El usuario solo ve métricas autoritadas según sus permisos.

---

# 30. Requisitos de experiencia, operación e integración

## 30.1 Experiencia de usuario

La experiencia de usuario deberá seguir los siguientes principios:

- Acceso sencillo para usuario visitante y cliente sin autenticación.
- Flujo minimalista desde catálogo hasta pedido.
- Formularios con validación clara en móviles y escritorio.
- Indicadores de estado del pedido, servicio y stock.
- Mensajes de error funcionales, sin revelar información sensible del sistema.

## 30.2 Integración con WhatsApp

La integración con WhatsApp debe implementarse como un servicio desacoplado del core de negocio. A nivel de requisito:

- El flujo de pedido generará un mensaje normalizado por tenant.
- El enlace se construirá dinámicamente con el número configurado por la empresa.
- La lógica de composición del mensaje debe mantenerse separada del frontend.
- La estructura del mensaje debe incluir: identificador del pedido, cliente, productos, cantidades, total estimado y canal de contacto.

## 30.3 Seguridad en flujos de negocio

- Toda operación administrativa deberá requerir autenticación y autorización.
- Los datos de un tenant no deben filtrarse a través de parámetros manipulables por el cliente.
- Los tokens y sesiones deben protegerse con políticas seguras de expiración y renovación.
- El backend debe validar permisos por recurso y no confiar en la lógica del frontend.

## 30.4 Observabilidad

La plataforma deberá registrar, como mínimo:

- Errores de aplicación y validaciones de negocio.
- Operaciones críticas de inventario y pedidos.
- Cambios de configuración del tenant.
- Accesos y acciones de usuarios administrativos.
- Métricas de tiempo de respuesta y volumen de transacciones.

## 30.5 Requisitos de datos y trazabilidad

La trazabilidad deberá ser una obligación funcional, no sólo un registro opcional:

- Cada movimiento de inventario deberá indicar origen, destino, cantidad y responsable.
- Cada pedido deberá conservar los productos, precios y cantidades originales.
- Cada servicio deberá conservar cambios de estado y observaciones del técnico.
- Cada evento relevante deberá poder vincularse a usuario, fecha y tenant.

---

# 31. Criterios de calidad del MVP

La primera versión será considerada exitosa si cumple los siguientes indicadores mínimos:

## 31.1 Requisitos funcionales mínimos

- Se soporta la creación de al menos un tenant con configuración básica.
- Un administrador puede gestionar catálogo e inventario.
- Un visitante puede consultar productos y agregarlos al carrito.
- Un cliente puede generar un pedido sin cuenta.
- El pedido genera una propuesta de confirmación mediante WhatsApp.
- Un técnico puede atender un servicio asignado.
- Un usuario solo accede a los módulos permitidos por sus permisos.

## 31.2 Requisitos no funcionales mínimos

| Área | Indicador mínimo |
| --- | --- |
| Seguridad | Autenticación obligatoria para usuarios administrativos y validación de permisos en backend |
| Multitenancy | Aislamiento de datos por tenant en base de datos y capa de servicio |
| Rendimiento | Pantallas críticas responden en tiempos adecuados para navegación web moderna |
| Disponibilidad | Servicios principales deben estar disponibles en entorno de producción con monitoreo |
| Mantenibilidad | Separación clara entre módulos, dominio y servicios transversales |
| Testabilidad | Casos críticos cubiertos por pruebas automatizadas |
| UX | Compatibilidad funcional en móvil, tablet y escritorio |

---

# 32. Supuestos, restricciones y riesgos

## 32.1 Supuestos

- El MVP no incluirá pagos electrónicos ni facturación automática.
- El canal oficial de cierre comercial será WhatsApp, con posibilidad de evolución a API.
- Los usuarios administrativos y técnicos serán gestionados dentro del tenant.
- No se contempla una estructura multi-sucursal compleja en esta fase.
- La primera implementación prioriza una operación comercial y técnica simple, pero escalable.

## 32.2 Restricciones

- El sistema debe diseñarse para un modelo SaaS multi-tenant.
- El tenant es la unidad de aislamiento y configuración autorizada.
- El inventario debe mantenerse trazable y consistente con la operación comercial.
- Los permisos no pueden depender del frontend para aplicarse de manera segura.

## 32.3 Riesgos principales

- Inconsistencia de stock si los movimientos no se validan adecuadamente.
- Fugas de información entre tenants por errores de filtrado de datos.
- Dependencia excesiva de WhatsApp si no se modulariza la integración.
- Complejidad de permisos si la gestión de roles no se modela desde el inicio.
- Duplicación de lógica comercial entre frontend y backend si no se centraliza la validación.

---

# 33. Matriz de trazabilidad MVP

| Dominio | Requisitos clave | Entidades / componentes principales | Evidencia esperada |
| --- | --- | --- | --- |
| Multi-tenancy | FR-TEN-001 a FR-TEN-004 | Tenant, User, BusinessProfile | Registro de tenant, aislamiento, configuración independiente |
| Catálogo | FR-CAT-001 a FR-CAT-009 | Category, Product, PriceList, ProductPrice | Crear, activar, desactivar, listar precios |
| Inventario | FR-INV-001 a FR-INV-008 | InventoryItem, InventoryMovement | Stock actual, historial, alertas y trazabilidad |
| Clientes | FR-CUS-001 a FR-CUS-004 | Customer, Order, Cart | Pedido sin cuenta, historial y datos básicos |
| Carrito y pedidos | FR-CART-001 a FR-CART-005, FR-ORD-001 a FR-ORD-006 | Cart, CartItem, Order, OrderItem | Validación, cálculo, persistencia y estados |
| WhatsApp | FR-WHA-001 a FR-WHA-004 | WhatsAppService, Order | Mensaje generado y redirección al canal |
| Servicios | FR-SRV-001 a FR-SRV-005 | ServiceRequest, ServiceAssignment | Solicitud, asignación, observación y cierre |
| Seguridad | FR-SEC-001 a FR-SEC-003, NFR-001, NFR-002 | User, Role, Permission | Autorización por permisos y contexto de tenant |
| Dashboard | FR-DAS-001 a FR-DAS-004 | DashboardMetric | Indicadores y stock crítico |
| Auditoría | FR-AUD-001 a FR-AUD-002 | AuditLog | Registro de eventos administrativos |

---

# 34. Cierre del documento

Este complemento consolida la base funcional, técnica y operativa del MVP de AXYRA. La intención de estas secciones es cerrar la brecha entre la definición del producto y la fase de diseño, asegurando que la arquitectura, los datos, la seguridad y la trazabilidad estén alineados con los requisitos del negocio.

La evolución posterior del documento deberá mantenerse con control de versiones y deberá rastrearse directamente con los artefactos de dominio, arquitectura, implementación y pruebas.

**Estado de actualización:** Complementado para la línea base técnica del MVP
