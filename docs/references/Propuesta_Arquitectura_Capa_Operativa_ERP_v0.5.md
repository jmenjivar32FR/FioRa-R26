# Propuesta de arquitectura y evolución del producto
## Capa de integración y gestión operativa entre ERP y documentación interna

**Versión:** 0.5 · **Estado:** Propuesta para discutir; las decisiones tecnológicas siguen abiertas.

**Actualización v0.5:** se ajusta el calendario a una dedicación de 6–10 horas semanales por integrante y se incorpora que ambos aprenderán las tecnologías y prácticas necesarias durante el desarrollo.

## 1. Visión del producto

El producto no nace como un ERP. Nace como una **capa intermedia de operación** que atiende procesos que quedan entre el ERP y la documentación interna de la empresa.

En muchas operaciones, el ERP conserva los registros formales, mientras que instrucciones, listas, cambios, excepciones, validaciones y acuerdos circulan en archivos o documentos separados. Esa transición puede depender de revisiones manuales y provocar información incompleta, duplicada o desactualizada.

La primera versión debe resolver **un proceso concreto y acotado**: recibir información desde las fuentes existentes, estructurarla, validar inconsistencias, facilitar su revisión y devolver el resultado al flujo de trabajo de la empresa. No debe intentar reemplazar el ERP ni duplicar todas sus funciones.

A futuro, si el producto demuestra utilidad y aparecen necesidades claras, podrá ampliarse hacia más procesos y eventualmente hacia módulos propios de un ERP. Esa posibilidad debe orientar la modularidad, pero no justificar construir infraestructura empresarial desde el primer día.

## 2. Principios de arquitectura

- **Empezar con un monolito modular:** una aplicación desplegable, con módulos y responsabilidades internas claras.
- **Mantener el dominio independiente:** las reglas del proceso no deben depender de Streamlit, React, Spring, FastAPI ni de un proveedor de IA.
- **Contratos explícitos:** los módulos y servicios se comunican mediante interfaces y esquemas definidos, no accediendo a las estructuras internas de otros módulos.
- **IA reemplazable:** OpenAI, Claude u otro modelo deben conectarse detrás de una interfaz común.
- **Datos bajo control del backend:** la IA propone interpretaciones o acciones; el backend valida permisos, reglas y transacciones.
- **Evolución por necesidad:** comenzar con un monolito modular y preparar límites claros para extraer posteriormente servicios de notificaciones e integración de correo. No introducir microservicios, colas distribuidas ni Kubernetes antes de que exista una necesidad real.

## 3. Propuestas tecnológicas

### Opción A — Python como backend completo

- **Backend/API:** Python + FastAPI.
- **Persistencia:** PostgreSQL + SQLAlchemy.
- **IA:** adaptadores Python para OpenAI, Claude u otros proveedores.
- **Procesamiento documental:** bibliotecas Python para Excel, PDF y otros formatos que realmente se necesiten.
- **Interfaz:** aplicación web independiente o interfaz ligera según el alcance.

**Ventajas:** desarrollo rápido; ecosistema amplio para IA y procesamiento documental; un solo lenguaje principal.

**Desventajas:** si el equipo quiere que Java/Spring sea la base de una futura plataforma empresarial, el núcleo inicial quedará en Python.

**Encaja cuando:** prima validar el primer proceso con la menor complejidad y el equipo se siente más productivo en Python.

### Opción B — Java como backend completo

- **Backend/API:** Java + Spring Boot.
- **Persistencia:** PostgreSQL + Spring Data JPA.
- **IA:** Spring AI o un cliente HTTP propio.
- **Procesamiento documental:** bibliotecas Java según los formatos requeridos.
- **Interfaz:** aplicación web que consume la API.

**Ventajas:** un solo backend; separación clara entre controladores, servicios y persistencia; continuidad si el producto evoluciona hacia módulos empresariales. Spring AI ofrece abstracciones para distintos proveedores y tool calling.

**Desventajas:** puede requerir más trabajo inicial si el equipo tiene menos experiencia con Java/Spring o con las bibliotecas documentales necesarias.

