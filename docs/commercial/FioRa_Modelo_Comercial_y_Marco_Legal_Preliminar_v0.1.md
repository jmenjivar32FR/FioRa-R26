# FioRa — Modelo comercial y marco legal preliminar

**Versión:** 0.1  
**Fecha de elaboración:** 19 de noviembre de 2026  
**Estado:** Documento de trabajo. Las decisiones indicadas como pendientes podrán modificarse en versiones posteriores.  
**Ámbito de referencia:** México. La legislación aplicable deberá revisarse nuevamente antes de comercializar o firmar contratos.

> **Aviso importante:** este documento registra decisiones de producto y propone una lista de aspectos jurídicos y comerciales que deberán formalizarse. No es un contrato, dictamen jurídico ni garantía de cumplimiento normativo. Antes de vender FioRa, un abogado mexicano con experiencia en software y propiedad intelectual deberá revisar y convertir las cláusulas aplicables en documentos contractuales. También deberá intervenir un contador para el tratamiento fiscal.

---

## 1. Objetivo del documento

Establecer una primera base para el modelo comercial, el licenciamiento, la propiedad intelectual, la distribución, la protección de datos, las garantías y los servicios relacionados con FioRa.

Este documento busca evitar que las decisiones técnicas del producto contradigan el modelo comercial. También identifica las cuestiones que deberán quedar resueltas antes de entregar una instalación a un cliente.

### Convenciones de estado

- **DECIDIDO:** criterio expresado y aceptado para el modelo inicial.
- **PENDIENTE:** falta definir el alcance o la regla concreta.
- **REQUIERE REVISIÓN LEGAL:** necesita validación y redacción por un profesional.
- **REQUIERE REVISIÓN FISCAL:** necesita validación de un contador o especialista fiscal.
- **RECOMENDACIÓN:** propuesta para discusión; todavía no constituye una decisión.

## 2. Naturaleza del producto

**DECIDIDO**

FioRa es una herramienta de gestión y control operativo intermedio. No pretende sustituir un ERP ni convertirse en un sistema de gestión empresarial generalista.

El producto busca controlar y documentar actividades que ocurren entre la ejecución física de un proceso y su registro en los sistemas formales de la empresa. Su finalidad es reducir la dependencia de documentos físicos, hojas de cálculo y validaciones informales que pueden provocar pérdida de información, inconsistencias o falta de trazabilidad.

FioRa tendrá un núcleo funcional común y adaptaciones específicas por cliente. Cada instalación conservará el funcionamiento general del producto, mientras que las particularidades de integración, persistencia y configuración de IA se atenderán mediante componentes o configuraciones encapsulados.

## 3. Modelo de comercialización

### 3.1 Licencia de pago único

**DECIDIDO**

- FioRa se comercializará mediante un pago inicial único.
- La licencia permitirá utilizar indefinidamente la versión adquirida, dentro del alcance y condiciones que establezca el contrato.
- No habrá una suscripción obligatoria por el simple uso de la versión licenciada.
- El pago inicial no incluirá automáticamente servicios externos ni costos operativos recurrentes.

**PENDIENTE**

- Definir si la licencia es exclusiva para una razón social, grupo empresarial, instalación, servidor, centro de trabajo o combinación de estos elementos.
- Definir si puede transferirse a otra entidad, venderse junto con la empresa o sublicenciarse a filiales y terceros.
- Definir el número de usuarios, equipos, ubicaciones o sucursales incluidos, cuando corresponda.
- Definir consecuencias y procedimiento ante incumplimiento material de la licencia.
- Definir el mecanismo de activación, recuperación o sustitución de una instalación o servidor.

### 3.2 Costos excluidos del pago único

**DECIDIDO**

El pago de la licencia no cubrirá, salvo que una propuesta comercial indique expresamente lo contrario:

- Consumo de tokens, llamadas o créditos de modelos de inteligencia artificial.
- Suscripciones o consumo de servicios de IA de terceros.
- Hosting, VPS, nube, dominios, certificados o conectividad.
- Servidores, computadoras, almacenamiento, redes, energía u otro mobiliario o infraestructura física.
- Servicios de correo, mensajería, almacenamiento externo, APIs o integraciones de terceros que generen cargos.
- Servicios recurrentes de terceros requeridos por la operación del cliente.

