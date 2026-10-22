# Arquitectura Inicial del Sistema de Gimnasio

## Diagrama de Arquitectura

```mermaid
flowchart TD

    subgraph ACTORES["ACTORES"]
        Socio["Socio"]
        Recep["Recepcionista"]
        Admin["Administrador"]
    end

    subgraph PRESENTACION["CAPA PRESENTACIÓN"]
        Web["Aplicación Web Frontend"]
    end

    subgraph APLICACION["CAPA APLICACIÓN / API"]
        API["API REST Gateway"]
    end

    subgraph CACHE["CAPA DE CACHÉ / EVENTOS"]
        Redis["Redis (Caché Cupos/Sesiones)"]
        Queue["Cola / Workers Asíncronos"]
    end

    subgraph NEGOCIO["LÓGICA DE NEGOCIO"]
        ModSocios["Módulo Socios y Membresías"]
        ModClases["Módulo Clases y Reservas"]
        ModPagos["Módulo Pagos y Asistencia"]
        ModIA["Módulo Asistente IA"]
    end

    subgraph DATOS["CAPA DE DATOS"]
        BD[("Base de Datos SQL")]
    end

    subgraph EXTERNOS["SERVICIOS EXTERNOS"]
        OpenAI["Servicio LLM / IA"]
    end

    ACTORES --> PRESENTACION
    PRESENTACION --> APLICACION
    APLICACION --> CACHE
    APLICACION --> NEGOCIO
    NEGOCIO --> BD
    NEGOCIO --> OpenAI
    Queue --> BD
```

## Descripción
La arquitectura inicial está diseñada con una estructura en capas escalable que responde a los requerimientos del proyecto:
- **Capa de Presentación:** Interfaz web interactiva para Socios, Recepcionistas y Administradores.
- **Capa de Aplicación y Caché:** La API REST responde a solicitudes y utiliza **Redis** para mantener en caché la información de clases y disponibilidad de cupos, reduciendo la carga en la BD. Además, utiliza procesos asíncronos para evitar bloqueos (*No Waitpoint*).
- **Lógica de Negocio:** Organizada en módulos independientes (Socios, Clases, Pagos y el Asistente IA).
- **Servicios Externos y Datos:** Integración con servicios de Inteligencia Artificial y persistencia en Base de Datos SQL.