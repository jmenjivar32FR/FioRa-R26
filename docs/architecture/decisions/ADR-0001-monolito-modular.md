# ADR-0001: Monolito modular como arquitectura inicial

- **Estado:** Aceptado como dirección inicial.
- **Fecha:** 2026-11-20
- **Contexto:** FioRa requiere un núcleo común y capacidades diferenciadas, pero se desarrollará inicialmente por un equipo de dos personas y tendrá una instancia independiente por cliente.

## Decisión

Comenzar con una aplicación backend principal en Python organizada como monolito modular. Los módulos tendrán responsabilidades y contratos internos explícitos. PostgreSQL será la persistencia principal.

## Consecuencias

- Un único despliegue backend inicial.
- Límites modulares para reducir acoplamiento.
- Sin microservicios ni broker distribuido en la primera etapa salvo necesidad demostrable.
- Los adaptadores de infraestructura permanecerán separados de las reglas del dominio.
- Framework web, ORM y sistema de migraciones siguen pendientes.

## Revisión

Revisar esta decisión cuando exista una razón medible para desplegar o escalar una capacidad de forma independiente.
