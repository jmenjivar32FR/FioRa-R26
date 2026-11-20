# FioRa — Documento Maestro de Producto y Arquitectura

**Versión:** 0.1  
**Fecha:** 19 de noviembre de 2026  
**Estado:** Documento maestro de decisiones recopiladas. Es una base de trabajo viva; las decisiones pendientes podrán resolverse y documentarse en versiones posteriores.  
**Ámbito:** definición de producto, alcance funcional, criterios arquitectónicos, despliegue, modelo comercial y marco legal preliminar.

> **Propósito del documento:** reunir en un solo lugar las decisiones expresadas para FioRa y distinguirlas de las propuestas y cuestiones todavía abiertas. Cuando una definición anterior haya sido corregida durante la conversación, prevalece la definición corregida que se registra aquí.

---

## 0. Convenciones de lectura

- **DECIDIDO:** decisión expresada y confirmada.
- **DIRECCIÓN DE DISEÑO:** orientación acordada o recomendada para guiar el diseño, pero que todavía requiere especificación.
- **PENDIENTE:** falta tomar una decisión o definir el detalle.
- **REQUIERE VALIDACIÓN:** necesita prueba técnica, validación con usuarios o revisión profesional.
- **NO INCLUIDO POR DEFECTO:** no forma parte de la capacidad estándar inicial, aunque podría incorporarse mediante configuración, adaptación o alcance contratado.

Este documento no convierte una sugerencia técnica en una decisión si el equipo todavía no la ha aceptado.

## 1. Resumen ejecutivo

FioRa es una herramienta especializada en el **control de estados, validaciones y trazabilidad de operaciones relacionadas con materiales y recursos físicos**. Complementa al ERP y a los sistemas de gestión existentes, atendiendo el espacio operativo entre lo que sucede físicamente en la empresa y lo que posteriormente queda registrado de manera formal en sus sistemas.

FioRa **no es un ERP, no es un sistema de inventario físico y no es un sistema de administración de producción**. Por defecto, no administra existencias ni verifica disponibilidad física. Su responsabilidad principal es recibir o registrar objetos operativos, identificarlos, mantener su estado, permitir cambios controlados y conservar la trazabilidad de esos cambios.

La información podrá ingresar desde archivos preparados por la empresa —por ejemplo, listas de producción en Excel adaptadas al formato de importación de FioRa— o mediante operaciones creadas dentro de FioRa. Los usuarios podrán modificar información manualmente o con asistencia de un chatbot con IA. Todas las modificaciones deberán pasar por el backend, que comprobará permisos, reglas y validaciones aplicables.

El producto tendrá un núcleo común y adaptaciones encapsuladas por cliente. Se desarrollará inicialmente en Python, utilizará PostgreSQL como base de datos principal y se distribuirá como una instancia independiente por cliente, instalable en infraestructura local o en un servidor alojado.

Comercialmente, la intención es vender una licencia de pago único que permita utilizar indefinidamente la versión adquirida. El hosting, infraestructura, consumo de IA y otros servicios de terceros no están incluidos. Las actualizaciones, adaptaciones y el soporte posterior a la garantía tendrán condiciones económicas separadas.

## 2. Visión, propósito y posicionamiento

### 2.1. Visión

FioRa busca atender una necesidad operativa que suele quedar desatendida en empresas medianas: procesos y controles intermedios que no justifican desarrollar o modificar un módulo empresarial completo, pero que requieren orden, seguimiento y trazabilidad.

En algunos entornos, las instrucciones, listas, confirmaciones, cambios y excepciones de operaciones relacionadas con materiales se gestionan mediante papel, hojas de cálculo y comunicaciones informales. Esto puede provocar pérdida de información, datos incompletos, registros desactualizados y dificultad para reconstruir quién hizo qué y cuándo.

FioRa convierte esos pasos intermedios en objetos operativos identificables y gestionables, con estados e historial.

### 2.2. Propósito

Conectar la ejecución física de operaciones relacionadas con materiales con su registro y seguimiento digital, proporcionando:

- Identificación de objetos operativos.
- Gestión controlada de estados.
- Validaciones previas a los cambios.
- Registro de responsables, fechas y modificaciones.
- Importación de información proveniente de archivos u otras fuentes.
- Modificación manual o asistida por IA.
- Preparación de información para el flujo formal de la empresa y, cuando se implemente, su retorno o integración con el ERP.