**Encaja cuando:** se prioriza Java/Spring desde el inicio y el equipo puede desarrollar con fluidez en él.

### Opción C — Híbrida: Java para el núcleo y Python para IA/documentos

- **Backend principal:** Java + Spring Boot.
- **Base de datos:** PostgreSQL; Java es el único dueño de las escrituras del dominio.
- **Servicio auxiliar:** Python para extracción, transformación documental u orquestación de IA cuando aporte una capacidad concreta.
- **Comunicación interna:** API REST con JSON y contratos versionados.
- **Proveedores:** OpenAI o Claude intercambiables mediante un adaptador.

**Responsabilidad de Java:** usuarios, permisos, reglas de negocio, transacciones, flujo operativo, auditoría y API pública.

**Responsabilidad de Python:** procesar documentos o preparar solicitudes de IA y devolver datos propuestos, estructurados y validados. No modifica directamente las tablas de negocio.

**Ventajas:** mantiene el núcleo transaccional en Java y aprovecha Python para tareas documentales o de IA especializadas.

**Desventajas:** dos procesos, dos entornos y una comunicación adicional que hay que probar y mantener. No conviene si Python solo envuelve una llamada HTTP trivial.

**Encaja cuando:** Python tiene una responsabilidad propia y medible, y el equipo quiere mantener el núcleo en Java.

## 4. Comparación rápida de las opciones

| Criterio | A. Python | B. Java | C. Híbrida |
|---|---|---|---|
| Rapidez inicial | Alta | Media | Media-baja |
| Un solo backend | Sí | Sí | No |
| Procesamiento documental/IA | Muy cómodo | Viable | Muy cómodo en Python |
| Continuidad con Java/Spring | Menor | Alta | Alta en el núcleo |
| Complejidad operativa | Baja | Baja | Media |
| Adecuada para crecer por módulos | Sí | Sí | Sí, con más componentes |

## 5. Comunicación entre componentes

### Interfaz y backend

Para una aplicación web, el navegador se comunica con el backend mediante **HTTPS + REST/JSON**. Los contratos de entrada y salida se documentan, idealmente con OpenAPI. La interfaz no debe contener reglas críticas del negocio.

### Backend y servicio Python (si se elige la opción híbrida)

Java envía una solicitud acotada a Python: texto o documento seleccionado, contexto mínimo e identificadores necesarios. Python devuelve una respuesta estructurada y validable. La comunicación será REST/JSON; no se comparte la base de datos ni se permiten escrituras de Python sobre las tablas del dominio.

### Backend y modelo de IA

El modelo recibe solo el contexto necesario y devuelve una interpretación estructurada o una solicitud de herramienta. La aplicación valida y ejecuta las herramientas. La IA no recibe credenciales de base de datos, no escribe SQL y no decide por sí sola si una operación está autorizada.

### Integración con ERP y documentos internos

Orden recomendado:
1. Importación/exportación de archivos (Excel/CSV y formatos confirmados).
2. API oficial del ERP, si existe y el cliente autoriza su uso.
3. Otras integraciones específicas cuando el proceso lo requiera.

Evitar escribir directamente en la base de datos del ERP. Si se necesita consultarla, hacerlo mediante una interfaz autorizada y preferentemente de solo lectura.

## 6. Flujo funcional propuesto

```text
ERP / archivos / documentos internos
              ↓
       Adaptadores de entrada
              ↓
  Datos originales + trazabilidad
              ↓
 Normalización y validaciones
              ↓
 Revisión humana de excepciones
              ↓
 Reglas y flujo operativo del backend
              ↓
 Registro de resultados y auditoría
              ↓
 Exportación o integración de regreso al ERP
```

La IA ayuda a interpretar, clasificar, extraer y proponer acciones. Las reglas del backend validan y ejecutan. Los casos dudosos se presentan a una persona.

## 7. Aplicación web frente a aplicación ejecutable

Son formas distintas de entregar la interfaz. Ninguna elimina por sí sola la necesidad de un backend, una base de datos, respaldos y actualizaciones.

