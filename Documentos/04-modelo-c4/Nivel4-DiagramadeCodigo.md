# 04. Modelo C4 - Nivel 4: Diagrama de Código (Code Diagram)

## 1. Visión General
El **Diagrama de Código** representa el nivel de mayor detalle técnico dentro del estándar **Modelo C4**[cite: 1]. Muestra cómo se implementan físicamente las clases, interfaces y patrones de diseño de *Clean Architecture* (`ADR-002`) en código TypeScript para resolver el caso de uso de **Reserva de Cupos y Notificación Asíncrona**.

---

## 2. Diagrama de Clases (UML / Mermaid)

```mermaid
classDiagram
    class ReservaController {
        -reservarCupoUseCase: ReservarCupoUseCase
        +reservar(req: Request, res: Response): Promise~void~
    }

    class ReservarCupoUseCase {
        -socioFacade: ISocioQueryFacade
        -cacheAdapter: ICacheCuposRepository
        -reservaRepo: IReservaRepository
        -eventPublisher: IDomainEventPublisher
        +execute(dto: ReservarCupoDTO): Promise~ReservaResponseDTO~
    }

    class ISocioQueryFacade {
        <<interface>>
        +verificarMembresiaActiva(socioId: string): Promise~boolean~
    }

    class ICacheCuposRepository {
        <<interface>>
        +decrementarCupo(claseId: string): Promise~boolean~
        +obtenerCuposDisponibles(claseId: string): Promise~number~
    }

    class IReservaRepository {
        <<interface>>
        +save(reserva: Reserva): Promise~void~
        +findById(id: string): Promise~Reserva | null~
    }

    class IDomainEventPublisher {
        <<interface>>
        +publish(event: DomainEvent): Promise~void~
    }

    class RedisCuposAdapter {
        -redisClient: Redis
        +decrementarCupo(claseId: string): Promise~boolean~
        +obtenerCuposDisponibles(claseId: string): Promise~number~
    }

    class ReservaPostgresRepository {
        -dbClient: Pool
        +save(reserva: Reserva): Promise~void~
        +findById(id: string): Promise~Reserva | null~
    }

    class SocioQueryFacadeAdapter {
        -socioService: SocioService
        +verificarMembresiaActiva(socioId: string): Promise~boolean~
    }

    class Reserva {
        -id: string
        -socioId: string
        -claseId: string
        -fechaReserva: Date
        -estado: EstadoReserva
        +crear(socioId: string, claseId: string): Reserva
        +cancelar(): void
    }

    ReservaController --> ReservarCupoUseCase : usa
    ReservarCupoUseCase --> ISocioQueryFacade : depende de (DIP)
    ReservarCupoUseCase --> ICacheCuposRepository : depende de (DIP)
    ReservarCupoUseCase --> IReservaRepository : depende de (DIP)
    ReservarCupoUseCase --> IDomainEventPublisher : depende de (DIP)
    ReservarCupoUseCase ..> Reserva : crea / manipula

    RedisCuposAdapter ..|> ICacheCuposRepository : implementa
    ReservaPostgresRepository ..|> IReservaRepository : implementa
    SocioQueryFacadeAdapter ..|> ISocioQueryFacade : implementa