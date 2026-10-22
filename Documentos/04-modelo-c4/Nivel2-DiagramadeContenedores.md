# 02. Modelo C4 - Nivel 2: Diagrama de Contenedores (Container Diagram)

## 1. Visión General
El **Diagrama de Contenedores** desacopla la caja azul del *Sistema de Gestión de Gimnasio* para mostrar las unidades de software ejecutables y de almacenamiento que componen la solución[cite: 1]. Define los límites tecnológicos, responsabilidades de cada contenedor y las decisiones de arquitectura adoptadas (`ADR-001` a `ADR-005`).

---

## 2. Diagrama de Contenedores (Sintaxis C4 / Mermaid)

```mermaid
C4Container
    title Diagrama de Contenedores - Sistema de Gestión de Gimnasio

    Person(socio, "Socio / Cliente", "Socio del gimnasio")
    Person(staff, "Personal (Admin / Recepción / Entrenador)", "Usuarios operativos del sistema")

    Container_Boundary(gymBoundary, "Sistema de Gestión de Gimnasio") {
        Container(appMobile, "App Móvil Socio", "React Native / Flutter", "Interfaz móvil para que los socios consulten clases, reserven cupos y realicen pagos.")
        Container(webAdmin, "Dashboard Web Admin", "Single Page Application (React / Angular)", "Portal web para administradores, recepcionistas y entrenadores.")
        
        Container(apiBackend, "Monolito Backend API", "Node.js / Express / TypeScript", "Contenedor principal con arquitectura modular en capas. Ejecuta lógica de negocio, reglas de membresía y endpoints REST.", $tags="backend")
        
        ContainerDb(dbSql, "Base de Datos Principal", "PostgreSQL", "Almacena de forma persistente socios, pagos, membresías, clases e historial.")
        ContainerDb(cacheRedis, "Caché de Cupos & Sesiones", "Redis In-Memory", "Almacena en memoria el conteo de cupos de clases en tiempo real y tokens de sesión activa (ADR-003).")
        Container(workerAsync, "Background Worker Queue", "Node.js / BullQueue", "Procesa tareas diferidas de pesada carga computacional: generación de reportes PDF y notificaciones (ADR-004).")
    }

    System_Ext(pasarelaPagos, "Pasarela de Pagos Externa", "API REST / Stripe - Culqi")
    System_Ext(asistenteIA, "Proveedor Externo de IA", "API REST / OpenAI LLM")

    Rel(socio, appMobile, "Usa", "HTTPS")
    Rel(staff, webAdmin, "Usa", "HTTPS")

    Rel(appMobile, apiBackend, "Consume servicios REST", "JSON / HTTPS / JWT")
    Rel(webAdmin, apiBackend, "Consume servicios REST", "JSON / HTTPS / JWT")

    Rel(apiBackend, dbSql, "Lee y escribe datos persistentes", "TCP/IP / SQL (Port 5432)")
    Rel(apiBackend, cacheRedis, "Consulta y actualiza cupos en <200ms", "TCP/IP / Redis Protocol (Port 6379)")
    Rel(apiBackend, workerAsync, "Encola tareas pesadas en segundo plano", "Redis Pub/Sub / Queue")

    Rel(workerAsync, dbSql, "Lee datos para generar PDF/Reportes", "SQL")
    Rel(apiBackend, pasarelaPagos, "Procesa pagos de membresías", "HTTPS / REST API")
    Rel(apiBackend, asistenteIA, "Genera rutinas inteligentes", "HTTPS / REST API")