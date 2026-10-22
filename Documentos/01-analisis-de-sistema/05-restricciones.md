# 05. Restricciones del Sistema (System Constraints)

## 1. Matriz Consolidada de Restricciones

| ID | Restricción | Tipo de Restricción | Descripción | Impacto en la Arquitectura |
| :--- | :--- | :--- | :--- | :--- |
| **RC-01** | **SDD Mandatorio** | Metodológica / Proceso | El diseño del sistema debe ser documentado mediante un *Software Design Document* (SDD) formal antes de la fase de desarrollo. | Exige la elaboración de especificaciones completas de arquitectura, modelos C4 y patrones de diseño antes del código. |
| **RC-02** | **No Waitpoints** | Arquitectura / Rendimiento | No se permiten bloqueos síncronos HTTP en procesos pesados (reportes, correos); se deben usar colas/workers asíncronos. | Obliga a implementar una arquitectura dirigida por eventos o tareas en segundo plano (*Background Processing*). |
| **RC-03** | **In-Memory Cache** | Tecnológica / Infraestructura | Es obligatorio implementar un servicio de caché en memoria (Redis) para garantizar la lectura rápida de datos de alta demanda. | Condiciona la capa de datos para incluir almacenamiento temporal en memoria para cupos y sesiones. |
| **RC-04** | **Integración IA** | Integración / Externa | El sistema debe consumir un servicio/API de Inteligencia Artificial externa para asistencia personalizada al usuario. | Exige el uso de patrones de diseño (como *Adapter*) para aislar las llamadas al proveedor LLM. |
| **RC-05** | **Aplicación Web / API REST** | Arquitectura / Protocolo | El sistema debe ser accesible desde navegadores web estándar y clientes móviles mediante una API REST bien definida. | Establece el protocolo de comunicación desacoplado mediante controladores REST y respuestas JSON. |

---

## 2. Detalle y Mecanismos de Cumplimiento

### RC-01: Documentación SDD Mandatoria
* **Descripción:** Requisito de gobernanza del proyecto que prohíbe el inicio del desarrollo sin la validación del SDD.
* **Mecanismo de Cumplimiento:** Estructuración del repositorio en las carpetas `01-analisis-de-sistema/`, `02-arquitectura-software/`, `03-diseño-de-software/` y `04-modelo-c4/` en formato Markdown técnico.

---

### RC-02: Principio No Waitpoints (Procesamiento Asíncrono)
* **Descripción:** Evita latencias innecesarias en la interfaz impidiendo la ejecución de tareas pesadas en el hilo principal del servidor HTTP.
* **Mecanismo de Cumplimiento:** Delegación de la generación de PDF de reportes masivos y notificaciones por correo a un gestor de colas asíncronas (*Message Queue / Background Worker*).

---

### RC-03: Servicio de Caché en Memoria (Redis)
* **Descripción:** Obligatoriedad de mitigar cuellos de botella en la base de datos PostgreSQL durante lecturas concurrentes de alta frecuencia.
* **Mecanismo de Cumplimiento:** Almacenamiento directo del conteo de cupos disponibles de clases grupales y estado de credenciales de socios en un clúster de **Redis In-Memory Cache**.

---

### RC-04: Integración con Servicios Externos de IA
* **Descripción:** Incorporación obligatoria de funciones inteligentes (asistencia en rutinas) consumiendo servicios de LLM en la nube.
* **Mecanismo de Cumplimiento:** Creación de un módulo especializado (`Módulo Asistente IA`) que implementa la interfaz del adaptador de IA para gestionar los *prompts* y *tokens* de forma segura.

---

### RC-05: Protocolo de Comunicación API REST
* **Descripción:** Estandarización de las interfaces de entrada para soportar múltiples clientes (Web Admin, Web Socio y App Móvil).
* **Mecanismo de Cumplimiento:** Exposición de endpoints RESTful bajo HTTPS con autenticación basada en *JSON Web Tokens* (JWT) y payloads en JSON.