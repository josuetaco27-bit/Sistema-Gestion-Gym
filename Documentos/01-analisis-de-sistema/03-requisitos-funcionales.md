# Requisitos Funcionales

| ID | Requisito Funcional |
| :--- | :--- |
| **RF01** | El sistema debe permitir el registro, actualización, consulta y baja de socios. |
| **RF02** | El sistema debe permitir la gestión de planes de membresía y el control de vencimientos. |
| **RF03** | El sistema debe permitir a los socios consultar la disponibilidad de cupos en tiempo real y realizar reservas. |
| **RF04** | El sistema debe impedir la reserva de clases si la membresía del socio está vencida o el cupo está lleno. |
| **RF05** | El sistema debe integrar un Asistente IA para asesoramiento y consultas frecuentes. |
| **RF06** | El sistema debe procesar tareas pesadas (generación de reportes y notificaciones) de forma asíncrona sin bloquear la interfaz. |
| **RF07** | El sistema debe registrar y consultar pagos asociados a socios y membresías. |
| **RF08** | El sistema debe generar reportes administrativos sobre asistencia, ingresos y tasa de ocupación. |

## Relación entre Historias de Usuario y Requisitos Funcionales

| Historia de usuario | Requisitos funcionales relacionados |
| :--- | :--- |
| **HU01** Reservar clase | RF03, RF04 |
| **HU02** Registrar socios y membresías | RF01, RF02 |
| **HU03** Asistente IA | RF05 |
| **HU04** Reportes administrativos | RF06, RF08 |
| **HU05** Control de entrenadores y clases | RF03, RF08 |
| **HU06** Registrar pagos | RF02, RF07 |