| Aspecto | Aplicación web | Aplicación ejecutable de escritorio |
|---|---|---|
| Instalación | Se accede desde navegador; el servidor se instala una vez | Se instala en cada equipo o se distribuye un paquete por sistema operativo |
| Actualizaciones | Se actualiza el servidor y los usuarios reciben la nueva versión | Hay que distribuir e instalar actualizaciones en los equipos |
| Varios usuarios | Naturalmente comparte un backend y una base de datos central | También puede hacerlo, pero normalmente necesita un backend central aparte |
| Datos compartidos | Centralizados | Si cada ejecutable guarda datos localmente, se fragmentan; debe evitarse para operaciones compartidas |
| Acceso | Navegador dentro de la red o por internet con seguridad adecuada | Aplicación instalada; puede ofrecer integración más directa con el sistema operativo |
| Despliegue | Un servidor y clientes ligeros | Instaladores, compatibilidad por sistema operativo y ciclo de actualización |
| Uso sin conexión | Requiere diseño específico; no debe asumirse | Puede diseñarse para trabajar offline, pero sincronizar datos después aumenta la complejidad |

**Recomendación provisional:** aplicación web para la primera versión, especialmente si varias personas consultarán y actualizarán la misma información. Se puede ofrecer como PWA o acceso directo instalado en el escritorio para que parezca una aplicación, sin duplicar el núcleo. Una aplicación de escritorio puede evaluarse más adelante si existe una necesidad concreta de trabajo offline o integración local.

## 8. Docker, Docker Compose y Kubernetes

### Docker

Docker permite empaquetar la aplicación y sus dependencias en imágenes y contenedores. Esto hace más reproducibles el desarrollo, las pruebas y la instalación: el entorno que funciona en una computadora puede desplegarse de forma más consistente en otro servidor compatible.

Docker **no es un lenguaje, una base de datos ni un proveedor de hosting**. Tampoco garantiza por sí solo persistencia o respaldos: los datos de PostgreSQL y los documentos deben guardarse en volúmenes o almacenamiento persistente fuera del ciclo de vida del contenedor.

### Docker Compose

Compose permite definir varios servicios —por ejemplo, backend, frontend, PostgreSQL y un trabajador documental— en un archivo YAML y levantarlos juntos. Es adecuado para desarrollo y para desplegar un sistema pequeño en un solo servidor.

**Recomendación:** utilizar Docker y Docker Compose cuando tengamos más de un servicio que instalar o cuando necesitemos entregar el mismo entorno en distintas empresas. No es obligatorio introducirlos el primer día; primero podemos lograr que la aplicación arranque localmente y después crear los contenedores.

### Kubernetes

Kubernetes administra aplicaciones contenerizadas en un clúster: despliegue, escalado, recuperación de contenedores, configuración, redes y almacenamiento. Es útil cuando existen varios servidores, muchos servicios, necesidades de alta disponibilidad o escalado operativo significativo.

**No se recomienda para el MVP ni para una instalación inicial en un solo servidor.** Añade conceptos, infraestructura y tareas de administración que no resuelven un problema actual del producto. Se reconsideraría solo si el número de clientes, servicios o requisitos de disponibilidad lo justifican.

| Tecnología | Para qué sirve | Recomendación |
|---|---|---|
| Docker | Empaquetar y ejecutar cada componente en contenedores | Adoptarlo al estabilizar el primer entorno |
| Docker Compose | Coordinar varios contenedores en un servidor | Opción preferida para despliegues pequeños |
| Kubernetes | Orquestar contenedores en clústeres y múltiples nodos | Posponer hasta que exista necesidad real |

## 9. Recomendaciones para permanencia, mantenimiento y evolución

Estas decisiones ayudan a que el producto pueda mantenerse durante años y ampliarse sin tener que reescribirlo completo.

