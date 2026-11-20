# Material transfer

Primer flujo operativo de referencia: seguimiento de la transferencia de materiales desde inventario hacia producción.

El módulo gestiona objetos, estados, validaciones y trazabilidad del proceso. No administra existencias, ubicaciones físicas ni producción como sistemas completos.

Estructura prevista:

- `domain/`: conceptos y reglas del flujo.
- `application/`: casos de uso que coordinan el flujo.
- `ports/`: contratos para persistencia y servicios externos requeridos por los casos de uso.

No crear entidades o estados definitivos hasta modelar el flujo y acordar sus reglas.
