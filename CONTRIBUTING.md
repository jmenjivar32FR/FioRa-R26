# Contribuir a FioRa

## Antes de trabajar

1. Lee `README.md`.
2. Revisa el Documento Maestro en `docs/product/`.
3. Comprueba los ADR aplicables en `docs/architecture/decisions/`.
4. No supongas que una propuesta pendiente ya es una decisión aprobada.

## Ramas

- `feature/<nombre>` para funcionalidades.
- `fix/<nombre>` para correcciones.
- `docs/<nombre>` para documentación.

No se trabaja directamente sobre `main`.

## Pull Requests

Cada Pull Request debe explicar:

- Qué cambia.
- Por qué cambia.
- Qué módulos o contratos afecta.
- Cómo se verificó.
- Si requiere actualizar documentación o un ADR.

Solicita revisión del otro integrante antes de integrar cambios en `main`.

## Principios de código

- Mantener las reglas del dominio fuera de controladores, pantallas y prompts.
- No permitir que el asistente de IA escriba directamente en la base de datos.
- No permitir que un módulo acceda directamente a las estructuras internas de otro módulo.
- Validar permisos y reglas en el backend.
- No incluir secretos ni datos reales de clientes.
- Evitar añadir dependencias hasta justificar su necesidad y registrar la decisión cuando sea relevante.
