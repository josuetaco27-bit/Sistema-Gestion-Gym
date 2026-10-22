# Enfoque Arquitectónico: Clean Architecture

## 1. Descripción Aplicada al Gimnasio
Se aplica **Clean Architecture** para aislar el núcleo de las reglas de negocio del gimnasio (control de membresías vencidas, reglas de reserva de cupos) de las tecnologías externas, interfaces web y bases de datos.

## 2. Definición de Capas
1. **Capa de Dominio (Domain):** Entidades como `Socio`, `Membresia`, `Clase`, `Reserva`. Contiene reglas puras como *impedir reserva si la membresía está vencida o el cupo está lleno*.
2. **Capa de Aplicación (Application):** Casos de uso como `ReservarClaseUseCase`, `RegistrarSocioUseCase`, `ConsultarAsistenteIAUseCase`. Define las interfaces (puertos) de repositorios.
3. **Capa de Presentación (Interface Adapters):** Controladores REST, vistas para recepción, entrenadores y socios.
4. **Capa de Infraestructura:** Conectores a la base de datos SQL, cliente Redis, cliente para API de IA y cola de procesos asíncronos.

## 3. Diagrama de Enfoque (Clean Architecture)


![Enfoque Clean Architecture](../../assets/Enfoque-Arquitectonico.jpg)