# Estilo Arquitectónico Seleccionado

## 1. Estilo Elegido
**Monolito Modular con Organización Interna en Capas**

## 2. Justificación
* **Simplicidad y Eficiencia:** Ideal para la gestión centralizada de socios, reservas y asistencias en el gimnasio sin la sobrecarga operativa de microservicios.
* **Modularidad:** Separa lógicamente los dominios clave (Socios, Clases/Reservas, Pagos, Asistente IA) facilitando el mantenimiento y las pruebas.

## 3. Diagrama del Estilo Arquitectónico
```mermaid
graph TD
    Client[Aplicación Web Frontend] -->|HTTP / REST| Gateway[API REST Gateway]
    
    subgraph Monolito Backend Gimnasio
        subgraph Modulo Socios
            PresS[Presentación] --> LogS[Lógica de Negocio] --> DatS[Acceso a Datos]
        end
        subgraph Modulo Clases y Reservas
            PresC[Presentación] --> LogC[Lógica de Negocio] --> DatC[Acceso a Datos]
        end
        subgraph Modulo Pagos
            PresP[Presentación] --> LogP[Lógica de Negocio] --> DatP[Acceso a Datos]
        end
        subgraph Modulo Asistente IA
            PresIA[Presentación] --> LogIA[Lógica de Negocio] --> DatIA[Acceso a Datos]
        end
    end

    Gateway --> PresS
    Gateway --> PresC
    Gateway --> PresP
    Gateway --> PresIA

    DatS --> DB[(Base de Datos SQL)]
    DatC --> DB
    DatP --> DB

    LogC -->|Caché| Redis[(Redis - Cupos/Clases)]
    LogIA -->|API REST| ExternalIA[Servicio Externo LLM / IA]