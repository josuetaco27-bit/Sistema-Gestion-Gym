# 01. Arquitectura Inicial del Sistema

## 1. Visión General de la Arquitectura
La **Arquitectura Inicial** del **Sistema de Gestión de Gimnasio** ha sido concebida para brindar una solución centralizada, de alto rendimiento y fácil mantenimiento. El objetivo principal es respaldar las operaciones cotidianas del gimnasio (registro de socios, control de membresías, reserva de clases grupales y pagos) integrando capacidades avanzadas como asistencia mediante Inteligencia Artificial y consultas de cupos en tiempo real.

---

## 2. Diagrama de Arquitectura Inicial (Alto Nivel)

```mermaid
graph TD
    %% Clientes / Canales de Entrada
    subgraph Canales_de_Acceso [Clientes / Interfaces]
        WebAdmin[Dashboard Recepción / Admin]
        AppSocio[App Móvil / Web Socio]
        BiometricQR[Lector QR / Biométrico Recep]
    end

    %% Capa Backend Central
    subgraph Core_Backend [Servidor de Aplicación - Monolito]
        APIGateway[REST API Gateway & Security Auth JWT]
        
        subgraph Modulos_de_Negocio [Módulos Internos]
            ModSocios[Módulo Socios]
            ModClases[Módulo Clases & Reservas]
            ModPagos[Módulo Pagos]
            ModIA[Módulo Asistente IA]
        end
    end

    %% Capa de Persistencia y Soporte
    subgraph Persistencia_Soporte [Servicios de Datos e Infraestructura]
        DBSQL[(PostgreSQL - Base de Datos Relacional)]
        CacheRedis[(Redis - Caché en Memoria)]
        QueueWorker[Cola de Tareas Asíncronas]
    end

    %% Servicios Externos
    subgraph Servicios_Externos [Integraciones de Terceros]
        ExternalIA[Proveedor API LLM / IA]
        PaymentGateway[Pasarela de Pagos Stripe/Culqi]
    end

    %% Conexiones
    WebAdmin -->|HTTP / REST| APIGateway
    AppSocio -->|HTTP / REST| APIGateway
    BiometricQR -->|HTTP / REST| APIGateway

    APIGateway --> ModSocios
    APIGateway --> ModClases
    APIGateway --> ModPagos
    APIGateway --> ModIA

    ModSocios --> DBSQL
    ModClases --> DBSQL
    ModPagos --> DBSQL
    ModIA --> DBSQL

    ModClases -->|Consulta rápida cupos| CacheRedis
    ModPagos -->|Proceso asíncrono comprobantes| QueueWorker
    ModPagos -->|Integración API| PaymentGateway
    ModIA -->|Integración Prompts| ExternalIA