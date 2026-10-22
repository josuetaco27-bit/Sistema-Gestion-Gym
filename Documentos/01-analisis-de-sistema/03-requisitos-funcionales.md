# 03. Requisitos Funcionales (Functional Requirements)

## 1. Matriz Consolidada de Requisitos Funcionales

| ID | Módulo de Dominio | Descripción del Requisito Funcional | Prioridad |
| :--- | :--- | :--- | :--- |
| **RF-01** | **Gestión de Socios** | El sistema debe permitir el registro, actualización, consulta y baja lógica de socios (`CRUD`). | Must Have |
| **RF-02** | **Membresías** | El sistema debe permitir la gestión de planes de membresía, asignación de tarifas y el control automático de fechas de vencimiento. | Must Have |
| **RF-03** | **Clases y Reservas** | El sistema debe permitir a los socios consultar la disponibilidad de cupos en tiempo real y realizar reservas de clases grupales. | Must Have |
| **RF-04** | **Reglas de Reserva** | El sistema debe impedir la reserva de clases si la membresía del socio está vencida, congelada o si el cupo de la sala está lleno. | Must Have |
| **RF-05** | **Asistente IA** | El sistema debe integrar un Asistente IA para el asesoramiento de rutinas personalizadas y atención de consultas frecuentes. | Should Have |
| **RF-06** | **Procesamiento Asíncrono** | El sistema debe procesar tareas pesadas (generación de reportes masivos y envío de notificaciones/correos) de forma asíncrona sin bloquear la interfaz de usuario. | Must Have |
| **RF-07** | **Gestión de Pagos** | El sistema debe registrar y consultar transacciones financieras asociadas a los socios, membresías y comprobantes emitidos. | Must Have |
| **RF-08** | **Analítica y Reportes** | El sistema debe generar reportes administrativos e indicadores sobre asistencia, ingresos financieros y tasa de ocupación por clase. | Should Have |

---

## 2. Matriz de Trazabilidad (Historias de Usuario vs Requisitos Funcionales)

| Historia de Usuario (HU) | Nombre de la Historia | Actor Principal | Requisitos Funcionales Relacionados |
| :--- | :--- | :--- | :--- |
| **HU-01** | Reservar clase grupal | `ACT-01` (Socio) | `RF-03`, `RF-04` |
| **HU-02** | Registrar socios y membresías | `ACT-02` (Recepcionista) | `RF-01`, `RF-02` |
| **HU-03** | Asistente IA de rutinas | `ACT-01` (Socio) | `RF-05` |
| **HU-04** | Reportes administrativos | `ACT-04` (Administrador) | `RF-06`, `RF-08` |
| **HU-05** | Control de entrenadores y clases | `ACT-03` (Entrenador) | `RF-03`, `RF-08` |
| **HU-06** | Registrar pagos en caja | `ACT-02` (Recepcionista) | `RF-02`, `RF-07` |

---

## 3. Detalle de Requisitos por Módulo

### Módulo: Gestión de Socios y Membresías
* **RF-01 (Gestión de Socios):**
  * **Entrada:** Datos del socio (DNI, nombres, correo, teléfono, foto).
  * **Procesamiento:** Validación de DNI único y creación de credencial digital.
  * **Salida:** Perfil activo del socio registrado en base de datos PostgreSQL.
* **RF-02 (Control de Membresías):**
  * **Regla de Negocio:** Al cumplirse la fecha fin del plan, el estado del socio cambia automáticamente a `VENCIDO`, inhabilitando accesos en puerta.

### Módulo: Clases, Reservas y Control de Aforo
* **RF-03 & RF-04 (Reservas y Bloqueos):**
  * **Caché de Cupos:** El conteo de cupos disponibles se gestiona mediante Redis para garantizar consultas inmediatas.
  * **Validación Estricta:** Si `membresia.estado != 'ACTIVA'` o `cupos_disponibles == 0`, la API rechaza la petición enviando un mensaje explícito.

### Módulo: Integraciones e Infraestructura
* **RF-05 (Asistente IA):**
  * **Integración:** Comunicación mediante adaptador con la API de Inteligencia Artificial externa para procesar los *prompts* del socio.
* **RF-06 (Tareas en Segundo Plano):**
  * **Cola de Procesos:** La compilación de reportes en PDF y el envío masivo de correos de recordatorio se delegan a un sistema de colas (Workers/Workers Asíncronos).
* **RF-07 & RF-08 (Pagos y Analítica):**
  * **Auditoría:** Registro inmutable de cada transacción con ID de comprobante, fecha, monto y método de pago (Efectivo/Tarjeta/Pasarela externa).