**RECOMENDACIÓN**

Cuando sea posible, las cuentas de hosting, IA y servicios externos deberían estar a nombre del cliente y ser pagadas directamente por este. Si FioRa contrata o administra alguna cuenta en nombre del cliente, el contrato deberá establecer titularidad, facturación, acceso, límites de consumo, renovación y terminación.

### 3.3 Modalidades de despliegue

**DECIDIDO**

FioRa podrá instalarse en:

1. **Infraestructura local (on-premises):** servidor o equipo administrado por el cliente, normalmente dentro de sus instalaciones o red.
2. **Servidor alojado (hosting o nube):** servidor contratado con un proveedor externo y asignado a la instalación del cliente.

El objetivo es mantener una instalación independiente por cliente, con sus propios datos y parámetros.

**PENDIENTE**

- Definir el alcance de licencia para cada modalidad.
- Definir cómo se contabilizan sucursales, centros de trabajo y accesos remotos.
- Definir quién administra el servidor, el sistema operativo, la red, las actualizaciones de infraestructura y los respaldos.
- Definir si FioRa ofrecerá hosting administrado opcional o si el alojamiento siempre será contratado por el cliente.

**Nota:** una sola instalación técnica no determina por sí sola cuántas sucursales quedan autorizadas. Esa regla debe expresarse en la licencia y en la propuesta comercial.

## 4. Licencia de uso y distribución

### 4.1 Alcance de la licencia

**DECIDIDO**

La compra otorga un derecho de uso indefinido de la versión licenciada; no transmite por sí misma la propiedad del producto FioRa ni de su código fuente.

**PENDIENTE / REQUIERE REVISIÓN LEGAL**

El contrato deberá especificar, al menos:

- Identidad del licenciante y del cliente licenciatario.
- Producto, edición, versión y módulos incluidos.
- Número de instalaciones permitidas y ubicación de despliegue.
- Usuarios, sucursales y centros de trabajo autorizados.
- Usos permitidos y actividades prohibidas.
- Reglas de copia de respaldo y recuperación.
- Transferencia, cesión, sublicencia y uso por empresas relacionadas.
- Tratamiento de versiones anteriores cuando se adquieran actualizaciones.
- Supuestos de incumplimiento y remedios contractuales.

No se presume que el cliente adquiera exclusividad, titularidad del producto o derecho a distribuir copias a terceros.

### 4.2 Entrega del software

**DECIDIDO**

La distribución ordinaria se realizará mediante software ejecutable y herramientas de instalación. No se entregará el código fuente del núcleo de FioRa.

La entrega deberá documentar los componentes recibidos, versión, requisitos técnicos, procedimiento de instalación, parámetros de configuración que correspondan y mecanismo de recuperación.

**PENDIENTE**

- Definir si se entrega una imagen de contenedor, instalador, paquete compilado u otra modalidad.
- Definir si el cliente recibe scripts, archivos de configuración y herramientas auxiliares.
- Definir si se entrega documentación técnica interna o solo documentación operativa.
- Definir el mecanismo de activación de licencia y el procedimiento en caso de sustitución de hardware.

### 4.3 Protección frente a copia y análisis no autorizado

**DECIDIDO**

Se intentará dificultar la copia, modificación, redistribución y análisis no autorizado del núcleo de FioRa mediante medidas técnicas razonables.

**RECOMENDACIÓN**

La protección debe combinar:

- Distribución sin el código fuente del núcleo.
- Empaquetado o compilación cuando sea adecuado para la tecnología utilizada.
- Ofuscación selectiva, sin considerarla una protección absoluta.
- Control de licencias y de integridad del paquete.
- Protección de secretos, credenciales y llaves fuera del ejecutable.
- Contrato que limite la reproducción, redistribución, modificación y uso no autorizado.

**REQUIERE REVISIÓN LEGAL**