1. **Separar dominio de tecnología:** las entidades y reglas del proceso deben estar en módulos propios, no dentro de controladores, pantallas o prompts de IA.
2. **Monolito modular primero:** separar por dominios y responsabilidades. Extraer un servicio independiente solo cuando haya una razón medible: carga, despliegue autónomo, aislamiento o equipo responsable distinto.
3. **Base de datos con migraciones:** usar PostgreSQL y una herramienta de migraciones (por ejemplo, Flyway/Liquibase en Java o Alembic en Python). No modificar tablas manualmente en producción.
4. **Respaldos recuperables:** programar copias de la base de datos y de los archivos originales; conservar copias fuera del servidor principal y probar restauraciones periódicamente. Un respaldo no verificado no demuestra que podamos recuperar el sistema.
5. **Trazabilidad:** registrar actor, fecha, acción, origen, entidad afectada y resultado en operaciones importantes. No borrar silenciosamente movimientos ni documentos de origen.
6. **Contratos versionados:** definir APIs, esquemas de datos e importadores con versiones. Un cambio de formato no debe romper todos los clientes sin una migración controlada.
7. **Idempotencia y control de duplicados:** las importaciones y operaciones repetibles deben reconocer reintentos para evitar duplicar movimientos o registros.
8. **IA sustituible y acotada:** encapsular cada proveedor en un adaptador; versionar prompts y esquemas; validar toda respuesta; probar el proceso sin IA y contemplar indisponibilidad del proveedor.
9. **Configuración fuera del código:** credenciales, URLs, rutas y parámetros por entorno en variables de entorno o un gestor de secretos. Nunca subir secretos al repositorio.
10. **Pruebas y observabilidad:** pruebas unitarias e integración para reglas críticas; logs estructurados, identificadores de correlación y comprobaciones de salud para diagnosticar fallas.
11. **Documentación operativa:** incluir instalación, actualización, respaldo, restauración, creación de usuarios, configuración de integraciones y resolución de incidentes.
12. **Portabilidad:** evitar depender innecesariamente de servicios propietarios; mantener exportaciones estándar y una ruta documentada para mover la aplicación y los datos.

## 10. Evolución futura: microservicios de notificaciones y correo

El producto puede evolucionar más adelante para incorporar servicios independientes de notificaciones e integración con correo electrónico. Esta evolución es compatible con comenzar como monolito modular; **no requiere construir microservicios desde la primera versión**.

### Servicio de notificaciones

Responsable de convertir eventos del sistema en avisos a los usuarios o áreas correspondientes. Ejemplos de eventos futuros:

- Una fabricación requiere revisión o tiene información incompleta.
- Se reserva o entrega material.
- Se detecta una excepción durante una importación.
- Una fabricación alcanza un estado o hito definido.

Los canales podrían incluir notificaciones dentro de la aplicación y correo electrónico. Las reglas de destinatarios, preferencias, prioridades y frecuencia deben configurarse; no conviene enviar un correo por cada cambio menor de manera indiscriminada.

### Servicio de integración con correo

Responsable de conectar el producto con las cuentas o plataformas de correo que autorice cada empresa. En el futuro podría:

- Enviar avisos y reportes.
- Recibir correos de buzones definidos.
- Detectar y procesar adjuntos autorizados, como Excel o PDF.
- Relacionar mensajes con fabricaciones o procesos cuando exista información suficiente.
- Mantener registro del mensaje, origen, fecha, resultado de procesamiento y errores.

La conexión deberá utilizar mecanismos autorizados por el proveedor —por ejemplo, OAuth y APIs oficiales cuando estén disponibles— y guardar credenciales o tokens de forma segura. No se deben solicitar ni almacenar contraseñas personales de correo en texto plano.

### Comunicación futura entre servicios

Cuando se extraigan estos componentes, el backend principal publicará eventos de dominio con contratos versionados. Los servicios de notificaciones y correo consumirán los eventos y realizarán su trabajo de forma asíncrona.

Para evitar que una operación se guarde en PostgreSQL pero su evento se pierda, se recomienda evaluar el patrón **Transactional Outbox**: registrar el cambio de negocio y el evento pendiente dentro de la misma transacción; un proceso posterior entrega el evento al mecanismo de mensajería.

Una cola o broker —por ejemplo, RabbitMQ— solo se incorporará cuando existan consumidores asíncronos reales o necesidades de reintentos y desacoplamiento. Cada consumidor deberá tolerar mensajes duplicados (idempotencia), registrar fallos y contar con una política de reintentos. Los mensajes que agoten los reintentos deberán poder revisarse, no desaparecer silenciosamente.

