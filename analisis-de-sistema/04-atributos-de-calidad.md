# Atributos de Calidad

| ID | Atributo de calidad | Escenario de calidad |
| :--- | :--- | :--- |
| **AC01** | Rendimiento | La consulta de clases y cupos disponibles debe responder en menos de 200 ms mediante el uso de caché en memoria (Redis). |
| **AC02** | Escalabilidad | El sistema debe soportar picos de alta concurrencia durante horas punta de reserva de clases sin degradar el servicio. |
| **AC03** | Disponibilidad | La arquitectura debe permitir que la caída del servicio de IA o envío de notificaciones no detenga el flujo principal de reservas y pagos. |
| **AC04** | Seguridad | Las operaciones administrativas y datos personales de los socios deben estar protegidos con autenticación JWT y roles (RBAC). |
| **AC05** | Mantenibilidad | El diseño modular en capas debe permitir añadir nuevas funciones (como app móvil) sin alterar los módulos existentes. |