# 02. Componentes Arquitectónicos del Sistema

## 1. Visión General de Componentes

El **Sistema de Gestión de Gimnasio** se estructura en componentes lógicos de software de alta cohesión y bajo acoplamiento. Cada componente encapsula un dominio funcional específico o una responsabilidad de infraestructura, asegurando la mantenibilidad (`AC-05`) y el cumplimiento de las decisiones de diseño (`ADR-001` a `ADR-005`)[cite: 4, 7, 8].

---

## 2. Mapa de Componentes del Backend

```mermaid
graph TD
    subgraph Capa_de_Entrada [Capa de Presentación / API]
        AuthMW[Security Auth Middleware - JWT/RBAC]
        SocioCtrl[Socio Controller]
        ClaseCtrl[Clase & Reserva Controller]
        PagoCtrl[Pago Controller]
        IACtrl[Asistente IA Controller]
    end

    subgraph Capa_de_Aplicación [Casos de Uso & Lógica de Negocio]
        SocioService[Servicio de Gestión de Socios]
        ReservaService[Servicio de Reservas & Aforo]
        PagoService[Servicio de Pagos & Facturación]
        IAService[Servicio de Recomendación IA]
    end

    subgraph Capa_de_Infraestructura [Acceso a Datos & Adaptadores]
        SocioRepo[Socio Repository PostgreSQL]
        RedisAdapter[Redis Cache Adapter - Cupos]
        QueueWorker[Background Worker Queue]
        PaymentAdapter[Payment Gateway Adapter]
        LLMAdapter[LLM IA External Adapter]
    end

    %% Conexiones internas
    AuthMW --> SocioCtrl
    AuthMW --> ClaseCtrl
    AuthMW --> PagoCtrl
    AuthMW --> IACtrl

    SocioCtrl --> SocioService
    ClaseCtrl --> ReservaService
    PagoCtrl --> PagoService
    IACtrl --> IAService

    SocioService --> SocioRepo
    ReservaService --> SocioRepo
    ReservaService --> RedisAdapter
    PagoService --> SocioRepo
    PagoService --> PaymentAdapter
    PagoService --> QueueWorker
    IAService --> LLMAdapter