### 2.3. Posicionamiento

**FioRa complementa al ERP; no lo reemplaza ni replica su administración empresarial.** El ERP o sistema formal del cliente continúa siendo responsable de sus funciones empresariales y de los registros oficiales que le correspondan.

FioRa se especializa en materiales y recursos físicos. No pretende ser un motor genérico para cualquier proceso de una empresa.

### 2.4. Dominio funcional

El dominio de FioRa comprende operaciones relacionadas con materiales y recursos físicos, incluidas las que ocurren en contextos de inventario y producción. Estos contextos describen dónde ocurre la operación; no significan que FioRa deba administrar inventarios o producción como sistemas completos.

La transferencia de materiales desde inventario hacia producción es el primer flujo operativo de referencia para diseñar y validar el producto. FioRa controlará los objetos y estados asociados a ese flujo, no la totalidad de los procesos de almacén o fabricación.

## 3. Límites funcionales: qué es y qué no es FioRa

### 3.1. Responsabilidades propias de FioRa

- Recibir, importar o registrar información de objetos operativos relacionados con materiales.
- Identificar los objetos y conservar sus datos relevantes.
- Gestionar estados y transiciones permitidas.
- Permitir modificaciones manuales y cambios asistidos por IA.
- Validar que un cambio esté permitido y que el usuario tenga autorización.
- Registrar el historial de estados y modificaciones.
- Facilitar la consulta del estado actual y la trazabilidad.
- Adaptar formatos de entrada y salida mediante mecanismos específicos.

### 3.2. Funciones que no forman parte del producto base

Salvo que un cliente las solicite y se contraten como alcance adicional, FioRa no:

- Administra las existencias oficiales de inventario.
- Mantiene por defecto un kardex o saldo oficial de existencias.
- Determina por sí mismo la disponibilidad física de materiales.
- Sustituye al ERP como fuente de los registros empresariales formales.
- Planifica la producción ni reemplaza un sistema de ejecución o planificación de manufactura.
- Administra integralmente compras, ventas, contabilidad, almacenes o producción.
- Verifica por defecto que una cantidad solicitada exista físicamente.

Las validaciones empresariales —por ejemplo, existencia, disponibilidad, ubicación, lote, serie o autorización especial— podrán añadirse cuando el cliente las requiera. No deben presumirse como comportamiento estándar del núcleo.

### 3.3. Distinción fundamental

FioRa puede registrar que una persona confirmó una entrega, que un objeto cambió de estado o que se reportó una diferencia. Eso no significa que FioRa sea el sistema que contabiliza la salida de inventario o determina el saldo oficial. El registro formal de esas operaciones permanece en el sistema empresarial correspondiente, salvo un alcance de integración expresamente definido.

## 4. Primer flujo operativo de referencia

### 4.1. Flujo elegido

**Transferencia de materiales desde inventario hacia producción.**

Este flujo sirve como primer caso para diseñar el modelo de objetos, estados, validaciones y trazabilidad. No constituye un compromiso de desarrollar un módulo completo de inventario o producción.

### 4.2. Origen de las operaciones

**DECIDIDO:** FioRa admitirá ambos orígenes:

1. Información u operaciones originadas en un sistema externo, como el ERP, que podrán llegar por archivos u otro mecanismo de integración.
2. Operaciones creadas directamente dentro de FioRa por usuarios autorizados.

Las listas de producción en Excel son un ejemplo concreto de fuente de información. La empresa podrá adecuarlas al formato de importación de FioRa, subirlas y permitir que el sistema identifique los objetos resultantes.

### 4.3. Funcionamiento general esperado

1. La empresa prepara o genera una lista de producción u otro conjunto de datos.
2. La información se adecua al formato de importación definido para FioRa.
3. FioRa recibe el archivo, valida su estructura e identifica los objetos operativos.
4. Los objetos quedan disponibles para consulta y seguimiento.
5. Los usuarios modifican sus estados desde la interfaz o mediante el chatbot.
6. El backend valida permisos, reglas de transición e integridad antes de aceptar cada cambio.
7. FioRa conserva el estado resultante y su historial.
8. Cuando el alcance de la implementación lo contemple, la información podrá exportarse o integrarse al ERP.

