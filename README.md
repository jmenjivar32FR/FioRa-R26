# FioRa

**Producto propietario · Estructura inicial del repositorio**  
**Versión de estructura:** 0.1  
**Estado:** esqueleto inicial; todavía no contiene una implementación funcional.

## 1. Qué es FioRa

FioRa es una herramienta especializada en el control de estados, validaciones y trazabilidad de operaciones relacionadas con materiales y recursos físicos. Complementa al ERP y atiende el espacio operativo entre la ejecución física y el registro formal de las operaciones.

FioRa **no es un ERP, no es un sistema de inventario físico y no administra la producción**. Por defecto, no determina existencias ni disponibilidad. Su función principal es recibir o registrar objetos operativos, identificarlos, gestionar sus estados, permitir cambios controlados y conservar su historial.

El primer flujo de referencia es la transferencia de materiales desde inventario hacia producción, entendida como seguimiento de objetos y estados, no como administración integral del almacén o de la producción.

## 2. Decisiones que guían esta estructura

- Backend desarrollado en **Python**.
- **PostgreSQL** como base de datos principal.
- **Monolito modular** como dirección arquitectónica inicial.
- Una instancia independiente de FioRa por cliente.
- Núcleo común y adaptaciones de cliente encapsuladas.
- Cambios manuales y cambios solicitados por el asistente de IA pasan por el backend.
- El backend aplica autorización, validaciones e integridad antes de modificar datos.
- El historial de modificaciones debe permitir identificar actor, momento, objeto afectado y cambio.
- Importación de listas (por ejemplo, Excel) para identificar objetos operativos.
- El framework web, ORM, librerías, proveedor de IA y tecnología frontend no se fijan en este esqueleto.

Consulta el [Documento Maestro](docs/product/FioRa_Documento_Maestro_Producto_y_Arquitectura_v0.1.md) para las decisiones completas y los pendientes.

## 3. Estructura del repositorio

```text
FioRa/
├── backend/
│   ├── src/fiora/
│   │   ├── core/                 # Capacidades comunes y estables
│   │   ├── modules/              # Flujos de dominio propios de FioRa
│   │   ├── capabilities/         # Importación y asistencia IA
│   │   ├── infrastructure/       # Adaptadores técnicos concretos
│   │   └── bootstrap/            # Ensamblaje e inicialización
│   └── tests/
├── frontend/                     # Reservado; tecnología por confirmar
├── extensions/                   # Política y ubicación conceptual de extensiones
├── docs/
│   ├── product/                  # Documento maestro y definición del producto
│   ├── commercial/               # Modelo comercial y marco legal preliminar
│   ├── architecture/             # Arquitectura y ADR
│   ├── development/              # Guías de trabajo
│   └── references/               # Propuestas y materiales históricos
├── deploy/                       # Despliegue; se completará al definirlo
└── scripts/                      # Herramientas de desarrollo y mantenimiento
```

La estructura es intencionalmente inicial. Las carpetas reservan límites de responsabilidad; **no significan que todos esos componentes ya estén aprobados en detalle ni que deban implementarse todos en el MVP**.

## 4. Estado de las decisiones técnicas

| Elemento | Estado |
|---|---|
| Python | Decidido |
| PostgreSQL | Decidido |
| Monolito modular | Dirección arquitectónica acordada |
| Framework API | Pendiente |
| ORM / acceso a datos | Pendiente |
| Herramienta de migraciones | Pendiente |
| Framework frontend | Pendiente |
| Proveedor y modelo de IA | Pendiente |
| Docker / Compose | A evaluar durante el diseño de despliegue |
| NoSQL | No incorporar sin una necesidad demostrable |

No se agregaron dependencias de Python ni archivos de instalación de paquetes porque todavía no se ha elegido el framework, ORM, gestor de dependencias ni versiones de runtime.

## 5. Primeros pasos con Git

Descomprime el ZIP en la ubicación donde guardarás el proyecto. Abre Git Bash dentro de la carpeta `FioRa` y ejecuta:

```bash
git init -b main
git add .
git commit -m "chore: initial FioRa project structure"
```

Después crea en GitHub un repositorio **privado y vacío** llamado `fiora` dentro de la cuenta u organización que hayas decidido. No marques las opciones para crear README, `.gitignore` o licencia, porque esos archivos ya están incluidos aquí.

Conecta el repositorio remoto (sustituye el propietario por tu usuario u organización):

```bash
git remote add origin https://github.com/PROPIETARIO/fiora.git
git push -u origin main
```

Comprueba la configuración con:

```bash
git status
git remote -v
git log -1 --oneline
```

Si Git solicita autenticación, utiliza la cuenta nueva de GitHub que ya vinculaste mediante Git Credential Manager.

## 6. Reglas básicas de seguridad del repositorio

- No subir archivos `.env` reales, contraseñas, tokens, claves API ni credenciales.
- No subir datos reales de empresas, listas de producción, documentos de clientes, exportaciones ni respaldos de bases de datos.
- Utilizar datos ficticios o anonimizados en pruebas y demostraciones.
- No añadir código de adaptaciones propiedad de clientes al repositorio común sin autorización contractual expresa.
- Mantener el repositorio privado.
- No añadir una licencia de código abierto. La política de distribución es propietaria y debe formalizarse legalmente.

## 7. Convención de trabajo inicial

- `main`: rama estable; no desarrollar directamente sobre ella.
- `feature/<nombre>`: nuevas funcionalidades.
- `fix/<nombre>`: correcciones.
- `docs/<nombre>`: cambios de documentación.
- Integrar mediante Pull Request y revisión del otro integrante.
- Registrar decisiones arquitectónicas relevantes en `docs/architecture/decisions/`.

Consulta [Git workflow](docs/development/git-workflow.md) antes de comenzar a trabajar en equipo.

## 8. Documentos de referencia

- [Documento Maestro de Producto y Arquitectura](docs/product/FioRa_Documento_Maestro_Producto_y_Arquitectura_v0.1.md): referencia principal.
- [Modelo Comercial y Marco Legal Preliminar](docs/commercial/FioRa_Modelo_Comercial_y_Marco_Legal_Preliminar_v0.1.md): complemento; no es un contrato listo para firma.
- [Propuesta de Arquitectura v0.5](docs/references/Propuesta_Arquitectura_Capa_Operativa_ERP_v0.5.md): antecedente histórico; cuando exista una diferencia, prevalece el Documento Maestro y las decisiones posteriores confirmadas.

---

**Importante:** este repositorio es una base de organización. Antes de escribir la implementación se deberán cerrar el modelo de objeto operativo, el motor de estados, el formato de importación y los contratos entre módulos.
