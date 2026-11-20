# Arquitectura

## Dirección inicial

FioRa parte de un **monolito modular en Python**, con PostgreSQL como persistencia principal y una instancia independiente por cliente.

La arquitectura debe mantener separadas las responsabilidades del núcleo, los flujos operativos, las capacidades auxiliares y los adaptadores de infraestructura.

## Límites importantes

- FioRa gestiona objetos operativos, estados, validaciones y trazabilidad.
- No es la fuente de verdad de existencias de inventario por defecto.
- El primer flujo de referencia es la transferencia de materiales desde inventario hacia producción.
- Importaciones, IA y persistencia son mecanismos que consumen capacidades del backend; no deben duplicar reglas del dominio.
- Las adaptaciones particulares deben permanecer encapsuladas y fuera del núcleo común.

## Dependencias entre módulos

- El núcleo no debe depender de frameworks web, proveedores de IA ni librerías específicas de Excel.
- Los módulos de flujo deben depender de contratos explícitos, no de tablas o estructuras internas de otros módulos.
- Los adaptadores implementan los contratos definidos por la aplicación/dominio.
- La composición concreta de dependencias se realizará en `bootstrap/`.

## Pendiente

El patrón exacto de capas, los límites finales de módulos, framework API, ORM, migraciones, frontend y proveedor IA se elegirán mediante decisiones registradas. No introducirlos por anticipado en el código.
