# 03. Modelo C4 - Nivel 3: Diagrama de Componentes (Component Diagram)

## 1. Visión General
El **Diagrama de Componentes** enfoca el interior del contenedor **Monolito Backend API**[cite: 1]. Para ilustrar la arquitectura interna bajo *Clean Architecture* (`ADR-002`), se toma como referencia central el **Módulo de Clases & Reservas**, mostrando sus componentes de presentación, aplicación, dominio e infraestructura, así como su conexión con los adaptadores compartidos.

---

## 2. Diagrama de Componentes (Sintaxis C4 / Mermaid)

```mermaid
C4Component
    title Diagrama de Componentes - Monolito Backend API (Módulo de Clases & Reservas)

    Container(appClients, "Clientes Web / Móvil", "React / Mobile App", "Consumen la REST API.")

    Container_Boundary(backendBoundary, "Monolito Backend API") {
        Component(authMiddleware, "Security Auth Middleware", "Express / JWT", "Valida la autenticidad del token y los roles de acceso (RBAC) [DA05].")
        
        Component(reservaController, "Reserva Controller", "REST Controller", "Punto de entrada HTTP. Recibe y valida DTOs de reservas y disponibilidad.")
        
        Component(reservaUseCase, "Reservar Cupo UseCase", "Application Service", "Coordina el flujo de reserva: verifica membresía, valida aforo en tiempo real y persiste la transacción.")
        Component(consultaCuposUseCase, "Consultar Disponibilidad UseCase", "Application Service", "Recupera la disponibilidad de clases leyendo la capa de caché rápida.")
        
        Component(socioFacade, "Socio Query Facade", "Domain Interface", "Interface de comunicación inter-módulo para validar el estado de la membresía del socio.")
        
        Component(cacheAdapter, "Redis Cupos Adapter", "Infrastructure Adapter", "Gestiona las lecturas y actualizaciones de cupos en tiempo real en Redis [ADR-003].")
        Component(reservaRepo, "Reserva Postgres Repository", "Infrastructure Repository", "Persiste las reservas confirmadas en la base de datos PostgreSQL.")
        Component(eventBus, "Domain Event Publisher", "Infrastructure Event Bus", "Publica el evento 'ReservaRealizadaEvent' hacia la cola asíncrona [ADR-004].")
    }

    ContainerDb(cacheRedis, "Redis Cache", "In-Memory Data", "Almacena la matriz de cupos [AC-01].")
    ContainerDb(dbSql, "PostgreSQL", "Relational Database", "Persistencia transaccional.")
    Container(workerQueue, "Background Worker Queue", "BullQueue", "Procesa notificaciones y correos.")

    Rel(appClients, authMiddleware, "Petición HTTP REST", "JSON / HTTPS / Bearer JWT")
    Rel(authMiddleware, reservaController, "Redirige si es válido")

    Rel(reservaController, reservaUseCase, "Ejecuta comandos de reserva")
    Rel(reservaController, consultaCuposUseCase, "Ejecuta consultas de cupos")

    Rel(reservaUseCase, socioFacade, "Verifica si el socio está VIGENTE")
    Rel(reservaUseCase, cacheAdapter, "Reserva y decrementa cupo en memoria")
    Rel(reservaUseCase, reservaRepo, "Guarda entidad Reserva si hay cupo")
    Rel(reservaUseCase, eventBus, "Notifica evento de reserva")

    Rel(consultaCuposUseCase, cacheAdapter, "Lee matriz de cupos en <200ms")

    Rel(cacheAdapter, cacheRedis, "TCP / Redis Protocol")
    Rel(reservaRepo, dbSql, "TCP / SQL")
    Rel(eventBus, workerQueue, "Redis Pub/Sub")