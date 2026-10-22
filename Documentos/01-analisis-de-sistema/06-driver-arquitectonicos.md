# 06. Drivers Arquitectónicos (Architectural Drivers)

Los **Drivers Arquitectónicos** son los factores clave (combinación de atributos de calidad, restricciones técnicas y requisitos de negocio) que moldean y guían las decisiones de diseño del sistema.

| ID | Driver Arquitectónico | Origen / Fuente | Prioridad Negocio | Impacto Técnico | ¿Por qué influye en la arquitectura? |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **DA01** | **Uso obligatorio de memoria caché para clases y cupos** | `RC-03` / `AC-01` | Alta | Alto | Define la incorporación de una capa de datos Redis en memoria entre la API Gateway y la base de datos PostgreSQL para resolver lecturas concurrentes rápidas. |
| **DA02** | **Prohibición de procesos de espera bloqueantes (No Waitpoint)** | `RC-02` / `AC-02` | Alta | Alto | Exige una arquitectura con procesamiento asíncrono en segundo plano mediante colas de tareas/workers para tareas pesadas como reportes PDF y correos. |
| **DA03** | **Asistente de Inteligencia Artificial integrado** | `RC-04` / `RF-05` | Media | Medio | Requiere diseñar un módulo de integración aislado mediante el patrón *Adapter* para interactuar con APIs externas de LLM sin acoplar el núcleo. |
| **DA04** | **Documentación mediante SDD desde la concepción** | `RC-01` / `AC-05` | Alta | Medio | Guía el diseño estructurado en carpetas estándar (`analisis`, `arquitectura`, `diseño`, `C4`) antes de cualquier fase de codificación. |
| **DA05** | **Control estricto de seguridad por roles y permisos** | `AC-04` / `RF-01` | Alta | Alto | Impone el uso de middlewares de autenticación y autorización mediante tokens JWT y roles estricto (RBAC) en todos los endpoints REST. |
| **DA06** | **Mantenibilidad y evolución modular independiente** | `AC-05` / `RC-01` | Alta | Alto | Exige una estricta separación de responsabilidades y aplicación de *Clean Architecture* para modificar o añadir módulos sin afectar el núcleo. |

---

## 1. Detalle del Impacto de los Drivers en el Diseño

### DA01: Gestión de Concurrencia y Caché (Redis)
* **Impacto en el Estilo:** Obliga a incluir en el Monolito Modular un cliente/adaptador de caché en memoria que actúa como lectura de alta velocidad para la reserva de cupos en tiempo real.

### DA02: Procesamiento Asíncrono en Segundo Plano
* **Impacto en el Estilo:** Requiere desacoplar el hilo de procesamiento HTTP principal mediante un gestor de colas asíncronas para la generación pesada de PDF y notificaciones masivas.

### DA03: Integración de Inteligencia Artificial (LLM)
* **Impacto en el Enfoque:** Aplica el principio de inversión de dependencias (*Clean Architecture*) mediante interfaces y adaptadores para aislar el proveedor externo de IA.

### DA04 & DA06: Gobernanza, SDD y Clean Architecture
* **Impacto en la Estructura:** Define la organización lógica interna del sistema en 4 capas concéntricas (*Dominio*, *Aplicación*, *Presentación* e *Infraestructura*), facilitando la testabilidad y mantenibilidad a largo plazo.

### DA05: Autenticación Stateless y Seguridad
* **Impacto en la Infraestructura:** Establece el protocolo de validación mediante middlewares en la capa de interfaz/controladores usando firmas cifradas JWT.