### 4.4. Pendientes del flujo

- Definir qué se considera exactamente un objeto operativo.
- Definir sus identificadores y reglas para detectar duplicados.
- Definir la estructura mínima de una lista de importación.
- Definir cómo se mapean columnas de archivos de distintos clientes.
- Definir los estados y transiciones del primer flujo.
- Definir qué información puede corregirse y cómo se registran las correcciones.
- Definir las condiciones para considerar cerrado un objeto o una operación.
- Definir qué eventos se exportan o devuelven al ERP y por qué mecanismo.

No se deben inventar estados, entidades o reglas definitivas antes de modelar el proceso real.

## 5. Modelo de producto: núcleo común y adaptaciones

### 5.1. Núcleo común

**DECIDIDO:** FioRa tendrá un núcleo funcional común que conserva la identidad y el comportamiento general del producto.

Como mínimo, el núcleo deberá atender las capacidades comunes que se definan para:

- Gestión de objetos y estados.
- Validaciones de cambios.
- Usuarios y permisos.
- Auditoría e historial.
- Importación y procesamiento de información mediante contratos definidos.
- Comunicación controlada con asistentes de IA.

La lista final de módulos y responsabilidades internas sigue pendiente de diseño.

### 5.2. Adaptaciones por cliente

Las diferencias entre clientes se resolverán mediante configuración, adaptadores, mapeos, extensiones o componentes particulares encapsulados. Los elementos variables pueden incluir:

- Formatos y estructuras de archivos.
- Mapeos de columnas y campos.
- Integraciones con ERP o sistemas externos.
- Reglas particulares contratadas.
- Configuración del proveedor y comportamiento de IA.
- Parámetros de despliegue y persistencia que se requieran.

Una particularidad de un cliente no deberá convertirse automáticamente en una modificación del núcleo. Si una necesidad requiere código específico, deberá quedar aislado detrás de interfaces o contratos definidos.

### 5.3. Adaptabilidad sin bifurcar el producto

**DIRECCIÓN DE DISEÑO:** mantener una sola línea de desarrollo del núcleo y evitar versiones independientes del producto por cliente. Las adaptaciones deben poder instalarse o configurarse sin alterar innecesariamente el núcleo.

La forma exacta de empaquetar y versionar esas adaptaciones queda pendiente.

## 6. Decisiones tecnológicas y arquitectónicas

### 6.1. Backend

**DECIDIDO:** Python será el lenguaje del backend de la primera versión.

La elección responde al interés de aprovechar el ecosistema Python para procesamiento de datos, automatización, tratamiento de archivos e integración con proveedores de IA.

El framework web/API todavía deberá seleccionarse. FastAPI fue una opción contemplada en la propuesta v0.5, pero su adopción debe confirmarse en el diseño técnico.

### 6.2. Persistencia

**DECIDIDO:** PostgreSQL será la base de datos principal.

PostgreSQL conservará los datos propios de FioRa, incluidos los objetos operativos, estados, permisos, usuarios y registros de auditoría, según el modelo de datos que se diseñe.

**PENDIENTE:** definir si existe algún caso concreto que justifique una base de datos NoSQL. No se incorporará NoSQL por el solo hecho de utilizar IA, JSON o procesar documentos. Primero se evaluará si PostgreSQL cubre adecuadamente las necesidades.

### 6.3. Arquitectura de aplicación

**DIRECCIÓN DE DISEÑO:** comenzar con un monolito modular, con responsabilidades internas claras y contratos explícitos entre módulos.

La aplicación será un backend principal desplegable como una unidad, aunque internamente esté separada en módulos. La separación lógica no implica crear microservicios.

Se evitará introducir microservicios, brokers distribuidos o Kubernetes en la primera versión sin una necesidad demostrable.

### 6.4. Separación de responsabilidades

La arquitectura deberá separar, como mínimo, las responsabilidades de:

- Reglas y comportamiento del dominio.
- Casos de uso y operaciones de aplicación.
- Acceso a PostgreSQL y persistencia.
- API y entrada/salida.
- Importación, exportación e integración.
- Proveedores de IA.
- Configuración específica de cada instalación.