No se deberá prometer que es técnicamente posible impedir toda ingeniería inversa. La Ley Federal del Derecho de Autor contempla derechos de autorización sobre reproducción, adaptación, distribución y ciertos procesos de ingeniería inversa, pero también prevé supuestos de excepción, entre ellos determinados actos de ingeniería inversa de buena fe para interoperabilidad y ciertas actividades de seguridad. Las restricciones contractuales y las medidas tecnológicas deben redactarse respetando los límites legales aplicables. Además, el decreto publicado el 14 de mayo de 2026 modificó, entre otros, el artículo 102 para mencionar expresamente los programas de computación, incluidos los de inteligencia artificial. Por ello, debe consultarse el texto consolidado junto con esa reforma posterior cuando se prepare el contrato.

## 5. Propiedad intelectual

### 5.1 Núcleo de FioRa

**DECIDIDO**

El núcleo funcional de FioRa, su arquitectura general, código fuente, componentes comunes, documentación interna, marca y elementos reutilizables generales permanecerán bajo titularidad de quien sea legalmente propietario del producto.

La identidad de la persona física o moral titular deberá definirse antes de comercializarlo.

**PENDIENTE**

- Definir quién será titular: persona física, sociedad existente o entidad que se constituya.
- Documentar las contribuciones de cada integrante del equipo.
- Formalizar la titularidad o cesión de derechos patrimoniales sobre el código, documentación, diseños y demás materiales creados para FioRa.
- Revisar derechos de terceros, bibliotecas, plantillas, código generado y componentes incorporados.

**RECOMENDACIÓN**

Todo colaborador, empleado o proveedor que participe en el desarrollo debe firmar un acuerdo escrito que determine confidencialidad, titularidad de las contribuciones, derechos de uso y entrega de materiales. No debe dejarse la propiedad del software a una interpretación informal del trabajo en equipo.

### 5.2 Adaptaciones particulares de clientes

**DECIDIDO**

Las adaptaciones particulares contratadas para un cliente serán propiedad de ese cliente.

Ejemplos posibles:

- Conectores exclusivos para el ERP del cliente.
- Transformaciones documentales específicas.
- Reglas o extensiones que respondan a un proceso exclusivo.
- Interfaces o módulos desarrollados expresamente para esa implementación.

**REQUIERE REVISIÓN LEGAL Y DEFINICIÓN TÉCNICA**

Debe separarse expresamente:

1. El núcleo de FioRa y sus componentes preexistentes.
2. La adaptación específica financiada por el cliente.
3. Las bibliotecas o componentes de terceros.
4. Las mejoras generales que puedan incorporarse al producto sin revelar información confidencial del cliente.

El contrato de cada adaptación deberá determinar qué derechos adquiere el cliente, si existe cesión de derechos patrimoniales, qué materiales se entregan y si el proveedor conserva una licencia para reutilizar técnicas, conocimientos o componentes genéricos que no contengan información confidencial ni código exclusivo del cliente.

**PUNTO DE CONFLICTO POR RESOLVER**

Se decidió que las adaptaciones particulares pertenecen al cliente, pero también se decidió que la entrega de FioRa será únicamente mediante ejecutables y que no se entregará el código fuente del producto. Hay que precisar si la regla de no entregar código fuente se limita al núcleo de FioRa o si también aplica a las adaptaciones del cliente. Si el cliente será titular de los derechos patrimoniales sobre una adaptación, el contrato deberá definir cómo se formaliza esa titularidad y qué archivos o fuentes se entregan.

### 5.3 Componentes de terceros y software de código abierto

**RECOMENDACIÓN / REQUIERE REVISIÓN**

FioRa podrá utilizar bibliotecas y componentes de terceros, pero deberá mantener un inventario de dependencias y sus licencias.

Antes de incorporar una dependencia se deberá revisar:

- Nombre, versión y procedencia.
- Licencia aplicable y obligaciones de atribución o redistribución.
- Si la licencia es compatible con la distribución comercial de FioRa.
- Obligaciones de incluir avisos, textos de licencia o código correspondiente.
- Riesgos de seguridad y mantenimiento.

