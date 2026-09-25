# Restricciones del Sistema

| ID | Restricción | Descripción |
| :--- | :--- | :--- |
| **RC01** | SDD Mandatorio | El diseño del sistema debe ser documentado mediante un Software Design Document (SDD) previo al desarrollo. |
| **RC02** | No Waitpoints | No se permiten bloqueos síncronos HTTP en procesos pesados; se deben usar colas/workers asíncronos. |
| **RC03** | In-Memory Cache | Es obligatorio implementar un sistema de caché (Redis) para garantizar la lectura rápida de datos de alta demanda. |
| **RC04** | Integración IA | El sistema debe consumir un servicio/API de Inteligencia Artificial para asistencia al usuario. |
| **RC05** | Aplicación Web | El sistema debe ser accesible desde navegadores web estándar mediante una API REST. |