La organización concreta —por capas, puertos y adaptadores u otra variante— permanece pendiente de diseño y justificación.

### 6.5. Comunicación con IA

La IA se integrará mediante un componente intercambiable o adaptador para evitar que el dominio dependa directamente de un proveedor.

El modelo podrá interpretar solicitudes y proponer o solicitar operaciones. No tendrá acceso directo a credenciales de base de datos ni permiso para ejecutar SQL libremente.

El backend será responsable de:

1. Interpretar la operación solicitada por el asistente.
2. Comprobar que la operación existe y está permitida.
3. Validar la identidad, permisos y reglas aplicables.
4. Solicitar confirmación humana cuando corresponda.
5. Ejecutar la modificación mediante los casos de uso del sistema.
6. Registrar el resultado en la auditoría.

### 6.6. Frontend e interacción

La interfaz web es la dirección de entrega contemplada en la propuesta inicial. El chatbot será una ayuda importante dentro de FioRa, pero no será la interfaz principal ni la única forma de trabajar.

Los usuarios podrán modificar los datos manualmente y también solicitar cambios mediante conversación. Ambas vías deberán utilizar las mismas reglas del backend.

El diseño visual definido como orientación es industrial, sobrio, compacto y funcional. La tecnología concreta del frontend y el detalle de sus pantallas deberán documentarse en su propia decisión técnica.

### 6.7. Contenedores y despliegue técnico

Docker y Docker Compose podrán utilizarse para reproducir y distribuir el entorno de cada instalación. Su configuración y momento de adopción se definirán al preparar el empaquetado.

Kubernetes no se considera necesario para la primera versión ni para una instalación inicial en un solo servidor.

## 7. Inteligencia artificial y operación asistida

### 7.1. Papel del chatbot

**DECIDIDO:** el chatbot será un asistente complementario de operación, no la herramienta principal ni un sistema separado de gestión.

Podrá ayudar a:

- Consultar información de objetos operativos.
- Interpretar solicitudes en lenguaje natural.
- Proponer cambios de estado o datos.
- Solicitar información faltante.
- Ejecutar cambios mediante operaciones autorizadas del backend.

### 7.2. Modificaciones de datos

El chatbot podrá provocar cambios en la base de datos únicamente a través de las operaciones del backend y después de las validaciones que correspondan. No se permitirá que el modelo escriba directamente en las tablas ni que omita controles de autorización.

Las modificaciones manuales y las solicitadas por IA deberán producir registros de auditoría equivalentes, incluyendo el origen del cambio.

### 7.3. Aspectos por definir

- Proveedor o proveedores de IA que se admitirán inicialmente.
- Modelo de configuración de instrucciones y contexto por cliente.
- Catálogo de operaciones o herramientas expuestas al asistente.
- Acciones que exigen confirmación humana.
- Tratamiento de ambigüedad, errores y respuestas incompletas.
- Gestión de límites de consumo y credenciales del proveedor.
- Política de conservación de conversaciones y datos enviados al modelo.

## 8. Usuarios, permisos y seguridad

### 8.1. Modelo general

**DECIDIDO:** FioRa contará con login y un sistema de seguridad básico, con un gestor de permisos almacenado en la base de datos y validaciones en el backend antes de permitir cambios.

El nivel de complejidad de los permisos dependerá de las necesidades de cada empresa. No se busca construir inicialmente un sistema de autorización excesivamente complejo.

### 8.2. Principios mínimos

- Autenticar al usuario antes de permitir acceso a funciones protegidas.
- Comprobar permisos en el backend, no confiar únicamente en la interfaz.
- Aplicar las mismas autorizaciones a las operaciones manuales y a las solicitadas por IA.
- Registrar quién ejecutó cada modificación.
- Mantener credenciales y secretos fuera del código fuente y de los paquetes públicos.
- Proteger contraseñas mediante mecanismos adecuados de almacenamiento seguro; nunca texto plano.

### 8.3. Pendientes