### Secuencia de evolución recomendada

| Etapa | Arquitectura | Alcance |
|---|---|---|
| 1. Primer producto | Monolito modular | Proceso operativo principal, usuarios, datos, IA e interfaz |
| 2. Preparación | Monolito + eventos internos | Definir eventos, contratos, outbox y módulos de notificación/correo dentro del mismo despliegue |
| 3. Extracción | Servicios independientes | Separar notificaciones y/o correo cuando necesiten despliegue, escalado o operación autónomos |
| 4. Plataforma ampliada | Varios servicios e integraciones | Añadir conectores ERP, correo y otros canales según necesidades verificadas |

**Criterio para extraer un microservicio:** debe existir una razón concreta, como desplegarlo de forma independiente, aislar fallas, atender una carga distinta, proteger credenciales e integraciones, o permitir que otro equipo lo opere. Separar por moda añade costos de red, observabilidad, despliegue, seguridad y depuración.

Aunque se extraigan servicios, cada uno debe tener una responsabilidad y un contrato claros. No se recomienda que distintos servicios escriban directamente en las mismas tablas del dominio; el backend dueño de los datos debe exponer operaciones o publicar eventos. La propiedad de datos y la estrategia de consistencia entre servicios deberán diseñarse al realizar esa extracción.

## 11. Forma de desarrollo propuesta

1. **Mapear un solo proceso real:** identificar quién entrega la información, qué documentos intervienen, qué validaciones se hacen y cuál debe ser el resultado.
2. **Reunir ejemplos reales anonimizados:** archivos correctos, incompletos, duplicados y casos excepcionales.
3. **Definir un modelo de datos común:** representar los conceptos del proceso sin copiar ciegamente la estructura de cada Excel o ERP.
4. **Construir una rebanada vertical:** importar un caso, validarlo, resolver una excepción, registrar el resultado y exportarlo de principio a fin.
5. **Probar con usuarios del proceso:** medir errores, tiempo ahorrado y excepciones que siguen requiriendo intervención.
6. **Preparar despliegue reproducible:** configuración por entorno, migraciones, respaldos y, cuando aporte valor, Docker Compose.
7. **Ampliar solo después de validar:** incorporar otro proceso o integración cuando el primero sea útil y estable.

Cada módulo debe tener pruebas; las operaciones importantes deben ser transaccionales y auditables. Los formatos de integración se adaptan mediante conectores, no se mezclan con las reglas centrales.

## 12. Plan de tiempos, dedicación y aprendizaje

El proyecto se desarrollará gradualmente. Ambos integrantes dedicarán aproximadamente **6–10 horas por semana cada uno** (12–20 horas-persona combinadas). El tiempo de estudio, experimentación y aprendizaje técnico se considera dentro de esas horas; no se presupone que ambos dominen desde el inicio todas las tecnologías.

El objetivo no es avanzar a máxima velocidad, sino construir conocimiento y producto al mismo tiempo, evitando que la arquitectura dependa de decisiones que ninguno pueda mantener después.

### Estimación global

- **Dedicación semanal:** 6–10 horas por persona; 12–20 horas combinadas.
- **Primera demostración de extremo a extremo:** aproximadamente 6–10 semanas. Debe resolver un flujo acotado con datos de prueba y revisión humana.
- **Piloto funcional del primer proceso:** aproximadamente 4–7 meses, sujeto a la experiencia inicial, la complejidad documental y el acceso a usuarios que validen el proceso.
- **Esfuerzo conjunto de planificación:** alrededor de 220–340 horas-persona incluyendo implementación, aprendizaje aplicado, pruebas, integración y correcciones. Es una banda de trabajo inicial, no una promesa ni una fecha garantizada.
- **Evolución hacia más procesos, conectores ERP o microservicios:** se estimará por separado después de validar el primer piloto.

La diferencia frente a un equipo con experiencia completa se debe a que una parte del tiempo se utilizará para comprender las herramientas, practicar patrones, revisar código generado o propuesto, depurar y documentar decisiones. Si ambos avanzan más rápido, el calendario puede reducirse; si una tecnología o integración presenta dificultades, se ampliará sin eliminar pruebas esenciales.

