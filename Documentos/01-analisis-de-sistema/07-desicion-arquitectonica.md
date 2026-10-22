# 07. Registro de Decisiones Arquitectónicas (ADR)

## 1. Matriz Consolidada de Decisiones Arquitectónicas

Una vez identificados los drivers arquitectónicos (`DA01` al `DA06`), definimos el registro formal de **Decisiones Arquitectónicas (ADR)** que fundamentan el diseño del **Sistema de Gestión de Gimnasio**.

| ID | Decisión Arquitectónica | Drivers Relacionados | Justificación | Resultado / Estado |
| :--- | :--- | :--- | :--- | :--- |
| **ADR-001** | **Monolito Modular en Capas** | `DA02`, `DA06` | Permite organizar las funcionalidades en módulos independientes (Socios, Clases, Pagos, IA) desplegados en una sola unidad ejecutable. | **Aceptado** — Módulos desacoplados dentro del mismo backend. |
| **ADR-002** | **Clean Architecture (Arquitectura Limpia)** | `DA04`, `DA06` | Separa las reglas de negocio centrales (membresías, cupos) de los detalles tecnológicos (bases de datos, APIs de IA). | **Aceptado** — Estructura interna en Dominio, Aplicación, Presentación e Infraestructura. |
| **ADR-003** | **Estrategia de Caché In-Memory con Redis** | `DA01` | Reduce la carga directa a la base de datos PostgreSQL para la consulta frecuente de disponibilidad de clases y cupos. | **Aceptado** — Caché rápida para respuestas HTTP en $< 200\text{ ms}$. |
| **ADR-004** | **Procesamiento Asíncrono con Colas / Workers** | `DA02` | Evita bloqueos en la interfaz al generar reportes masivos o procesar notificaciones. | **Aceptado** — Tareas pesadas ejecutadas en segundo plano (*No Waitpoint*). |
| **ADR-005** | **Integración de Servicios Externos mediante Adaptadores** | `DA03`, `DA05` | Desacopla la lógica del sistema del proveedor externo de Inteligencia Artificial (LLM API) y pasarelas de pago. | **Aceptado** — Contrato de interfaz e implementación de adaptadores concretos. |

---

## 2. Detalle de Fichas de Decisión (MADR Format)

### ADR-001: Selección del Estilo Monolito Modular
* **Contexto:** Se requiere un sistema ágil de desplegar y mantener, pero capaz de escalar en complejidad a medida que se agreguen nuevos deportes o servicios.
* **Opciones Consideradas:**
  1. *Monolito Tradicional (Spaghetti Code):* Descartado por alto acoplamiento.
  2. *Microservicios:* Descartado por sobrecarga operativa inicial e innecesaria complejidad de infraestructura.
  3. *Monolito Modular:* Elegido.
* **Decisión Elegida:** Implementar un **Monolito Modular** donde cada dominio funcional (*Socios*, *Membresías*, *Clases*, *Pagos*, *IA*) está delimitado contextualmente.
* **Consecuencias:**
  * **Positivas:** Despliegue simple como una sola unidad, bajo costo de infraestructura y fácil refactorización hacia microservicios futuros.
  * **Compromiso (Trade-off):** Requiere disciplina estricta de equipo para evitar importaciones directas no autorizadas entre módulos.

---

### ADR-002: Adopción de Clean Architecture
* **Contexto:** Las reglas de negocio centrales (cálculo de membresías, bloqueo de cupos) deben permanecer aisladas de cambios en frameworks o bases de datos.
* **Decisión Elegida:** Organizar cada módulo aplicando **Clean Architecture** mediante 4 capas concéntricas con regla de dependencia hacia el centro (*Dominio*).
* **Consecuencias:**
  * **Positivas:** Alta testabilidad unitaria de las reglas de negocio sin requerir BD activa; fácil intercambio de componentes tecnológicos.
  * **Compromiso (Trade-off):** Incremento inicial en el número de clases, interfaces y mapeadores de objetos.

---

### ADR-003: Almacenamiento en Memoria con Redis
* **Contexto:** En horas pico, la consulta recurrente de disponibilidad de clases grupales saturaría la base de datos relacional PostgreSQL.
* **Decisión Elegida:** Integrar **Redis In-Memory Cache** como capa intermedia de lectura rápida para cupos y sesiones activas.
* **Consecuencias:**
  * **Positivas:** Tiempos de respuesta asegurados en $< 200\text{ ms}$ (`DA01`).
  * **Compromiso (Trade-off):** Necesidad de gestionar políticas de invalidación de caché para evitar inconsistencias de aforo.

---

### ADR-004: Procesamiento Asíncrono para Tareas Pesadas (No Waitpoints)
* **Contexto:** La generación de reportes financieros en PDF y el envío masivo de notificaciones degradan el tiempo de respuesta si se ejecutan síncronamente.
* **Decisión Elegida:** Implementar colas de tareas en segundo plano (*Background Workers*) para procesar eventos pesados de forma asíncrona.
* **Consecuencias:**
  * **Positivas:** Interfaz de usuario siempre fluida e inmediata sin bloqueos.
  * **Compromiso (Trade-off):** Requiere monitoreo de fallos en tareas de segundo plano y reintentos.

---

### ADR-005: Desacoplamiento de Servicios Externos (IA y Pagos)
* **Contexto:** El proveedor de la API de IA o la pasarela de pagos puede cambiar de proveedor o actualizar su API externa en cualquier momento.
* **Decisión Elegida:** Utilizar el patrón de diseño **Adapter** (*Puertos y Adaptadores*).
* **Consecuencias:**
  * **Positivas:** La lógica del gimnasio depende de una interfaz propia; cambiar de proveedor solo implica crear un nuevo adaptador.