- Definir perfiles o roles iniciales.
- Determinar si habrá permisos individuales además de permisos por rol.
- Definir alcance por planta, área, centro de trabajo o almacén, si el cliente lo requiere.
- Elegir mecanismo de autenticación y gestión de sesiones.
- Definir recuperación de cuentas, políticas de contraseña y cierre por inactividad.
- Definir qué registros de auditoría podrán consultar los usuarios y quién podrá administrar permisos.

## 9. Auditoría y trazabilidad

**DECIDIDO:** FioRa deberá conservar un historial de cambios asociado a los usuarios y operaciones.

La finalidad es poder reconstruir qué ocurrió con un objeto operativo y cómo cambió su estado.

Como base de diseño, la auditoría deberá evaluar el registro de:

- Identificador del objeto afectado.
- Tipo de operación o modificación.
- Usuario o actor que originó la acción.
- Fecha y hora.
- Estado o valor anterior y nuevo, cuando corresponda.
- Origen del cambio (interfaz, chatbot, importación, integración o automatización).
- Resultado de la validación y de la operación.
- Motivo o comentario, cuando se requiera.

El esquema final y la política de retención quedan pendientes. La auditoría no debe confundirse con un registro oficial de movimientos de inventario del ERP.

## 10. Importación, exportación e integración

### 10.1. Producto general

**DECIDIDO:** FioRa se desarrollará como un producto general, no alrededor de un ERP específico conocido de antemano.

No se diseñará el núcleo suponiendo un proveedor ERP particular. Las integraciones particulares se atenderán mediante adaptadores y contratos.

### 10.2. Archivos

Las listas de producción en Excel constituyen un caso de entrada relevante. La empresa adecuará su información al formato de importación que FioRa defina. El proceso deberá permitir recibir el archivo, validar su estructura, identificar objetos y reportar errores de importación.

La estructura final de plantilla, mapeos y tratamiento de duplicados está pendiente.

### 10.3. Otras fuentes

La arquitectura deberá poder admitir otros mecanismos —por ejemplo, API, CSV u otros formatos— si el alcance de un cliente lo requiere. No se asume que todos se implementarán en la primera versión.

### 10.4. Relación con el ERP

FioRa podrá recibir información originada en el ERP y también crear operaciones dentro de su propia interfaz. La forma de devolver o sincronizar datos al ERP dependerá de las capacidades y acuerdos de cada implementación.

No se permitirá escribir directamente en la base de datos del ERP sin autorización y diseño específico. Se preferirán interfaces oficiales o intercambios de archivos acordados.

## 11. Instalación, despliegue y operación por cliente

### 11.1. Instancia independiente

**DECIDIDO:** cada cliente tendrá su propia instancia de FioRa, con datos, configuración y credenciales independientes. No se plantea inicialmente una instalación SaaS multiempresa compartida.

La separación por cliente permite mantener instancias distintas sin mantener bifurcaciones del núcleo del producto.

### 11.2. Modalidades

Cada instancia podrá instalarse:

- En un servidor local o infraestructura administrada por el cliente.
- En un servidor alojado o servicio de hosting contratado para el cliente.

La modalidad concreta dependerá de las condiciones y necesidades de la empresa.

### 11.3. Responsabilidad de costos externos

La licencia de FioRa no incluirá automáticamente:

- Hosting, VPS, nube o servidores.
- Hardware, mobiliario o infraestructura física.
- Electricidad, conectividad o redes.
- Consumo de tokens, llamadas o créditos de IA.
- Servicios externos de correo, almacenamiento, APIs o mensajería.

Estos costos serán independientes del pago único de la licencia, salvo acuerdo comercial expreso.

### 11.4. Sucursales

**DECIDIDO EN PRINCIPIO:** el alcance para una o varias sucursales dependerá de la modalidad de implementación (local o alojada).

**PENDIENTE:** documentar la regla exacta de licenciamiento. Una instancia técnica no determina automáticamente el número de sucursales autorizadas; la propuesta comercial deberá precisar ubicaciones, usuarios y alcance incluidos.

### 11.5. Actualización y recuperación

Cada instalación necesitará una estrategia de versionado, actualización, migraciones, respaldo y restauración. Los procedimientos concretos todavía no están definidos.

## 12. Desarrollo y organización del equipo

### 12.1. Equipo

El equipo inicial está compuesto por dos integrantes.

La división principal de responsabilidades establecida en la propuesta v0.5 es:

