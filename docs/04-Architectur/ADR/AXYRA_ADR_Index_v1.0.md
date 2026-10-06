# AXYRA — Architecture Decision Records Index

**Versión:** 1.0
**Estado:** Baseline
**Fecha:** 2026-10-06
**Proyecto:** AXYRA
**Documento:** Architecture Decision Records Index

---

# 1. Propósito

Este documento funciona como índice central de las decisiones arquitectónicas de AXYRA.

Los Architecture Decision Records (ADR) documentan decisiones técnicas relevantes que afectan la arquitectura, implementación, seguridad, infraestructura o evolución del sistema.

Cada ADR debe responder:

* ¿Qué decisión se tomó?
* ¿Por qué se tomó?
* ¿Qué alternativas fueron consideradas?
* ¿Qué consecuencias tiene?
* ¿Qué restricciones introduce?
* ¿Cuándo debe revisarse?

---

# 2. Objetivos

Los ADR tienen como objetivos:

1. Mantener trazabilidad de las decisiones técnicas.
2. Evitar decisiones contradictorias.
3. Registrar las razones detrás de las tecnologías seleccionadas.
4. Facilitar mantenimiento futuro.
5. Facilitar incorporación de nuevos desarrolladores.
6. Reducir decisiones repetidas.
7. Documentar trade-offs.
8. Permitir revisar decisiones cuando cambien las condiciones del proyecto.

---

# 3. Estructura de un ADR

Todos los ADR deberán utilizar la siguiente estructura:

```text id="h0j3du"
# ADR-NNN: Título

## Status

## Date

## Context

## Decision

## Alternatives Considered

## Consequences

## Risks

## Revisit Conditions
```

---

# 4. Estados

Un ADR puede tener uno de los siguientes estados:

```text id="5hj9hp"
PROPOSED
ACCEPTED
SUPERSEDED
DEPRECATED
REJECTED
```

### PROPOSED

Decisión propuesta pero todavía no aprobada.

### ACCEPTED

Decisión oficialmente adoptada.

### SUPERSEDED

La decisión fue reemplazada por otro ADR.

### DEPRECATED

La decisión dejó de ser necesaria.

### REJECTED

La alternativa fue evaluada y descartada.

---

# 5. Convenciones

Los ADR utilizarán numeración secuencial:

```text id="k4w8kl"
ADR-001
ADR-002
ADR-003
...
```

Los ADR aceptados no deberán modificarse retroactivamente para cambiar su significado histórico.

Si una decisión cambia:

```text id="j3v7bq"
ADR anterior
     ↓
SUPERSEDED
     ↓
Nuevo ADR
```

---

# 6. ADR Registry

| ID      | Decisión                      | Estado   | Documento                       |
| ------- | ----------------------------- | -------- | ------------------------------- |
| ADR-001 | Arquitectura Modular Monolith | PROPOSED | `ADR-001-modular-monolith.md`   |
| ADR-002 | Motor de base de datos        | PROPOSED | `ADR-002-database.md`           |
| ADR-003 | Estrategia Multi-Tenant       | PROPOSED | `ADR-003-multi-tenancy.md`      |
| ADR-004 | Row Level Security            | PROPOSED | `ADR-004-row-level-security.md` |
| ADR-005 | Framework Frontend            | PROPOSED | `ADR-005-frontend.md`           |
| ADR-006 | Runtime y Backend             | PROPOSED | `ADR-006-backend.md`            |
| ADR-007 | Autenticación                 | PROPOSED | `ADR-007-authentication.md`     |
| ADR-008 | Autorización                  | PROPOSED | `ADR-008-authorization.md`      |
| ADR-009 | API Strategy                  | PROPOSED | `ADR-009-api.md`                |
| ADR-010 | Object Storage                | PROPOSED | `ADR-010-storage.md`            |
| ADR-011 | Deployment                    | PROPOSED | `ADR-011-deployment.md`         |
| ADR-012 | CI/CD                         | PROPOSED | `ADR-012-ci-cd.md`              |
| ADR-013 | Testing Strategy              | PROPOSED | `ADR-013-testing.md`            |
| ADR-014 | Observability                 | PROPOSED | `ADR-014-observability.md`      |
| ADR-015 | WhatsApp Integration          | PROPOSED | `ADR-015-whatsapp.md`           |
| ADR-016 | Background Jobs               | PROPOSED | `ADR-016-background-jobs.md`    |
| ADR-017 | Caching Strategy              | PROPOSED | `ADR-017-caching.md`            |
| ADR-018 | File/Image Management         | PROPOSED | `ADR-018-file-management.md`    |
| ADR-019 | Money and Currency Handling   | PROPOSED | `ADR-019-money.md`              |
| ADR-020 | Date and Time Handling        | PROPOSED | `ADR-020-date-time.md`          |
| ADR-021 | AI Integration Strategy       | PROPOSED | `ADR-021-ai.md`                 |

---

# 7. Decision Categories

Las decisiones de AXYRA se agrupan en:

## Architecture

* Arquitectura general.
* Modularización.
* Dependencias.
* Escalabilidad.

## Data

* PostgreSQL.
* Multi-tenancy.
* RLS.
* Identificadores.
* Integridad.

