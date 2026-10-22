# 01. Modelo C4 - Nivel 1: Diagrama de Contexto del Sistema (System Context)

## 1. Visión General
El **Diagrama de Contexto del Sistema** muestra la posición del **Sistema de Gestión de Gimnasio** en su entorno operativo. Ilustra los usuarios (actores primarios) que interactúan con él y los sistemas/servicios externos con los que se integra para completar sus operaciones.

---

## 2. Diagrama de Contexto (Sintaxis C4 / Mermaid)

```mermaid
C4Context
    title Diagrama de Contexto del Sistema - Sistema de Gestión de Gimnasio

    Person(socio, "Socio / Cliente", "Usuario final del gimnasio. Consulta clases, reserva cupos, realiza pagos y consulta al Asistente IA.")
    Person(recepcion, "Recepcionista", "Personal operativo. Registra socios, cobra membresías presenciales y gestiona asistencias.")
    Person(entrenador, "Entrenador", "Personal técnico. Consulta lista de participantes e imparte las clases dirigidas.")
    Person(admin, "Administrador", "Gerencia y TI. Gestiona planes, usuarios, parámetros y revisa reportes financieros.")

    System(gymSystem, "Sistema de Gestión de Gimnasio", "Permite la administración integral del gimnasio, control de membresías, reserva de clases grupales y asistencia inteligente.")

    System_Ext(pasarelaPagos, "Pasarela de Pagos Externa", "Procesa transacciones electrónicas (tarjetas de crédito/débito/QR) de forma segura.")
    System_Ext(asistenteIA, "Proveedor Externo de IA (LLM)", "Servicio inteligente de modelo de lenguaje para generación de rutinas personalizadas.")

    Rel(socio, gymSystem, "Consulta clases, reserva cupos, paga membresías y consulta rutinas", "HTTPS / App Móvil & Web")
    Rel(recepcion, gymSystem, "Registra socios, cobra membresías y valida accesos", "HTTPS / Dashboard Web")
    Rel(entrenador, gymSystem, "Consulta participantes y valida asistencias", "HTTPS / Dashboard Web")
    Rel(admin, gymSystem, "Configura planes, administra usuarios y analiza reportes", "HTTPS / Dashboard Web")

    Rel(gymSystem, pasarelaPagos, "Procesa transacciones monetarias y cobros de membresías", "API REST / Webhooks")
    Rel(gymSystem, asistenteIA, "Envía prompts de socios para generar recomendaciones de ejercicio", "API REST")