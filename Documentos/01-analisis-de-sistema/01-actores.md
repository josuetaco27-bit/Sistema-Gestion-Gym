# 01. Actores del Sistema (System Actors)

## 1. Resumen Ejecutivo de Actores
El **Sistema de Gestión de Gimnasio** interactúa con diversos perfiles de usuario y servicios externos/internos. A continuación, se presenta la clasificación formal y ficha de responsabilidades de los actores identificados.

---

## 2. Matriz General de Actores

| ID | Actor | Tipo | Canal de Acceso | Responsabilidades / Necesidades Principales |
| :--- | :--- | :--- | :--- | :--- |
| **ACT-01** | **Socio** | Humano (Primario) | App Móvil / Portal Web | Consultar clases, realizar/cancelar reservas de cupos, verificar estado de membresía, realizar pagos y consultar al Asistente IA. |
| **ACT-02** | **Recepcionista** | Humano (Primario) | Dashboard Web | Registrar socios, gestionar inscripciones presenciales, cobrar membresías en caja y tomar asistencia manual/ingresos. |
| **ACT-03** | **Entrenador** | Humano (Primario) | App Móvil / Web | Consultar agenda de clases asignadas, listar participantes inscritos y validar asistencia. |
| **ACT-04** | **Administrador** | Humano (Primario) | Dashboard Web Admin | Gestión de usuarios/roles, administración de planes y tarifas, revisión de reportes financieros y configuración global del sistema. |
| **ACT-05** | **Asistente IA** | Sistema Externe/IA | API REST / Microservicio | Servicio inteligente que genera recomendaciones de rutinas personalizadas y responde dudas frecuentes de los socios. |
| **ACT-06** | **Pasarela de Pagos Externa** | Sistema Externo | API / Webhooks | Procesa transacciones electrónicas (tarjetas de crédito/débito) para pagos de membresías y suscripciones. |
| **ACT-07** | **Servicio de Caché (Redis)** | Sistema Interno | En Memoria (TCP) | Optimiza la entrega de disponibilidad de clases y control de cupos en tiempo real sin saturar la base de datos relacional. |

---

## 3. Descripción Detallada por Actor

### ACT-01: Socio
* **Perfil:** Cliente final del gimnasio.
* **Objetivo:** Disfrutar de los servicios del gimnasio, mantener el control de sus pagos y reservar sus clases de manera rápida e intuitiva.
* **Nivel de Privilegio:** Usuario final autenticado.

### ACT-02: Recepcionista
* **Perfil:** Personal operativo de recepción.
* **Objetivo:** Atender de manera ágil el ingreso presencial de los socios y gestionar pagos rápidos en mostrador para evitar colas.
* **Nivel de Privilegio:** Operador con permisos de lectoescritura en ventas e ingresos.

### ACT-03: Entrenador
* **Perfil:** Personal técnico / deportivo.
* **Objetivo:** Gestionar el aforo de sus salas y llevar el registro de asistencia en sus sesiones dirigidas.
* **Nivel de Privilegio:** Operador restringido a sus clases asignadas.

### ACT-04: Administrador
* **Perfil:** Gerencia y gestión TI.
* **Objetivo:** Monitorear la rentabilidad del negocio, ajustar tarifas de planes y asegurar la operabilidad global de la plataforma.
* **Nivel de Privilegio:** Superusuario (Access Control de nivel máximo).

### ACT-05 & ACT-06: Asistente IA y Pasarela de Pagos
* **Perfil:** Componentes tecnológicos integrados.
* **Objetivo:** Automatizar el autoservicio inteligente (IA) y garantizar transacciones monetarias seguras sin intervención humana directa.

---

> **Nota de Trazabilidad:** Los identificadores `ACT-01` a `ACT-07` se utilizarán como referencia cruzada en los documentos `02-historias-de-usuario.md` y `03-requisitos-funcionales.md`.