No se deberá asumir que todo componente disponible públicamente puede incorporarse sin condiciones.

## 6. Marca FioRa

**PENDIENTE / REQUIERE REVISIÓN LEGAL**

- Realizar búsqueda de antecedentes y disponibilidad de la denominación FioRa ante el Instituto Mexicano de la Propiedad Industrial (IMPI).
- Determinar las clases de productos y servicios apropiadas.
- Decidir quién será titular de la marca.
- Definir si se registrarán denominación, logotipo o ambos.
- Acordar reglas de uso de marca por clientes, distribuidores y colaboradores.

El uso comercial de un nombre no equivale por sí solo a obtener el derecho exclusivo marcario. La estrategia deberá revisarse conforme a la Ley Federal de Protección a la Propiedad Industrial.

## 7. Datos, documentos e información del cliente

### 7.1 Titularidad y control

**DECIDIDO**

Los datos operativos, documentos, archivos y registros propios de la empresa permanecerán bajo control y titularidad del cliente. La licencia de FioRa no transfiere al proveedor la propiedad de esos datos.

El producto no se transfiere al cliente por el hecho de que sea dueño de sus datos.

### 7.2 Exportación y devolución

**DECIDIDO EN PRINCIPIO**

El cliente conservará la posibilidad de obtener sus datos y exportarlos, incluso si termina el soporte o deja de adquirir actualizaciones.

**PENDIENTE**

- Definir formatos de exportación (por ejemplo, CSV, XLSX, JSON, PDF o respaldo de base de datos, según corresponda).
- Definir qué elementos se consideran datos del cliente: registros, adjuntos, documentos originales, historial, configuraciones, prompts y logs.
- Definir procedimiento y costo de asistencia para una exportación extraordinaria.
- Definir periodo de conservación y eliminación de copias que pudiera mantener el proveedor durante soporte.
- Definir cómo se gestiona una devolución de datos al terminar un contrato de hosting administrado, si se ofrece.

## 8. Protección de datos personales y privacidad

**REQUIERE REVISIÓN LEGAL ANTES DE OPERAR CON DATOS PERSONALES**

FioRa puede tratar datos personales de empleados, operadores, usuarios, responsables, proveedores o contactos, por ejemplo nombres, identificadores de usuario, registros de actividad, comunicaciones y datos incluidos en documentos. La aplicabilidad de la Ley Federal de Protección de Datos Personales en Posesión de los Particulares deberá analizarse según los datos tratados y el papel que desempeñe cada parte.

### 8.1 Roles de las partes

No debe asumirse que FioRa o su proveedor siempre será el responsable de los datos personales. La función dependerá del flujo real:

- La empresa cliente normalmente determinará las finalidades y medios del tratamiento de los datos de sus trabajadores y operaciones.
- El proveedor de FioRa podría actuar como encargado cuando trate datos siguiendo instrucciones del cliente, por ejemplo durante soporte remoto o alojamiento administrado.
- El proveedor de FioRa será responsable independiente respecto de los datos personales que recabe para sus propios fines, como contactos comerciales, facturación o administración de licencias.
- Los proveedores de hosting, IA, correo y almacenamiento pueden intervenir como encargados, receptores o terceros, según el servicio y el contrato.

Los roles deberán reflejarse en los contratos y avisos correspondientes, no solo en el nombre que las partes les asignen.

### 8.2 Documentos y medidas

Antes de una implementación que trate datos personales deberán revisarse, según corresponda:

- Aviso de privacidad del cliente y del proveedor de FioRa para los datos que cada uno trate como responsable.
- Contrato de tratamiento por cuenta del cliente, cuando FioRa actúe como encargado.
- Identificación de finalidades, categorías de datos, personas titulares y periodos de conservación.
- Mecanismos para atender derechos ARCO.
- Medidas administrativas, técnicas y físicas de seguridad proporcionales al riesgo.
- Deber de confidencialidad de empleados y proveedores.
- Procedimiento para gestionar incidentes y vulneraciones de seguridad.
- Reglas de transferencia nacional o internacional de datos, cuando aplique.