### Distribución semanal orientativa por integrante

| Actividad | Horas por semana | Propósito |
|---|---:|---|
| Implementación de tareas asignadas | 3–4 h | Construir una funcionalidad pequeña y verificable |
| Aprendizaje aplicado | 1–2 h | Estudiar solo los conceptos necesarios para la tarea actual |
| Pruebas, depuración y revisión | 1–2 h | Probar casos normales, errores y cambios del otro integrante |
| Planificación e integración compartida | 1–2 h | Alinear contratos, revisar avances y resolver bloqueos |

La distribución suma 6–10 horas semanales por persona. La reunión compartida se contabiliza dentro de esas horas, no como tiempo adicional.

La distribución es flexible. En semanas de integración o depuración puede dedicarse más tiempo a pruebas; en semanas de aprendizaje de una tecnología nueva puede reducirse el volumen de funcionalidades.

### Fases y entregables

| Fase | Duración orientativa | Entregable verificable |
|---|---:|---|
| 0. Descubrimiento y alcance | 1–2 semanas | Proceso seleccionado, mapa del flujo, documentos, actores, excepciones y criterios de aceptación |
| 1. Fundamentos y aprendizaje inicial | 2–4 semanas | Repositorio, entorno local, base de datos, migraciones, configuración y primer endpoint/pantalla funcional |
| 2. Primera rebanada operativa | 3–5 semanas | Importar un documento, validar datos, resolver una excepción, persistir el resultado y exportarlo |
| 3. Interfaz e integración | 3–5 semanas, parcialmente en paralelo | Vistas web, formularios, tablas, estados de carga/error y conexión con la API |
| 4. Asistencia con IA | 3–5 semanas | Adaptador de proveedor, interpretación estructurada, herramientas controladas, confirmaciones y ambigüedades |
| 5. Pruebas y piloto | 3–5 semanas | Pruebas integrales, restauración verificada, documentación de instalación y validación con usuarios |

Las duraciones por fase pueden solaparse y no deben sumarse de manera mecánica. El rango global de 4–7 meses considera el trabajo paralelo, la dedicación parcial y el aprendizaje de ambos integrantes.

### Método de aprendizaje durante el desarrollo

1. **Aprender lo necesario para el siguiente entregable:** evitar estudiar frameworks completos o tecnologías que todavía no se utilizarán.
2. **Primero comprender el concepto, después implementarlo:** revisar brevemente el propósito, las alternativas y los riesgos antes de adoptar una biblioteca o patrón.
3. **No aceptar código que ninguno pueda mantener:** el código generado o asistido por herramientas debe revisarse, probarse y explicarse lo suficiente para que ambos puedan modificarlo.
4. **Registrar decisiones y aprendizajes:** documentar brevemente por qué se eligió una tecnología, cómo se ejecuta y qué convenciones sigue el proyecto.
5. **Mantener un pequeño margen para depuración:** cada funcionalidad incluye pruebas y correcciones; no se considera terminada al compilar.
6. **Reevaluar el ritmo cada cuatro semanas:** comparar horas previstas y reales, entregables completados y bloqueos. Ajustar alcance o calendario de manera explícita.

### Hitos de control

1. **Hito 1 — Proceso comprendido:** ambos pueden explicar entradas, salidas, reglas y excepciones.
2. **Hito 2 — Flujo sin IA:** el proceso principal funciona con formularios o acciones explícitas, sin depender del modelo.
3. **Hito 3 — Interfaz integrada:** un usuario puede completar el flujo desde la aplicación.
4. **Hito 4 — IA asistida:** el modelo interpreta solicitudes, pero el backend mantiene validación, permisos y ejecución.
5. **Hito 5 — Piloto:** el proceso se prueba con documentos representativos y usuarios reales, con recuperación ante fallos.

No se considerará completada una fase solamente porque el código esté escrito. Debe existir un resultado demostrable, probado y comprensible para ambos integrantes.

## 13. División de responsabilidades del equipo

El equipo inicial estará compuesto por dos integrantes. La división propuesta es por responsabilidad principal, no una separación rígida que impida colaborar.

