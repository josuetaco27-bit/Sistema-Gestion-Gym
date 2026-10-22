# 01. Diseño Interno de Módulos (Module Internal Design)

## 1. Visión General de Organización
En alineación con el **Monolito Modular** (`ADR-001`) y la **Clean Architecture** (`ADR-002`), la aplicación backend organiza el código fuente dividiéndolo en carpetas aisladas por dominio funcional. Cada módulo posee internamente sus propias capas de Dominio, Aplicación, Presentación e Infraestructura.

---

## 2. Estructura de Directorios del Código Fuente

```text
src/
├── modules/
│   ├── socios/
│   │   ├── domain/
│   │   │   ├── entities/          # Socio.entity.ts, EstadoSocio.vo.ts
│   │   │   └── repositories/      # ISocioRepository.ts (Puerto)
│   │   ├── application/
│   │   │   ├── use-cases/         # RegistrarSocio.use-case.ts, RenovarMembresia.use-case.ts
│   │   │   └── dtos/              # CrearSocioDTO.ts, SocioResponseDTO.ts
│   │   ├── infrastructure/
│   │   │   ├── persistence/       # SocioPostgresRepository.ts, Socio.schema.ts
│   │   │   └── controllers/       # SocioController.ts
│   │   └── socios.module.ts
│   │
│   ├── clases-reservas/
│   │   ├── domain/
│   │   │   ├── entities/          # Clase.entity.ts, Reserva.entity.ts
│   │   │   └── repositories/      # IReservaRepository.ts, ICacheCuposRepository.ts
│   │   ├── application/
│   │   │   └── use-cases/         # ReservarCupo.use-case.ts, ListarDisponibilidad.use-case.ts
│   │   ├── infrastructure/
│   │   │   ├── cache/             # RedisCuposRepository.ts
│   │   │   └── controllers/       # ReservaController.ts
│   │   └── clases.module.ts
│   │
│   ├── pagos/
│   │   ├── domain/
│   │   │   ├── entities/          # Pago.entity.ts, Comprobante.entity.ts
│   │   │   └── ports/             # IPaymentGatewayPort.ts
│   │   ├── application/
│   │   │   └── use-cases/         # ProcesarPagoTarjeta.use-case.ts
│   │   ├── infrastructure/
│   │   │   ├── adapters/          # StripeAdapter.ts, CulqiAdapter.ts
│   │   │   ├── workers/           # EmisionComprobanteWorker.ts (No Waitpoint)
│   │   │   └── controllers/       # PagoController.ts
│   │   └── pagos.module.ts
│   │
│   └── asistente-ia/
│       ├── domain/
│       │   └── ports/             # ILLMProviderPort.ts
│       ├── application/
│       │   └── use-cases/         # GenerarRutinaIA.use-case.ts
│       ├── infrastructure/
│       │   ├── adapters/          # OpenAIAdapter.ts
│       │   └── controllers/       # IAController.ts
│       └── asistente-ia.module.ts
│
├── shared/                       # Kernel Compartido
│   ├── domain/                   # Entity.ts, AggregateRoot.ts, ValueObject.ts
│   ├── infrastructure/           # Middleware Auth JWT, DatabaseConnection.ts
│   └── errors/                   # DomainError.ts, NotFoundError.ts
│
└── main.ts                       # Punto de Entrada Unificado