La ley mexicana vigente exige informar el tratamiento mediante aviso de privacidad, observar principios de tratamiento y contar con medidas de seguridad; también regula incidentes de seguridad, confidencialidad y derechos ARCO.

### 8.3 Uso de proveedores de IA

**RECOMENDACIÓN**

La integración de IA no debe enviar indiscriminadamente documentos o datos del cliente a proveedores externos.

Antes de activar una integración deberá definirse:

- Qué información se enviará al proveedor de IA.
- Si se enviarán datos personales, confidenciales, industriales o secretos comerciales.
- Qué proveedor, modelo, región y condiciones de tratamiento se utilizarán.
- Si el proveedor utiliza o no datos para entrenamiento y bajo qué condiciones.
- Periodos de retención, controles de acceso y opciones de eliminación.
- Transferencias internacionales y subencargados.
- Quién administra y paga la cuenta y el consumo.
- Qué alternativas existen si el proveedor no está disponible.

Se recomienda minimizar, filtrar o seudonimizar información antes de enviarla, y obtener autorización expresa del cliente para las categorías de datos y servicios externos acordados. La IA no debe recibir credenciales de base de datos ni autoridad directa para ejecutar operaciones no validadas por el backend.

## 9. Seguridad, confidencialidad y continuidad

**DECIDIDO EN ALCANCE GENERAL**

La primera versión contemplará autenticación, permisos y registro de cambios asociado al usuario que realizó la acción.

**RECOMENDACIÓN / PENDIENTE DE ESPECIFICACIÓN**

La solución técnica y los contratos deberán contemplar, según el alcance contratado:

- Gestión de usuarios, roles y revocación de accesos.
- Protección de credenciales y secretos.
- Cifrado en tránsito y controles de acceso al servidor y a la base de datos.
- Registro de acciones relevantes y conservación de auditoría.
- Actualización de dependencias y corrección de vulnerabilidades.
- Respaldos y pruebas de restauración.
- Procedimiento de respuesta a incidentes.
- Confidencialidad para personal, contratistas y proveedores.
- Separación de ambientes y datos de prueba, cuando corresponda.

No se deberá afirmar que FioRa garantiza seguridad absoluta ni que cumple automáticamente cualquier certificación o estándar. La seguridad depende tanto del producto como de la infraestructura, configuración, operación y prácticas del cliente.

### 9.1 Responsabilidad de infraestructura

En instalación local, el cliente normalmente controlará hardware, red, energía, acceso físico y respaldos, salvo acuerdo distinto.

En hosting administrado, el contrato deberá distribuir responsabilidades entre cliente, proveedor de hosting y proveedor de FioRa, incluyendo actualizaciones del sistema operativo, monitoreo, respaldos, recuperación, incidentes y costos.

**PENDIENTE:** definir RPO, RTO, frecuencia de respaldo, retención, ubicación y prueba de restauración para cada modalidad de despliegue.

## 10. Garantía, soporte y mantenimiento

### 10.1 Garantía inicial

**DECIDIDO**

FioRa incluirá un periodo inicial de garantía, cuya duración todavía no está definida.

**PENDIENTE**

- Duración y fecha de inicio.
- Definición de defecto cubierto.
- Procedimiento para reportar y reproducir fallas.
- Tiempos de respuesta y corrección, si se ofrecen.
- Exclusiones por cambios no autorizados, infraestructura, uso indebido, datos de entrada defectuosos o servicios de terceros.
- Criterios de aceptación de la instalación y de las adaptaciones.

### 10.2 Soporte después de garantía

**DECIDIDO**

Terminado el periodo de garantía, cualquier intervención de soporte se atenderá mediante una contratación y tarifa independiente.

**PENDIENTE**

- Modalidad de cobro: por incidente, hora, bolsa de horas o proyecto.
- Horarios y canales de atención.
- Clasificación de severidad.
- Tiempo de respuesta objetivo.
- Costos de diagnóstico, desplazamiento y asistencia presencial.
- Tratamiento de incidencias ocasionadas por terceros.