- **Integrante A — Arquitectura y backend:** arquitectura, modelo de dominio, base de datos, API, reglas, seguridad, integraciones y pruebas del backend.
- **Integrante B — Desarrollo full stack con foco en frontend:** interfaz, navegación, formularios, tablas, integración con API, experiencia de usuario y pruebas de interfaz.
- **Responsabilidades compartidas:** comprensión del proceso, priorización, criterios de aceptación, revisión de cambios, pruebas integrales y documentación.

La división es de responsabilidad principal, no una prohibición de colaborar en otras áreas.

### 12.2. Dedicación y aprendizaje

La propuesta de trabajo vigente contempla aproximadamente 6–10 horas semanales por integrante (12–20 horas-persona combinadas). El aprendizaje técnico, experimentación, depuración y revisión de código forman parte de esas horas.

El objetivo es construir producto y conocimiento simultáneamente. No se aceptará código que el equipo no pueda comprender, revisar y mantener.

### 12.3. Forma de trabajo

La propuesta inicial contempla:

- Git como control de versiones.
- Trabajo mediante ramas y revisión antes de integrar cambios a la rama principal.
- Contratos explícitos entre frontend y backend.
- Documentación de decisiones que afecten dominio, seguridad, datos o compatibilidad.
- Entregables demostrables y probados, no solo código escrito.
- Revisión periódica del avance y ajuste realista de alcance.

El flujo exacto de ramas, revisiones, pruebas y liberaciones debe formalizarse.

## 13. Modelo comercial y licenciamiento

Esta sección registra las decisiones comerciales expresadas. Las cláusulas concretas deberán formalizarse en contratos revisados profesionalmente.

### 13.1. Licencia de pago único

**DECIDIDO:** FioRa se comercializará mediante un pago único que concede al cliente el derecho de utilizar indefinidamente la versión adquirida, dentro del alcance de su licencia.

No habrá una suscripción obligatoria por el simple uso de la versión licenciada.

### 13.2. Costos no incluidos

El pago único no incluye el consumo de tokens o llamadas de IA, el precio del hosting, el mobiliario o hardware, ni otros servicios externos necesarios para operar la instalación.

### 13.3. Actualizaciones

**DECIDIDO:** las actualizaciones futuras no estarán incluidas automáticamente en el pago inicial. Se prevé una tarifa base para actualizaciones generales y un cargo adicional cuando la especialización de la empresa o sus adaptaciones requieran trabajo extra.

La tarifa, el alcance y la compatibilidad de cada actualización se definirán comercialmente.

### 13.4. Garantía y soporte

La instalación tendrá un periodo de garantía inicial cuyo alcance y duración están pendientes de definición.

Una vez terminado ese periodo, cada instancia de soporte necesario se manejará mediante una contratación y tarifa separadas. El soporte no será una obligación gratuita y permanente incluida en la licencia.

### 13.5. Titularidad de datos y producto

- Los datos del cliente seguirán siendo del cliente y deberán poder exportarse.
- La licencia no transfiere al cliente la propiedad de FioRa ni de su núcleo.
- El producto se entregará sin el código fuente del núcleo, mediante ejecutables y herramientas de instalación.
- Se intentará implementar barreras razonables contra copia y análisis no autorizado, sin prometer que la ingeniería inversa sea técnicamente imposible.

### 13.6. Adaptaciones particulares

**DECIDIDO:** las adaptaciones particulares contratadas para una empresa serán propiedad del cliente.

**PENDIENTE CRÍTICO:** aclarar si el cliente recibirá el código fuente de sus adaptaciones y cómo se distinguirá ese código del núcleo preexistente de FioRa. La propiedad de una adaptación y la entrega de su código son cuestiones distintas. El contrato y el esquema técnico deberán resolver esta relación antes de vender adaptaciones.

## 14. Marco legal y comercial: materias por formalizar

El documento separado **“FioRa — Modelo comercial y marco legal preliminar v0.1”** contiene el desarrollo de las materias jurídicas y comerciales identificadas. Se mantiene como documento complementario y se considera parte del conjunto documental del proyecto.

Los principales puntos que requieren formalización son:

