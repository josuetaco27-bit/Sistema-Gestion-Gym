# Drivers Arquitectónicos

| ID | Driver arquitectónico | Origen | ¿Por qué influye en la arquitectura? |
| :--- | :--- | :--- | :--- |
| **DA01** | Uso obligatorio de memoria caché para clases y cupos. | RC03 / AC01 | Define la incorporación de una capa de datos Redis entre la API y la base de datos principal. |
| **DA02** | Prohibición de procesos de espera bloqueantes (No Waitpoint). | RC02 / AC02 | Exige una arquitectura con procesamiento asíncrono en segundo plano para tareas como reportes. |
| **DA03** | Asistente de Inteligencia Artificial integrado. | RC04 / RF05 | Requiere diseñar un módulo de integración con APIs externas de LLM. |
| **DA04** | Documentación mediante SDD desde la concepción. | RC01 / AC05 | Guía el diseño estructurado antes de cualquier fase de codificación. |
| **DA05** | Control estricto de seguridad por roles y permisos. | AC04 / RF01 | Impone el uso de middleware de autenticación/autorización basado en tokens. |
| **DA06** | El sistema debe permitir modificar o añadir módulos (como el Asistente IA o Reservas) sin afectar el núcleo de gestión de socios. | AC05 / RC01 | Exige separación de responsabilidades y Clean Architecture para mantener el código desacoplado. |