### 10.3 Diferencia entre garantía, soporte y mantenimiento

Para evitar interpretaciones ambiguas, los documentos comerciales deberán distinguir:

- **Garantía:** corrección de defectos cubiertos durante el periodo acordado.
- **Soporte:** asistencia para diagnosticar, operar o resolver problemas concretos.
- **Mantenimiento:** tareas planificadas para conservar el funcionamiento, actualizar dependencias o revisar el entorno.
- **Desarrollo:** nuevas funciones, integraciones o cambios de alcance.

## 11. Actualizaciones y nuevas versiones

**DECIDIDO**

Las actualizaciones futuras no estarán incluidas automáticamente en el pago único de la licencia.

Se establecerá una tarifa base para actualizaciones generales y un cargo adicional de acuerdo con la especialización o adaptación que requiera cada empresa.

**PENDIENTE**

- Qué se entiende por actualización general y qué por especialización.
- Si cada actualización es opcional o si algunas serán necesarias por motivos de seguridad o compatibilidad.
- Política de versiones soportadas y duración del soporte de versiones anteriores.
- Precio de migraciones de base de datos y adaptación de conectores.
- Procedimiento de pruebas, respaldo y reversión antes de actualizar.
- Qué sucede si el cliente decide permanecer en una versión antigua.

La licencia de uso indefinido de la versión adquirida debe distinguirse del derecho a recibir nuevas versiones, soporte o mantenimiento futuro.

## 12. Servicios y dependencias de terceros

**DECIDIDO**

Los costos de IA, hosting, infraestructura y otros servicios externos no quedan cubiertos por la licencia de pago único.

**RECOMENDACIÓN**

La documentación de implementación deberá identificar los servicios externos requeridos y sus responsables. Los contratos deberán aclarar que:

- El tercero establece sus propios términos, disponibilidad, precios y límites.
- Cambios o interrupciones del tercero pueden requerir una adaptación cotizable.
- El cliente debe mantener las cuentas, pagos y credenciales cuando sea el titular de esos servicios.
- El proveedor de FioRa no debe solicitar ni almacenar contraseñas personales cuando exista un mecanismo autorizado de OAuth, tokens o API.
- Las condiciones de privacidad y transferencia de datos de cada proveedor deben revisarse antes de integrarlo.

## 13. Implementación y aceptación por cliente

**RECOMENDACIÓN**

Cada venta deberá tener una propuesta y un alcance de implementación que identifiquen:

- La modalidad de despliegue.
- La versión y módulos incluidos.
- Los procesos y casos de uso contratados.
- Las integraciones y formatos incluidos.
- Las adaptaciones que serán propiedad del cliente.
- Requisitos de infraestructura y servicios externos.
- Responsabilidades y accesos que proporcionará el cliente.
- Datos de prueba, criterios de aceptación y entregables.
- Plan de instalación, capacitación y puesta en marcha.
- Precio, calendario de pago y condiciones de cambio de alcance.

No se deberá considerar incluida una función, integración o nivel de servicio que no aparezca en el alcance contratado.

## 14. Condiciones económicas, facturación e impuestos

**REQUIERE REVISIÓN FISCAL**

Antes de realizar ventas deberán definirse:

- Quién será la persona física o moral que venderá y facturará FioRa.
- Régimen fiscal, obligaciones y actividad económica correspondiente.
- Tratamiento fiscal de la licencia, implementación, adaptaciones, soporte, mantenimiento y actualizaciones.
- Impuestos aplicables, retenciones y tratamiento de pagos nacionales o extranjeros.
- Emisión de CFDI cuando exista obligación fiscal.
- Tratamiento de anticipos, pagos parciales, reembolsos y cancelaciones.
- Facturación de servicios externos cuando se contraten por cuenta del cliente.

No se fija aquí una tasa ni tratamiento tributario específico. Debe confirmarlo un contador con base en la estructura legal y operación real del negocio.

## 15. Confidencialidad y secretos industriales

**RECOMENDACIÓN / REQUIERE REVISIÓN LEGAL**

El contrato deberá establecer obligaciones de confidencialidad para la información técnica, comercial, operativa y documental de ambas partes.

