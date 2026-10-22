# 08. Escenarios de Atributos de Calidad (SEI Format)

## 1. Introducción
Para validar las decisiones arquitectónicas (`ADR-001` a `ADR-005`), se formalizan los **Escenarios de Atributos de Calidad** bajo la estructura estándar del *Software Engineering Institute (SEI)*. Cada escenario define 6 elementos fundamentales:
1. **Fuente del Estímulo:** Entidad que genera la interacción (Usuario, Sistema Externo, Atacante, Desarrollador).
2. **Estímulo:** Condición o evento que requiere una respuesta del sistema.
3. **Artefacto:** Parte del sistema afectada por el estímulo (API, Base de datos, Caché, Módulo).
4. **Entorno:** Condiciones operativas bajo las cuales ocurre el estímulo (Carga normal, Hora pico, Fallo de red).
5. **Respuesta:** Acción realizada por la arquitectura tras recibir el estímulo.
6. **Medida de Respuesta:** Métrica cuantificable con la que se evalúa el éxito de la respuesta.

---

## 2. Matriz Consolidada de Escenarios

| ID | Atributo de Calidad | Artefacto Afectado | Estímulo | Medida de Respuesta Objetivo |
| :--- | :--- | :--- | :--- | :--- |
| **ESC-01** | **Rendimiento** (`AC-01`) | Módulo de Clases & Caché Redis | Consulta simultánea de cupos para clases populares. | Tiempo de respuesta $\le 200\text{ ms}$ en el 95% de las solicitudes. |
| **ESC-02** | **Escalabilidad** (`AC-02`) | Monolito Backend / API Gateway | Pico de tráfico en hora punta de reservas de gimnasio. | Soportar hasta 500 solicitudes/minuto con $0\%$ de pérdida de datos. |
| **ESC-03** | **Disponibilidad** (`AC-03`) | Adaptador IA / Colas Asíncronas | Caída repentina de la API externa del Asistente IA. | Aislamiento del fallo; el core del gimnasio opera al $99.9\%$ de uptime. |
| **ESC-04** | **Seguridad** (`AC-04`) | Middleware de Autenticación JWT / RBAC | Intento de acceso no autorizado a reportes financieros. | Rechazo inmediato (HTTP `401`/`403`) e inspección en log de auditoría. |
| **ESC-05** | **Mantenibilidad** (`AC-05`) | Capa de Infraestructura / Clean Arch | Incorporación de un nuevo método de pago o integración. | Modificación realizada sin alterar la lógica del núcleo ni reiniciar otros módulos. |

---

## 3. Especificación Detallada de Escenarios (Formato SEI)

### ESC-01: Consulta de Cupos en Tiempo Real (Rendimiento)
* **Fuente del Estímulo:** Socio activo desde la App Móvil o Web (`ACT-01`).
* **Estímulo:** Solicita visualizar la disponibilidad de cupos de las clases grupales del día.
* **Artefacto Afectado:** Módulo de Clases & Reservas / Servicio de Caché Redis (`RC-03`).
* **Entorno:** Operación normal e inicio de inscripciones.
* **Respuesta:** El backend atiende la petición leyendo directamente la información desde la caché en memoria (**Redis**) sin consultar la base de datos PostgreSQL.
* **Medida de Respuesta:** La respuesta HTTP retorna la lista de cupos actualizados con un tiempo de latencia $\le 200\text{ ms}$.

---

### ESC-02: Alta Concurrencia en Hora Pico (Escalabilidad)
* **Fuente del Estímulo:** Grupo masivo de socios intentando reservar clases a primera hora.
* **Estímulo:** Incremento rápido de peticiones concurrentes de reserva en el endpoint REST (`DA01`).
* **Artefacto Afectado:** REST API Gateway / Módulo de Reservas / PostgreSQL.
* **Entorno:** Hora punta operativa del gimnasio.
* **Respuesta:** El sistema gestiona las reservas bloqueando temporalmente el cupo en la caché para evitar sobre-reservas (*overbooking*) y confirma la transacción.
* **Medida de Respuesta:** Capacidad de procesar hasta 500 peticiones por minuto manteniendo una tasa de error de transacción $< 0.1\%$.

---

### ESC-03: Tolerancia a Fallas en Servicios Externos (Disponibilidad)
* **Fuente del Estímulo:** Proveedor externo de la API de Inteligencia Artificial (LLM).
* **Estímulo:** Tiempo de espera agotado (*Timeout*) o respuesta de error `500 Internal Server Error` del servicio de IA.
* **Artefacto Afectado:** Adaptador de IA / Módulo Asistente IA.
* **Entorno:** Operación continua del gimnasio.
* **Respuesta:** El adaptador atrapa la excepción y retorna un mensaje amigable al socio indicando que la asistencia IA no está disponible, asegurando que los módulos de pagos, reservas e ingreso sigan funcionando con normalidad.
* **Medida de Respuesta:** Disponibilidad total de la gestión de socios y reservas mantenida al $99.9\%$ sin caídas globales.

---

### ESC-04: Protección contra Accesos No Autorizados (Seguridad)
* **Fuente del Estímulo:** Usuario no autenticado o atacante externo.
* **Estímulo:** Intento de consumo de endpoints sensibles (ej. `/api/v1/admin/reportes-financieros`).
* **Artefacto Afectado:** Middleware de Autenticación / Control de Acceso Basado en Roles (RBAC).
* **Entorno:** Conexión a través de la red abierta/Internet.
* **Respuesta:** El middleware intercepta la petición, valida la ausencia o invalidez del token JWT y deniega el acceso registrando la IP en la bitácora de seguridad.
* **Medida de Respuesta:** 100% de peticiones no autorizadas bloqueadas con respuesta HTTP `401 Unauthorized` o `403 Forbidden`.

---

### ESC-05: Incorporación de Nuevos Módulos o Canales (Mantenibilidad)
* **Fuente del Estímulo:** Equipo de Desarrollo / Evolutivo del Proyecto.
* **Estímulo:** Requerimiento de añadir un nuevo adaptador de ingreso por lector biométrico/QR en recepción.
* **Artefacto Afectado:** Capa de Infraestructura / Adaptadores de Clean Architecture (`ADR-002`).
* **Entorno:** Fase de mantenimiento y evolución del software.
* **Respuesta:** Se crea una nueva clase que implementa la interfaz de control de acceso existente sin necesidad de refactorizar ni modificar la lógica central del módulo de socios o membresías.
* **Medida de Respuesta:** Integración completada con $0$ líneas de código modificadas en el dominio de la aplicación.