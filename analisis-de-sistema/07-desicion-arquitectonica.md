# Registro de Decisiones Arquitectónicas (ADR) - Sistema de Gestión de Gimnasio

Ahora que ya tenemos identificados los drivers arquitectónicos, definimos las decisiones arquitectónicas (ADR) que permitirán responder a las necesidades y requisitos del sistema.

| ID | Decisión arquitectónica | Driver relacionado | Justificación | Resultado |
| :--- | :--- | :--- | :--- | :--- |
| **ADR-001** | Monolito Modular en Capas | DA02, DA06 | Permite organizar las funcionalidades en módulos independientes (Socios, Clases, Pagos, IA) desplegados en una sola unidad. | Módulos desacoplados dentro del mismo backend. |
| **ADR-002** | Clean Architecture (Arquitectura Limpia) | DA04, DA06 | Separa las reglas de negocio centrales (membresías, cupos) de los detalles tecnológicos (bases de datos, APIs de IA). | Estructura interna dividida en Dominio, Aplicación, Presentación e Infraestructura. |
| **ADR-003** | Estrategia de Caché In-Memory con Redis | DA01 | Reduce la carga directa a la base de datos SQL para la consulta frecuente de disponibilidad de clases y cupos. | Caché rápida para respuestas en < 200 ms. |
| **ADR-004** | Procesamiento Asíncrono con Colas/Workers | DA02 | Evita bloqueos en la interfaz al generar reportes pesados o procesar notificaciones. | Tareas pesadas ejecutadas en segundo plano (No Waitpoint). |
| **ADR-005** | Integración de Servicios Externos mediante Adaptadores | DA03 | Desacopla la lógica del sistema del proveedor externo de Inteligencia Artificial (LLM API). | Contrato de interfaz e implementación de adaptador para IA. |