La información del cliente puede incluir procesos, listas, precios, diseños, documentos, datos de fabricación, configuraciones, credenciales y otros conocimientos empresariales. Se deberán definir:

- Qué información se considera confidencial.
- Usos autorizados y personas con acceso.
- Excepciones para información pública, conocida previamente o recibida legítimamente de terceros.
- Medidas para protegerla.
- Plazo de confidencialidad.
- Devolución o destrucción al terminar la relación.
- Tratamiento de información incorporada a respaldos y registros de auditoría.

Cuando la información reúna los requisitos legales de secreto industrial, el cliente deberá identificarla y adoptar medidas razonables de confidencialidad; FioRa y sus colaboradores deberán respetar las obligaciones acordadas.

## 16. Responsabilidad, disponibilidad y límites del producto

**PENDIENTE / REQUIERE REVISIÓN LEGAL**

Los contratos deberán definir con precisión:

- Responsabilidad del proveedor por defectos atribuibles al software.
- Responsabilidad del cliente por datos incorrectos, permisos, operación e infraestructura que administra.
- Responsabilidad de proveedores externos por sus servicios.
- Alcance de la obligación de respaldar y recuperar información.
- Tratamiento de interrupciones, errores de integración y cambios de sistemas externos.
- Exclusiones permitidas por ley y límites de responsabilidad que sean jurídicamente válidos.
- Procedimiento de reclamaciones y mitigación de daños.
- Fuerza mayor y eventos fuera del control razonable de las partes.

No se deberá introducir una exención general que pretenda eliminar responsabilidades que legalmente no puedan excluirse.

## 17. Documentos contractuales que se deberán preparar

La comercialización de FioRa probablemente requerirá un conjunto de documentos coordinados, no un único texto genérico:

1. **Contrato marco de licencia:** regula el derecho de uso del núcleo de FioRa.
2. **Anexo de alcance o pedido:** versión, módulos, instalación, usuarios, sucursales y precio.
3. **Orden de implementación / Statement of Work:** instalación, integraciones, entregables, pruebas y aceptación.
4. **Acuerdo de adaptación:** alcance, propiedad intelectual, archivos entregables y derechos sobre componentes específicos.
5. **Acuerdo de confidencialidad:** cuando la información compartida lo justifique.
6. **Anexo de tratamiento de datos personales:** cuando FioRa o sus proveedores traten datos por cuenta del cliente.
7. **Política de privacidad:** para los datos personales que el titular del producto recabe para sus propios fines.
8. **Anexo de soporte y mantenimiento:** garantía, servicios, tarifas y tiempos de atención.
9. **Inventario de componentes de terceros:** dependencias, licencias y avisos aplicables.

La estructura definitiva dependerá de si el cliente es una persona moral, de la modalidad de despliegue y de los servicios efectivamente contratados.

## 18. Lista de decisiones pendientes

### Producto y licencia

- [ ] Identificar al titular legal de FioRa.
- [ ] Definir alcance territorial de la licencia.
- [ ] Definir si la licencia cubre una instalación, varias sucursales o centros de trabajo.
- [ ] Definir límites por usuarios, equipos o servidores, si se aplican.
- [ ] Definir transferibilidad, sublicencias y uso por empresas afiliadas.
- [ ] Definir mecanismo de activación y recuperación de licencia.

### Propiedad intelectual

- [ ] Documentar derechos de cada integrante y colaborador.
- [ ] Formalizar titularidad del núcleo de FioRa.
- [ ] Resolver si se entrega código fuente de adaptaciones propiedad del cliente.
- [ ] Definir qué componentes genéricos derivados de una adaptación pueden reutilizarse.
- [ ] Revisar licencias de terceros.
- [ ] Realizar búsqueda y, en su caso, solicitud de registro de la marca FioRa.

### Datos y privacidad

- [ ] Definir roles de responsable y encargado por modalidad.
- [ ] Preparar avisos de privacidad necesarios.
- [ ] Definir contrato de tratamiento de datos.
- [ ] Determinar qué datos pueden enviarse a proveedores de IA.
- [ ] Definir exportación, conservación, devolución y eliminación.
- [ ] Definir respuesta a incidentes y atención de derechos ARCO.