1. Titular legal de FioRa y derechos de los integrantes y colaboradores.
2. Búsqueda y eventual registro de la marca FioRa.
3. Contrato marco de licencia, alcance por instalación, usuarios y sucursales.
4. Propiedad del núcleo y régimen de adaptaciones del cliente.
5. Licencias de bibliotecas y componentes de terceros.
6. Titularidad, exportación, devolución y eliminación de datos.
7. Avisos de privacidad y acuerdos de tratamiento de datos personales, cuando apliquen.
8. Condiciones para utilizar proveedores de IA y transferirles información.
9. Confidencialidad y protección de información empresarial.
10. Garantía, soporte, mantenimiento, actualizaciones y límites de responsabilidad.
11. Facturación, impuestos y estructura de comercialización.
12. Criterios de aceptación de instalación y adaptación.

**Aviso:** este documento maestro no es un contrato ni un dictamen jurídico. Antes de comercializar FioRa se requiere revisión de un abogado mexicano en propiedad intelectual, software y protección de datos, además de revisión fiscal por un contador. La legislación y su vigencia deberán verificarse nuevamente antes de usar documentos contractuales.

## 15. Principios que deben orientar las decisiones futuras

1. **FioRa complementa al ERP:** no duplica sus responsabilidades empresariales.
2. **El dominio es especializado:** materiales y recursos físicos, no cualquier proceso de negocio.
3. **El foco es el estado y la trazabilidad:** no la administración de inventario físico.
4. **Validar cambios, no inventarios por defecto:** las comprobaciones de existencias o disponibilidad solo se incorporan si el cliente las solicita.
5. **Un núcleo común:** las particularidades se encapsulan en adaptaciones y configuraciones.
6. **Backend como autoridad:** interfaz e IA solicitan operaciones; el backend valida y ejecuta.
7. **Auditoría de cambios:** las modificaciones deben poder atribuirse a un actor y reconstruirse.
8. **PostgreSQL como persistencia principal:** NoSQL solo si aparece una necesidad concreta.
9. **Instancia independiente por cliente:** no introducir multiempresa compartido en la primera versión.
10. **Complejidad proporcional:** no introducir microservicios o infraestructura avanzada sin una necesidad medible.
11. **Producto mantenible:** cada adaptación debe evitar romper el núcleo o crear una bifurcación innecesaria.
12. **Entrega comprensible y recuperable:** instalación, actualización, exportación y respaldo deben contar con procedimientos documentados.

## 16. Decisiones pendientes consolidadas

Esta lista evita volver a preguntar asuntos ya decididos. Incluye únicamente áreas que todavía requieren definición técnica, funcional, comercial o legal.

### 16.1. Dominio y flujo

- [ ] Definición formal de objeto operativo.
- [ ] Identificadores, relaciones y reglas de unicidad.
- [ ] Estados y transiciones del primer flujo.
- [ ] Datos obligatorios y opcionales por objeto.
- [ ] Reglas de corrección, cancelación, cierre y reapertura.
- [ ] Criterios para detectar duplicados durante la importación.

### 16.2. Importación y exportación

- [ ] Plantilla inicial de Excel y formato de importación.
- [ ] Mapeo de columnas y campos por cliente.
- [ ] Reporte de errores y tratamiento de filas rechazadas.
- [ ] Política para reimportaciones y cambios en archivos ya procesados.
- [ ] Formatos de exportación.
- [ ] Primer mecanismo de intercambio con ERP, si entra en el MVP.

### 16.3. Arquitectura de aplicación

- [ ] Framework de API/backend en Python.
- [ ] Organización concreta por capas, módulos, puertos o adaptadores.
- [ ] ORM o estrategia de acceso a PostgreSQL.
- [ ] Herramienta de migraciones.
- [ ] Fronteras y contratos internos entre componentes.
- [ ] Mecanismo de configuración por instancia y por cliente.
- [ ] Estrategia de instalación de adaptaciones particulares.

### 16.4. IA

- [ ] Proveedor y modelos iniciales.
- [ ] Interfaz común para cambiar de proveedor.
- [ ] Herramientas/operaciones autorizadas para el chatbot.
- [ ] Confirmaciones humanas requeridas.
- [ ] Configuración y versionado de instrucciones.
- [ ] Política de privacidad, conservación y minimización de datos.

