# 02. Historias de Usuario (User Stories)

## 1. Matriz Consolidada de Historias de Usuario

| ID | Actor Relacionado | Historia de Usuario | Prioridad |
| :--- | :--- | :--- | :--- |
| **HU-01** | **ACT-01 (Socio)** | Como **socio**, quiero **buscar y reservar una clase grupal**, para **asegurar mi cupo según mi disponibilidad**. | Must Have |
| **HU-02** | **ACT-02 (Recepcionista)** | Como **recepcionista**, quiero **registrar nuevos socios y gestionar sus membresías**, para **habilitar sus accesos al gimnasio**. | Must Have |
| **HU-03** | **ACT-01 (Socio)** | Como **socio**, quiero **interactuar con el Asistente IA**, para **recibir recomendaciones de rutinas y consultar dudas frecuentes**. | Should Have |
| **HU-04** | **ACT-04 (Administrador)** | Como **administrador**, quiero **visualizar reportes de ingresos, ocupación y membresías vencidas**, para **tomar decisiones administrativas estratégicas**. | Must Have |
| **HU-05** | **ACT-03 (Entrenador)** | Como **entrenador**, quiero **consultar la lista de inscritos en mis clases**, para **llevar el control de la sesión y asistencia**. | Should Have |
| **HU-06** | **ACT-02 (Recepcionista)** | Como **recepcionista**, quiero **registrar los pagos de las membresías**, para **mantener actualizado el historial financiero del socio**. | Must Have |

---

## 2. Detalle y Criterios de Aceptación por Historia

### HU-01: Reserva de Clases Grupales
* **Actor:** ACT-01 (Socio)
* **Descripción:** Permite al socio explorar la parrilla de horarios de clases (spinning, yoga, crossfit) y reservar un cupo en tiempo real.
* **Criterios de Aceptación:**
  * **Dado** que el socio tiene una membresía activa y vigente.
  * **Cuando** selecciona una clase con cupos disponibles y confirma la reserva.
  * **Entonces** el sistema actualiza el conteo de cupos en la caché (Redis) en menos de 1 segundo y le emite un comprobante digital de reserva.
  * **Dado** que la clase ya alcanzó su aforo máximo.
  * **Entonces** el botón de reserva se deshabilita y se le ofrece la opción de ingresar a una lista de espera.

---

### HU-02: Registro de Socios e Inscripción
* **Actor:** ACT-02 (Recepcionista)
* **Descripción:** Permite la alta de nuevos socios en el gimnasio desde el panel de recepción.
* **Criterios de Aceptación:**
  * **Dado** que la recepcionista ingresa los datos personales del cliente (DNI, Nombres, Correo, Teléfono) y selecciona un plan.
  * **Cuando** guarda el registro.
  * **Entonces** el sistema genera un perfil único de socio, emite el contrato digital y deja habilitado su credencial para validación en puerta.

---

### HU-03: Asistente Virtual de Rutinas e IA
* **Actor:** ACT-01 (Socio)
* **Descripción:** El socio consulta sugerencias de ejercicios o preguntas operativas mediante un chat inteligente alimentado por IA.
* **Criterios de Aceptación:**
  * **Dado** que el socio ingresa sus objetivos (ej. *hipertrofia*, *pérdida de peso*) al Asistente IA.
  * **Cuando** realiza una consulta sobre su plan de entrenamiento.
  * **Entonces** el sistema se conecta mediante el adaptador del servicio LLM y le retorna una rutina personalizada estructurada.

---

### HU-04: Reportes de Gestión y Ocupación
* **Actor:** ACT-04 (Administrador)
* **Descripción:** Dashboard ejecutivo para evaluar la salud financiera e indicadores de uso del gimnasio.
* **Criterios de Aceptación:**
  * **Dado** que el administrador ingresa al módulo de analítica.
  * **Cuando** selecciona un rango de fechas.
  * **Entonces** el sistema consolida las métricas de socios activos, tasa de renovación de membresías y total de ingresos procesados.

---

### HU-05: Control de Lista de Participantes
* **Actor:** ACT-03 (Entrenador)
* **Descripción:** Permite al instructor verificar la asistencia de las personas inscritas a su clase.
* **Criterios de Aceptación:**
  * **Dado** que el entrenador abre su sesión programada desde su dispositivo.
  * **Cuando** visualiza la lista de la clase actual.
  * **Entonces** puede marcar la presencia/ausencia de cada socio registrado con un solo clic.

---

### HU-06: Procesamiento de Pagos en Caja
* **Actor:** ACT-02 (Recepcionista)
* **Descripción:** Registro e impresión/envío de comprobantes de pago recibidos en mostrador.
* **Criterios de Aceptación:**
  * **Dado** que un socio realiza el pago presencial (efectivo, tarjeta, transferencia).
  * **Cuando** la recepcionista procesa la transacción.
  * **Entonces** el sistema actualiza inmediatamente la fecha de vencimiento de la membresía del socio e integra el evento contable en la base de datos.