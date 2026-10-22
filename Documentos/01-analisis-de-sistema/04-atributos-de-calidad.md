# 04. Atributos de Calidad (Quality Attributes)

## 1. Matriz Consolidada de Atributos de Calidad

| ID | Atributo de Calidad | Categoría / Estándar | Escenario Resumido | Medida / Métrica Objetivo |
| :--- | :--- | :--- | :--- | :--- |
| **AC-01** | **Rendimiento (Performance)** | Eficiencia | La consulta de clases y disponibilidad de cupos debe responder en tiempo real mediante caché en memoria (Redis). | Tiempo de respuesta $< 200\text{ ms}$ en el 95% de las peticiones. |
| **AC-02** | **Escalabilidad (Scalability)** | Capacidad | El sistema debe soportar picos de alta concurrencia durante horas punta de reserva de clases sin degradar el servicio. | Capacidad de hasta 500 solicitudes concurrentes por minuto sin caídas. |
| **AC-03** | **Disponibilidad (Availability)** | Resiliencia | La arquitectura debe permitir que la caída del servicio de IA o envío de notificaciones no detenga el flujo principal de reservas y pagos. | Aislamiento de fallos (Mantenimiento del core al 99.9% de uptime). |
| **AC-04** | **Seguridad (Security)** | Protección | Las operaciones administrativas y datos personales de los socios deben estar protegidos con autenticación JWT y roles (RBAC). | Cifrado de datos, tokens expirables y control estricto de accesos. |
| **AC-05** | **Mantenibilidad (Maintainability)** | Modificabilidad | El diseño modular en capas debe permitir añadir nuevas funciones (como app móvil o nuevos deportes) sin alterar los módulos existentes. | Bajo acoplamiento interno; tiempo de adaptación de nuevos componentes reducido. |

---

## 2. Detalle de Escenarios de Calidad (Estructura SEI)

### AC-01: Rendimiento (Performance)
* **Fuente del Estímulo:** Socio activo desde la App Móvil o Web.
* **Estímulo:** Consulta de la parrilla de clases grupales y estado de cupos.
* **Artefacto Afectado:** Módulo de Clases & Reservas / Servicio de Caché Redis.
* **Entorno:** Operación normal y horas con alta demanda.
* **Respuesta:** El backend recupera el estado de cupos desde el servicio de caché en memoria sin golpear la base de datos PostgreSQL.
* **Medida de Respuesta:** Latencia total de la respuesta HTTP $\le 200\text{ ms}$.

---

### AC-02: Escalabilidad (Scalability)
* **Fuente del Estímulo:** Gran masa de socios intentando reservar clases populares en el horario de apertura de cupos (horas punta).
* **Estímulo:** Incremento repentino de peticiones HTTP en el endpoint de reservas.
* **Artefacto Afectado:** REST API Gateway / Módulo de Reservas.
* **Entorno:** Hora pico del gimnasio (ej. 06:00 a.m. o 07:00 p.m.).
* **Respuesta:** Asignación eficiente de recursos mediante el patrón Monolito Modular y persistencia ágil.
* **Medida de Respuesta:** Cero pérdida de transacciones y tasa de error $< 0.1\%$ bajo carga pesada.

---

### AC-03: Disponibilidad y Tolerancia a Fallos (Availability)
* **Fuente del Estímulo:** Interrupción o caída de la API externa del Asistente IA o del servicio de correo.
* **Estímulo:** Falla de conexión en servicios de terceros (Timeout / 500 Internal Error).
* **Artefacto Afectado:** Adaptador de IA / Cola de tareas asíncronas.
* **Entorno:** Operación en producción.
* **Respuesta:** El sistema maneja la excepción de forma aislada sin comprometer el registro de pagos, la validación de entrada o el calendario de clases.
* **Medida de Respuesta:** Disponibilidad del core funcional (socios, reservas y pagos) del $99.9\%$.

---

### AC-04: Seguridad y Control de Acceso (Security)
* **Fuente del Estímulo:** Usuario no autenticado o atacante que intenta acceder a endpoints administrativos.
* **Estímulo:** Petición HTTP a la API de reportes sin credenciales válidas.
* **Artefacto Afectado:** Middleware de Autenticación / RBAC (Role-Based Access Control).
* **Entorno:** Operación continua en red abierta/Internet.
* **Respuesta:** Rechazo inmediato de la solicitud mediante código HTTP `401 Unauthorized` o `403 Forbidden`.
* **Medida de Respuesta:** 100% de las rutas sensibles protegidas con firmas de tokens JWT y encriptación HTTPS/TLS.

---

### AC-05: Mantenibilidad y Modularidad (Maintainability)
* **Fuente del Estímulo:** Equipo de desarrollo / Requerimiento de nuevas funcionalidades.
* **Estímulo:** Solicitud para integrar un nuevo canal de acceso (ej. lector biométrico en puerta o app de entrenadores).
* **Artefacto Afectado:** Capa de Infraestructura / Adaptadores de Clean Architecture.
* **Entorno:** Fase de mantenimiento y evolución del sistema.
* **Respuesta:** Incorporación del nuevo adaptador sin necesidad de modificar el dominio ni las reglas centrales de membresía.
* **Medida de Respuesta:** Cero impacto o refactorización en los módulos funcionales preexistentes.