### Operación y servicios

- [ ] Definir duración y alcance de garantía.
- [ ] Definir tarifas y condiciones de soporte.
- [ ] Definir tarifa base de actualizaciones y cargos por especialización.
- [ ] Definir responsabilidades de hosting y respaldos.
- [ ] Definir RPO, RTO y retención de respaldos.
- [ ] Definir criterios de aceptación de cada implementación.

### Comercial y fiscal

- [ ] Definir entidad o persona que comercializará FioRa.
- [ ] Revisar régimen y obligaciones fiscales.
- [ ] Definir estructura de cotización, pagos, anticipos y CFDI.
- [ ] Definir ley aplicable, jurisdicción, idioma y mecanismo de solución de controversias en contratos.

## 19. Referencias normativas iniciales — México

Las siguientes fuentes oficiales sirven como punto de partida. Deben verificarse nuevamente antes de la comercialización y al redactar los contratos.

1. **Ley Federal del Derecho de Autor (LFDA).** En particular, artículos 101–106 sobre programas de computación, bases de datos y derechos patrimoniales; artículos 114 Bis y 114 Quáter sobre medidas tecnológicas y determinados supuestos de excepción. El PDF consolidado de Cámara de Diputados consultado indica última reforma de 15 de enero de 2026; debe complementarse con el decreto publicado en el DOF el 14 de mayo de 2026, que reformó, entre otros, el artículo 102.  
   Texto consolidado: <https://www.diputados.gob.mx/LeyesBiblio/pdf/LFDA.pdf>  
   Reforma del 14 de mayo de 2026: <https://dof.gob.mx/nota_detalle.php?codigo=5787418&fecha=14/05/2026>
2. **Ley Federal de Protección de Datos Personales en Posesión de los Particulares (LFPDPPP).** Nueva ley publicada el 20 de marzo de 2025; texto consultado con última reforma publicada el 14 de noviembre de 2025. Revisar, entre otros, principios, aviso de privacidad, seguridad, confidencialidad, derechos ARCO y transferencias.  
   <https://www.diputados.gob.mx/LeyesBiblio/pdf/LFPDPPP.pdf>
3. **Ley Federal de Protección a la Propiedad Industrial (LFPPI).** Revisar el régimen de marcas y secretos industriales; en particular, artículos 170–172 y disposiciones sobre secretos industriales. Texto consultado con última reforma publicada el 15 de enero de 2026.  
   <https://www.diputados.gob.mx/LeyesBiblio/pdf/LFPPI.pdf>
4. **Código de Comercio.** Revisar las disposiciones sobre comercio electrónico, mensajes de datos y firmas electrónicas para formalizar propuestas, aceptaciones y contratos electrónicos.  
   <https://www.diputados.gob.mx/LeyesBiblio/pdf/CCom.pdf>
5. **Código Fiscal de la Federación.** Revisar artículos 29 y 29-A y reglas fiscales vigentes para la emisión de CFDI, según corresponda a la operación real. Texto consultado con última reforma publicada el 14 de noviembre de 2025.  
   <https://www.diputados.gob.mx/LeyesBiblio/pdf/CFF.pdf>
6. **Servicio de Administración Tributaria — CFDI.** Información y especificaciones oficiales para comprobantes fiscales digitales.  
   <https://www.sat.gob.mx/>
7. **Instituto Mexicano de la Propiedad Industrial (IMPI).** Búsqueda de antecedentes y trámites de registro de marca.  
   <https://www.gob.mx/impi>

---

## 20. Estado del documento

Este documento reúne el modelo comercial y las decisiones jurídicas iniciales discutidas para FioRa. No sustituye la revisión de un abogado ni del contador que asesore al titular del producto.

Las decisiones pendientes se conservarán como tales hasta que el equipo las resuelva. Cualquier cambio deberá reflejarse en una nueva versión para mantener trazabilidad de las decisiones comerciales y legales.
