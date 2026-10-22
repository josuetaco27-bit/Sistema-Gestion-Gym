# 03. Principios de Diseño de Software (SOLID & Clean Code)

## 1. Visión General
Para garantizar la mantenibilidad (`AC-05`), extensibilidad y alta calidad del **Sistema de Gestión de Gimnasio**, el diseño detallado del código se rige rigurosamente por los principios **SOLID**, además de prácticas esenciales de *Clean Code* y diseño guiado por el dominio (*DDD*).

---

## 2. Aplicación Práctica de los Principios SOLID

### S - Single Responsibility Principle (Principio de Responsabilidad Única)
* **Definición:** Una clase debe tener una sola razón para cambiar, encapsulando una única responsabilidad funcional.
* **Aplicación en el Gimnasio:** 
  * Se separa la lógica de validación de aforo de clases (`ReservaService`) de la lógica de generación física del comprobante en PDF (`ComprobantePdfGenerator`).
  * Los controladores REST (`SocioController`) solo manejan la serialización HTTP/JSON y delegan el procesamiento de negocio a sus respectivos casos de uso (`RegistrarSocioUseCase`).

---

### O - Open/Closed Principle (Principio de Abierto/Cerrado)
* **Definición:** Las entidades de software deben estar abiertas a la extensión, pero cerradas a la modificación.
* **Aplicación en el Gimnasio:**
  * Si el gimnasio decide incorporar un nuevo método de pago (ej. *Yape/Plin* o *PayPal*), no se altera el código del caso de uso `ProcesarPagoUseCase`. Simplemente se crea una nueva clase que implementa la interfaz `IPaymentGatewayPort` y se registra en la fábrica.

---

### L - Liskov Substitution Principle (Principio de Sustitución de Liskov)
* **Definición:** Objetos de una subclase o implementación deben ser sustituibles por objetos de la superclase o interfaz sin alterar el comportamiento correcto del programa.
* **Aplicación en el Gimnasio:**
  * Las implementaciones `SocioPostgresRepository` y `SocioInMemoryRepository` (utilizada para pruebas unitarias) respetan estrictamente el contrato definido por `ISocioRepository`. El sistema funciona de manera idéntica independientemente de la implementación inyectada.

---

### I - Interface Segregation Principle (Principio de Segregación de Interfaces)
* **Definición:** Los clientes no deben estar obligados a depender de interfaces o métodos que no utilizan.
* **Aplicación en el Gimnasio:**
  * En lugar de definir una interfaz monolítica e inflada `IGymRepository`, se crean interfaces pequeñas, cohesionadas y especializadas por contexto: `ISocioRepository`, `IReservaRepository` e `ICacheCuposRepository`.

---

### D - Dependency Inversion Principle (Principio de Inversión de Dependencias)
* **Definición:** Los módulos de alto nivel (reglas de negocio) no deben depender de módulos de bajo nivel (bases de datos o APIs); ambos deben depender de abstracciones.
* **Aplicación en el Gimnasio:**
  * Los casos de uso de la capa de Aplicación (`GenerarRutinaIAUseCase`) no importan la SDK del proveedor de IA (OpenAI). Dependen de la abstracción/puerto `ILLMProviderPort`. La implementación concreta (`OpenAIAdapter`) se inyecta dinámicamente mediante Inyección de Dependencias (DI).

---

## 3. Matriz Resumen de Cumplimiento SOLID vs Módulos

| Principio SOLID | Componente / Ejemplo en Código | Beneficio Técnico Logrado |
| :--- | :--- | :--- |
| **SRP** | `ReservarCupoUseCase` vs `PdfService` | Aislamiento claro de responsabilidades; cambios en PDF no rompen reservas. |
| **OCP** | `IPaymentGatewayPort` / Adaptadores | Incorporación de nuevas pasarelas sin refactorizar el core del sistema. |
| **LSP** | Sustitución de `ISocioRepository` | Facilidad para ejecutar suite de *unit tests* velozmente con repositorios en memoria. |
| **ISP** | Puertos segregados (`ISocioRepository`, `ICacheCuposRepository`) | Componentes ligeros que solo conocen las operaciones que realmente consumen. |
| **DIP** | Contrato `ILLMProviderPort` inyectado | Cero acoplamiento con proveedores externos de Inteligencia Artificial (`ADR-005`). |

---

## 4. Principios Complementarios Aplicados

* **KISS (Keep It Simple, Stupid):** Soluciones sencillas y legibles antes que sobre-ingeniería innecesaria.
* **DRY (Don't Repeat Yourself):** Reutilización de componentes transversales (manejo centralizado de excepciones, validación de JWT) mediante la carpeta `shared/`.
* **YAGNI (You Aren't Gonna Need It):** Implementación exclusiva de las características requeridas en los drivers y requisitos funcionales (`RF-01` a `RF-08`).