### Integrante A — Arquitectura y backend (usuario)

**Responsable principal de:**

- Arquitectura general, límites de módulos y decisiones técnicas.
- Modelo de dominio, entidades, relaciones y reglas del proceso.
- Diseño de base de datos, migraciones y estrategia de persistencia.
- Backend y API: endpoints, servicios de aplicación, validaciones y transacciones.
- Autenticación, autorización, trazabilidad y protección de datos.
- Integraciones de backend con ERP, almacenamiento y proveedores de IA.
- Adaptador de IA y definición de herramientas/acciones disponibles para el modelo.
- Pruebas unitarias e integración del dominio y backend.
- Revisión técnica de cambios que afecten arquitectura, datos o contratos.

**Entregables principales:** diagramas y decisiones arquitectónicas, modelo de datos, OpenAPI, servicios backend, migraciones, pruebas y documentación técnica.

### Integrante B — Desarrollo full stack (amigo)

**Responsable principal de:**

- Diseño e implementación de la interfaz web.
- Navegación, páginas, formularios, tablas, dashboard y experiencia conversacional.
- Integración de la interfaz con la API y manejo de estados de carga, éxito y error.
- Validaciones de presentación y accesibilidad; las reglas de negocio permanecen en el backend.
- Diseño visual consistente y componentes reutilizables.
- Pruebas de interfaz y recorridos completos desde la perspectiva del usuario.
- Apoyo en empaquetado, configuración de entorno y despliegue junto con el responsable de backend.
- Cuando convenga, contribuciones full stack a endpoints o adaptadores, coordinadas mediante los contratos y revisión del responsable de backend.

**Entregables principales:** interfaz integrada, componentes visuales, navegación, vistas funcionales, pruebas de experiencia y guía breve de uso.

### Responsabilidades compartidas

- Entrevistar a usuarios y entender el proceso real.
- Priorizar el backlog y acordar qué queda dentro o fuera de cada entrega.
- Definir criterios de aceptación antes de desarrollar cada funcionalidad.
- Revisar cambios entre ambos y validar que la solución corresponda a la necesidad operativa.
- Ejecutar pruebas de integración y regresión.
- Revisar incidencias, preparar demostraciones y decidir cuándo una versión está lista.
- Documentar decisiones importantes para que el conocimiento no dependa de una sola persona.

### Acuerdos de colaboración

1. El integrante A mantiene la responsabilidad final sobre arquitectura, modelo de datos y contratos del backend; el integrante B mantiene la responsabilidad principal sobre interfaz y experiencia de usuario.
2. Antes de trabajar en paralelo, acuerdan el contrato de cada endpoint (ruta, método, parámetros, respuesta y errores). La especificación OpenAPI será la referencia compartida.
3. Cada cambio se trabaja en una rama de Git y se integra mediante pull request o revisión por el otro integrante. Evitar cambios directos y simultáneos sobre `main`.
4. Las reglas de negocio no se duplican en frontend. La interfaz puede validar para ayudar al usuario, pero el backend vuelve a validar antes de modificar datos.
5. Las decisiones que afecten el dominio, seguridad, persistencia o compatibilidad se documentan brevemente en un registro de decisiones arquitectónicas (ADR).
6. Si un integrante se ausenta, el repositorio, las instrucciones de ejecución y la documentación deben permitir que el otro pueda continuar.

### Ritmo de trabajo recomendado

- Reunión breve de planificación semanal: objetivos y entregables verificables.
- Revisión de avance al cierre de cada semana: qué funciona, qué falta y qué riesgos surgieron.
- Integración frecuente: evitar ramas largas y acumulación de cambios difíciles de combinar.
- Demostración de cada flujo terminado con datos de prueba representativos.
- Reservar capacidad para corregir errores; no planificar el 100 % del tiempo en funcionalidades nuevas.

## 14. Criterios para ajustar el calendario

El calendario se actualizará cada cuatro semanas o cuando haya un cambio significativo de alcance. Se tomarán en cuenta:
- Experiencia práctica de ambos con Java/Spring, Python, frontend, SQL y despliegue.
- Complejidad y variabilidad de los documentos que alimentan el primer proceso.
- Disponibilidad de un entorno de pruebas y de usuarios que validen el flujo.
- Requisitos de seguridad, privacidad, conectividad e integración con el ERP.

