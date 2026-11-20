# Flujo de trabajo Git

## Configuración inicial

Configura tu identidad de commits (no es lo mismo que autenticarte en GitHub):

```bash
git config --global user.name "Tu nombre"
git config --global user.email "correo-verificado-en-github"
```

Comprueba:

```bash
git config --global --list
```

## Inicializar este proyecto

Desde la carpeta raíz `FioRa/`:

```bash
git init -b main
git add .
git commit -m "chore: initial FioRa project structure"
```

Crea en GitHub un repositorio privado vacío y conecta el remoto:

```bash
git remote add origin https://github.com/PROPIETARIO/fiora.git
git push -u origin main
```

## Trabajo por tarea

```bash
git switch -c feature/nombre-de-la-tarea
# realizar cambios
git status
git add ruta/de/los/archivos
git commit -m "feat: descripcion breve"
git push -u origin feature/nombre-de-la-tarea
```

Abre un Pull Request hacia `main`, solicita revisión y fusiona después de validar.

## Convención de commits sugerida

- `feat:` funcionalidad.
- `fix:` corrección.
- `docs:` documentación.
- `refactor:` reorganización sin cambio funcional intencional.
- `test:` pruebas.
- `chore:` mantenimiento o configuración.

## Nota

No configures un remoto ni ejecutes `git init` desde tu carpeta personal. Los comandos de inicialización deben ejecutarse desde la raíz descomprimida de este proyecto.