### 16.5. Seguridad y auditoría

- [ ] Esquema de roles y permisos.
- [ ] Autenticación y sesiones.
- [ ] Administración y recuperación de cuentas.
- [ ] Alcance de permisos por instalación, área o ubicación, si se requiere.
- [ ] Estructura y retención de auditoría.
- [ ] Política de secretos y credenciales.

### 16.6. Frontend y despliegue

- [ ] Framework y herramientas frontend.
- [ ] Contrato de comunicación frontend/API.
- [ ] Requisitos de acceso local y remoto.
- [ ] Empaquetado y procedimiento de instalación.
- [ ] Uso de Docker/Compose y configuración final del servidor.
- [ ] Actualización de versiones y migraciones.
- [ ] Respaldo, restauración y pruebas de recuperación.
- [ ] Requisitos mínimos de infraestructura y concurrencia.

### 16.7. Pruebas y liberación

- [ ] Estrategia de pruebas unitarias, integración y extremo a extremo.
- [ ] Datos de prueba representativos y casos excepcionales.
- [ ] Criterios de aceptación del primer flujo.
- [ ] Versionado del producto y de adaptaciones.
- [ ] Procedimiento para publicar y distribuir una versión.

### 16.8. Comercial y legal

- [ ] Titular legal de la marca y del software.
- [ ] Alcance exacto de licencias por instalación, ubicación y sucursales.
- [ ] Duración y alcance de la garantía.
- [ ] Tarifas de actualización y soporte.
- [ ] Tratamiento de código fuente de adaptaciones propiedad del cliente.
- [ ] Contratos, privacidad, propiedad intelectual y revisión fiscal.

## 17. Registro de correcciones de criterio

Este apartado conserva correcciones relevantes para evitar que vuelvan a introducirse interpretaciones descartadas.

- FioRa no es un ERP ni un sustituto del ERP.
- FioRa no es un sistema de inventario físico.
- FioRa no administra existencias ni valida disponibilidad por defecto.
- Inventario y producción son contextos operativos relacionados, no sistemas empresariales completos que FioRa deba replicar.
- El primer flujo de referencia es la transferencia de materiales de inventario a producción, entendida como gestión de objetos y estados, no como administración integral del almacén o de la producción.
- El chatbot no es la interfaz principal; es un asistente complementario.
- La IA puede solicitar cambios, pero no tiene acceso directo a la base de datos ni puede saltarse validaciones del backend.
- FioRa es un producto especializado en materiales y recursos físicos, no una plataforma genérica configurable para cualquier proceso empresarial.
- Las personalizaciones deben encapsularse; no deben producir una bifurcación permanente del núcleo.

## 18. Documentos de referencia integrados

Este documento maestro consolida decisiones de los siguientes materiales y conversaciones:

1. **Propuesta_Arquitectura_Capa_Operativa_ERP_v0.5(1).md** — propuesta inicial de arquitectura, desarrollo, despliegue, organización del equipo y evolución. Sus alternativas tecnológicas anteriores deben leerse a la luz de las decisiones posteriores registradas en este documento.
2. **FioRa_Modelo_Comercial_y_Marco_Legal_Preliminar_v0.1.md** — desarrollo de licenciamiento, propiedad intelectual, datos, privacidad, garantía, soporte, actualizaciones y formalización legal.
3. **Decisiones posteriores del usuario** — correcciones del dominio funcional, selección de Python y PostgreSQL, modelo de instancia independiente, gestión de estados, uso auxiliar del chatbot y límites de la primera implementación.

Cuando exista contradicción entre una propuesta anterior y una decisión posterior confirmada, prevalece la decisión posterior reflejada en este documento maestro.

---

## 19. Control de versiones

| Versión | Fecha | Cambio |
|---|---|---|
| 0.1 | 19-11-2026 | Primera consolidación maestra de visión, alcance, decisiones tecnológicas, criterios de arquitectura, despliegue, operación comercial y pendientes. |

**Estado actual:** documento maestro inicial. No constituye todavía una especificación técnica detallada, un modelo de datos definitivo ni un contrato comercial. Las próximas versiones deberán actualizar las decisiones y cerrar los pendientes con trazabilidad.