## Application

* Backend.
* API.
* Validación.
* Manejo de errores.

## Frontend

* Framework.
* Rendering.
* State management.
* UI architecture.

## Security

* Authentication.
* Authorization.
* Tenant isolation.
* Secrets.
* Rate limiting.

## Infrastructure

* Hosting.
* Storage.
* CI/CD.
* Monitoring.

## Integrations

* WhatsApp.
* Email.
* Payments.
* Automation.

## Quality

* Testing.
* Observability.
* Performance.

## AI

* AI architecture.
* Agents.
* Models.
* Tools.
* Knowledge.

---

# 8. Decision Process

Una decisión arquitectónica deberá seguir:

```text id="6br4tp"
Problem
   ↓
Context
   ↓
Alternatives
   ↓
Evaluation
   ↓
Decision
   ↓
Consequences
   ↓
ADR
   ↓
Implementation
```

No se debe seleccionar una tecnología únicamente porque sea popular.

La decisión deberá considerar:

* Requisitos de AXYRA.
* Complejidad.
* Costos.
* Seguridad.
* Mantenibilidad.
* Escalabilidad.
* Experiencia del equipo.
* Ecosistema.
* Portabilidad.
* Riesgos.

---

# 9. Decision Authority

Durante el desarrollo inicial de AXYRA, las decisiones arquitectónicas serán evaluadas contra:

1. Requisitos del producto.
2. Modelo de dominio.
3. Requisitos no funcionales.
4. Seguridad.
5. Costos.
6. Complejidad operacional.
7. Evolución futura.

Una tecnología no debe introducirse únicamente para resolver un problema hipotético.

---

# 10. ADR Dependencies

Algunas decisiones dependen de otras.

```text id="jv8d6g"
ADR-001
Modular Monolith
      │
      ├──────────────┐
      ▼              ▼
ADR-005          ADR-006
Frontend         Backend
      │              │
      └──────┬───────┘
             ▼
          ADR-009
             API
             │
             ▼
          ADR-002
          Database
             │
       ┌─────┴─────┐
       ▼           ▼
   ADR-003      ADR-004
Multi-Tenant      RLS
```

---

# 11. ADR Lifecycle

Un ADR comienza como:

```text
PROPOSED
```

Después de evaluación:

```text
PROPOSED
   ↓
ACCEPTED
```

Si posteriormente se reemplaza:

```text
ACCEPTED
   ↓
SUPERSEDED
```

---

# 12. Review Policy

Un ADR debe revisarse cuando:

* Cambien requisitos fundamentales.
* Cambie la escala esperada.
* Aparezca una nueva restricción técnica.
* El proveedor seleccionado deje de ser adecuado.
* Aumenten significativamente los costos.
* Se detecte un riesgo arquitectónico.
* Se introduzca un nuevo contexto tecnológico.

---

# 13. ADR Naming Convention

Los archivos deberán utilizar:

```text
ADR-NNN-short-description.md
```

Ejemplo:

```text
ADR-001-modular-monolith.md
ADR-002-database.md
ADR-003-multi-tenancy.md
```

---

# 14. Documentation Rules

Cada ADR debe:

* Ser autocontenido.
* Evitar información contradictoria con otros ADR.
* Referenciar ADR relacionados.
* Explicar alternativas.
* Explicar consecuencias.
* Identificar riesgos.
* Indicar condiciones de revisión.

Los ADR no deben utilizarse como documentación general del sistema.

Su propósito específico es documentar **decisiones**.

---

# 15. Current Architectural Baseline

A la fecha de este documento existen las siguientes decisiones conceptuales:

```text id="0lfz6d"
Architecture:
    Modular Monolith

Domain:
    Domain-oriented

API:
    API-first

Database:
    PostgreSQL candidate

Multi-tenancy:
    Shared Database / Shared Schema candidate

Security:
    Database-level isolation candidate

Deployment:
    Managed cloud infrastructure candidate
```

Las decisiones marcadas como `candidate` deberán convertirse en decisiones formales mediante los ADR correspondientes.

---

# 16. Priority Order

Los ADR se resolverán en el siguiente orden:

### Phase 1 — Core Architecture

```text
ADR-001
ADR-002
ADR-003
ADR-004
```

### Phase 2 — Application Stack

```text
ADR-005
ADR-006
ADR-007
ADR-008
ADR-009
```

### Phase 3 — Infrastructure

```text
ADR-010
ADR-011
ADR-012
```

### Phase 4 — Quality

```text
ADR-013
ADR-014
ADR-017
ADR-019
ADR-020
```

### Phase 5 — Integrations

```text
ADR-015
ADR-016
ADR-018
```

### Phase 6 — AI

```text
ADR-021
```

---

# 17. Current Status

Los ADR todavía no constituyen decisiones técnicas definitivas.

La siguiente etapa será evaluar y aceptar los ADR de la Phase 1.

---

# 18. Next Step

El siguiente documento será:

```text id="l7u6v1"
docs/04-Architecture/ADR/ADR-001-modular-monolith.md
```

Este ADR formalizará la decisión de utilizar una arquitectura Modular Monolith para la primera versión de AXYRA.