Con la dedicación acordada de 6–10 horas por persona cada semana, si las tareas reales requieren más tiempo del estimado se ampliará el calendario o se reducirá el alcance del siguiente entregable. No se eliminarán controles críticos de integridad, auditoría, seguridad o recuperación para conservar una fecha artificial.

## 15. Recomendación provisional de arquitectura

- Mantener un **monolito modular** para el primer producto.
- Construir una **aplicación web** con backend central, para que varios usuarios compartan datos y reglas.
- Elegir **Java + Spring Boot** si ambos desarrolladores quieren que Java sea el núcleo a largo plazo y dominan suficientemente el ecosistema; elegir **Python + FastAPI** si la velocidad de validación y el procesamiento documental pesan más.
- Elegir la opción híbrida únicamente si Python hará extracción o transformación documental sustancial. Java también puede comunicarse directamente con OpenAI o Claude; no se necesita Python solo para llamar a una API.
- Usar **PostgreSQL** como base de datos transaccional.
- Diseñar un adaptador de IA intercambiable y mantener las reglas críticas fuera del modelo.
- Adoptar Docker para reproducibilidad y Docker Compose para instalaciones pequeñas cuando el empaquetado aporte valor.
- **No utilizar Kubernetes en la primera etapa.**
- Diseñar límites de módulo y eventos internos que permitan extraer más adelante los servicios de notificaciones e integración de correo, sin convertirlos prematuramente en microservicios.

La propuesta preferida por simplicidad es un backend principal (Java o Python), PostgreSQL y una aplicación web. La decisión de lenguaje debe basarse en la experiencia real de ambos desarrolladores y en una prueba pequeña del primer flujo, no en la idea de que un lenguaje sea automáticamente más escalable.

## 16. Decisiones que siguen abiertas

- ¿Cuál es exactamente el primer proceso intermedio que resolverá el producto?
- ¿Qué fuentes/documentos intervienen (Excel, PDF, Word, correo, API del ERP u otros)?
- ¿El sistema solo prepara información o también devuelve cambios al ERP?
- ¿Qué nivel de revisión y aprobación humana requiere el proceso?
- ¿Qué experiencia tiene cada integrante con Python, Java/Spring, bases de datos y despliegue?
- ¿El producto se instalará en servidores del cliente, en nube propia o en una nube administrada?
- ¿Qué requisitos existen para acceso remoto, disponibilidad, privacidad y conservación de datos?
- ¿Qué proveedor de IA y qué datos pueden enviarse externamente, según las políticas del cliente?
- ¿La futura operación requerirá múltiples empresas aisladas, varios centros de trabajo o integraciones simultáneas?

## Referencias técnicas

- [Spring AI — API](https://docs.spring.io/spring-ai/reference/api/): abstracción de modelos y proveedores.
- [Spring AI — Tool Calling](https://docs.spring.io/spring-ai/reference/api/tools.html): el modelo solicita herramientas, pero la aplicación controla su ejecución.
- [Docker — Docker Compose](https://docs.docker.com/compose/): definición y ejecución de aplicaciones multi-contenedor.
- [Docker — Compose en producción](https://docs.docker.com/compose/how-tos/production/): configuración de Compose para entornos de producción y servidor único.
- [Kubernetes — Documentación oficial](https://kubernetes.io/docs/home/): orquestación y gestión de aplicaciones contenerizadas.
- [PostgreSQL — pg_dump](https://www.postgresql.org/docs/current/app-pgdump.html): exportación consistente de una base de datos; la política completa de respaldos debe contemplar también restauración y recuperación.
- [RabbitMQ — Documentation](https://www.rabbitmq.com/docs): referencia para evaluar un broker de mensajería si aparecen necesidades reales de procesamiento asíncrono.

---

**Estado del documento:** propuesta de arquitectura, despliegue y evolución inicial, incluyendo una posible separación futura de notificaciones e integración de correo. No sustituye todavía una especificación funcional ni constituye una selección definitiva de tecnologías.
